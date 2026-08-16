# MYCELIUM
### If I had an open hand to build one thing for developer productivity with AI

---

## The thesis: typing was never the bottleneck

Every AI dev tool on the market today attacks the same target: the keystroke. Autocomplete, chat-to-code, agent-writes-the-PR. That target is already dead — code generation is deflationary, it gets cheaper and better every month, and it will be table stakes for everyone.

Meanwhile the actual bottleneck in every engineering org I can model goes untouched: **knowledge evaporates faster than code accumulates.**

- The *why* behind every decision lives in a Slack thread that ages out, a hallway conversation, a departed engineer's head.
- Every incident teaches a lesson that is learned once, by the people on call that night, and then forgotten by the org.
- Every task is explored *serially* — one approach tried, others never even discovered, the counterfactuals lost forever.
- Tacit workflow knowledge ("oh, you have to run the seed script twice, then clear the cache") dies with team turnover.
- Human attention — the genuinely scarce, genuinely expensive resource — is allocated by diff size and squeaky wheels, not by risk.

A codebase is a fossil record: it preserves *what* was decided and loses *why*. Software orgs are amnesiac by construction. AI's killer productivity app isn't writing code faster — it's **stopping the evaporation**, and then re-deploying the captured knowledge everywhere, forever.

So I'd build **Mycelium**: the underground network for an engineering organization. In a forest, the fungal network is invisible, connects every tree, moves nutrients from where they're abundant to where they're needed, and lets the whole forest share one immune system. Forests with mycelium outcompete forests without. Same deal here.

Six organs, one organism:

---

## 1. The Ledger of Why

`git blame` tells you *who* and *when*. Nothing tells you *why* — and "why" is the single most expensive question in software. Engineers burn hours a week reconstructing intent, and refactors break things because nobody knew which weird behavior was load-bearing.

Mycelium captures intent **at the moment it exists and costs nothing to record**: when a change merges, the agent (or the human's AI session) files the reasoning — the constraint that forced the design, the alternatives considered and *why they were rejected*, the assumption that makes this correct. Not as prose in a wiki that rots, but as structured, queryable records pinned to code regions and kept in sync as the code moves.

Then:

- Point at any line and ask "why is this here?" — get the actual provenance, not a hallucinated guess. **Chesterton's Fence as a service**: every fence comes with the reason it was built and the conditions under which it's safe to remove.
- Refactoring agents consult the Ledger before touching anything, so they know which oddities are accidents and which are armor.
- When an assumption recorded in the Ledger stops being true ("we assume payments are always USD"), Mycelium notices and flags every decision that rested on it. Decisions get **expiry conditions**, not just timestamps.

## 2. The Immune System

Today, a production incident produces a postmortem doc nobody reads and a single regression test. The lesson is learned by ~4 people and retained for ~6 months.

In Mycelium, every incident and every subtle bugfix mints an **antibody**: an executable invariant plus a generalized pattern-memory of the pathogen. Antibodies don't sit in a doc — they patrol. Every new diff, in every repo in the org, is screened against the entire accumulated pathogen history. The race condition that took down checkout in 2024 attacks its lookalike in the new inventory service in 2026, on sight, in CI.

Crucially, **immunity is transferable**: an antibody minted in one repo inoculates all the others. The org stops paying for the same lesson twice. Your incident count doesn't just go down — it compounds down, because the immune memory only grows.

## 3. The Dream Cycle

Biological memory consolidates during sleep. Codebases never sleep, so they never consolidate. Fix that.

Every night, while the humans are gone, Mycelium dreams:

- **Replay**: re-runs the day's diffs against fuzzed inputs and yesterday's production traffic shapes, hunting for the edge nobody thought about.
- **Consolidate**: distills everything the humans taught the agents that day — every correction, every "no, we do it this way here" — into updated skills, conventions, and Ledger entries. Corrections stop being one-conversation memories and become permanent org behavior.
- **Pre-bake**: prepares draft options for tomorrow — the refactor that got discussed but never scheduled, the dependency upgrade with its full blast-radius analysis attached — as ready-to-review branches. You wake up to a menu, not a backlog.

The org's knowledge base is measurably smarter every morning than it was the night before, without anyone doing "documentation work."

## 4. The Many-Worlds Engine

Humans explore solution space serially, so 90% of it never gets visited, and the roads not taken aren't even *recorded* — they're simply lost. That's an insane waste now that exploration is cheap.

Every nontrivial task in Mycelium is attempted by N agents in parallel isolated worktrees, each with a deliberately different prior: one optimizes for minimal diff, one for performance, one designs as if the requirements will double, one is paranoid about failure modes. A judge tournament scores them; a synthesizer builds the final diff from the winner plus the best organs of the losers.

The human reviews **one diff — plus a "Roads Not Taken" appendix**: here are the three other shapes this solution could have had, and here's precisely why each lost. That appendix is the innovation. It turns review from "is this code OK?" into "is this the right *world*?" — a decision humans are uniquely good at and almost never get to make explicitly. And the counterfactuals get filed in the Ledger, so two years later "why didn't we just use a queue here?" has a real answer.

## 5. The Tacit Miner

Watch what a senior engineer actually does (opt-in, on-device): the four-command dance before every deploy, the ritual for reproducing that one flaky failure, the exact sequence for a safe schema migration. This knowledge is *tribal* — it exists only as muscle memory and dies with turnover.

The Tacit Miner notices the third time anyone performs an unnamed multi-step dance and shows up with a gift: "You've done this sequence three times. Here it is as a named, tested, documented tool — want it?" One click and a piece of tribal knowledge becomes an artifact: versioned, shareable, improvable, and taught to every agent and every new hire automatically.

The org's tooling stops being something someone has to *decide* to build and starts precipitating continuously out of actual work.

## 6. The Attention Compiler

The scarcest compute in any org is senior-engineer attention, and today it's allocated by diff size, recency, and who pings loudest. Mycelium treats attention as a resource to be *compiled*:

- Every diff region gets a risk score from real signals: incident history in that subsystem, antibody near-misses, churn, Ledger density ("this area is full of load-bearing weirdness"), blast radius.
- Hot regions get the full treatment — adversarial multi-agent review, then routed to the *specific human* whose Ledger history shows they hold the relevant context.
- Cold regions get auto-merged with insurance: auto-generated characterization tests that snapshot current behavior, so even an unreviewed change can't silently drift.

Review effort becomes proportional to expected loss, not to line count. Senior engineers stop rubber-stamping renames and start spending their attention where it actually changes outcomes. It also fixes the interrupt economy: inbound questions get answered by Mycelium *with provenance* when it can, and batched into focus-respecting windows when it can't.

---

## A Tuesday with Mycelium

**9:02** — You open the morning menu. The Dream Cycle found that yesterday's pagination change breaks under a traffic shape from last Black Friday; a fix branch is waiting with the failing replay attached. You merge it before your coffee cools.

**10:30** — You pick up a task: add rate limiting to the export API. The Many-Worlds Engine already ran four approaches overnight. You read the Roads Not Taken appendix, disagree with the judge — the "requirements will double" world is right, because you know Enterprise is coming — override, and the synthesis rebuilds around it. Your override is filed in the Ledger with your reasoning.

**11:45** — Your diff touches a retry loop with a weird `sleep(70)`. You ask why. The Ledger answers: 2023, vendor rate-limit window is 60s, +10s clock-skew margin, safe to remove *if* the vendor contract changes — and flags that the vendor's new API, adopted last month, made this obsolete. You delete it with confidence. An antibody screens the deletion against the 2023 incident that created it. Clean.

**14:00** — You do a fiddly three-step log-correlation dance during a debug session. The Tacit Miner offers it back to you as a tool called `trace-join`. You accept. A teammate in another timezone uses it four hours later without knowing you "wrote" it.

**16:20** — Your PR lands. The Attention Compiler waves through 80% of it with characterization-test insurance, and routes the 40 risky lines — the ones touching the subsystem with two prior incidents — to the one person who carries that context, with the relevant Ledger entries pre-attached. Review takes eleven minutes instead of a day of ping-pong.

**Overnight** — the org gets a little smarter. It does that every night now.

---

## Why this and not another copilot

Because of what compounds. Code generation is a *flow* improvement — you get the speedup once per keystroke, every vendor has it, and its price is racing to zero. Mycelium is a *stock*: an accumulating, queryable, self-defending body of organizational memory that gets more valuable every single day it runs, and that a competitor cannot catch up on by buying a better model — because the moat isn't the model, it's *your* ledger, *your* antibodies, *your* mined skills. It's the first dev tool whose value curve bends upward with age instead of flattening.

And it attacks the real cost centers. Studies keep finding developers spend well over half their time not writing code: understanding existing systems, re-deriving lost context, reviewing, coordinating, firefighting repeats of known failures. That's where the org's money goes. That's what Mycelium eats.

---

## Wild cards (the other bets I'd fund)

Smaller, sharper ideas that could each stand alone:

- **Ghost Production Twin** — a shadow environment continuously replaying sampled real traffic against your *working branch* as you type. "Works on my machine" becomes "worked against yesterday's production, eleven seconds ago," surfaced in-editor.
- **Archaeology Mode** — point at any subsystem and get its reconstructed *story*: the pressures, the pivots, the abandoned migration half-visible in the strata. History as a first-class debugging tool.
- **Onboarding: Campaign Mode** — new hires don't read docs; they play a generated campaign through the codebase: replaying pivotal historical decisions as interactive scenarios, doing safe sandboxed quests calibrated to what they'll actually touch first. Time-to-first-meaningful-PR drops from weeks to days.
- **The Entropy Budget** — SRE gave us error budgets, which ended the feature-vs-reliability shouting match with a number. Do the same for tech debt: a measured entropy budget (dead code, drift between doc and behavior, antibody-flagged fragility) that gates feature velocity mechanically. When the budget's blown, the Dream Cycle's refactor branches jump the queue — no meeting required.
- **The Calibration Market** — an internal prediction market where agents *and* humans stake on "where will the next incident be?" Scored on calibration over time. The output isn't the bets — it's a continuously honest org-wide risk map, and a record of who (human or agent) actually knows where the bodies are buried.
- **Spec-Anchored Development** — the natural-language spec becomes the source-of-truth artifact and code becomes a build product; AI keeps them bidirectionally synced and *spec drift fails CI*. Requirements documents stop being fiction.

---

## Design notes

### How is any of this enforced?

Every knowledge system in history has died the same death: it depended on human discipline, and humans won't do documentation chores. Mycelium's enforcement principle is therefore: **no behavior may depend on a human remembering to do anything.**

1. **Capture at chokepoints, as a byproduct.** Software work already flows through a handful of unavoidable gates — the merge, the PR, the incident ticket, the CI pipeline, and increasingly the AI coding session itself. Mycelium lives *inside* those gates. The biggest unlock is that reasoning used to live only in someone's head (uncapturable); with AI-assisted development it lives in the session transcript (capturable). The agent files the Ledger entry as part of finishing the task — zero human keystrokes. For human-authored changes, the system *drafts* the why from the diff, the ticket, and the PR discussion, and the human only confirms or corrects it during the review they were already doing. People won't compose; they will happily correct.

2. **Mechanical gates where it matters, silence where it doesn't.** Antibody screening runs in CI — you can no more "forget" it than forget to run tests; the status check is the one enforcement mechanism the industry has proven works. But blanket gates breed garbage compliance (the way commit-message rules produce "fix"), so gating is risk-tiered by the Attention Compiler: hot paths require a human-signed why; cold paths get silent automatic capture and no ceremony at all.

3. **Consumption is the real enforcement.** Systems survive when the person doing the work gets value the same day. The Ledger answers *your own* "why is this here?" an hour after you need it; a dense Ledger visibly makes *your* AI agent smarter on *your* repo; an antibody saves *you* from re-shipping an old bug. Once the tool pays you daily, compliance stops being a policy and becomes self-interest — people defend the system instead of gaming it.

4. **Quality is audited, not assumed.** A ledger full of auto-generated boilerplate is worse than none. The Dream Cycle audits entries (does this actually explain the diff? does it now contradict the code?), unverified entries decay, and entries are calibration-scored by use: one that answered a question and helped gets reinforced; one that misled gets flagged.

5. **Agents are the easy half.** A growing share of commits are authored by AI agents, and agents don't need convincing — they need configuring. Mycelium's policies are enforced on the agent workforce by construction; humans get the softer confirm-not-correct path.

### What about brownfield projects?

The cold-start objection: a 12-year-old codebase, original authors gone, the whys already evaporated. Four answers, in order of leverage:

1. **Archaeology, not paperwork.** A brownfield repo isn't silent — it's a fossil record: tens of thousands of commit messages, PR threads, tickets, postmortems, chat archives, comments, test names. An offline agent fleet reads all of it (a job no org would ever pay humans to do, and agents do overnight) and reconstructs *provisional* Ledger entries — explicitly marked as inferred, with confidence levels and citations: "this 70-second timeout appeared in commit `abc123`, the same week as incident #442; the PR thread mentions vendor rate limits." Inferred entries are hypotheses to confirm, never truths.

2. **Excavate on demand.** Don't boil the ocean on day one. Deep excavation of a region is triggered the first time someone actually touches or asks about it — cost tracks value, and every answer is cached in the Ledger forever. Meanwhile churn concentrates: the ~20% of a codebase under active change accumulates dense, fully-captured coverage within months, so a brownfield repo converges to greenfield along exactly the paths that matter.

3. **Armor what can't be explained.** Where the why is unrecoverable, pin down the *what*: auto-generated characterization tests snapshot the current behavior of the murkiest regions. We can't recover why the fence is there, but we can electrify it — and when someone eventually trips it, the resulting "is this behavior load-bearing?" conversation becomes the Ledger entry. Lost knowledge gets regenerated at precisely the moments it's needed.

4. **Interview the elders — urgently.** The densest knowledge store in a brownfield org is the two engineers who've been there nine years. An agent conducts targeted interviews — not "please write docs" but "I found this retry logic from 2019; the commit says 'fix for the thing'; was this the Cassandra migration?" Thirty minutes of confirm/deny yields entries no archaeology could reach, prioritized by which hot regions only they understand. And when anyone gives notice, their final weeks include guided extraction of the knowledge only they hold.

One more asymmetry worth naming: the Immune System needs no cooperation from the codebase at all — it feeds on incident history, which is usually a brownfield org's best-preserved record. Old scar tissue converts straight into antibodies, which means the messiest, oldest orgs get the immune payoff *first*.

---

*Written by Claude (Anthropic) in response to: "If you were given an open hand to build something that can improve developer productivity with AI, what would you build?"*
