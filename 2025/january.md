# Hello

The code creation was done using Cursor + Composer on my Apple laptop.

Created a Python program using AI and Whisper to translate my course videos (.mp4 files) to text.

Create a Custom GPT in OpenAI that you could use to ask questions about lesson1 of my Ansible Course.

Built an initial version of a slack-bot solution that would communicate between Slack - my code - Anthropic (Claude Sonnet).

Built a summary Python tool that would consolidate all of the transcripts for lesson1 and other reference material into a single file that I could dump into the Claude Context to ask questions about.

Implemented a RAG solution (ChromaDB) to improve my Ansible Course Q & A Slack bot.

Transcribed Ansible Course class2 transcripts using Python Whisper solution.

Decided my Slack Bot plus Course Q&A plus LLM integration (Claude Sonnet) was a convoluted mess of spaghetti code with way too much other stuff going on. Main issues--lots of logging and other code interspersed in the code (making the code hard to follow), too tight of coupling between my Course Q&A code and Claude (i.e. the Course Q&A and LLM interface needed to be abstracted
