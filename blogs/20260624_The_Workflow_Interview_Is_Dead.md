# The Workflow Interview Is Dead

*I record my clients' workflows for a living. Then I built the thing that does it without me. Two days inside SerenDesktop. I think I just AI'd myself out of my own job. Again.*

## The job I do

- I'm a Forward Deployed Engineer. The whole game is the workflow interview.
- Sit with a client, screen shared, pull the workflow out one click at a time: which tool, which permission, which exception, the thing nobody wrote down.
- It does not scale. Ceiling is 3–5 interviews a day.
- More onboards land next week. I'm one human.
- The real cost is the context-switching whiplash — packaging supplier, then family office, then crypto desk, workflow 1 to workflow Z. Some days I block an hour after the calls just to breathe.

## So we killed the interview

- Took the person-to-person screenshare and pushed it to a person-to-AI screenshare.
- Hit record. Walk through your real work, narrate it like you're onboarding a new coworker. Stop.
- SerenDesktop turns that recording into a working Seren Skill — an installable agent that does the thing you just demonstrated.
- Built in two days. Shipped. No waitlist. You can do it tonight on your own subscription.

## What you get

- The employee records their own workflow, Loom-style, and just tells the AI what skill they want. Video and voice both go to the model. A skill draft comes back.
- I review the recording and the generated skill side by side. Confirm we hit the target. Never book the call.
- Client's happy. I get my evening back to watch Cape Fear. Wait — did I just AI myself out of my own job? Again? 😭

## What happens when you press record

- Capture uses your OS's native engine: `screencapture` on macOS, `xcap` on Windows and Linux.
- You pick the scope: whole screen, one window, or one browser tab.
- While it records, it listens to your mic and tags the moments that matter — clicks, navigations, the places you stop to explain something.
- On stop, the audio goes to seren-whisper and comes back as a clean, time-aligned transcript.
- Video, transcript, and the action trace land together in the composer. The AI drafts the skill there: a `SKILL.md` with instructions, the tools the agent needs, and an as-built record of what you did and said.
- You read it. Edit it like any document. Save it locally or publish it.

## Two details that make it safe for a real company

- **Redaction gate.** Before anything ships to a public skill, SerenDesktop scans the recording for secrets, credentials, and personal data. It shows you what it found. You strip what you want. The publish button stays disabled until you sign off. It's the default, not a setting — that's why I'll let a client's employee record unsupervised.
- **Bring your own brain.** SerenDesktop doesn't sit between you and your model. It spawns your local Claude Code, Codex, or Gemini CLI as a child process and streams straight to the provider on your subscription or your API key. Run LM Studio and an open-weights model on your laptop does the work — nothing leaves your hardware. We ship a sandboxed Node runtime for every platform (Mac silicon, Mac Intel, Windows, Linux) so it just runs.

## The second recording is faster than the first

- There's a local memory layer — SQLite with vector search, on your machine — that remembers across recordings.
- Tomorrow, the AI already knows your role, the tools you've shown it, the compliance rule you explained, the half-finished workflow you parked.
- Skill #2 is faster than #1. Skill #10 is faster than #2. You stop re-explaining your own company.
- Every external tool call stops at an approval prompt. No agent burns a token, sends a message, or moves a dollar without your yes.

## Publish it

- Private to your team. Public on the Seren marketplace. Paid, if you want to charge for it.
- It installs like any other skill — including for the FDE reviewing it the next morning.

## So who's actually out of a job

- The client gets working software instead of a twelve-minute video rotting in a queue.
- The FDE gets review fidelity we've never had: the recording and the skill artifact in one view, action trace lined up underneath.
- Me? Evenings back. De Niro chewing scenery. Not on a call.
- The interview that defined my job is dead, and I'm the one who shot it. The job that's left — deciding which workflows are even worth recording — is the human one.

---

SerenDesktop is the open-source client. Your subscription, your API key, or a local model in LM Studio. The skills are yours. The evenings are too. Your boss is recording theirs today — don't be the last one still booking the meeting. Download link's in the comments.

Taariq Lewis
CEO, SerenAI
https://serendb.com

---

*Published: June 24, 2026*
*Tags: #SerenDesktop #AgentSkills #ForwardDeployed #ScreenRecording #AIWorkflows*
