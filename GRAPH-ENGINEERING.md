# How the kit maps to graph engineering

This note is for readers who know the term and want to see where each piece of the kit lives.

Graph engineering means deciding an agent's process once, as a map of steps with explicit rules
between them. A graph has nodes (units of work), edges (the rules for moving between them) and
state (what carries from one node to the next). Reliable graphs add validation, repair and a way
to run independent work in parallel.

## The mapping

- **Nodes are sessions.** Each session is one bounded unit of work, run in its own chat, with a
  fixed contract written into its block in `docs/spec/SESSIONS.md`: what it reads (*Read*), what
  it may change (*Scope*), which documents it keeps true (*Docs*), what proves it worked
  (*Gates*) and its known traps (*Watch for*).
- **Edges are the plan's order and the entry gate.** A session starts only when the tree is
  clean, the handoff names it, and the previous session's block has its done-note. A session
  that did not finish blocks the next one.
- **State lives in the repository.** The handoff (`docs/next-session-prompt.md`) and the
  done-notes carry everything the next session needs, so each chat starts from what is written
  down.
- **Validation is `verify` and the review.** Every commit passes one verification command
  (typecheck, lint, format, tests, hygiene), and every session ends with a code review whose
  fixes land as their own commit. Every new check is falsified before it is trusted, so the
  gates themselves are tested.
- **Repair is an inserted session.** When a review or a session shows the plan is missing
  something, a new session is added in place, with the id before it plus a letter (6a, say),
  and a logged reason. The rest of the plan stands.
- **Fan-out and fan-in are parallel parts.** Inside one session, an independence test decides
  which steps may run as parts, each in its own chat, checkout and branch. A contract commit
  fixes the interfaces where they meet, and integration merges them back and runs the full
  gates over the combined code.
- **Whole-graph checks are architecture reviews.** A long round places a review after each
  milestone. It reads the code of every session so far, cold, against `ARCHITECTURE.md` and the
  spec, to catch drift between sessions that a single diff hides.

## Two deliberate choices

**The graph is written as documents.** Each round's intake writes a new plan from what was asked
for, so the graph changes every round, and one intake can reshape it. A graph encoded for a
runtime suits a fixed pipeline that runs the same way many times.

**A human stands at every edge.** You approve every plan before any edit, decide every finding
that changes behaviour or scope, start every session with its opener, and approve every push.

## And loop engineering?

Loop engineering means designing the cycle that prompts, checks and re-runs an agent until a
goal is met.

- **Inside a session, the kit loops.** Work, `verify`, review, fix, falsify, until nothing the
  review found would make the product or a session act wrong.
- **Between sessions, you are the loop.** You open each session with its opener and review it
  as one unit. The stop rule holds throughout: a session that meets a wrong spec or an
  underdetermined decision stops and asks.
