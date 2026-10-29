# Kickoff: Session 0 of a new project

You have been given three files: this one, `DEVELOPMENT-WORKFLOW.md`, and a product design draft
(the **PDD**). Read `DEVELOPMENT-WORKFLOW.md` in full first, templates included, then the PDD.
This project will be built under that workflow, and this chat is its **Session 0: the intake
of round 1**.

Session 0 writes documents and no product code. Its output is:
- a repository in which Session 1 can start cold;
- the spec, the architecture, the session plan and the rules file;
- the prompt that opens Session 1.

## Step 1: intake (a conversation, not a guess)

The PDD is a draft, and it will leave things open that a build cannot. Before writing anything,
list what it leaves underdetermined, including at least:
- **What v1 is.** The smallest version that is worth using, and everything in the PDD that is
  explicitly *not* v1.
- **Platform and stack.** Language, framework, runtime, any version that must be pinned, and
  where the test runner's runtime differs from the shipped one.
- **External systems.** Every app, service or OS facility it talks to or runs inside: what
  crosses each boundary, which versions are supported, and how each one fails.
- **Where data lives.** Storage, the shape of the data model, import and export, backups.
- **Boundaries and invariants.** What must never happen (data loss, a layer reaching past
  another, secrets in logs…). These become the rules every session is held to.
- **How it is verified.** Unit tests, UI or end-to-end harnesses, runtime probes, and what
  each cannot see. There is no CI: `verify` and the harnesses are the gate, run by the session.
  Which of them, and what else, only one chat can use at a time (a live external app, a
  single-instance desktop app, a fixed port, one real device): these are the project's
  exclusive resources, which parallel parts never run.
- **Distribution.** How it is built, signed, installed and updated, or explicitly not.
- **Versions and the remote.** Each session is a release by default; say if this project should
  release at milestones instead. Which versions are milestones that get a tag. Whether there is
  a git remote, which decides pushes, tags and the changelog's links.
- **Names.** The product's name, and the machine identifiers (package name, storage keys,
  bundle id) that must never change once data exists under them.
- **Privacy** (the workflow's *A clean repository*). Ask what is private beyond what is
  already public, and which git identity the commits use. Ask about other people, my
  unannounced projects or games, agent personas, and what I publish on purpose (a copyright
  holder, a bundle id). The answer may be nothing. Check `~/.config/hygiene/terms.txt` exists
  (empty is fine); creating it if it does not, and adding any new term I name, go into the
  plan.

Ask me in small batches. For every question, give your recommended answer and one line on why,
so I can accept it quickly. Where the PDD contradicts itself, ask which part wins. Do not
move to step 2 until every open item is answered or explicitly deferred.

## Step 2: write the round-1 documents

This chat runs in plan mode like every session: once the intake is done, propose the documents
below, and the term-list changes from step 1, as your plan, and write them when I approve it.
Use the templates at the end of `DEVELOPMENT-WORKFLOW.md`, and the two at the end of this file,
for every file that has one.

1. **`docs/spec/PDD.md`**: the draft as I gave it, frozen, with two edits only: private details
   are replaced by roles ("the owner", "the first game"), and lines crediting or addressing an
   AI tool are removed. The original stays outside the repository, and a resolved decision
   says the draft was edited this way.
2. **`docs/spec/SPEC.md`**, the contract for v1, from the template below. Three of its sections
   carry the whole round:
   - **Resolved decisions**, dated and numbered, one per intake answer about what v1 delivers,
     never re-litigated. The last is **Out of v1**, and each item in it also goes to
     `docs/BACKLOG.md`.
   - **PDD amendments.** The draft stays frozen, so this table is how a session knows which of
     the two wins.
   - **Traceability.** Every requirement in the draft, mapped to the session(s) that build it
     or to the backlog. A requirement with no session is a gap in the plan, and this table is
     what lets each session read only its own parts of the draft.
3. **`docs/ARCHITECTURE.md`**, how the system is built, from the outline in the workflow's
   templates. It outlasts the round, so every intake answer about how the system is built goes
   into its Decided parts and not into the spec. Its Built parts say "Not yet" and name the
   session that builds each; the baseline is left for Session 1.
4. **`docs/spec/SESSIONS.md`**, the plan for round 1, from the template: the header, the table
   of sessions with their versions, and one block per session with **Read**, **Scope**,
   **Docs**, **Gates** and **Watch for**.
   - **Session ids are numbers.** This chat is Session 0 and has no block; the plan starts at 1.
   - **Session 1 is always the scaffold** (`0.1.0`): everything in the workflow's *Day one*,
     with *The hygiene stage's design*, below, copied into its block. Its Read line also names
     the workflow's *Day one* and *A clean repository*, which the design relies on.
   - **Plan the walking skeleton early, and fill in every block's Confirms line** (the
     workflow's *Rounds*): each Decided part of `ARCHITECTURE.md` is listed in the block that
     first builds on it.
   - **Where the workflow's *Rounds* places architecture reviews,** name the milestones, and
     what each review measures against the sessions after it.
   - **The last session of the round is the documentation pass,** and where each session is a
     release, it releases the round's last version. Its Read line is everything, like an
     architecture review's:
     - `docs/ARCHITECTURE.md` read cold against the code: every Built part true, what the
       round did not build pointing at its backlog item, and the baseline current;
     - `docs/USER-GUIDE.md`;
     - the README;
     - `CLAUDE.md` pointed at the user guide too;
     - anything still open moved to `docs/BACKLOG.md`;
     - then, after its review, the separate `docs: close round 1` commit.
5. **`CLAUDE.md`**, from the template below, filled in from the spec and the architecture.
   Keep it lean: rules that are always true, not history. Seed its *Watch for* section with the
   standing traps already known: `< /dev/null` for any shell command that reads stdin, and what
   the intake learned about this machine, such as where the toolchain lives.
6. **`docs/DEVELOPMENT-WORKFLOW.md`**: a verbatim copy of the file I gave you. If the intake
   decided this project deviates from it (no user guide, releases at milestones…), add the *In
   this repository* paragraph under its introduction and change nothing else.
7. **`docs/BACKLOG.md`**, from the template: everything in the PDD that is not v1, under
   Deferred, Directions and Decided against. Keep the *Not yet checked by hand* heading; it
   starts empty.
8. **`docs/next-session-prompt.md`**: the prompt for Session 1, from the mid-round template.
   It has no previous session to report, so its state is what Session 0 decided that the spec
   and the architecture do not say.

## Step 3: close Session 0

- `git init` if needed, with a `.gitignore` suited to the stack that also keeps out what the
  OS, editors and merges leave behind (`.DS_Store`, `Thumbs.db`, `*.swp`, `*~`, `*.orig`,
  `*.rej`) and local secrets (`.env` and `.env.*`, with `!.env.example`).
- Before the first commit, confirm `~/.claude/settings.json` has `"includeCoAuthoredBy": false`
  and `"attribution": { "commit": "", "pr": "" }`, so no commit or PR carries a Claude co-author
  line. If either is missing, tell me before committing. The settings keep attribution out at
  the source; the hygiene stage catches it if they change.
- Before the first commit, check every file you wrote, and the commit message, against the
  term list. An empty list passes with nothing to check, so test for it first: some greps,
  ugrep among them, match every line against an empty pattern file.
  `[ ! -s ~/.config/hygiene/terms.txt ] || grep -n -i -w -f ~/.config/hygiene/terms.txt <files>`
  The hygiene stage does not exist yet. Its first run in Session 1 is a full scan, since it has
  no stamp yet, so it checks this commit too.
- Commit locally with explicit paths: `docs: open round 1 — <product>`. Do not add a remote and
  do not push unless I ask.
- I run `/code-review high` on the commit. It checks the spec, the architecture and the plan
  for contradictions, gaps and sessions too big for one chat, every Read line against what its
  session needs, the traceability table for a requirement with no session, and the
  architecture for a subsystem no session builds. Fix what it finds; a finding that
  changes a decision is mine to call. Commit the fixes as
  `review: <the findings, in plain words>, and <n> smaller`.
- Print the one-line opener as your last message, ready to paste, and stop:
  `Read docs/next-session-prompt.md and do the session it names: Session 1, the scaffold, release 0.1.0.`
  Never print the handoff itself; Session 1 reads it from the repository. Session 1 runs in a
  new chat, which I open in plan mode.

---

## The hygiene stage's design

Session 1 builds the stage in the project's own language. Copy this design into Session 1's
block as the stage's scope, and its last item into the block's gates; each part closes a gap
someone found. What the stage checks and why is the workflow's *A clean repository*; this is
how it is built.

- **The terms come from `~/.config/hygiene/terms.txt`,** never from the repository, not even
  as hashes. A missing file fails the stage; an empty one passes. Tests point the stage at a
  fixture file of fictional terms, through a path argument or an environment variable, and
  never read the owner's file.
- **Each line is split into tokens:** runs of letters and digits in any script (an ASCII-only
  `[a-z0-9]+` misses a name written in Cyrillic), plus the camel-case words and digit groups
  inside each run, read before lowercasing, so a term in `myTermNotes` or `term2026` is still
  found. Terms and tokens alike are normalised to NFC and lowercased before they are compared,
  so a letter such as й matches however it was encoded.
- **A routine run reads what a commit could still change:** the path and contents of each path
  that differs from `HEAD` in the index or the working tree, both versions, since they can
  differ; untracked files that are not ignored, so a new file is checked before it is staged;
  the drafted commit message (`--message <file>`); the commits on no remote
  (`git log HEAD --not --remotes`, which is every commit when there is no remote): their
  messages, and the paths they changed as they stand at `HEAD`. Files containing NUL are
  skipped as binary, a path no longer in the tree is skipped, and submodule entries are
  skipped. A repository with no commits yet passes.
- **A full scan reads everything:** every tracked file and every untracked one that is not
  ignored, the staged contents of every path, every message reachable from `HEAD`, and the
  draft. It runs when `--all` is given, or when the stamp
  `"$(git rev-parse --git-common-dir)/hygiene-scanned"` is missing or older than the term list
  or the stage's own source. A new checkout writes the source anew, so the first run in a
  part's checkout may scan in full; the shared stamp then serves the rest. The stamp holds
  nothing: its modification time is the record. A hit in a file, in the draft or in a commit on
  no remote fails. A hit in a pushed commit does not fail, and the stage's output ends with a
  line of its own counting them. The stamp is renewed after a full scan that does not fail.
  Tests build scratch repositories and never read or touch the real stamp.
- **Attribution is checked in messages,** in the forms the workflow lists. The patterns name
  the tools, so "generated with the codegen script" passes.
- **Secrets have exact shapes,** each bounded by characters that cannot belong to it:
  - a private-key header: `-----BEGIN `, any words, then `PRIVATE KEY-----`;
  - an AWS access key id: `AKIA` or `ASIA`, then 16 capital letters or digits;
  - a GitHub token: `ghp_`, `gho_`, `ghu_`, `ghs_` or `ghr_`, then 36 letters or digits; or
    `github_pat_`, 22 letters or digits, `_`, then 59 more;
  - a Slack token: `xoxb-`, `xoxp-`, `xoxa-`, `xoxr-` or `xoxs-`, a group of digits, `-`, then
    more;
  - an Anthropic key: `sk-ant-`, a kind and two digits, `-`, then 80 or more letters, digits,
    `_` or `-`;
  - an OpenAI key: `sk-proj-`, `sk-svcacct-` or `sk-admin-`, then 40 or more letters, digits,
    `_` or `-`; or `sk-` and exactly 48 letters or digits;
  - a file named `.env` or `.env.<anything>` that a commit could record (tracked, staged, or
    untracked and not ignored), except `.env.example`, `.env.sample` and `.env.template`.

  A dry run over real repositories found two families of false alarms in looser shapes and none
  in these: a bare `sk-` inside hyphenated names, and a `github_pat_` prefix in test fixtures.
- **Conflict markers** are matched at the start of a line only, in the shapes the workflow
  gives.
- **It never holds what it bans.** The attribution patterns are assembled from pieces, so the
  stage's own source does not match them. Tests build their token-shaped strings and conflict
  markers from pieces at run time, so no file in the repository holds one.
- **A hit never prints the term or the secret.** It names the file and line, or the commit and
  message line.
- **What it cannot see** goes into `ARCHITECTURE.md`'s testing strategy: images and other
  binaries, their metadata included; a term joined to another word inside one lowercase run
  (matching every substring instead would flag ordinary words that contain a short term);
  attribution in files, which is checked only in messages, because a document stating this
  rule would match it; a home-directory path; and a secret of any shape not listed.
- **Each part is falsified once:**
  - a missing term file fails and an empty one passes;
  - a Cyrillic term is found, and so is the same term in NFD; `myTermNotes` and `term2026` are
    found;
  - a term or a co-author trailer in the drafted message fails; "generated with the codegen
    script" passes;
  - each secret shape and a tracked `.env` fail; `.env.example`, a hyphenated name containing
    `sk-` and a `github_pat_TEST` fixture pass;
  - a conflict marker fails; a heading underline passes;
  - a change left unstaged is read; a routine run reads a commit on no remote and skips a
    pushed one; a file committed without the stage, in a commit on no remote, is read;
  - a missing stamp, or a term list touched after it, turns a routine run into a full scan,
    and that scan still reads the draft; in a full scan, a hit in a pushed message passes and
    is counted on the last line, and a hit in a file fails and leaves the stamp as it was.

## `docs/spec/SPEC.md` template

Fill every `<…>`. Keep every section, writing "None" where one has nothing: the sessions' Read
lines point at sections and decisions by number.

````markdown
# <Product> — the round 1 contract

Read-only from the first line of Session 1. Session 0 derived it on <YYYY-MM-DD> from
`docs/spec/PDD.md` (<the draft's version>, frozen). The PDD states intent and this file is the
contract for what v1 delivers: where they differ, this file wins, and §3 lists every place they
do. How the system is built is in `docs/ARCHITECTURE.md`, which Session 0 wrote beside it and
which outlasts the round.

## 1. Resolved decisions (<YYYY-MM-DD>, at intake)

Decisions <n–m> were answered by the user at intake. Decisions <n–m> were proposed by the
intake from the PDD and approved with the Session 0 plan. The last collects what round 1 leaves
out. None is re-litigated by an implementing session; a change is added beside the decision as
an **Amended <YYYY-MM-DD> by the user** note. An answer about how the system is built is not
here: it is in `ARCHITECTURE.md`, in the section it belongs to.

<One decision per intake answer about what v1 delivers, in the order the intake reached them.
Among them, every project has the ones below.>

1. **<The decision, in one sentence>.** <The detail a session needs, and the why where the why
   changes the action. Name the PDD place it settles.>
2. **<n> sessions** (`SESSIONS.md`): <n> build sessions releasing 0.1.0 to <x.y.z>, and
   architecture reviews after the milestones <names>.
3. **Versions.** <Each session a release, or milestones only>; shown in <where>; <the versions
   that are milestones and get a tag>.
4. **The draft was edited on the way in.** Private details are replaced by roles (<"the owner",
   …>) and lines crediting or addressing an AI tool are removed. The original is not in the
   repository.
5. **Nothing private enters the repository.** <What the user publishes on purpose: the copyright
   holder, the bundle id.> Commits use <the git identity>. The hygiene stage of `verify`
   checks what `ARCHITECTURE.md`'s testing strategy lists, and the last read of each diff
   covers the rest.
6. **Licence:** <…>.
7. **The remote:** <none yet, or where>. It decides pushes, tags and the changelog's links.
<n>. **Out of v1** (each in `docs/BACKLOG.md`): <…>.

## 2. What v1 is, and is not

**v1 is** <the core loop a person runs, end to end, and the platform it is proven on>.

**v1 is not:** <the non-goals, a clause each>.

## 3. PDD amendments

| PDD place | PDD says | Now | Where decided |
|---|---|---|---|
| <Part, section> | <…> | <…> | <decision <n>, or the `ARCHITECTURE.md` section> |

## 4. Traceability

| Requirement | Session |
|---|---|
| <the draft's requirements, by id or name> | <session id(s), or Backlog> |

## 5. Definition of done for v1

1. Every block in `SESSIONS.md` has its done-note with its Review section; every inserted
   session and amendment is logged with its reason.
2. <The product's own acceptance: what is built, and what passes>.
3. `verify` is green and every harness passes, with the counts in `ARCHITECTURE.md`'s baseline.
4. Every invariant's and every boundary's enforcement exists and is described in
   `ARCHITECTURE.md`.
5. `docs/ARCHITECTURE.md`'s Built parts describe the system as built; `docs/USER-GUIDE.md` and
   a truthful README exist; `CLAUDE.md` points at them.
6. Everything still open is in `docs/BACKLOG.md`, and `docs: close round 1` has emptied
   `docs/spec/`.
````

## `CLAUDE.md` template

Fill every `<…>`, and delete a section only when it truly does not apply.

```markdown
# <Product name>

<One paragraph: what it is, for whom, and the platform. Name any machine identifiers that
must keep their spelling even if the product is renamed (package name, storage keys, bundle
id), because data lives under them.>

**Read `docs/ARCHITECTURE.md`'s overview, invariants and baseline, and the sections your
session's block names, before changing code.** It describes how the system is built: the stack
and its pins, the layers and their boundaries, the data layer, the invariants, the testing
strategy, and the deliberate decisions that look wrong until you know why. It keeps what is
**Decided**, which changes only with the owner, apart from what is **Built**, which every
session keeps true (*The documents* in `docs/DEVELOPMENT-WORKFLOW.md`). **The round's spec
files in `docs/spec/` are the contract for what the round delivers**: read the parts your block
names. `docs/DEVELOPMENT-WORKFLOW.md` is the session-based workflow this repo follows.
`docs/BACKLOG.md` lists what has been named but not planned.

## Working protocol

Every session runs *The session cycle* in `docs/DEVELOPMENT-WORKFLOW.md`. One session per
chat. A push or tag needs explicit approval, each time.

**Scope lives in `docs/spec/SESSIONS.md`** (one block per session, in order) and the round's
spec files (`SPEC.md` in round 1; after it, one `FEATURE-*.md` per request, holding the request
verbatim plus the decisions resolved at intake — read-only, not re-litigated). `docs/spec/`
holds only the round in progress: when a round closes its files are deleted (git history keeps
them) and anything still open moves to `docs/BACKLOG.md`. **State lives in
`docs/next-session-prompt.md`**, rewritten by every session as its last act.

1. `<verify command>` (typecheck + lint + format check + tests + hygiene) must be green before
   you commit. A commit that changes only docs runs `<the docs-only command>` instead. Run the
   heavier harnesses (`<harness commands>`) when your change could affect what they
   cover; expected results are in ARCHITECTURE.md's baseline table.
2. Falsify any new check before trusting it: break the assertion, watch it fail at its own
   step, restore it.
3. User-visible changes get a line in `CHANGELOG.md`. Version is `major.minor.fix` in
   `<manifest>` (surfaced in <where the product shows it>).
4. If a decision is genuinely underdetermined, stop and ask rather than guessing — a guess
   becomes load-bearing for everything after it.
5. Comments state the present: why the code is shaped this way now. No session ids, no
   "until v0.x", no review stories — git history keeps those.

## Invariants — these are rules, not preferences

<Numbered, copied from ARCHITECTURE.md's invariants, and changed with them. Say how each is
enforced (a lint rule, a type, a test) where it is.>

## <A pinned dependency or stack trap, as a heading that states the rule>

<Optional. One section for each pin or trap that code written from memory would get wrong:
the version range and why, what changed in it, and where the call sites are.>

## Nothing private goes in

The history may be published one day, so every file, test, fixture, done-note, handoff and
commit message follows these rules:
- **No personal names, no names of <the owner's other projects or games>, no agent personas.**
  Write the role: <"the owner", "the first game", …>.
- **No home-directory paths.** Write `~`.
- **No AI attribution:** no co-author trailers, no lines saying a tool generated the work, no
  AI session links, no AI credited as author, co-author or addressee. Naming <the tools the
  product supports or the workflow uses> is fine.
- **Fixtures are fictional.**
- **Done-notes and handoffs never name the owner's own data** (<their documents, folders…>).
- **Unsure whether something is public? Ask the owner before writing it.** A word they call
  private goes on the term list.

<What the owner publishes on purpose, e.g. the copyright holder and the bundle id.> The
hygiene stage of `verify` (`<hygiene command>`) checks the term list in
`~/.config/hygiene/terms.txt`, attribution in messages, secrets and conflict markers; the last
read of each diff covers the rest of this list.

## Commands

| Command | Does |
|---|---|
| <filled in by Session 1> | |
| `<the docs-only command>` | The stages that read docs, for a commit that changes only docs: hygiene, plus <the format check and lint, where they cover them> |
| `<verify command> hygiene --all` | Hygiene's full scan, on demand; it also runs by itself when the term list or the stage changes |

<Below the table, what a session would otherwise learn the hard way about running them: what
the unit tests cannot see and which harness can; that no harness touches a person's real data;
where temp files go and what collects them.>

**Exclusive resources:** <what only one chat can use at a time: a harness that drives a live
app, a single-instance desktop app, a fixed port, a device; or "none">. A parallel part never
runs them; the session's integration does.

## Git

- Commits use <the git identity>.
- Commit locally; a push or tag needs explicit approval, each time. Before any push, list
  `git log origin/main..HEAD`; parallel sessions may share this checkout.
- Stage explicit paths, never `git add -A`. Never undo with `git checkout <path>`.
- As the last act before each commit, after the done-note, the handoff and a last read of the
  diff for what no script judges (the workflow's *A clean repository*): draft the message into
  `"$(git rev-parse --git-dir)/COMMIT_DRAFT"`, run `<verify command> --message` with it
  (`<the docs-only command>` for a commit that changes only docs), commit with `git commit -F`
  from it, then delete it. The message is checked before it exists in history.

## Watch for

The standing traps and forbidden moves, a line or two each: the trap, then what to do instead.
One goes in when the intake knows it or once it has cost a session time, and comes out once a
check, a script or a pin makes it impossible.

- <Forbidden moves, e.g. editing an applied migration, upgrading a pinned dependency in
  passing, touching a person's real data from a test, renaming a machine identifier.>
- <The environment's traps, e.g. a shell command that reads stdin hangs without `< /dev/null`;
  where the toolchain lives.>
- Do not finish with `<verify command>` red, or with user-visible changes missing from
  `CHANGELOG.md`.
```

**At round 1's close,** the documentation pass adds a sentence to the paragraph that opens with
`docs/ARCHITECTURE.md`, before the one about the workflow:

```markdown
`docs/USER-GUIDE.md` describes behaviour from the user's side.
```
