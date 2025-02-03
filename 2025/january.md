# What happened in January

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
17. Integrating a custom prompt loaded from a file in.
18. Adding / updating tests for this custom prompt.
19. Adding "tools" section into the LLM Interface. The tools initially will be `list_files`, `file_loader`, `retrieve_rag`. Basically Slack will communicate to LLM via my CourseQA code and the LLM will make decisions on retrieving files and retrieving RAG based on what is asked.
20. Adding tests for this initial tool definition mainly focussed on the file_loader tool.


What else?

Worked on reducing AWS daily costs particularly in the EC2-Other category. This mainly consists of deleting old AMIs and Snapshots. Created an Ansible Playbook to list all of my AMIs in the US-West1 region. Created an Ansible playbook with a set of checks to remove an AMI and Snapshot that I specify.

Reduced AWS daily EC2-other costs from $17.15/day to $14.34/day. This saves $2.81 per day ($84 per month or about $1000 per year).


What else?

Ran the Ansible Network Automation Course session including building the lab environment and all the standard items associated with this.


What else?

Running in January ~145 miles, 8750ft in elevation (22 runs)  24 hours
9 other workouts (ex. bike, treadmill, ex bike)  7 hours

Ran ~40 minute 10K (39:20 on Strava / 40:40 on Garmin) so depending on which one you believe. Garmin was a bit wonky at points during the run.


What else?

Trip to Aspen with friends. Skied two days.

Anything else?
