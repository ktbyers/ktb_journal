# Hello

The code creation was done using Cursor + Composer on my Apple laptop.

Created a Python program using AI and Whisper to translate my course videos (.mp4 files) to text.

Create a Custom GPT in OpenAI that you could use to ask questions about lesson1 of my Ansible Course.

Built an initial version of a slack-bot solution that would communicate between Slack - my code - Anthropic (Claude Sonnet).

Built a summary Python tool that would consolidate all of the transcripts for lesson1 and other reference material into a single file that I could dump into the Claude Context to ask questions about.

Implemented a RAG solution (ChromaDB) to improve my Ansible Course Q & A Slack bot.

Transcribed Ansible Course class2 transcripts using Python Whisper solution.

Decided my Slack Bot plus Course Q&A plus LLM integration (Claude Sonnet) was a convoluted mess of spaghetti code with way too much other stuff going on. Main issues--lots of logging and other code interspersed in the code (making the code hard to follow), too tight of coupling between my Course Q&A code and Claude (i.e. the Course Q&A and LLM interface needed to be abstracted such that different LLMs could be used more easily), not easy enough for me to follow the code flow and the interactions of the different parts of the system. Decouple RAG implementation from CourseQA class.

Revamped the code basically starting from scratch to layer on: 
1. LLM Interface using a Python Abstract Base Class
2. Implement tests for LLM Interface and for Anthropic implemenation of this interface.
3. Start implementing CourseQA class.
4. Implement tests for CourseQA class.
5. Implement RAG class.
6. RAG tests
7. Integrate CourseQA and RAG; expand on tests
8. Expand LLM Interface to add the `ask_question` method.
9. Expand Anthropic Tests
10. Verify communication with Claude Anthropic is working via the API
11. Add tests for `ask_question` method.
12. Add integration tests that actually test the CourseQA to Claude communication.
13. Add in the Slack Bot code.
14. Get Slack Bot working.
15. Add Slack Bot test code.
16. Integration test of Slack Bot. This took a lot of time as I ultimately needed two Bots here--the actual Ansible Course Bot and then a PyTest bot. The PyTest bot connects via the WebClient and asks questions to the Ansible Course Bot. The Ansible Course Bot runs in a separate thread and answers questions. The PyTest bot verifies these responses.
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
