# The session workflow kit

<img src="assets/kit.png" alt="Kit, the fox mascot" width="180" align="right">

Build real software with AI coding agents and keep it on track across dozens of chats. The kit
keeps the plan, the rules and the current state in the repository, so every new chat picks up
exactly where the last one stopped. Each session plans with you before it writes a line, proves
its work with tests and checks, and leaves a clear note for the next one. You approve every
plan, review every session and decide every push.

The method builds on spec-driven development: a versioned spec in the repository is the source
of truth, and the agent writes code against it. The kit adds what holds that together over many
chats: fixed-scope sessions with a handoff between them, gates whose checks are falsified before
they are trusted, and a review of every session.

<br clear="right">

https://github.com/user-attachments/assets/cdff5ef4-48a0-4956-a090-c02d07113df2

| File | What it is |
|---|---|
| `DEVELOPMENT-WORKFLOW.md` | The method and its templates. Copied verbatim into every project as `docs/DEVELOPMENT-WORKFLOW.md`. |
| `KICKOFF.md` | The instructions for Session 0, the intake that sets a project up, with the `SPEC.md` and `CLAUDE.md` templates. Used once per project. |
| `PDD-TEMPLATE.md` | Optional. A template for the product design draft, to capture your idea as a structured document. Fill what your product needs and delete the rest. |
| `GRAPH-ENGINEERING.md` | Optional reading. How the kit maps onto graph and loop engineering, for readers who know the terms. |
| `README.md` | This file. |

## Starting a project

1. Make an empty folder for it, and write the product design draft (the PDD): what it is, for
   whom, what v1 should do. It can be rough; Session 0 asks about everything it leaves open.
   `PDD-TEMPLATE.md` gives it a structure if you want one.
2. Open a Claude Code chat in that folder, in plan mode. Attach `KICKOFF.md`,
   `DEVELOPMENT-WORKFLOW.md` and the PDD, and say: "Read KICKOFF.md and do it."
3. Answer the intake's questions. Each comes with a recommended answer; accept it, change it,
   or defer it. One asks what is private. Your private terms live in one file outside every
   repository, `~/.config/hygiene/terms.txt`, one word per line, shared by all your projects;
   the intake checks it and adds what you name. Each project's `verify` then checks every
   commit against that list, and for AI attribution, secrets and conflict markers.
4. Approve the plan of documents. Session 0 writes the spec, the architecture, the session plan,
   `CLAUDE.md`, the backlog and the handoff for Session 1, and commits them locally.
5. Run `/code-review high`. The session fixes what the review finds, commits the fixes, and
   prints a one-line opener for Session 1.

On a very complex project, have the plan reviewed by an adversary before you see it. Add one
line to the prompt in step 2: "Before you show me the plan, have [a model of your choice] review
it adversarially, and tell me what it found and what you changed."

## Every chat after that

1. Open a new chat in plan mode and paste the one line the last chat printed. It points at
   `docs/next-session-prompt.md`, the handoff, which the session reads from the repository.
2. The session reads the spec, the rules and the code, and proposes a plan. Answer its
   questions and approve the plan.
3. It works, runs the gates, keeps the docs true, writes its done-note and the handoff, and
   commits locally.
4. Run `/code-review high`. It fixes the findings, asks you about the ones that change behaviour
   or scope, commits the fixes separately, and prints the next opener.
5. Try the build if you like. If you want the work on the remote, say so: it lists what would be
   pushed, records the push in the handoff's last commit, pushes, and prints the opener again.
6. Close the chat. The next session is always a new one.

After you approve a plan, the session tells you whether parts of it could run in parallel,
each in a chat of its own, and splits nothing without your yes. It brings you a part's questions
that change behaviour, scope or where the parts meet, and you review it all as one session.
This is *Parallel parts* in `DEVELOPMENT-WORKFLOW.md`.

When a round is finished, the printed line says so and the next chat is an intake: paste it,
then say what you want next.
