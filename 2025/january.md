# January? It's done already?

Journal of things I worked on and some other items I accomplished in January of 2025.

#### _"In rode the Lord of the Nazgûl. A great black shape against the fires beyond he loomed up, grown to a vast menace of despair. In rode the Lord of the Nazgûl, under the archway that no enemy ever yet had passed, and all fled before his face._

#### _"All save one. There waiting, silent and still in the space before the Gate, sat Gandalf upon Shadowfax: Shadowfax who alone among the free horses of the earth endured the terror, unmoving, steadfast as a graven image in Rath Dínen. "You cannot enter here," said Gandalf, and the huge shadow halted. "Go back to the abyss prepared for you! Go back! Fall into the nothingness that awaits you and your Master. Go!"_ 

Finished reading the Lord of the Rings Trilogy which I had started reading with my son during the pandemic. Unfortunately, my son is now too old for "reading with pops" (no, no I am not crying, something must have just gotten into my eyes).

## High level

Started using Cursor + Composer as the main workflow for coding. Generally had a very positive experience with it—except it has annoying aspect of constantly crashing and needing to restart which I haven't been able to fix yet. Using Sonnet 3.5 almost exclusively as my coding LLM (though I will occassionally use ChatGPT for questions that require direct Internet access).


## AI Coding Items

Created a Python program using AI and Whisper to translate my course videos (.mp4 files) to text.

Built a summary Python tool that would consolidate all of the transcripts for Ansible Course lesson1 and other reference material into a single file that I could dump into the Claude context (to later ask questions of Claude with respect to the course material).

Create a Custom GPT in OpenAI that you could use to ask questions about lesson1 of my Ansible Course.

Built an initial version of a Slack-bot solution that would communicate between Slack - my code - Claude Sonnet to answer questions about lesson1 of my Ansible Course.

Implemented a RAG solution (ChromaDB) to improve my Ansible Course Q & A Slack bot.

Transcribed Ansible Course class2 using the Python Whisper solution.

Started refactoring my Ansible Course Q & A Slack bot. Basically, the original code was a convoluted mess—way too much coupling of various parts of the system, too much logging and token tracking mixed into the code, and very hard to follow the program flow. The refactoring included:
- Created a separate LLMInterface using a Python Abstract Base Class
- Implemented tests for this LLMInterface and for the Anthropic implemenation of this Interface.
- Started implementing a new CourseQA class with tests.
- Implemented a new RAG class with tests.
- Integrated CourseQA class with the RAG class.
- Verified communication with Claude Anthropic is working via the API and via the CourseQA class (using the new LLMInterface)
- Added integration tests that actually test the CourseQA to Claude communication.
- Added in the Slack Bot code including tests.
- Integration test of the Slack Bot code. This took quite a bit of time as I ultimately needed two Bots here—the actual Ansible Course Bot and then a PyTest bot. The PyTest bot connects via the WebClient and asks questions to the Ansible Course Bot. The Ansible Course Bot runs in a separate thread and answers questions. The PyTest bot verifies these responses.
- Added a "tools" section into the LLMInterface. The tools initially will be `list_files`, `file_loader`, `retrieve_rag`. Basically Slack will communicate to the LLM via my CourseQA code and the LLM will make decisions on retrieving files and retrieving RAG based on what is asked via Slack. In other words, the LLM will call these tools when it needs to (hopefully).


## Internal Tools and Cost Reductions

Worked on reducing AWS daily costs particularly in the EC2-Other category. This mainly consists of deleting old AMIs and Snapshots. 

Created an Ansible playbook to list all of my AMIs in the us-west1 region (to assist in the cost reduction process). 

Created an Ansible playbook with a set of verifications that remove an AMI and a Snapshot that I specify.

Reduced AWS daily "EC2-Other" costs from $17.15/day to $14.34/day. This savings reduces my AWS spend by about $1000 per year.


## Courses and Misc

Started the Ansible Network Automation Course (session number27) including building the lab environment and all the standard items associated with this.

Finished reviewing business accounting books for 2024 so that I could hand them over to the accountant.


## Open Source Work (Netmiko / NAPALM / Nornir)

Released Nornir 3.5

Reviewed and merged three PRs into Netmiko.

Fixed and verified Nornir license classifier breakage.

Reviewed and merged one PR for NAPALM.

_Not really the best month for open source work_


## Exercise and My Crazy Running Addiction

Ran about 145 miles in January with 8750ft in elevation (roughly 22 runs and about 24 total hours running).

Roughly nine other workouts during the month (roughly 7 hours, exercise bike, treadmill, some weights).

Races / time trials — Ran a ~40 minute 10K time trial depending on which data you believe (39:20 on Strava; 40:40 on Garmin). Pretty happy with this for age 53.

Favorite running trail of the month—Coastal Trail in Marin. Watch the [Miller vs Hawks YouTube Video]([https://pages.github.com/](https://www.youtube.com/watch?v=7DCR03UDggA&t=318s).

Went skiing with friends for two days in Aspen.


## Favorite Family Board Game

Definitely Terraforming Mars. We purchased TM Prelude for Christmas so this added a nice wrinkle to our all time family favorite game.


## Favorite Song / Music

Became a bit addicted to listening to "Present Tense" by Pearl Jam. With a second place to "Stick Season" by Noah Kahan.

