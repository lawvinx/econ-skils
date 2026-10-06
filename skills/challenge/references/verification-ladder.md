# Specification-search ledger

Excerpt from the upstream Verification Ladder, MIT licensed. The full source is preserved in `../upstream/verification-ladder.md`. Only the following ledger contract is bundled as active challenge guidance.

## Rung 5 — The ledger (make adaptive search inspectable)

Agentic tooling makes specification search fast and cheap, which widens hidden researcher
degrees of freedom. The answer is not to forbid search — it is to **record** it.

Four artifacts:

1. **An instruction contract** — objective, admissible modifications, and a **search budget**.
2. **An immutable evaluator** — the scoring harness, never edited during the search.
3. **A single editable surface** — the one file the agent may change.
4. **An append-only ledger** — every attempt: identifier, **label** (below), score, outcome
   (`keep / discard / crash`), and a one-line description of the strategy.

Then: **pre-commit the interpretation before running the test** — write what result would
SUPPORT versus WEAKEN the claim *first*. The ledger is the arbiter. This closes the
garden-of-forking-paths gap that hypothesis-only preregistration leaves open.

**Label each run when it is queued, never when it lands.** Three labels, and the label is part
of the append-only row, so it cannot be revised in the direction of the result:

- **pre-specified** — named in the instruction contract before the search began.
- **confirmatory** — committed before this particular run, as a test of a claim the search has
  already produced.
- **exploratory** — everything else: a look, a variant, a hunch.

**Only pre-specified and confirmatory runs can support a headline claim.** An exploratory run
produces a **hypothesis for the next pre-specification** — it is a reason to write a contract,
not evidence for the current one. Relabelling after the fact is precisely the forking path this
rung exists to close ([`/preregister`](https://github.com/pedrohcgs/claude-code-my-workflow/blob/main/.claude/skills/preregister/SKILL.md) is where the
pre-specified set gets written down).

**Record every attempt, including nulls and failures.** A ledger showing only supporting
tests is a fishing expedition with good PR.

**Reserve a holdout and evaluate it only after the search.** In-sample improvement does not
generalize: published runs of this protocol show relative RMSE going 0.510 → 0.811 and
0.808 → **1.089** out of sample.

Two elements worth carrying over from practice:

- **A fixed budget** makes runs comparable by construction.
- **A simplicity criterion.** A gain that adds twenty lines of hacky code is probably not
  worth it; an *equal* result from deleting code is a win. A robustness result that survives
  with **fewer** controls is a stronger result — the ledger should say so.

---

## The withdraw disposition — when the number misses its bound

Rung 5 pre-commits the interpretation and records the search. This is the other half of that
sentence: **what you owe everyone when the recorded number misses the bound you wrote down
first.**

When a capability, a default, or an automatic behaviour fails the numeric bound it
preregistered, the disposition is **WITHDRAW** — demote it to **explicit opt-in**. **Never
widen the bound.** A bound moved after seeing the number is not a bound; it is a description of
the result, and every claim that later cites it is circular. Demote rather than delete: opt-in
keeps the thing usable by someone who has read the number and accepted it, while deletion
destroys the trail and re-opens the question from scratch next release.

**A single noisy estimate does not fire a disposition.** Three conditions gate the trigger, and
all three belong in the preregistration, written before the number lands:

1. **The bound carries its uncertainty.** A bound is a number *and* the Monte Carlo or sampling
   error of the measurement that will be compared against it. *"Coverage ≥ 0.93"* is not a bound
   you can miss; *"coverage ≥ 0.93, at `R` chosen for MCSE ≈ 0.005, missed when the estimate
   falls below the bound by more than 2 MCSE"* is. Without the second half, every borderline draw
   is a disposition and the rule fires on noise
   ([`https://github.com/pedrohcgs/claude-code-my-workflow/blob/main/.claude/rules/simulation-conventions.md`](https://github.com/pedrohcgs/claude-code-my-workflow/blob/main/.claude/rules/simulation-conventions.md) §4).
2. **A re-measurement precedes the disposition.** Re-run the campaign on fresh seeds, same budget
   and same bound, and demote only if it misses again. Where a re-measurement is genuinely
   impossible — a one-shot measurement, an external service that changed underneath you, a
   campaign nobody will fund twice — **state that reason in the verdict artifact** and record
   that the disposition rests on one draw. Obligation 2 below is not in tension with this: the
   confirming re-run is pre-committed and happens *before* the withdrawal; what that obligation
   forbids is re-rolling a failed campaign *afterwards* until it passes.
3. **The withdrawal names its own scope.** Exactly one of: the **default** (the capability stays,
   reachable by explicit opt-in), the **guarantee** (the numeric claim is retracted; the
   capability ships without it), or the **method** (the procedure itself is withdrawn from
   recommendation). A bare *"WITHDRAW"* leaves every reader to infer which, and they infer the
   cheapest one.

Three obligations, and the withdrawal is not finished until all three are discharged:

1. **Publish it where users read.** The failing number, the bound it missed, and the
   disposition, together, in the release notes (`NEWS` / `CHANGELOG`) — *and* recorded in the
   verdict artifact. The two are not substitutes: the artifact serves the replicator, the
   release notes serve the person deciding whether to switch it back on.
2. **Preserve the failing campaign as negative calibration evidence.** Never deleted, and
   **never re-run until the underlying thing changes.** A re-run against unchanged inputs is a
   second draw, and the second draw is the one that gets published.
3. **Enumerate the impact.** Every adjacent surface listed and dispositioned — affected, or
   explicitly *not* affected and why. A withdrawal naming only the surface that failed leaves
   readers to guess at the rest, and they guess low.

**The bound is the promise; the default is the endorsement.** Shipping something on by default
asserts its bound holds for a user who never looked. Once the measurement says otherwise, the
honest options are to fix the thing or to stop asserting it — *widen the bound to match what we
measured* is neither, and it is the single most tempting move on this page.

---

