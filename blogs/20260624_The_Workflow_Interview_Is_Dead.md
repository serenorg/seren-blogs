# The Workflow Interview Is Dead

*I record my clients' workflows for a living. Then I built the thing that does it without me. It took two days inside SerenDesktop, and I think I just AI'd myself out of my own job. Again.*

If your job is to sit across from someone and ask them to explain how they do their work, your job is changing today. I should know. That job is mine.

I'm a Forward Deployed Engineer. The whole game is the interview. You sit with a client, screen shared, and you pull the workflow out of them one click at a time. Which tool, which permission, which exception, which thing-they-do-that-nobody-wrote-down. Do it well and you can automate their day. Do it forty times a week across six industries and you start losing your own mind.

I've had days where I blocked an hour afterward just to sit and breathe. Not because the calls were bad. Because the context switching from a packaging supplier to a family office to a crypto desk, from workflow 1 to workflow Z, hollows you out. And here's the part nobody at a growing company wants to hear: it does not scale. I can run three to five of these a day. That's the ceiling. We have more onboards landing next week, and I am exactly one human.

So we killed the interview.

## The fix: point the camera at the AI, not at me

The move is stupid in hindsight. We took the person-to-person screenshare and pushed it to a person-to-AI screenshare. You hit record. You walk through your real work and narrate it like you're showing a new coworker. You stop. SerenDesktop turns that recording into a working Seren Skill — an installable agent that does the thing you just demonstrated.

That buys three things at once:

1. The employee records their own workflow, like a Loom, and just *tells the AI* what skill they want. Video and voice both go to the model. A skill draft comes back.
2. The Forward Deployed Engineer — me — reviews the recording and the generated skill side by side. I confirm we hit the target without ever booking the call.
3. The client is happy, and I get my evening back to watch Cape Fear. Wait. Did I just automate myself out of my own job? Again? 😭

This took us two days to build and it shipped inside SerenDesktop. No waitlist. You can do it tonight, after Cape Fear, on your own subscription. Here's what's actually happening under the hood, because the details are where this stops being a gimmick.

## What happens when you press record

SerenDesktop captures with your operating system's native engine — `screencapture` on macOS, `xcap` on Windows and Linux. You pick the scope: your whole screen, a single app window, or one browser tab. While the video rolls, it's listening to your microphone and tagging the moments that matter — the clicks, the navigations, the places where you stop and explain something out loud. Those become the anchors the AI reasons about later.

When you stop, the audio goes to our seren-whisper service and comes back as a clean, time-aligned transcript. Video, transcript, and the action trace all land together in the composer. That's where the AI drafts the skill: a `SKILL.md` spec, the instructions, the tools the agent needs, and an as-built record of what you actually did and said. You read it. You edit it like any other document. When you're happy, you save it locally or publish it to your team.

Two boring details make this safe enough to put in front of a real company.

**The redaction gate.** Before anything ships to a public skill, SerenDesktop scans your recording for secrets, credentials, and personal data. It shows you what it found. You decide what to strip. The publish button stays *disabled* until you sign off. If you forget, nothing leaves your machine. That's not a setting you have to remember — it's the default, and it's the reason I'll let a client's employee record their own workflow without a lawyer in the room.

**Bring your own brain.** SerenDesktop does not sit between you and your model. Talk to Claude and it spawns the Claude Code CLI on your machine as a child process and streams the conversation straight to Anthropic over its native stream-json protocol, on your subscription or your API key. Same shape for Codex. Same for Gemini. Run LM Studio and an open-weights model on your own laptop does the work — nothing leaves your hardware at all. We ship a sandboxed Node runtime for every platform so none of this depends on whether your system Node is healthy. Mac silicon, Mac Intel, Windows, Linux. It just runs.

## The part that compounds

Here's where recording-as-a-feature turns into something a "just use Loom" answer can't touch.

Every conversation you have inside SerenDesktop feeds a local memory layer — SQLite with vector search, on your machine. Come back tomorrow to record skill number two, and the AI already remembers what you built yesterday. It remembers your role, the tools you've shown it, the compliance rule you explained, the half-finished workflow you said you'd return to. So your second skill is faster than your first, and your tenth is faster than your second. Forward Deployed work has always been about reusing one client's context. The memory layer does that without making you re-explain your own company.

And when the AI needs to reach outside — look something up, hit a marketplace publisher, verify an API — every one of those tool calls runs through the MCP gateway and stops at an approval prompt. You see what it wants to do before it does it. No agent burns a token, sends a message, or moves a dollar without your explicit yes.

When the skill is done, you publish it. Privately to your team. Publicly to the Seren marketplace. Paid, if you want to charge for it. Anyone running SerenDesktop installs it the same way they install any other skill — including the FDE reviewing it the next morning.

## So who's actually out of a job

The client gets a Loom-style flow that ends in working software instead of a twelve-minute video rotting in someone's queue. The FDE gets review fidelity we have never had: the recording and the skill artifact in one view, action trace lined up underneath. The AI gets context that compounds across every recording.

And me? Theoretically, I get my evenings back. Robert De Niro is chewing scenery for two hours and I am not on a call. The workflow interview that defined my job is dead, and I'm the one who shot it. I'm fine with that. The next job — designing which workflows are worth recording at all — is the one humans keep.

SerenDesktop is the open-source client. Your subscription, your API key, or a local model in LM Studio — your call. The skills are yours. The evenings are too. Your boss is recording theirs today. Don't be the last one still booking the meeting.

The download link is in the comments.

Taariq Lewis
CEO, SerenAI
https://serendb.com

---

*Published: June 24, 2026*
*Tags: #SerenDesktop #AgentSkills #ForwardDeployed #ScreenRecording #AIWorkflows*
