# February

A journal of things I worked on, find interesting, and accomplished in February 2025. Not, the best month in terms of productivity—oh well.

#### _"It was the best of times, it was the worst of times, it was the age of wisdom, it was the age of foolishness, it was the epoch of belief, it was the epoch of incredulity"_

<br />

## High level

Diverse set of items that I worked on this month:
- New form of open-source sponsorship, education, and support packages.
- Corporate training proposal.
- Tool implementation and testing for my AI-RAG Ansible Course Slackbot.
- Finished 2024 business taxes (good times).
- Numerous contributions to both Netmiko and NAPALM projects.

<br />

## Open-source Sponsorship / Education / Support Packages

For a long time I have realized that the open-source sponsorship model doesn't really work. This should not be surprising, developers are basically saying "I will do this work for free" and then holding out a tip jar. This is not a workable business model.

At AutoCon2 in Denver, I decided I would try a new model where (hopefully) I could continue supporting both Netmiko and Nornir, but receive increased financial support. I also felt that in 2023 and 2024, I had done a relatively poor job of maintaining Netmiko. Finally, I was somewhat losing interest in maintaining Netmiko and was not certain I wanted to continue doing so.

In February, I read a bunch of articles on developers that had been (somewhat) successful in funding their open-source projects. From this I started to formalize a new structure which is a combination of education, sponsorship, and maintenance/support.

At about the same time, I also continued discussions with the teams at Netpicker and Slurp'it regarding Netmiko sponsorship. Both of these products use Netmiko internally and they also have made meaningful contributions to open-source especially with the Mockit library. This ultimately led to a Netmiko sponsorship from the Netpicker and Slurp'it teams.

So thanks to Wim, Pieter, and the Netpicker/Slurp'it team. :tada: 

These products both look fairly interesting so it might be worthwhile for you and your organization to investigate them:

- [https://netpicker.io/](https://netpicker.io/) - Test the design, security and compliance of your network
- [https://slurpit.io/](https://slurpit.io/) - Easy network inventory discovery

<br />

_If this post leads you to becoming a Netpicker/Slurp'it customer please mention it to Wim, Pieter, and the Netpicker/Slurp'it folks (as this will help my cause that Netmiko sponsorship is a win for them)._

I hope to have a web page available for these new packages shortly, but currently it is left as work-in-progress (somewhere Eliyahu Goldratt is looking down on me and upset at this).

<br />

## AI Coding Items

I only did limited work here and mostly at the beginning of the month (so I am a bit disappointed on this).

Early in February, I continued working on my Ansible Course Slackbot and RAG solution. I added some additional tool calling to the LLM interface including tests for this code.

I continue to use Cursor plus Composer as my main AI coding assistant.

<br />

## Open Source Work (Netmiko / NAPALM / Nornir)

I did quite a bit of work on open-source work this month.

I ended up creating, reviewing, and/or merging about ten pull requests in the Netmiko. 

Besides the Netmiko PR work, I also worked on numerous Netmiko issues, merged 1 pull-request for NAPALM-Ansible, and reviewed/merged two pull-requests for NAPALM. I also did a set of very minor dependency management for the NAPALM project.

Probably the most interesting issue I worked on was a Juniper ScreenOS issue. Basically, ScreenOS can be configured to, "Accept this agreement" as part of the SSH login process. I fixed this issue originally in 2021, but a regression had occurred in Netmiko 4.x that had caused this to break again.

I tried three different fixes for the problem:

1. Fix1 converted the matching regex pattern to a "non-capture" group.

```diff
- pattern = rf"(Accept this|{terminator})"
+ pattern = rf"(?:Accept this|{terminator})"
```

I implemented this fix as the debugging/logs indicated that the regex pattern was using parenthesis and I know from past experience (and from the log message) that parenthesis usage can be problematic.

Basically, parenthesis in regex can be used as a logical-or (for example, `(pattern1|patern2)`) and it can also be used as a capture-group whereby you intend to save what is between the parenthesis so you can use it later. 

Usually, Netmiko patterns are using it for the logical-or case and you do NOT want the capture group behavior so you add the "?:" to the beginning of the parentheses `(?:pattern1|pattern2)`. There is a reason capture groups cause problems in Netmiko, but that would be an even deeper white rabbit search.

Anyways this fix didn't work :-)

2. I noticed in the logging/debug output for the failure that an 'enter' (\n) was being sent just before the "Accept this agreement" message and it made me realize that this 'enter' was probably being interpreted by the ScreenOS device as a rejection of the "Accept this agreement". The literal message on the ScreenOS device is "Accept this agreement y/[n] ". So you can see the default for an "enter" is a "n" (i.e. reject the agreement). I was also seeing an "EOF received" message right after this "enter". So this was telling me that the remote device was closing the SSH channel right after the enter:

```
2025-02-25 14:11:23,097 DEBUG, netmiko, write_channel: b'\n'
2025-02-25 14:11:23,177 DEBUG, paramiko.transport, [chan 0] EOF received (0)
```

Now the question was—what in the heck is sending the extra "enter".

My first thought was that it was due to Netmiko's `_test_channel_read()` method which is being called very shortly after the standard SSH login process has completed. This basically ensures Netmiko can read from the remote device. I (vaguely) recalled that this method in certain contexts might send "enters" in order to get data back. For example, to get the prompt to come back to you.

So I implemented a fix whereby I guaranteed I would not send any "enters" here.

```python
pattern = rf"(?:Accept this.*|{terminator})"
data = self.read_until_pattern(pattern=pattern)
```

This fix didn't work and closer inspection of how `_test_channel_read()` was being called revealed it would not send an "enter" in this case anyways. Looking at the logs after implementing this fix showed that the "enter" and subsequent "EOF received" were still there.

3. Fix number 3 (the one that worked)

So I looked more closely at the Netmiko code particularly what happens immediately after the SSH login completes and I was lead to this:

```python
    def _try_session_preparation(self, force_data: bool = True) -> None:
        """
        In case of an exception happening during `session_preparation()` Netmiko should
        gracefully clean-up after itself. This might be challenging for library users
        to do since they do not have a reference to the object. This is possibly related
        to threads used in Paramiko.
        """
        try:
            # Netmiko needs there to be data for session_preparation to work.
            if force_data:
                self.write_channel(self.RETURN)
                time.sleep(0.1)
            self.session_preparation()
        except Exception:
            self.disconnect()
            raise
```

So basically, very shortly after the SSH login process Netmiko wants to call the `session_preparation` method, but `session_preparation` generally requires there to be data present for reading so by default I send an "enter".

The simple fix was to not send the "enter" by overriding the `_try_session_preparation()` method in the child class:

```python
    def _try_session_preparation(self, force_data: bool = False) -> None:
        return super()._try_session_preparation(force_data=force_data)
```

There is a slight possibility that this fix will break things if the 'Accept this' banner is not present so I am trying to have that case regression tested.

Anyways that was an interesting problem :-)

Besides the Netmiko PR work, I also worked on numerous Netmiko issues, merged 1 pull-request for napalm-ansible and reviewed/merged two pull-requests for NAPALM. I also did a bunch of very minor dependency management for NAPALM.

My last contribution in the open-source world was finally convincing the Juniper PyEZ folks to implement a fix for telnetlib and PyEZ. Basically PY3.13 decided to remove telnetlib from being included as part of the Python standard library. Consequently, other libraries that depended on telnetlib needed to implement some fix for it (in order to suppoort PY3.13).

I had fixed this in Netmiko in June of 2024, but had been unable to fix it in NAPALM since NAPALM depends on PyEZ. This had been an outstanding issue in PyEZ since at least Sept of 2024 (PY3.13 was officially released in October of 2024). I proposed a fix for it in November of 2024 (or at least told them what I did for Netmiko). I created a PR to PyEZ for it in December of 2024.

Then I nagged in various ways.

Finally, in February I nagged more publicly and this got the issue fixed. I don't super like nagging publicly, but this issue did really need to get fixed.


## Exercise and My Crazy Running Addiction

Ran about 124 miles in February with about 10,500 in elevation (roughly 18 runs and about 24 total hours running).

Roughly seven other workouts during the month (roughly 4.5 hours, exercise bike, treadmill, some weights).

Six days of skiing at Northstar.

Favorite running trail of the month—Dipsea Trail from Stinson Beach to Cardiac and back again. It is hard to beat running downhill through the Moors and heading into Stinson Beach.

In total, including the skiing was pretty happy with total exercise.


## Favorite Family Board Game

We were still addicted to Terraforming Mars with Preludes. By the end of the month, I was a bit burned out on this game so we switched over and played Under Water Cities which I quite enjoyed.


## Favorite Song / Music

Probably my favorite new song was "Love It If We Made It". Yes, I am pretty out of touch with pop culture so I might be five to ten years late on hearing a song.


## Courses and Misc

Started Netmiko course instance number9. Probably the last standard session of this course that I will run.

Started my free Learning Python for Network Engineers Course also in February.
