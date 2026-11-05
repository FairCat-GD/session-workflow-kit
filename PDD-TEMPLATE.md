# Product Design Document: [APP NAME]

> **Legend:** `_[bracketed italic text]_` is a prompt to replace; plain italic paragraphs are guidance you can keep or delete. Fill sections to the depth your product needs and delete what doesn't apply. A weekend utility needs Parts 0, 1, 4, 5, 12 and 13; a regulated multi-platform product uses everything.
>
> **Decisions are marked.** Anything a build would rely on is marked **Decided** (the owner settled it) or **Proposed** (a recommendation waiting for the owner). Unmarked text is intent, not a decision. What nobody has settled yet goes to Appendix C. A build guesses wherever this document is silent, and an unmarked guess becomes load-bearing.
>
> **One home per fact.** State each rule once, in the part it belongs to, under an ID (F-, N-, I-, T-). Everywhere else, refer to the ID. A rule copied into three parts drifts into three different rules.
>
> **When the build starts,** this draft is frozen as it stands: private names are replaced by roles, and nothing in it is edited again. Changes after that live in the build's own documents: what the build delivers in its spec, how it is built in its architecture document. So keep build state (what is done, what is in progress) out of this document; it goes stale the day it is written.
>
> **Intake map.** The build's first session (Session 0 in the session workflow kit) asks about everything below. Where this draft already answers, mark it Decided and the intake confirms instead of asking.
>
> | Intake asks | Answered in |
> |---|---|
> | What v1 is, and is not | Part 1 non-goals, Part 13 MVP and scope cuts |
> | Platform, stack, pins | Part 1 platforms, Part 8 stack |
> | External systems and how each fails | Part 8 external systems |
> | Where data lives; names that never change | Part 6 |
> | Boundaries and invariants | Part 8 stack & architecture (boundaries), Part 5 invariants |
> | How it is verified | Part 5 check column, Part 12 quality strategy |
> | Distribution | Part 8 build, release & operations |
> | Versions and the remote | Part 13 releases |
> | Privacy | Part 0 private names |

---

## Part 0: Document Control

| Field | Value |
|---|---|
| **Product name** | _[working name]_ |
| **Document owner** | _[name / role]_ |
| **Version** | _[e.g. 0.3]_ |
| **Status** | Discovery / Design / Build / Beta / Live |
| **Current focus** | _[the part(s) being actively worked on]_ |
| **Last updated** | _[date]_ |
| **Private names** | _[people, unannounced projects, clients and anything else that must not reach a repository, each with the role that replaces it: "the owner", "the first game". This field is removed when the draft enters a repository.]_ |

**Changelog**

| Version | Date | Author | Change |
|---|---|---|---|
| 0.1 | _[date]_ | _[name]_ | Initial draft |

---

## Part 1: Vision (The One-Pager)

*If someone reads only this page, they should understand what the product is, who it's for, and why it should exist. Write it first and let everything else expand from here.*

**Elevator pitch**

_[one or two sentences. "It's a ___ for ___ that ___."]_

**Problem statement**

_[the specific pain, who has it, how they cope today, and what that costs them. If you can't describe the current workaround, you haven't found the problem yet.]_

**Product pillars**

_[3-4 non-negotiable principles every decision is measured against, ranked. When two conflict, the higher one wins; saying so once settles every later argument between them.]_
1. _[e.g. "Capture in under three seconds"]_
2.
3.

**Non-goals (what this product is not)**

_[2-3 things this product deliberately will not do, even though the category expects them: e.g. "Not a team tool", "No social features", "No web version". These kill scope creep before it starts.]_

**Target user**

_[who this is for, in one sentence, and the tools they use today. Be narrow; a product for everyone is a product for no one.]_

**Value proposition & the hook**

_[what the user gets that they can't get elsewhere, and the single moment that makes them say "oh, I need this". If you can't demo that moment in 15 seconds, the value is not yet clear.]_

**Category**

_[e.g. personal knowledge tool, habit tracker, developer utility]_

**Platforms**

_[iOS / Android / macOS / Windows / web / CLI / a chat platform / a server, and why. Which comes first.]_

**Usage shape**

_[how often, for how long, and where in the user's day: a 10-second capture twenty times a day is a different product from a 40-minute weekly review. Give expected volumes where you know them.]_

---

## Part 2: Market & Competitive Positioning

*An honest look at the landscape. This keeps the vision grounded and forces you to articulate the gap you're actually filling. Delete this part for private or internal tools.*

**Market context**

_[the category and its state: growing/saturated, key trends, platform economics, pricing norms.]_

**Direct competitors & substitutes**

_[the closest 3-5 products, plus the non-product substitutes (spreadsheets, paper, "just remembering"). For each: what they do well, where they fall short, what you learn from them.]_

| Competitor / substitute | Strengths | Weaknesses | Takeaway for us |
|---|---|---|---|
| _[name]_ | | | |

**Positioning statement**

_["For [user] who [need], [product] is a [category] that [key benefit], unlike [main competitor], because [differentiator]."]_

**The gap we fill**

_[the specific unmet need or underserved user this product addresses.]_

---

## Part 3: Users & Jobs

*Design backwards from the user's situation. Define what they're trying to get done, then build the product that does it.*

**Personas or roles**

_[1-3 concrete user types: context, goals, frustrations, skill level, devices. Name them. Rank them; the primary persona wins every conflict. For a private or internal tool, a table of roles (role, who fills it, what they may do) usually serves better.]_

**Jobs to be done**

_[the jobs the user hires this product for, in the form "When [situation], I want to [motivation], so I can [outcome]." Include the emotional job, not just the functional one.]_

**User journey**

_[the arc from discovery to habit: find → try → first value → daily use → recommend (or churn). Name where each stage is won or lost.]_

**First-run experience**

_[the first five minutes, in order: what the user sees, what they must do before they get value, what they are never asked. Time to first value is the number to minimize. Most users who leave, leave here.]_

**Accessibility intent**

_[target standard (e.g. WCAG 2.2 AA), screen reader support, dynamic type, contrast, motor and cognitive considerations. Decide the ambition early.]_

---

## Part 4: Core Experience

*The heart of the document. This is what the user actually does, and what the product does in response.*

**Core loop**

_[the repeating cycle the product is built on: e.g. capture → organize → retrieve → act. Name the loop lengths (per use, per day, per week).]_

**Primary user flows**

_[the 3-5 flows that deliver the value. For each: trigger, steps, decision points, outcome, and what "done" looks like. Diagram if it helps.]_
- Flow: _[name]_: _[trigger → steps → outcome]_

**Worked examples**

_[2-4 concrete cases: the exact input, and the exact output or behaviour you expect. At least one is the nastiest edge case you can think of. Invented data only. One example settles more than a paragraph of intent, because it can't be read two ways.]_

| Case | Input | Expected |
|---|---|---|
| _[the common case]_ | | |
| _[the nasty edge case]_ | | |

**Feature set**

_[each core feature: what it is, how it behaves, why it earns its place, and how it interacts with the others. Rules go in Part 5 under an ID; refer to it here.]_
- Feature: _[name]_: _[behaviour, edge cases, requirement IDs]_

**Information architecture**

_[the structure of the product: top-level sections, navigation model, how content is organized and found. For a product with no screens (a bot, a CLI, a service), how each kind of input is represented inside it.]_

**States & edge cases**

_[empty states, loading, errors, offline, permissions denied, first use vs. hundredth use, very large data sets. The unhappy paths are most of the product.]_

| State | Behaviour |
|---|---|
| _[e.g. network unavailable]_ | _[what the user sees, what the product does]_ |

**Input & interaction model**

_[primary input per platform (touch, keyboard-first, voice, drag, commands), shortcuts, gestures. Include a mapping table for anything non-obvious.]_

---

## Part 5: Requirements

*Everything in this part should be testable. If you can't write a check that passes or fails, it's not a requirement yet.*

**Functional requirements**

_[user stories or requirement statements with priority. The Check column is one line a test or a person can pass or fail: "a DM gets no reply and no API call", not "handles DMs correctly". Leave it empty only for a Could.]_

| ID | Requirement | Priority (Must / Should / Could / Won't) | Check | Notes |
|---|---|---|---|---|
| F-01 | _[As a ___, I can ___ so that ___]_ | Must | _[the observable pass/fail]_ | |

**Non-functional requirements**

_[performance budgets (launch time, response time, memory), reliability targets, data durability, security posture, scalability limits, battery and network behaviour, cost. Give numbers.]_

| ID | Requirement | Target | How verified |
|---|---|---|---|
| N-01 | _[e.g. cold launch to interactive]_ | _[< 1.5 s on baseline device]_ | _[automated perf test]_ |

**Invariants**

_[what must never happen, whatever else changes: data loss, a secret in a log, a layer reaching past another, data leaving where it is allowed to go. These become the rules every build session is held to, so keep the list short; if it runs past ten, most of them are requirements. Say how each is enforced.]_

| ID | Never | Enforced by |
|---|---|---|
| I-01 | _[e.g. message text appears in a log]_ | _[canary test on every commit]_ |

**Tunables**

_[every number a week of use or a playtest might change: timings, caps, thresholds, prices, model ids. Each lives in config (or an inspector field), never as a constant in code.]_

| ID | Name | Default | Range | Set in |
|---|---|---|---|---|
| T-01 | _[e.g. debounce]_ | _[4 s]_ | _[1-10 s]_ | _[config file]_ |

**Constraints & assumptions**

_[what you're taking as given: platform rules, budget, team, deadlines, legal, dependencies you don't control. A verified item names its source and date; an assumption names the milestone that checks it.]_

| Item | Status | Verify by |
|---|---|---|
| _[e.g. the API keeps undelivered updates for 24 h]_ | Verified _[source, date]_ / Assumed | _[milestone, or "done"]_ |

---

## Part 6: Data & Content Model

**Domain model**

_[the core entities, their fields at a scope level, and the relationships between them. A diagram beats prose here. The full schema lives with the code.]_

**Names that never change**

_[machine identifiers that data will live under: package name, bundle id, storage keys, database file names. Renaming one after data exists under it loses or orphans that data, so decide them before the first build.]_

**Data lifecycle**

_[where data is created, stored, synced, backed up, exported, and deleted. Who owns it, and who can read it. How long each copy is kept, including copies held by third parties. What happens on device loss, account deletion, or app uninstall.]_

**Import / export & portability**

_[what formats you read and write, and how a user leaves with their data intact. Lock-in is a business decision; make it explicitly or don't make it.]_

**Content & defaults**

_[bundled content, templates, sample data, onboarding content. Target magnitudes only; the itemized inventory lives in a linked tracker.]_

---

## Part 7: Experience Design

*Fill the variant that fits and delete the other. A product with screens fills **UX & visual**. A product people talk to or run (a bot, an agent, a CLI, an API) fills **Voice & behaviour** instead.*

### UX & visual

**Design principles**

_[the 3-5 rules that resolve UI arguments: e.g. "one primary action per screen", "never block on network", "defaults over settings".]_

**Visual direction**

_[aesthetic, references/mood board, colour language, typography, iconography, density. Keep the visual direction (what we build) separate from mood references (what it should feel like).]_

**Key screens & flows**

_[the design of the screens that matter most, and the transitions between them. The exhaustive screen inventory (every screen, every state) is production tracking; keep it in a linked design file.]_

**Design system & components**

_[platform-native vs. custom, component library, theming (light/dark), platform conventions you follow and the ones you deliberately break.]_

**Motion & feedback**

_[how actions feel confirmed: transitions, haptics, sound, progress. Name the feedback for each core action.]_

**Style guide reference**

_[link to the design system / brand guide if one exists.]_

### Voice & behaviour

**Character or voice**

_[who it is or how it sounds: register, sentence length, humour, what it never says, how it behaves when it doesn't know. For an agent with a persona, where the persona is written down and who may change it.]_

**Response rules**

_[length, format, language, when it stays silent, when it drops its voice and speaks plainly.]_

**Precedence**

_[when instructions conflict (hard rules, the user's wishes, the persona, learned behaviour), which wins, top to bottom.]_

### Both variants

**Voice, tone & microcopy**

_[how the product talks: labels, empty states, errors, notifications, replies. Give the voice in a good moment and a bad one; a pair of examples defines the voice better than adjectives.]_

| Situation | Good | Bad |
|---|---|---|
| _[e.g. an error]_ | | |

---

## Part 8: Technical

*Implementation detail you already know (a schema, a module map, a protocol) is welcome here, marked Decided or Proposed. When the build starts, what the intake confirms of it becomes the decided part of the architecture document, and this draft stays as written.*

**Trade-off order**

_[when engineering goals conflict, which wins: e.g. "privacy > correctness > simplicity > latency > cost". One line saves the build from guessing the same trade-off ten times.]_

**Stack & architecture**

_[languages, frameworks, key libraries, architectural style, notable technical bets. Pin every version you rely on and say when you checked it: libraries move faster than any document. Say where the tests run on a different runtime from the one that ships. Justify each bet by naming the alternative you rejected.]_

| Layer | Choice and pinned version | Rejected alternative, and why |
|---|---|---|
| _[e.g. storage]_ | _[SQLite 3.45+]_ | _[a JSON file: rewritten whole, no search]_ |

**Platform targets & performance**

_[minimum OS versions, baseline devices, performance and size budgets per platform.]_

**Storage & sync**

_[local persistence, cloud sync strategy, conflict resolution, offline behaviour, migration approach for schema changes.]_

**External systems**

_[every service, platform API, OS facility or app this talks to or runs inside. A test can't see these, so each one's failure must be designed, not discovered.]_

| System | What crosses the boundary | Version / API | How it fails | What we do then |
|---|---|---|---|---|
| _[name]_ | _[data in, data out]_ | | _[outage, rate limit, auth expiry]_ | _[fallback]_ |

**Security & privacy**

_[authentication, authorization, encryption at rest and in transit, secrets handling, data minimization. What you collect, why, and for how long. Then the threats.]_

| Threat | Mitigation | Residual risk |
|---|---|---|
| _[e.g. a dependency sends data elsewhere]_ | | _[what remains, and whether it is accepted]_ |

**Build, release & operations**

_[how it is built, signed, installed and updated, or explicitly not. Environments, monitoring, rollback, and the kill switch: how to stop it fastest if something goes wrong.]_

**Technical risks**

| Risk | Fallback |
|---|---|
| _[the hard problem]_ | _[the plan if it bites]_ |

---

## Part 9: Analytics & Telemetry

**Success metrics**

_[the few numbers that tell you the product works: activation, retention (D1/D7/D30), core-action frequency, task completion. Define each precisely.]_

**Event model**

_[the events you record, their properties, and the question each one answers. If an event doesn't map to a decision, don't collect it.]_

**Privacy constraints**

_[opt-in/opt-out, anonymization, what is never collected, retention period, and how this shows in the consent flow.]_

**Dashboards & review cadence**

_[where the numbers are seen and how often someone looks.]_

---

## Part 10: Localization & Compliance

*Delete this part only if the product is single-region, single-language, and outside any store or regulatory review. Treat these as design inputs, not a pre-launch checklist.*

**Localization scope**

_[target languages at launch and post-launch, and why.]_

**Internationalization readiness**

_[externalized strings, Unicode and font coverage, text-expansion headroom, right-to-left support, locale-aware dates/numbers/currency, pluralization rules.]_

**Legal & data compliance**

_[privacy law obligations (GDPR, CCPA, COPPA), consent flows, terms and privacy policy, data processing agreements, accessibility law where applicable.]_

**Store & platform review**

_[App Store / Google Play / Microsoft Store / extension marketplace requirements: review guidelines, required disclosures, in-app purchase rules, sign-in rules, permission justifications. Name a review owner.]_

**Licensing**

_[licenses of every dependency and asset, and whether they are compatible with your distribution model.]_

---

## Part 11: Business & Operations

*Optional. Delete this part for private or internal tools.*

**Business model**

_[free, freemium, one-time purchase, subscription, ads, open source with services. Justify the fit with the product and the user.]_

**Pricing & packaging**

_[what's paid, what's free, tiers, trial model, and the ethical guardrails (no dark patterns, clear cancellation).]_

**Retention & engagement**

_[what brings users back: the habit hook, notifications policy, progress and history, integrations into existing workflows.]_

**Support & feedback**

_[how users reach you, response expectations, how feedback gets into the backlog.]_

**Key metrics (KPIs)**

_[what business success looks like in numbers: conversion, churn, revenue, support load, rating. Product usage metrics (activation, retention) live in Part 9. Marketing plan and go-to-market live in a separate linked document.]_

**Roadmap**

_[post-launch direction by theme, not by feature list. What the next two releases are about.]_

---

## Part 12: Validation & Quality Plan

*A design document makes claims about what users need and will do. This part turns those claims into things you can test, so "we think they want it" becomes "we checked." Treat each core bet as a hypothesis to prove or kill.*

**Key hypotheses**

_[the riskiest assumptions the product rests on, stated so they can be proven false.]_

| Hypothesis | How we'll test it | Success signal | Status |
|---|---|---|---|
| _[e.g. "Users will capture daily without reminders"]_ | _[2-week diary study, 8 users]_ | _[median 5+ captures/day by week 2]_ | Open / Validated / Killed |

**Discovery & research plan**

_[interviews, surveys, competitor teardown, diary studies: what each exists to learn, in priority order. Tackle the scariest unknown first.]_

**Prototype & usability testing**

_[fidelity ladder (sketch → clickable → coded), who tests, how often, and what each round must answer: is it understood? is it wanted? where do users stall?]_

**Quality strategy**

_[what is tested and how: unit, integration, UI, performance, accessibility, device matrix, evals for anything a model decides. What each layer cannot see. What runs automatically, what runs by hand, and the release gate (what must be green to ship). Name anything only one test run or build session can use at a time: a live external app, a single real device, a fixed port.]_

**Definition of "validated"**

_[the bar the core loop must clear before full build commits. Be specific; this protects you from building at scale on an unproven need.]_

---

## Part 13: Production

**Scope statement**

_[the honest size of this product and the team/time available. Rough screen and feature counts belong here; the build's session plan itemizes them.]_

**Team & roles**

_[who does what, including contracted work (design, backend, QA, localization). For a solo project: which hats you wear and which you buy.]_

**Work done outside the build**

_[anything already built, cleaned, researched or set up elsewhere that the build depends on, and where its output lives. Its outputs only, not its progress.]_

**MVP**

_[the smallest shippable thing that delivers the core value to the primary persona. Define it precisely; this is your first real milestone.]_

**Milestones**

_[each milestone ends on an exit check that passes or fails. Owner tasks are what only you can do (accounts, purchases, consent, reviews, a human-only step): a build session can't do them, so name them where they block.]_

| Milestone | Exit check | Owner tasks | Target date or effort |
|---|---|---|---|
| Prototype | Core flow clickable, tested with 5 users | | _[date]_ |
| MVP | Core loop shippable to beta users | | _[date]_ |
| Beta | Full feature set, external testers | | _[date]_ |
| Launch | Store-approved, monitoring live | | _[date]_ |

**Releases**

_[whether every build session cuts a version or only milestones do; which versions get a tag; whether there is a git remote, and where.]_

**Scope cuts / stretch goals**

_[what gets cut first if time runs short, and what gets added if there's room. Rank them now, before the pressure hits. Then name what is never cut, whatever happens.]_

**Risks & mitigations**

_[biggest risks to the project (time, people, money, market) and how you'll reduce them. Technical risks live in Part 8.]_

---

## Appendices

**A. Glossary**

_[product-specific terms and their agreed definitions.]_

**B. References & inspiration**

_[products, patterns, articles, and design references you're borrowing from, and specifically what you're taking from each.]_

**C. Open questions**

_[what nobody has settled yet. When one is resolved, its answer goes into the part it settles, marked Decided, and its reasoning into Appendix E; the row is deleted, so this table only ever shows what is still open.]_

| # | Question | Recommendation | Owner | Needed by |
|---|---|---|---|---|
| 1 | _[the question]_ | _[the proposed answer, and one line on why]_ | _[who decides]_ | _[milestone]_ |

**D. Living log (research & discovery)**

_[running notes from user research, testing, support, and unexpected insights. Date each entry.]_

| Date | Insight | Action taken |
|---|---|---|
| _[date]_ | _[what you learned]_ | _[what changed]_ |

**E. Decision log**

_[settled decisions and the reasoning behind them, so choices stay settled instead of being re-litigated. ADR-style: what was decided, why, and what alternatives were rejected. A changed decision gets a new row, and the old row says what superseded it.]_

| # | Date | Decision | Rationale | Alternatives considered |
|---|---|---|---|---|
| 1 | _[date]_ | _[what was decided]_ | _[why]_ | _[what was rejected, and why]_ |

**F. Graveyard (cut & rejected ideas)**

_[features and ideas that were planned or proposed and then cut, with the reason. Check here before re-proposing anything; an idea returns only if its reason for dying no longer holds.]_

| Date | Idea | Why it was cut | Could return if |
|---|---|---|---|
| _[date]_ | _[what it was]_ | _[the reason]_ | _[the condition, or "never"]_ |

**G. Cost model**

_[optional: for anything with metered APIs, hosting or paid services. Unit prices with the date you checked them, the usage assumptions, the cost per day and per month, and the one lever that moves the total most.]_

| Line | Per unit | Per day | Per month |
|---|---|---|---|
| _[service]_ | | | |
