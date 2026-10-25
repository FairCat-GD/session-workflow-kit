# Session-based development workflow

A versioned specification, not the conversation, is the source of truth, and AI coding agents
and developers write code against it. Work is divided into fixed-scope **sessions**. Each runs
in a single chat, with no context carried between them. Because no session can rely on
remembering a previous one, the repository holds everything a session needs: the contract, the
scope, the current state and the known pitfalls. The same discipline suits human teams with
turnover or long gaps between working periods.

Use it for any project expected to outlast its author's working memory. It is more process than
a weekend prototype needs. If you adopt only part of it, keep the split between scope and state
and the handoff rules; they are what keep a long build on track.

This file is copied verbatim into every project as `docs/DEVELOPMENT-WORKFLOW.md`, templates
included. The one edit a project makes to it is an **In this repository** paragraph here, under
the introduction, naming where the project deviates and why (see *Adapting the workflow*). The
project's own standing rules live in its `CLAUDE.md`, not here.

## The documents

| Document | Job | Mutability |
|---|---|---|
| `docs/spec/` | **the round in progress**: its spec file(s), any reviews and its `SESSIONS.md`; while a session runs parallel parts, their notes in `parts/` | spec read-only, except dated amendments the human makes; SESSIONS amendable with a logged reason; part notes deleted by the session commit; the folder is emptied when the round closes |
| `docs/next-session-prompt.md` | **state**: what the next session needs that the docs do not say | rewritten by every session as its last act |
| `CHANGELOG.md` | what shipped, per version, written for the person using the product | append-only for content |
| `docs/BACKLOG.md` | work that has been named but belongs to no round, and checks nobody has done by hand | items move out when a round picks them up |
| `CLAUDE.md` | standing rules, loaded into every session automatically | edited as rules change; kept lean |
| `docs/ARCHITECTURE.md` | the system **as built**: its invariants, its deliberate decisions, its known debt and the baseline counts | written by round 1's documentation pass; kept true by every session after |
| `docs/USER-GUIDE.md` | behaviour from the user's side | written by round 1's documentation pass; kept true by every session after |
| `README.md` | what the thing is, how to run it, where the docs are | written by Session 1; kept true |
| `docs/DEVELOPMENT-WORKFLOW.md` | this file: the method and its templates | copied verbatim; changes only with the kit |

**Scope and state are different jobs, and the split is what makes the workflow hold.** Scope
(`SESSIONS.md`) is fixed in advance, so no session can quietly redefine what a later one owes.
State cannot be written in advance, so the last act of every session is writing the next
session's prompt.

**The spec files of a round.**
- **Round 1, the build:** `docs/spec/PDD.md` (the product design draft, frozen as received,
  with private details replaced by roles) and `docs/spec/SPEC.md` (the contract derived from
  it). They are not two copies of one thing. The spec holds the decisions, the architecture,
  the invariants, where it overrides the draft and which session builds each requirement; for
  the detail it points into the draft rather than copying it, so the draft stays until the
  round closes.
- **Every later round:** one `docs/spec/FEATURE-<name>.md` per request, holding the request
  verbatim and a dated list of the decisions resolved at intake.
- **A round that comes from an audit** keeps the audit beside them as
  `docs/spec/REVIEW-<date>.md`, and so does each architecture review a long round places at its
  milestones. A review is evidence and a session block is scope, and neither is the other: a
  finding becomes scope only when the human turns it into a decision or a block, at an intake
  or in the review session's plan mode.
- **Either way,** the spec is read-only once the round's first session opens. When the human
  changes a resolved decision mid-round, the change is added to the spec file, next to what it
  changes, as a dated **Amended <date> by the user** note saying what changed and why. The
  original text stays. A session never amends the spec on its own authority.

## Rounds

Work happens in **rounds**, and every round has the same lifecycle.

**Where a round comes from:**
- a request from the human;
- a walkthrough, where the human uses the build and their corrections are the request;
- an audit run at the human's request and recorded as `REVIEW-<date>.md`;
- items picked from `docs/BACKLOG.md`.

However it starts, it goes through an intake.

1. **Intake.** The intake is a session of its own, in its own chat, and writes no product code.
   - The request is written down verbatim.
   - A request that asks for judgement ("what is missing before 1.0?") is answered by an audit
     first, of the code, the docs and whatever the product promises. The intake then proposes
     groups of work for the human to pick from.
   - What the request leaves underdetermined is asked in small batches, each question with a
     recommended answer and one line on why. A contradiction is asked about, not resolved by a
     guess.
   - The answers are recorded as numbered, dated *resolved decisions*. An implementing session
     never re-litigates them. What the human declines goes to `docs/BACKLOG.md`, saying which
     intake left it out.
   - A question that only the implementing session will have the context to answer can be
     left to it, written into its block as **Ask at the start of <id>**. A proposal the human
     has not confirmed says so.
2. **Plan.** `docs/spec/SESSIONS.md` gets the standard header, a table of sessions with their
   versions, and one block per session, in order (templates at the end of this file). Each
   block has:
   - its **Read** line: exactly what the session reads of the spec files, `ARCHITECTURE.md` and
     any review, by number and by name (decisions, invariants, the draft's parts and items,
     sections, findings). The intake has read everything, so it is the one placed to say what
     each session needs;
   - its **Scope**, pointing at the files and lines it changes where the intake could see them;
   - its **Docs**: which documents it keeps true, and how;
   - its **Gates**: `verify`, the tests it must add, and the harnesses it must run;
   - its **Watch for**: the known traps.

   A session should be finishable in one chat with every gate green, and deliver a vertical
   slice rather than one layer.
   - **A round of more than five sessions places an architecture review after each
     milestone**, a point where a layer that later sessions build on is finished. A session's
     review sees one diff; an architecture review reads the code of every session so far, cold,
     against the spec, and finds the drift between sessions that no diff shows. It is a session
     with its own block, whose Read line is everything, and it writes
     `docs/spec/REVIEW-<date>.md` (template at the end): findings only, no product code, no
     version. In its plan mode the human picks which findings become scope. Each becomes an
     amendment to a later block or part of an inserted fix session.
   - **Session ids.** Round 1 numbers its sessions (0, 1, 2…). Later rounds give each session a
     letter for its theme and a number, for example C1–C4 for a cleanup, D1 for the data
     layer, V1 for a release.
3. **The intake ends like any session.** The items it picked leave `docs/BACKLOG.md`, and the
   handoff is rewritten to open the round's first session. Then comes a local commit
   (`docs: open round <N> — <theme>`) and the human's review. At an intake the review checks
   the plan against the code: the pointers and the Read lines, the feasibility, and where the
   spec and the code disagree. Its fixes land in the spec and the plan before the first
   session opens.
4. **Sessions**, one per chat (the cycle below). The plan can change on the way, always with a
   logged reason:
   - **A session can be inserted, with the human,** when a review or a session shows the plan
     is missing one. It gets its own block, which says why it exists, in the human's words
     where they gave them, and takes the id before it plus a letter (6a). The table's versions
     shift, and the handoff says so.
   - **Scope the human adds mid-session** is recorded in the block as "added mid-session by
     the user".
5. **Close.** The round's last block closes it.
   - **Anything left open** moves to `docs/BACKLOG.md`: deferred work, and checks nobody has
     done by hand.
   - **The round's last session is its release.** Where each session is a release, it cuts a
     version even when its own work is documentation, so every round ends on a version a person
     can name and a milestone's tag can mark. A project that releases at milestones cuts one
     only when the round ends on a milestone.
   - **The release commit comes first, with the last done-note in it.** Then a separate
     `docs: close round <N>` commit deletes `docs/spec/`'s files and rewrites the handoff to
     say no round is in progress. Git history keeps the specs, and the release commit is where
     the last done-note can still be read in full; the handoff names that commit.
   - **The review follows the close.** `SESSIONS.md` is gone by then, so the findings are
     recorded in the review commit's message and in the handoff.
   - **Round 1 also owes a documentation pass**, planned as a real session, usually its last:
     - `docs/ARCHITECTURE.md` (the system as built, the invariants, the deliberate
       decisions, the known debt, a baseline table of counts);
     - `docs/USER-GUIDE.md`;
     - a truthful README.

     From then on `CLAUDE.md` points at the as-built documents, and every session keeps them
     true.

**Round 1 starts with Session 0 (intake) and Session 1 (scaffold).** Session 0 turns the
product draft into the spec, the plan and the rules file, and writes no product code. Session 1
builds the skeleton and the day-one checklist, and is the first block of every build plan.

## Day one (Session 1)

Each of these is cheap on day one and expensive to retrofit.

- **One verification command.** `verify` = typecheck + lint + format check + tests + hygiene.
  Formatting is in the gate from the first commit. It also runs named stages alone
  (`verify hygiene --message <draft>`), so a commit that changes only docs runs only the stages
  that read them (the cycle, step 6).
- **The hygiene stage** (see *Nothing private in the repository*), reading the owner's term
  list. Its first run checks the whole history, Session 0's commit included.
- **The gate is held by the session and the human; there is no CI.** `verify` is green before
  every commit (only the docs stages, for a commit that changes only docs), the human reviews
  every session, and nothing reaches the remote without their
  approval.
- **Version and changelog.** Stamp `0.1.0` in the manifest, surface it in the product, and
  start `CHANGELOG.md` from the template, with an empty `[Unreleased]` section.
- **A fixtures directory** (`tests/fixtures/`). Tests never read documentation or design
  assets; a test that depends on a mockup makes the mockup undeletable.
- **Fictional test data only.** No real names, no real content, no author's identity and no
  home-directory paths in fixtures or seeds.
- **Checkout tuning lives in the repo.** Anything set in `.git/config` or an editor does not
  travel to the next clone; put it in scripts or docs.
- **Part checkouts are ignored.** `.gitignore` and every tool `verify` runs skip
  `.claude/worktrees/` at the checkout's root, where Claude Code creates a parallel part's
  checkout (on a branch `claude/<name>`; see *Parallel parts*), so neither `verify` nor the
  hygiene stage reads a part's copy. Anchor the skip to the root: a part's own `verify` runs
  inside such a checkout and must still read its files.
- **A truthful README**: what the thing is, how to run it, and where the docs are.
- **The Commands table in `CLAUDE.md`**, filled in: every script a session runs, one line
  each on what it does. It names the docs-only command too: the stages that read docs, hygiene
  plus the format check and lint where they cover them
  (`verify format hygiene --message <draft>`).

## The session cycle

The human opens every session, intakes included, in a new chat in plan mode, by pasting the
one-line opener the previous session printed (step 12). It points at the handoff, which is in
the repository.

1. **Entry gate.** The tree is clean, `docs/next-session-prompt.md` names the session you are
   about to do, and, unless this is the round's first session, the previous session's block in
   `SESSIONS.md` has its done-note. If it does not, that session did not finish; do not stack
   on it. A session the handoff says paused at integration is the exception: it resumes, and
   its open parts, named in the handoff, stay. Then check that no parallel part was left
   behind: where the previous done-note has a *Parts* section, the branches it names (the
   parts' and any contract change's) and their checkouts are gone (`git branch`,
   `git worktree list`). An intake finds that done-note in the release commit its handoff names
   (`git show <sha>:docs/spec/SESSIONS.md`). Remove any that remain the way integration does
   (*Parallel parts*, integration step 6), and take what git refuses to the human; a branch the
   done-note lists as kept stays. A branch or checkout no done-note names is not this
   session's: leave it.
2. **Read in order:**
   - what the block's **Read** line names, in full;
   - `CLAUDE.md`, which carries every invariant;
   - `docs/ARCHITECTURE.md` (once it exists): its overview, invariants and baseline, plus the
     sections the Read line names;
   - from `SESSIONS.md`, its header, this session's block and the previous block's done-note;
   - then the prompt.

   No session reads the spec files or `ARCHITECTURE.md` whole, except an architecture review,
   whose Read line is everything. They grow past what one reading holds, and most of each is
   about ground the session does not touch. The invariants are always in view through
   `CLAUDE.md`, and a long round's milestone reviews read everything (see *Rounds*). The Read
   line is a floor, not a ceiling: where the code leads somewhere it does not name, read that
   too. An intake has no block yet: it reads what the between-rounds prompt names. The prompt
   adds context; it never replaces the sources.
3. **Plan with the human before any edit.** In plan mode, read the code the block points at
   and propose a concrete plan. Ask the block's *Ask at the start* questions here, and agree
   here on anything the plan changes about the block. Those changes are logged as amendments
   ("amended at plan time, with the user"). The human approves the plan, and only then does the
   session edit.
4. **Look for independent parts.** Once the plan is approved, check each of its steps against
   the independence test (see *Parallel parts*) and tell the human, in a few lines, which steps
   could run in parallel and why each is independent, which must stay sequential and what ties
   them, where the parts meet (the interfaces the contract would fix), and what the split would
   cost: how many parts, and how many chats they may be asked to answer in. If no step passes,
   say so in one line and go on. Parts start only on the human's clear yes; without one, the
   session runs sequentially.
5. **Work inside the scope.** Stop the moment the spec turns out wrong, a decision is genuinely
   underdetermined, or something does not behave as specified. Say so, leave the tree clean
   or clearly marked, and resolve it with the human. A session that guesses to avoid stopping
   is the expensive failure mode, because the guess becomes load-bearing for everything after
   it.
6. **Verification gate.** `verify` is green before the session may commit. Run the harnesses
   the block names (UI, end-to-end, runtime probes), and any other whose ground the change
   covers. Falsify every new check before trusting it. A commit that changes only docs runs
   only the stages that read what changed: the docs-only command in `CLAUDE.md`, which is
   hygiene, plus the format check and lint where they cover docs. A file that a test or a build
   reads, such as a README whose examples run as doctests, is not docs for this rule; no test
   reads the rest (*Day one*), so the other stages have nothing new to check. A commit that
   changes anything
   else (code, a test, a fixture, a config, the manifest) runs the full `verify`, and so does
   every merge.
7. **Keep the docs true**, in the same commit as the code:
   - `CHANGELOG.md`: an entry for everything user-visible, under the session's version, and
     the version bumped in the manifest (see *Versions and the changelog*);
   - `docs/ARCHITECTURE.md`: the sections the change touches, any new deliberate decision or
     known debt, and the baseline table for every count that moved;
   - `docs/USER-GUIDE.md`, for any behaviour a user now sees differently;
   - `README.md`, when what the thing is, how it runs, or a number it states changes;
   - `CLAUDE.md`, when a standing rule, a command or a pin changes;
   - `docs/BACKLOG.md`, for anything the session deferred or could not check by hand.

   The block's **Docs** line names the ones the plan expected; it is a floor, not a ceiling.
8. **Log.** Append a done-note to the session's block in `SESSIONS.md` (template at the end):
   - what was decided with the human at the start;
   - what actually shipped;
   - the **amendments**, where the block's intent held but its letter did not, each with its
     reason and who decided it;
   - what was measured, and what was checked by hand, with the outcome of each;
   - what was falsified;
   - the numbers, with their deltas.
9. **Handoff.** Rewrite `docs/next-session-prompt.md` for the next session.
10. **Commit locally**: one commit for the session's work, and in the round's last session the
    close commit after it. A session that ran parallel parts has its contract, the parts'
    commits and the merges before it (see *Parallel parts*). The last act before each commit is
    `verify` run with its drafted message (the docs-only command, for a commit that changes
    only docs; step 6), after the done-note and the handoff are written, so they are checked
    too (see *Nothing private in the repository*). Never push without explicit approval for that
    specific push, and never tag without approval for that tag.
11. **Review.** The human runs a code review over the session's commit at high effort, in the
    same chat (in Claude Code, `/code-review high`); after parallel parts it covers the
    contract, the parts' commits and the merges too. In the round's last session that is the
    release commit; the close after it only deletes. Fix what it finds. A finding that changes
    behaviour or scope is the human's call, and a finding left unfixed is recorded as skipped,
    with its reason. Falsify the checks the fixes add. Then:
    - add a **Review** section to the done-note: the findings, what was fixed and what was
      skipped, what was falsified, and the numbers after the review. In the round's last
      session the close has already deleted `SESSIONS.md`, so that section's content goes into
      the review commit's message and the handoff instead (see *Rounds*);
    - update that version's CHANGELOG entry and the docs the fixes touched (no new version);
    - update the handoff for anything the review changed, including a session it opened;
    - commit the fixes as a separate `review:` commit.

    Expect it to find something; the worst finds are routinely in code the session had just
    called safe.
12. **Print the opener** as the last message of the chat: one line, ready to paste, that points
    at the handoff and never repeats it. Mid-round it is
    `Read docs/next-session-prompt.md and do the session it names: Session <id>, <one line>, release <x.y.z>.`
    A session that cuts no version leaves out the release. Between rounds it is
    `Read docs/next-session-prompt.md: no round is in progress, so the next session is an intake.`
    If the human then approves a push, follow *Git*: the handoff's push note is folded into the
    last commit before it goes, so the push adds no commit. Print the opener again after the
    push. The chat ends there.

The session is the human's review unit. Between sessions they read the diff and try the build,
and work that runs past the boundary arrives unreviewed and too large to judge.

## Parallel parts

**Tried once:** one session has run two parts, and the rules below include what it taught.

A session can run independent pieces of its approved plan in parallel, each in a chat of its
own. It is an option inside the session, never a way around the cycle. The plan agreed before
any edit, the stop rule, decisions not re-litigated, every check falsified, nothing private,
one review per session and pushes only with approval all still hold. A session that cannot keep
all of that runs the pieces one after another.

- **The coordinator** is the session the human opened. It plans, proposes the split, commits
  the contract, starts the parts, answers what the plan settles and integrates them, and it
  stays the human's one review unit.
- **A part** is one independent piece of the approved plan, run in a chat of its own, in its
  own checkout and on its own branch. Parts are numbered after their session: 8.1, 8.2, C2.1.
  Letters already mark inserted sessions (6a).
- **Integration** is the coordinator's last stretch: merging the parts, running the full gates
  over the combined code, writing the shared documents and cleaning up.

**The independence test.** A step can become a part only if all of these hold:
- **It needs no other step's result**, and no other step needs its result.
- **Where it meets another part, the interface is fixed.** Parts that meet at an interface (one
  calls what another builds, or both build against one type) are independent only when the plan
  fixes the interface and every brief carries it word for word. Changing it is a stop (see
  *A part's questions*).
- **It needs nothing the coordinator changes in this session, beyond the contract.** Parts are
  cut from the contract commit, or from the commit the session started on when they meet at no
  interface, and the coordinator commits nothing else before they start.
- **It touches no file that another part, or the coordinator's own steps, touch.** That
  includes the files every feature tends to touch. Shared registries are the common trap: one
  list of routes, tools, channels, commands or menu entries, a schema or migration sequence, a
  generated file, a lockfile, a translations file. Two steps that each add one entry to the same
  registry are not independent, however separate their modules look: two tools built in
  different modules but registered in one tool map stay sequential, unless the contract
  registers both, pointing at stubs, so neither part touches the map.
- **It does not have to land in one change with another step.** A data migration and the code
  that reads it belong together.
- **Its checks run on their own.** If the only proof that it works is a harness that needs the
  other parts too, it is not a part.
- **Its checks need no exclusive resource.** A harness that drives a live external app, a
  single-instance desktop app, a fixed port or one real device runs once at a time, so no part
  runs it; integration does. A step whose only proof needs one is not a part. Each project
  names its exclusive resources in `CLAUDE.md`.

When in doubt, a step stays sequential. A wrong split costs more than the time it saves.

**The contract.** When parts meet at an interface, the coordinator commits it before any part
starts, and the parts are cut from that commit. It holds the interfaces as code (types,
signatures, stubs that fail if called) and the shared-registry entries each part would
otherwise add, pointing at those stubs, so each part's own checks, a type check above all, pass
on its branch without anyone else's work. Where an existing test already calls every entry of
a registry, the stub returns the smallest valid result instead, so the full `verify` stays
green. It is typed like any work commit, with no version, ending `(<id>, the parts' contract)`,
and the full `verify` runs on its drafted message last.
- **A part takes in nothing from outside its branch but a contract change** (below).
  Cherry-picking the coordinator's work puts the same patch in history twice. What a part lacks
  is a gap in the contract, and a stop.
- **The contract does not change under running parts** without the human (see *A part's
  questions*). On their yes, the coordinator commits the change on a branch of its own, cut
  from the contract commit in a checkout of its own (`git worktree add -b <branch> <path>
  <sha>`, in the ignored path), merges it into its own line, and sends the new wording to
  every part whose brief carries the interface; each of those parts merges that branch in.
  Each of these merges is drafted and checked like any other:
  `merge: the contract change — <what changed>`. The branch holds the change and nothing else,
  so no part takes in the coordinator's other work, and a merge keeps one copy of the patch in
  history.

**A part's questions.** A part that stops asks the coordinator first, through the human or by
messaging the coordinator's chat where the tool allows it.
- **The coordinator answers** what the approved plan already settles, and a gap in the contract
  it can fill without changing behaviour, scope, the fixed interface or which files each part
  owns.
- **Everything else goes to the human,** an interface change included, and the part waits for
  the answer. When the coordinator is unsure which it is, the question is the human's.
- **Every answer is logged:** the part writes the question, who answered and the answer in its
  note, and the done-note's *Parts* section carries them.

**The shared documents are the coordinator's alone.** Parts writing them at once would
conflict at every merge, so only the coordinator writes them, at integration.
- **The coordinator's alone:** `SESSIONS.md`, the handoff, `CHANGELOG.md`, the version in the
  manifest, `ARCHITECTURE.md`'s baseline table, `docs/BACKLOG.md` and `CLAUDE.md`.
- **A part writes a part note instead,** `docs/spec/parts/<id>.<n>.md`, in its own branch
  (template at the end). It holds what the part built, measured and falsified, its numbers, and
  anything it could not finish. It is a file rather than a chat report because a report may
  reach the coordinator only in part and does not outlive the chat. The coordinator folds the
  notes into the done-note and deletes `parts/` in the session commit.
- **A part may change a doc of its own area,** such as one section of `ARCHITECTURE.md` or
  `USER-GUIDE.md`, when no other part touches that file and its brief names it.

**How a session runs with parts.**
1. **Plan, then look for parts** (the cycle, steps 3 and 4). No split is proposed before the plan
   is approved, and none starts without the human's clear yes. The coordinator never starts
   parts on its own initiative.
2. **Brief each part** from the template at the end. A part starts cold and is not a session:
   its brief replaces the cycle for it, whatever `CLAUDE.md` says every session does, and
   carries everything it needs, the setup a fresh checkout needs before `verify` runs included
   (ignored files do not come with a new checkout). Where parts meet, each brief carries the
   interface word for word, and names the tests that prove the seam only after the merge; the
   coordinator adds those at integration. Its stop rule is any session's: it never guesses to
   keep the parallel run going.
3. **Commit the contract,** when the parts meet at an interface (see *The contract*).
4. **Start each part** in a fresh checkout on its own branch, cut from the contract commit, or
   from the commit the session started on when there is none. The checkout lives outside the
   repository's folder, or in a path the project's tools ignore, so the coordinator's `verify`
   and harnesses never read a part's copy. Any tool that gives a chat its own checkout will do.
   In Claude Code, the coordinator can start each part as a parallel session in its own
   worktree, with the brief as its prompt; Claude Code puts the checkout under
   `.claude/worktrees/`, on a branch `claude/<name>`, which *Day one* has every tool ignore.
5. **While the parts work,** the coordinator edits no file a part owns. It may do a sequential
   step that no part touches, filling in its own stubs from the contract included, and commits
   it before the merges as a work commit, typed, with no version:
   `refactor: <what changed> (<id>, before the merges)`. It answers the parts' questions as
   *A part's questions* says. A part that waits for the human holds only the merge, and the
   other parts keep going.
6. **Each part commits on its branch** like any work commit: typed, with no version, naming the
   part (`feat: the search index for saved notes (part 8.2)`), with `verify` run on the drafted
   message last.

**Integration**, in this order:
1. **Wait for every part.** A part that stopped for the human holds integration until the human
   answers in that part's chat, or drops the part. A dropped part's steps go back into the plan
   as an amendment logged with the human, and with their yes the session commit removes or
   neutralises that part's stubs and registry entries from the contract, so nothing ships
   pointing at a stub. If the chat has to end first, the coordinator writes in the handoff
   which parts are open and on which branches (the mid-round template has the line), commits
   only the handoff as `docs: <id> paused at integration` with the docs-only command, and the
   next chat finishes this session from integration. Its done-note comes then, so rule 5 holds
   when the session ends.
2. **Check each branch against its brief:** `git diff --stat HEAD...<branch>` lists only what
   the part itself changed, even after it merged a contract change. If a part changed a file
   outside its list (its own note is always on it, and a file the human added is logged there),
   stop and show the human before merging anything.
3. **Merge, with the human's yes,** onto a tree with no uncommitted change outside the shared
   documents (no part touches those, and they wait for the session commit), in a stated
   order, one `--no-ff` merge per part, so history keeps each part visible. A merge is a
   commit like any other: `git merge --no-ff --no-commit <branch>`, a typed message drafted
   (`merge: part 8.1 — <what it adds>`), `verify` run with it, then `git commit -F`. A conflict
   stops the merge and goes to the human; it is never resolved by a guess.
4. **Prove the seam, then run the full gates once, on the combined code.** First add the
   end-to-end tests the briefs left to integration. Then run `verify`, every harness the block
   names, and the exclusive harnesses the parts could not run. The baseline counts must equal
   the starting counts plus each part's delta, the coordinator's own and integration's.
   Whatever fails here, the coordinator fixes in the session commit and records in the
   done-note's *Parts* section. A fix that changes behaviour or scope is the human's call first.
5. **Write the shared documents:** the done-note with its *Parts* section, the CHANGELOG entry,
   the version, the baseline and the handoff. Then the session commit, with `verify` run on its
   drafted message last, as always.
6. **Clean up,** right after the session commit, so no part leaves anything behind. For each
   part, `git worktree remove <path>`, then `git branch -d <branch>`, and the same for a
   contract-change branch; then `git worktree prune`. It is safe by construction: git refuses
   to remove a checkout that holds uncommitted or untracked work, and `-d` refuses a branch
   that is not merged. A refusal stops the cleanup and goes to the human; never `--force` or
   `-D` without their yes. A dropped part's branch was never merged, so it goes only with the
   human's yes, asked before the session commit; a branch they keep is listed as kept in the
   done-note's *Parts* section. A branch kept after a refusal here is recorded there by the
   review commit, which adds to the done-note anyway. The next session's entry gate checks
   that nothing else is left.
7. **Review as one session.** The human's review (the cycle, step 11) covers the contract, every
   part's commits, the merges and the session commit together, because integration bugs show
   only in the combined code.

**Limits.**
- **At most three parts per session,** to begin with. Each is another chat the human may need to
  answer and another branch to review. The limit can rise as more sessions run parts.
- **Parts do not split further.** A part never starts parts of its own.
- **Parts live inside one session.** Running whole sessions of a round in parallel touches the
  round plan, the handoff, versions and reviews at once, and is not part of this workflow.

## What each commit touches

| Commit | Adds | Updates | Removes |
|---|---|---|---|
| **Intake**: `docs: open round <N> — <theme>` | the round's spec files and `SESSIONS.md` | `docs/BACKLOG.md` (what the intake declined); the handoff, which now opens the first session | from `docs/BACKLOG.md`, the items the round picked up |
| **Contract** for a session's parts: `feat:` or `refactor:`, ending `(<id>, the parts' contract)` | the interfaces the parts meet at, as code: types, signatures and stubs that fail if called | the shared registries, with the entries the parts would each add, pointing at the stubs | — |
| **Part** of a session: `feat:`, `fix:` or `refactor:`, ending `(part <id>.<n>)` | the code and tests in the files its brief lists; its part note, `docs/spec/parts/<id>.<n>.md` | only the area docs its brief names | code the part retires, inside its own files |
| **Session**: `feat:`, `fix:` or `refactor:`, ending `(x.y.z)` | the code and its tests; the version's CHANGELOG entry; the block's done-note | the manifest's version; `ARCHITECTURE.md` and its baseline; `USER-GUIDE.md`; `README.md`; `CLAUDE.md`; `docs/BACKLOG.md`; the handoff | code the session retires, and every doc line that described it; after parallel parts, `docs/spec/parts/` |
| **Review**: `review: …` | the fixes and their tests; the done-note's Review section (after a close, the commit message carries it) | that version's CHANGELOG entry; the docs the fixes touched; the handoff; `SESSIONS.md`, if the review opened a session | — |
| **Architecture review**: `docs: architecture review — <milestone>` | `docs/spec/REVIEW-<date>.md`; the block's done-note | `SESSIONS.md` (the blocks amended from it, a session it inserted, the versions that shift); the spec, where the human amended a decision; the handoff | — |
| **Close**: `docs: close round <N>` | — | the handoff, which now says no round is in progress | every file in `docs/spec/` |

- **After parallel parts,** the contract (when the parts meet at an interface), the
  coordinator's own work commit (if it did a sequential step) and one `--no-ff` merge per part
  come first, each typed and checked like any commit. The
  session commit follows them with the integration's fixes, the version and the shared
  documents (see *Parallel parts*).
- **An intake's review** fixes the spec and the plan. It has no CHANGELOG entry and no version.
  Neither has an architecture review, whose `/code-review` follows like any session's.
- **A session nobody can see** (docs, comments, an invisible refactor) has no CHANGELOG entry
  and no version bump, and its subject carries no version. Where each session is a release,
  the round's last session is the exception: it releases the round (see *Rounds*).
- **Before round 1's documentation pass**, `ARCHITECTURE.md` and `USER-GUIDE.md` do not exist
  yet. Sessions keep the README and `CLAUDE.md` true, and their done-notes carry what the pass
  will need: the deliberate decisions and the counts.
- **Never removed or rewritten:** a CHANGELOG entry's content; a done-note, which the review
  only adds to; a resolved decision, which is amended beside itself rather than edited.

## The rules

1. **One chat per session, never two sessions in one chat.** The split exists so no session
   inherits stale context or a half-formed plan. A session's parallel parts run in chats of
   their own, which the session starts and which end with it.
2. **The spec is read-only.** A session that finds the spec wrong stops and says so. Agreed
   divergence is recorded as an amendment in the block, and a decision the human changes is
   added to the spec as a dated amendment. The spec's original text stays true to its moment.
3. **The plan is amendable, but every amendment carries a reason, logged.** An unexplained
   scope change six sessions deep is indistinguishable from a session quietly dropping work
   it could not finish.
4. **Nothing ships past a red gate.** `verify` green is the condition for every commit; a commit
   that changes only docs needs only the stages that read them (the cycle, step 6). No CI
   enforces it; the session does, and the human's review and push approval stand behind it.
5. **Every session ends with a done-note, a handoff and a commit.** No exceptions for short
   sessions. An intake's plan stands in for its done-note.
6. **Pushes and tags are a human decision**, approved one at a time.
7. **When a session goes wrong, stop** (step 5 of the cycle).

## Git

- **Commit locally; push only on explicit approval, each time.** Approval for one push does not
  extend to the next, and approving a plan is not approving a push. The human's review comes
  before the push.
- **Before any push, list `git log --format='%h %s' origin/main..HEAD`.** Parallel sessions may
  share a checkout, and a push carries whatever is on the branch, not only this session's work.
  If a commit is not this session's, stop and say so. A session's contract, its part commits,
  its work commit before the merges and the merges are its own.
- **Stage explicit paths; never `git add -A`.** Same reason.
- **Each parallel part has its own checkout and branch**, cut from the contract commit, or from
  the commit the session started on when there is none, its checkout outside the repository's
  folder or in an ignored path (see *Parallel parts*). A part never cherry-picks. Only the
  coordinator merges, onto a tree clean outside the shared documents (*Parallel parts*,
  integration step 3): one `--no-ff` merge per part, after the human's yes,
  made with `--no-commit` and committed from a drafted message that `verify` has checked, never
  with git's own message. Part branches are never pushed. Right after the session commit they
  are deleted and their checkouts removed (*Parallel parts*, integration step 6).
- **Never undo with `git checkout <path>`.** It silently discards changes that may not be
  yours. Revert your own edits by editing, and falsify a check by copying the file aside and
  back.
- **A commit subject is typed and names what changed in plain words**, the way a reader of the
  product would put it. The body says what changed and why.
  - `docs: open round <N> — <theme>` for an intake.
  - `feat:`, `fix:` or `refactor:` for a session's work, ending with the version when it cuts
    one: `feat: quiet copies — one a day, kept 7 days, restored from Help (0.41.0)`.
  - The same types for a parallel part's work, ending with the part and no version:
    `feat: the search index for saved notes (part 8.2)`. The coordinator's own step before the
    merges ends `(<id>, before the merges)`, and its contract `(<id>, the parts' contract)`:
    `refactor: the search index's interface, stubbed (8, the parts' contract)`.
  - `merge: part <id>.<n> — <what it adds>` for each part's merge, and
    `merge: the contract change — <what changed>` for each merge of a contract change.
  - `docs: <id> paused at integration` for a session whose chat ends with parts still open.
  - `docs: <what it wrote> (x.y.z)` for the round's last session when its own work is
    documentation.
  - `review: <the findings, in plain words>, and <n> smaller` for the review commit.
  - `docs: architecture review — <milestone>` for an architecture review.
  - `docs: close round <N>` for the close.
- **An approved push carries its own note.** The handoff was written before the push, so before
  pushing, the session rewrites the handoff's push status to say what the remote will hold,
  writes that commit's message into the draft (`git log -1 --format=%B`) and runs the
  docs-only command with it (hygiene, plus the format check and lint where they cover docs,
  not the full `verify`: the note changes only the handoff), folds the note into the last
  unpushed commit (`git commit --amend`; it is this session's own and not yet on the remote),
  lists
  `origin/main..HEAD` again, and pushes once. The note names that commit by its subject, since
  amending changes its sha. No commit follows the push, so the next session starts with
  `origin/main..HEAD` empty. If the push fails, amend the note back before anything else.
- **Tags mark milestones only**, such as a first build for testers or 1.0, and the intake names
  them: the session table marks a milestone's version as tagged. The session that cuts it asks
  before tagging; on a yes it tags locally, and the tag is pushed only with its own approval.

## Nothing private in the repository

Every repository is written as if its history will be published, because any of them may be,
and the history is the one part that cannot be cleaned afterwards without rewriting it. Every
file, test, fixture, done-note, handoff and commit message follows these rules:

- **No personal names, no names of the owner's other projects or games, no agent personas.**
  Write the role: "the owner", "the first game", "the coding agent".
- **No home-directory paths.** Write `~`.
- **No AI attribution:** no co-author trailers, no lines saying a tool generated the work, no
  links to AI sessions, no AI credited as author, co-author or addressee. Naming a tool the
  product supports or the workflow uses is fine.
- **Fixtures are fictional** (see *Day one*).
- **Done-notes and handoffs never name the owner's own data:** their documents, folders,
  presets. Record what was measured, not whose it was.
- **Unsure whether something is public? Ask before writing it.** A session about to write a
  name, a place or a detail it cannot tell is public stops and asks the owner. A "private"
  answer goes on the term list, so the stage catches that word from then on; a "public" answer
  needs nothing. The list grows from real cases instead of a definition written up front.

What is already public is not private. The owner's name and email are on every commit as its
author, so they are private only when the commits use another identity; the same goes for what
the owner publishes on purpose, such as the copyright holder in the licence or a bundle id. The
intake decides what is private, and which git identity the commits use: the stage below reads
files and messages, not author fields.

**The hygiene stage of `verify` enforces the rules.** It checks every file, path and commit
message, the drafted one included, against the owner's term list, and every commit message for
AI attribution. Session 1 builds it in the project's own language, from the design in the
workflow kit's `KICKOFF.md`, which Session 0 copies into Session 1's block; from then on the
stage's code holds the design, and ARCHITECTURE's testing strategy describes it.
- **The term list lives outside every repository,** in one plain-text file,
  `~/.config/hygiene/terms.txt`: one word per line, shared by all of the owner's projects,
  because a name private in one is private in all. No repository holds the terms, nor hashes
  of them. A missing file fails the stage, so a new machine cannot pass by accident; copying
  that one file is the whole setup. An empty file is a valid answer: nothing is private beyond
  what is already public.
- **A term is one word.** A two-word or hyphenated name is given as its words, leaving out any
  that is an ordinary word on its own. The owner edits the file, or names a new term in chat
  and the session adds it with their yes.
- **A hit never prints the term.** It names the file and line, or the commit and message line;
  the line itself shows which word it is.

**When it runs: once, as the last act before each commit, message included.** The session
writes the done-note and the handoff, drafts the commit message into the draft file
`COMMIT_DRAFT` in the git directory (`"$(git rev-parse --git-dir)/COMMIT_DRAFT"`: `.git/` in a
plain checkout, elsewhere in a worktree, never committed either way), runs `verify` with it
(`<verify command> --message <draft>`, or the docs-only command for a commit that changes only
docs), and commits with `git commit -F <draft>`. The draft is
written fresh for every commit and deleted after it, so an old message is never committed
again. The stage checks the draft beside the files and every earlier message, so nothing is
checked after the fact and no commit is amended for it. The review commit and the push note's
amend go the same way.

**Watch for:** a commit message that quotes an attribution form, even to describe this rule,
fails the stage. Describe it in words.

## Versions and the changelog

- **Each session is a release.** `SESSIONS.md`'s table gives each session its version:
  - a minor bump for a session that adds or changes behaviour;
  - a fix bump for one that only fixes;
  - no bump for one whose changes no user can see, except the round's last session, which
    releases the round.

  The version is `major.minor.fix` in the manifest and is shown in the product.
- **One entry per version**, `## [x.y.z] — YYYY-MM-DD`, using only Keep a Changelog's headings
  (Added, Changed, Deprecated, Removed, Fixed, Security). Prose that fits none of them goes in
  the entry's lead paragraph. `[Unreleased]` stays at the top and stays empty, because each
  session writes its own version's entry.
- **Written for the person using the product.** Each line leads with what they will notice, in
  bold, then what it means for them. No file names, no session ids.
- **Review fixes join the session's entry**, under the same version.
- **Append-only for content.** A shipped entry's substance is never reworded. Normalising a
  heading to the set above is allowed.
- **Every version has a link reference** at the foot of the file once the repo has a remote.
  A tagged version links to its tag, and an untagged one to the commit that bumped the manifest
  to it. `[Unreleased]` compares the newest tag with `HEAD`; until the first tag it has no link.

## The handoff

`docs/next-session-prompt.md`, rewritten by every session (the templates are at the end of this
file). The chat's last message is never the handoff itself, only the one-line opener that
points at it (the cycle, step 12), so the next session reads the handoff from the repository,
as committed. It holds:

- **The opening paragraph:** the read order (the block's Read line, copied), the session to do
  and what it covers, and the version it releases.
- **State you need that is not in the specs:**
  - the round's progress: which sessions are done, which remain;
  - the push status: what the remote holds (`origin/main` holds everything through `<sha>`),
    and which local commits wait for the next approved push;
  - what shipped that this session builds on, specifically: the names, files and mechanisms
    it will touch, as they are now;
  - what deferred work this session now owns;
  - what the spec said that turned out wrong or harder than described (be specific — this is
    the part that saves a session);
  - decisions made that are written down nowhere else;
  - the baseline counts this session must not move, or must move by a stated amount;
  - what was not done, and why.
- **Watch for:**
  - **the standing traps**, the environment's gotchas that every session hits (the shell, the
    toolchain, the harnesses, git). They are carried forward verbatim from handoff to handoff,
    and edited only when one stops being true, or when a new one has cost a session time;
  - this session's own traps: the block's, plus anything newly discovered.

Two rules govern it:

1. **Report what happened, not what was intended.** If something was skipped, say it was
   skipped and why. A handoff that describes the plan rather than the outcome is worse than
   no handoff, because the next session trusts it.
2. **It adds context; it does not replace the contract.** The next session still reads the
   spec first.

When no round is in progress, the handoff says so in its first line, the opener says so too,
and the next session is an intake.

## Engineering practices

- **Falsify every new check before believing it.** Break the assertion, watch it fail at its
  own step and nowhere else, restore it. A check that cannot fail looks identical to a passing
  one. When two layers guard one promise, break the layer the check is about, or the
  falsification passes for the wrong reason.
- **Keep a baseline of counts** (tests passed and skipped, harness checks, endpoints or
  channels) in `ARCHITECTURE.md`. A refactor or a docs change must not move any of them; if
  one moves, that is the finding.
- **Measure, don't assume.** A claim about how the runtime behaves (a timestamp, a platform
  default, a size) is measured in the real runtime and recorded in the done-note as measured,
  with the version it was measured on.
- **Real-runtime probes for anything version-sensitive.** When the test runner and the shipped
  runtime differ, keep one-command probes that re-answer the load-bearing questions inside
  the real runtime.
- **Record checks by hand too.** When the human tries the build, or a session runs something no
  harness can, the done-note records each outcome. What nobody checked goes to BACKLOG's *Not
  yet checked by hand*, with the automated check that covers most of it.
- **Design for headless drivability.** Automation never opens anything it cannot dismiss
  (native dialogs, OS prompts, modal pickers). Split every dialog-then-write flow into two
  separately drivable steps, so a harness asserts the control and drives the write directly.
- **Harnesses own their state and clean it up structurally.** A harness creates its own data
  directory, never touches one a person owns, and registers its cleanup with the runner
  rather than relying on each caller to remember. Sweep stale leftovers at startup, which
  covers a run that was killed. Measure the leak before and after rather than assuming.
- **Measurements are committed tests.** A performance number that matters lives in a gated
  benchmark with a budget that fails rather than warns, not in a table typed by hand.
- **A test whose outcome depends on the clock pins the clock.** Otherwise it can pass
  vacuously for weeks.
- **One home per shape.** A duplicate that has drifted once will drift again. The second
  copy of a component, hook or rule is the moment to share it, not the fifth.
- **Comments and names state the present.** A comment, a name or a test title says why the
  code is shaped the way it is now. It never carries a session id, "used to", "until v0.x" or
  a review story; git history and the done-notes keep those. Where the story *is* the reason,
  keep one clause.
- **"Watch for" lines.** Every session block ends with its specific traps, because traps are
  what a fresh context misses. When a trap costs a session an hour anyway, the next prompt
  names it.
- **Deviations over corrections.** When mockups, spec and build disagree, resolve the conflict
  once, write down which one wins and why, and leave the artifacts as they are.
- **Keep the rules file lean.** `CLAUDE.md` holds what is always true; the done-notes and
  `ARCHITECTURE.md` hold why. A rule keeps its "why" only when the why changes the action.
  Duplicating prose between them guarantees drift.
- **Keep `ARCHITECTURE.md` readable by section.** A session reads its overview, invariants and
  baseline, and only the sections its block names, so each section stands on its own. A
  deliberate decision lives in the section of the subsystem it belongs to; the closing
  *Deliberate decisions* holds only the ones that span subsystems, and a decision about a
  single module is that module's comment. Without this the file grows into one list nobody can
  read in a session: one project's reached 3,200 lines, half of them a single list of
  decisions.

## Adapting the workflow

The method was written on a desktop app with a UI, a local database and several harnesses.
Other projects keep its core, adapt the rest, and say which parts they adapted in their *In
this repository* paragraph.

- **The core, which no project drops:**
  - scope and state kept apart: `docs/spec/` and the handoff;
  - one session per chat, planned with the human before any edit;
  - stopping to ask rather than guessing;
  - intake decisions that are not re-litigated, and amendments logged with their reasons;
  - a done-note, a handoff and a local commit at the end of every session, then the human's
    review and a separate review commit;
  - pushes and tags only with approval;
  - every new check falsified;
  - nothing private in the repository, enforced by the hygiene stage.
- **What depends on the project:**
  - what `verify` runs, and which harnesses exist beside it;
  - whether every session cuts a version and a CHANGELOG entry, or the project releases at
    milestones;
  - whether there is a user guide, and what "user-visible" means;
  - headless drivability, which matters only where there is something to drive;
  - the git remote, which decides pushes, tags and changelog links;
  - CI, which this workflow does not use; a project that adds it says so;
  - parallel parts, which are optional (see *Parallel parts*), and the project's exclusive
    resources, named in `CLAUDE.md`: the harnesses, apps, ports and devices only one chat can
    use at a time.

## Templates

Each template is generalised from a real round. Fill every `<…>`; the text around it is the
real wording, to be kept unless it is wrong for the project.

### `docs/spec/SESSIONS.md`: the header and the table

In round 1 the spec files are `SPEC.md` and `PDD.md`, and the read order drops
`docs/ARCHITECTURE.md` until the documentation pass writes it.

```markdown
# The session plan: round <N>, <theme>

One chat per session. Never run two sessions in one chat: the point of the split is that no
session inherits stale context.

- **Scope** lives here, fixed in advance. The request and its resolved decisions are in
  `<FEATURE-name.md>` beside this file, which is read-only; do not re-litigate them. If a
  session finds a block wrong, it amends the block and records the reason both in the block
  and in `docs/next-session-prompt.md`.
- **State** is `CHANGELOG.md` (what shipped) and `docs/next-session-prompt.md` (what the next
  session needs that the docs do not say). Every session rewrites the prompt as its last act.
- **When this round closes,** <last id> deletes this file and the feature file(s). Git history
  keeps them, and anything left open moves to `docs/BACKLOG.md`.

**Before starting**, confirm the tree is clean and that `docs/next-session-prompt.md` names the
session you are about to do. **Read** in the workflow's order (the cycle, step 2), starting
with what your block's **Read** line names. **Then plan, in plan mode,** and ask the block's
*Ask at the start* questions before any edit.

**Every session ends the same way:**
1. `<verify command>` green, plus the harnesses the block names; for a commit that changes only
   docs, `<the docs-only command>`.
2. Every new check falsified.
3. A `CHANGELOG.md` entry for anything user-visible, and the version bumped as the table says.
4. The docs the block names kept true, and ARCHITECTURE's baseline table updated for any count
   that moved.
5. A done-note appended to the block, then the prompt rewritten, then the same command run
   with the drafted commit message, then a local commit.
6. The user's `/code-review high`, then a review-fixes commit.
7. The one-line opener for the next session printed as the chat's last message.

Never push without being asked.

| Session | Scope | Decisions | Version |
|---|---|---|---|
| <id> | <one line> | <decision numbers> | **<x.y.z>** |
| <id> | Architecture review: <milestone> | <decision numbers> | — |
| <last id> | Docs, changelog, manual runs; close the round | <decision numbers> | **<x.y.z>**<, tagged, when the intake made it a milestone> |
```

### A session block

```markdown
---

## <id>: <title>

<Only for a session inserted mid-round: when and why it was added, in the user's words where
they gave them.>

**Read:** `<spec file>` <decisions and invariants by number, sections by name>; `PDD.md`
<parts and items, in round 1>; `ARCHITECTURE.md` <sections>; `REVIEW-<date>.md` <findings,
where a review amended this block>.

- **<Scope item>** (decision <n>).
  - <What changes, with `file:line` pointers to the code as the intake saw it.>
  - <The constraints: which invariant it touches, which layer the new code belongs in.>
- **<Scope item>** (decision <n>).
- **Docs.**
  - `CLAUDE.md`: <the rule that changes>.
  - `USER-GUIDE.md`: <the sections that change>.
  - `ARCHITECTURE.md`: <the sections that change>.
  - README: <what changes>.
- **Ask at the start of <id>:** <a decision left to this session, because only it will have
  the context>. <For a proposal the user has not confirmed: "Proposed at intake, not yet
  confirmed.">

**Gates:**
- `<verify command>`, with these tests:
  - <each behaviour the session must prove>.
- `<harness>`: <what it must show>.

**Watch for:**
- **<The trap>.** <Why it bites, and what to do instead.>
```

### The done-note

The session appends it to its block before the local commit; the review adds its section
before the review commit. Leave out a line that has nothing in it.

```markdown
**Done (<YYYY-MM-DD>, <x.y.z>).**
- **Decided with the user at the start:**
  - <each *Ask at the start* answer, and anything settled in plan mode>.
- **<What shipped, one bullet per area>.** <The names, files and mechanisms, as built.>
- **Amendments:**
  - **<What differs from the block>.** <Why, and who decided: "amended at plan time, with the
    user", "added mid-session by the user", or the session's own finding.>
- **Measured:** <each claim about runtime behaviour, and the version it was measured on>.
- **Checked by hand:** <each manual run, and the user's look at the build, with the outcome.
  What was left unchecked goes to `docs/BACKLOG.md`.>
- **Falsified:**
  - <each new check: what was broken, and that it failed at its own step>.
- **Numbers:** <n> unit tests (<±d>), <n> `<harness>` checks (<±d>), <the other baseline
  counts>.
- **Parts** (<n>, run in parallel with the user's yes; <the contract-change branch, if there
  was one>; cut from <the contract commit, by its
  subject, or the commit the session started on>; merged in the order <id>.<n>, <id>.<n>):
  - **<id>.<n>, <title>,** on `<branch>`. <What it built, measured and falsified, from its
    note; each stop, who answered it and the answer; anything it could not finish.> Wall time
    <hh:mm>–<hh:mm>.
  - **The coordinator, meanwhile:** <the contract, its own steps, and its commits before the
    merges>.
  - **Integration fixes:** <the end-to-end tests that prove the seam, and each failure the
    combined gates found, with its fix>.
  - **The counts add up:** <the starting counts, plus each part's delta, the coordinator's and
    integration's, summing to the baseline>.
  - **Time saved:** <about how much, against an estimate of the same steps one after another;
    what the split did not shorten>.
  - **What the briefs missed:** <each gap a part had to stop on, or "nothing">.
  - **Merge trouble:** <each conflict and how the user resolved it, or "none">.
  - **Kept:** <a part branch the user chose to keep, and why; the cleanup removed the rest>.
- **Review (`/code-review high`), <n> findings: <n> fixed and <n> skipped.**
  - **<The finding, as fixed>.** <One line; "the user chose" where they decided.>
  - **Skipped:** <the finding>, because <the reason>.
  - **Falsified:** <the checks the fixes added>.
  - **Numbers after the review:** <counts>.
```

### A part brief

The coordinator writes one for each parallel part and gives it to the part as its first message
(see *Parallel parts*). Fill every `<…>`.

```markdown
You are part <id>.<n> of Session <id>, in a checkout of your own, on a branch cut from
`<sha>` (<the parts' contract, or the commit the session started on>). The scope below is from
the plan the user approved in Session <id>'s chat; work inside it. Read <the Read line>,
CLAUDE.md, then this brief.

**Background.** <One paragraph: what Session <id> builds and why, what the other parts and the
coordinator build meanwhile, and where this part meets them.>

You are not a session. This brief replaces the session cycle for you, whatever CLAUDE.md says
every session does: no entry gate, no plan mode, no done-note, no handoff, no CHANGELOG entry,
no version and no `/code-review`; the coordinator's review covers your commits.

**Scope**, in the approved plan's words:
- <each plan step this part owns>.

**The interface**, word for word from the plan: <only where this part meets another: the
signatures, types and registry entries, as the contract holds them>. Changing it is a stop.

**Files you may change:** `<path>`, `<path>`, and your note. To touch any other file, stop and
ask.

**Setup:** `<what a fresh checkout needs before verify runs>`.

**Starting counts** at `<sha>`: <n> unit tests, <the other baseline counts>.

**Gates:**
- `<verify command>` green on this branch, with these tests:
  - <each behaviour the part must prove>.
- Each new check falsified once.
- Do not run `<the exclusive harnesses>`; integration runs them.
- **Proved only after the merge:** <the end-to-end tests across the seam>. Do not write them;
  the coordinator adds them at integration.

**Your note:** `docs/spec/parts/<id>.<n>.md`, committed with your work. It holds what you built,
measured and falsified; your numbers, from the starting counts above; your wall time, from
start to commit; what the shared documents need (CHANGELOG lines, ARCHITECTURE sections and
counts, backlog items); each question you asked, who answered it and the answer; and anything
not finished.

**Commit** on this branch: `<type>: <what changed> (part <id>.<n>)`, with no version, and
`verify` run on the drafted message last.

**Never:** write `SESSIONS.md`, the handoff, `CHANGELOG.md`, the version, ARCHITECTURE's
baseline, `docs/BACKLOG.md` or `CLAUDE.md`; cherry-pick; merge, except a contract change the
coordinator sends you; push; start parts of your own.

**Stop** when the spec turns out wrong, a decision is underdetermined or the interface does not
hold, and ask the coordinator: <how this part reaches it: through the user, or a message to
Session <id>'s chat>. The coordinator answers what the approved plan settles; anything that changes
behaviour, scope, the interface or your file list goes to the user. Wait for the answer, and
never guess to keep the parallel run going.

**Watch for:**
- **<A trap of the block's that reaches this part>.** <Why it bites, and what to do instead.>
```

### `docs/spec/parts/<id>.<n>.md`, a part note

The part commits it with its work. The coordinator folds it into the done-note's *Parts* section
and deletes it in the session commit. Leave out a line that has nothing in it.

```markdown
# Part <id>.<n>: <title>

- **Built:** <the names, files and mechanisms, as built>.
- **Measured:** <each claim about runtime behaviour, and the version it was measured on>.
- **Falsified:**
  - <each new check: what was broken, and that it failed at its own step>.
- **Numbers:** from <the brief's starting counts> to <n> unit tests (<±d>), <the other counts
  this part moved>.
- **Wall time:** <hh:mm>–<hh:mm>.
- **For the shared documents:** <the CHANGELOG lines, the ARCHITECTURE sections and baseline
  counts, and the backlog items, for the coordinator to write>.
- **Stopped:** <each question, who answered it (the coordinator or the user), and the answer>.
- **Not finished:** <what, and why>.
```

### `docs/spec/FEATURE-<name>.md`

```markdown
# FEATURE: <name> (round <N>)

## The request, verbatim (<YYYY-MM-DD>)

> <The user's words, exactly as given.>

<One paragraph: what the intake did (an audit, a proposal of groups of work), what the user
took and what they declined, and anything they said about audience or platform.>

## <Optional: the promise or contract the round exists to keep>

## Resolved decisions (<YYYY-MM-DD>, at intake)

1. **<The decision, in one sentence>.** <The detail a session needs, and the why where the
   why changes the action.>
2. **<The sessions, in order>:** <ids and versions>, when the intake decided them.
3. <…>
<n>. **Out of this round:** <what was declined or deferred; each item also goes to
   `docs/BACKLOG.md`>.
```

A decision the user changes later is added beside the one it changes, never edited in:

```markdown
**Amended <YYYY-MM-DD> by the user, in <id>.** <Which decision changes, to what, and why.>
```

### `docs/spec/REVIEW-<date>.md`, an architecture review

Two rules make a review worth its session. The read is done twice, the second time cold,
without sight of the first read's notes, and a finding is kept only once it has been verified
on the code path. Every claim points at `file:line` on a named commit, or at a measurement.

```markdown
# Review <id>: the architecture after the <milestone> milestone

Session <id> of round <N>, <YYYY-MM-DD>. The code and tests of sessions <ids> were read cold
against `<spec file>` <sections>. This file is evidence, not scope: it changes no product code
and cuts no version. The user chose, in plan mode, which findings become scope; §6 says where
each landed.

**Method.** <What was read in full, and against what; the second, independent read, and how its
findings were verified; what was run and measured, before and after.>

**Severity.** *High*: an invariant is weakened, or a planned session is blocked as written.
*Medium*: a defect or debt that will cost a later session. *Low*: drift or untidiness. *Info*:
recorded, no remedy.

## 1. The baseline as found

| Check | Found |
|---|---|
| `<verify command>` | <stages, counts, times> |
| `<harness>` | <counts, wall time> |

## 2. What holds

<Each boundary and invariant that holds, and where it is enforced (`file:line`).>

## 3. Findings

### High

**F1. <The finding, in one sentence>.** <The evidence, at `file:line` or as a measurement;
what it costs, and which session it reaches.> *Remedy:* <the proposed fix>.

### Medium

### Low

### Info

## 4. Duplication and drift

## 5. <The subsystems> against sessions <ids>

<What the sessions ahead will put through the code as it stands, and what has to change first.>

## 6. Where each finding landed

Decided by the user in plan mode, <YYYY-MM-DD>:

| Finding | Where |
|---|---|
| F1 | Block <id>: <the amendment> |
| F<n>, F<n> | Inserted session **<id>a: review fixes — <milestone>**, <x.y.z>; later versions shift |
| F<n> | Recorded here; nothing to do |

## 7. Measured

<Each measurement, and the version it was measured on.>
```

### `docs/next-session-prompt.md`, mid-round

```markdown
Read <the block's Read line, copied>, CLAUDE.md, docs/ARCHITECTURE.md's overview, invariants
and baseline (once it exists), your block in docs/spec/SESSIONS.md and the previous block's
done-note, then this file, and do Session <id>: <one line>. <id> covers:
- <part>;
- <part>.

Release <x.y.z>.

## State you need that is not in the specs

- **Round <N> is <theme>**: <ids> are done; <ids> remain.
- **<Previous id> and its review fixes are committed locally, not pushed.** `origin/main` holds
  everything through `<sha>`. List `origin/main..HEAD` before any push.
- **What <previous id> left that <id> builds on:**
  - <the names, files and mechanisms this session will touch, as they are now>.
- **What the spec said that turned out wrong or harder:** <specifically>.
- **Decisions written down nowhere else:** <…>.
- **Baseline** (ARCHITECTURE's table, v<x.y.z>): <the counts>.
- **Not done:** <what was skipped, and why>.
- **<Only when a session ended with a parallel part still open:> <id> is unfinished.** Part
  <id>.<n> is open on `<branch>`, waiting for <the question>; parts <ids> are finished on
  `<branches>` and not merged. The next chat finishes <id> from integration and starts no other
  session first.

## Watch for

- **The standing traps:**
  - **Never `git checkout <path>`.** Falsify by copying the file aside and back.
  - **Stage explicit paths.**
  - **Give stdin-reading commands `< /dev/null`.** A bare `cat > file` with no heredoc hangs.
  - **Nothing private in any file or commit message.** Last before each commit: draft the
    message into `"$(git rev-parse --git-dir)/COMMIT_DRAFT"`, run `verify` with it (the
    docs-only command when the commit changes only docs), commit with `git commit -F`, delete
    the draft.
  - <the environment's other gotchas, carried forward verbatim>.
- **<A trap specific to this session>.** <Why it bites, and what to do instead.>
- **Comments state the present.** No session ids, no "used to", no review stories.
- **Every push needs its own approval.** List `origin/main..HEAD` first, because parallel
  sessions share this checkout.
```

### `docs/next-session-prompt.md`, between rounds

```markdown
No round is in progress. Read CLAUDE.md, docs/ARCHITECTURE.md's overview, invariants and
baseline, then this file; then the sections of docs/ARCHITECTURE.md the request touches. The
next session is an **intake**: write the user's request down verbatim, ask what it leaves open,
and plan the round in `docs/spec/`, a Read line in every block (see
`docs/DEVELOPMENT-WORKFLOW.md`, *Rounds*).

## State you need that is not in the docs

- **<Product> <x.y.z> is released.** Round <N> (<theme>: <ids>) is closed. Its spec files are
  deleted; the last done-note is in `docs/spec/SESSIONS.md` as of the release commit (`<sha>`).
- **Push status:** <what the remote holds, and what waits for the next approved push>.
- **Open items live in `docs/BACKLOG.md`**, <including what this round added there>.
- **Baseline** (ARCHITECTURE's table, v<x.y.z>): <the counts>.

## Watch for

- **The standing traps:**
  - **Nothing private in any file or commit message.** Last before each commit: draft the
    message into `"$(git rev-parse --git-dir)/COMMIT_DRAFT"`, run `verify` with it (the
    docs-only command when the commit changes only docs), commit with `git commit -F`, delete
    the draft.
  - <the others, carried forward verbatim>.
- **Comments state the present.** No session ids, no "used to", no review stories.
```

### `docs/BACKLOG.md`

```markdown
# Backlog

Work that has been named but belongs to no session. Each item needs its own intake before it runs
(a `docs/spec/FEATURE-*.md` with the request verbatim and the decisions resolved, then a block
in `docs/spec/SESSIONS.md`; see `docs/DEVELOPMENT-WORKFLOW.md`). When an item is picked up, it
moves out of this file.

## Deferred

- **<The item>.** <What it is, and where it was left out: "left out at the round <N> intake".>

## Not yet checked by hand

Checks that need a person at the machine. Each has an automated check covering most of it;
these are the parts only a person can see.

- **<What to check>** (<version>): <how to check it, and which automated check covers the rest>.

## Directions

- **<A possible future>.** <What it would be, and what already points towards it.>

## Decided against

- <What, and the reason in a clause.>
```

### `CHANGELOG.md`

```markdown
# Changelog

All notable changes to <Product> are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/); versions follow
`major.minor.fix`.

## [Unreleased]

## [0.2.0] — <YYYY-MM-DD>

### Added

- **<What the user notices>.** <What it means for them.>

### Fixed

- **<What now works>.** <What went wrong before.>

## [0.1.0] — <YYYY-MM-DD>

### Added

- **<…>.**

[Unreleased]: <repo URL>/compare/<newest tag>...HEAD
[0.2.0]: <repo URL>/commit/<full sha of the commit that bumped the manifest to 0.2.0>
[0.1.0]: <repo URL>/commit/<full sha>
```

### `docs/ARCHITECTURE.md`: the outline

```markdown
# <Product> — architecture, as built

<One paragraph: this is the system as it stands, kept true by every session.>

## Overview

## The invariants

<Numbered, each with how it is enforced: a lint rule, a type, a test.>

## <The contract between the layers>

## <The data layer: storage, migrations, backups, durability>

## <One section per subsystem>

<Each section stands on its own, because a session reads only the sections its block names:
what the subsystem does, its contracts, and the deliberate decisions that belong to it, each
with its why.>

## Testing strategy — <n> layers, and what each cannot see

## Packaging

## Deliberate decisions

<Only the choices that span subsystems and look wrong until you know why, each with its why. A
decision that belongs to one subsystem lives in that subsystem's section.>

## Known debt, deliberately deferred

## Baseline (v<x.y.z>)

| Check | Expected |
|---|---|
| `<verify command>` | green — <n> unit tests passed (<n> skipped: <which, and why>) |
| `<harness>` | <n> PASS; <how long it takes, and its quirks> |
| <an interface count: channels, endpoints, commands> | <n> |

A documentation or refactoring change should not move any of these. If one moves, that
is the finding.
```
