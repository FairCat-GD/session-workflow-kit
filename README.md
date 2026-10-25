# Workflow kit

Everything needed to start a new project under this session-based workflow.

| File | What it is |
|---|---|
| `DEVELOPMENT-WORKFLOW.md` | The method and its templates. Copied verbatim into every project as `docs/DEVELOPMENT-WORKFLOW.md`. |
| `KICKOFF.md` | The instructions for Session 0, the intake that sets a project up, with the `SPEC.md` and `CLAUDE.md` templates. Used once per project. |
| `README.md` | This note, for you. |

## Starting a project

1. Make an empty folder for it, and write the product design draft (the PDD): what it is, for
   whom, what v1 should do. It can be rough; Session 0 asks about everything it leaves open.
2. Open a Claude Code chat in that folder, in plan mode. Attach `KICKOFF.md`,
   `DEVELOPMENT-WORKFLOW.md` and the PDD, and say: "Read KICKOFF.md and do it."
3. Answer the intake's questions. Each comes with a recommended answer; accept it, change it,
   or defer it. One asks what is private. Your private terms live in one file outside every
   repository, `~/.config/hygiene/terms.txt`, one word per line, shared by all your projects;
   the intake checks it and adds what you name. Each project's `verify` then checks every
   commit against that list, and for AI attribution, secrets and conflict markers.
4. Approve the plan of documents. Session 0 writes the spec, the session plan, `CLAUDE.md`, the
   backlog and the handoff for Session 1, and commits them locally.
5. Run `/code-review high`. The session fixes what the review finds, commits the fixes, and
   prints a one-line opener for Session 1.

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

After you approve a plan, the session checks whether parts of it could run in parallel. If any
could, it tells you which ones, why, where they meet, and what the split would cost, then asks.
On a yes, where parts meet it first commits the interfaces between them, and each part runs in
a chat of its own, cut from there. A part's questions go to the session first: it answers what
the approved plan already settles, and brings you anything that changes behaviour, scope or
where the parts meet. The session merges the parts only with your yes, removes their branches
and checkouts, and you review it all as one session. This is *Parallel parts* in
`DEVELOPMENT-WORKFLOW.md`, tried once so far.

When a round is finished, the printed line says so and the next chat is an intake: paste it,
then say what you want next.

## Keeping the kit current

When a project finds a gap in the kit, fix it here first. Then copy `DEVELOPMENT-WORKFLOW.md`
into the projects that should follow the change, keeping each project's *In this repository*
paragraph. A change to *Day one* also has to be built in a project already past Session 1: plan
it as scope at its next intake, or in a session's plan mode. A project whose copy predates
*Parallel parts* gets them the same way, and names its exclusive resources in `CLAUDE.md`: the
harnesses, apps, ports and devices only one chat can use at a time.
