# Seren Speeds Up Human-to-AI Knowledge Transfer

(**Prologue:** *So the verdict is in: Our best blog performing blog posts by PostHog for the past six months are any blogs written by humans without cartoons. This is a shock as I've drunk the automation koolaid for marketing as deeply as anyone. However, lack of demand is indicative of the change. We need more humans, and AIs, to read our blogs because humans guide prompts and humans create original content that LLMs need for training better models. Welcome to our new blogs where each one will be written by human hands and human brains.*)

## Announcing SerenDesktop v3.60.0
We don't usually announce versions, but this time let's enjoy the exception. SerenDesktop v3.60.0 comes with a robust and improved number of features. However, our biggest is Seren-Screen-Record. Now, with SerenDesktop, you can record your entire screen and talk to your LLM (subscription, API, or Local Via LM Studio). It will transcribe your voice and read your video to create your skill while reviewing your work tasks. In the world of massively customizable software, you must be asking the question: Why did we build this and why should you care? After all, you can build your own version!

## The Need for Speed
Well, the answer is that it's for our enterprise customers who are knowledge employees who don't get compensated for vibe-coding their own desktop harnesses. This software is also for our growing team as well. Here's our story: As a Forward Deployed Engineering team, we spend hours interviewing our enterprise clients about their daily workflows, security, permissions, tools. Once your day becomes filled with these knowledge-transfer and instructional meetings, the context switching from industry to industry and from workflow 1 to workflow z becomes exhausting. I've had to take hours just to sit and decompress and try to take deep breaths. Once you realize how much knowledge work and skills humans have stored in our heads, you will be overwhelmed. It's amazing to consider that much of our knowledge work is active memory we call upon and it differs for each individual.

As a startup, we want to interview every customer and learn every unique workflow. However, the hard truth is: Individual interviews on employee workflow automation is NOT scalable. There's just so many ways to do knowledge work. It's impossible to try to personally interview everyone. At most, I was able do 3 - 5 of these interviews a day and I already have SerenDesktop transcribing the interviews. However, my backlog of interviews began to grow and loom ominously in my Desktop Recordings. Not to mention, we are a growing company and we have more onboards coming, next week. Given limited resources and time, but a desire to reach everyone, I had a need. A need for speed!

## Knowledge Transfer from Humans to AI
ChatGPT and Convey already affirmed the best path: Push the transfer of knowledge from human-to-human to human-to-AI with screenshare and recording. This approach would give us three strong solutions so speeding up knowledge transfer to immediate skills: 
1. Seren Employee clients can create their own recordings (just like Loom) and tell the AI the skill they are trying to create. The skills is transcribed immediately via text and video sent to the AI to generate a skill that will be used by the Seren Employee.
2. Seren Forward Deployed Engineers (me) can now review both the skill and the webinar to confirm we hit the target.
3. Seren's clients are happy and I have more time to watch Cape Fear. Wait..did I just AI myself out of my own job...again? 😭 😭 😭 

We needed to go fast so prompting this feature into SerenDesktop was the best path. The Seren-Screen-Record & Skill feature took us just 2 days to build and is now in SerenDesktop. Anyone can use it and create their own Seren Employees and Seren Skills using your subscriptions to Claude, Codex, Gemini, and even LM Studio for your own Local Models. It's ready today. No need to wait to schedule a meeting with me. You can do this right after you watch Cape Fear, tonight!

## The P0 Features of v3.60.0

### Seren screen record & skill creator
Record your screen (full display or a single window) directly from the chat composer while a companion browser extension captures a structured interaction trace, then turn that recording into a draft skill bundle with SKILL.md, spec, agent scaffold, config, and smoke tests. Prior to this release, there was no path from "a thing I do on my desktop" to a shareable, executable Seren Skill or Seren Employee.  Want to keep your secrets from the screen recording? You can even redact details from the video and transcript!

### Offline cloud-employee history
Seren Employee conversations that run in the cloud are now mirrored to your local computer. This means, your full Seren Employee chat history is available offline and survives SerenDesktop restarts. Previously those cloud-run chats lived only server-side, in your SerenDB instance. If you closed SerenDesktop or you lost connectivity, that working context was no longer accessible once you restarted SerenDesktop. This feature lets users review, resume, and stay connected to long-running Seren Employee work from anywhere. No live connection will now be required. 

### Syncing skills from the chat composer
Problem: Seren Employee skills change quickly when teams are working and collaborating on an agent. If you're in active chat and the skill upgrades remotely, how will you know to refresh your local version? Before v3.60.0, you had no idea. You had to find out the hard way: Your co-worker yelled that your Seren Employee was operating off the reservation. Syncing skills in the chat composer changes this. We added a new Sync Skill button that now appears in the composer whenever the active chat is using an installed skill. You now can pull the latest published version, of an employee's skill, in place without leaving the conversation. Before overwriting, Seren tells you exactly what files will change with the fresh install, the version bump. Syncing of course will overwrite your local edits, but you get a confirmation dialogue to confirm you want to upgrade. This makes it easier and faster to work with actively updating skills for you and your team.

### Lots of maintenance fixes
There were a host of maintenance fixes in SerenDesktop that included focus on the meeting recording and transcription services. We had to increase gateway size handling for Seren Skills processing 100MB and greater files for agentic work! Improved detection of your voice during meeting transcriptions. Improved integration with Seren-notes so that all your recorded meetings now have action items and details that can connect to any skills. So many other fixes and bugs from adversarial testing of the code base. There will be more fixes in the coming days as we really fully test more screen recording and skill creation for Seren Employees.

SerenDesktop is the open-source LLM development harness for knowledge workers. You can use SerenDesktop with your LLM subscription, your API keys, or even a local model in LM Studio. All the skills you create in Seren are yours and can be shared publicly or kept private. Our objective is to speed up the creation and management of LLM automation agents that not only unlock faster and deeper work paths, but also more free time to enjoy what's most important: Watching the next episode of Cape Fear.

**Download SerenDesktop v3.60.0:** [serendb.com](https://serendb.com)

Taariq Lewis
CEO, Seren
https://serendb.com

---

*Published: June 24, 2026*
*Tags: #SerenDesktop #AgentSkills #ForwardDeployed #ScreenRecording #AIWorkflows*
