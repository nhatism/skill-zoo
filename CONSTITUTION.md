# Skill Zoo Constitution

**Version:** 1.0
**Status:** Active
**Scope:** Repository-level policy for reusable AI-agent skills

---

## 1. Purpose

Skill Zoo maintains a deliberately small collection of agent skills that prevent concrete, recurring, or expensive failures.

It is not a prompt dump, a catalogue of generic advice, a skill-count competition, or a place to preserve every useful instruction. It optimizes for behavioral protection under a bounded context budget.

> **Every resident instruction pays rent in tokens. Make it earn its room.**
> **Every invocation runs a tab. Make it relevant to the task.**

---

## 2. Three Different States

```text
installed ≠ resident ≠ invoked
```

- **Installed** — exists in the repo / available to the agent. Costs ~nothing when unused.
- **Resident** — the skill, description, or routing metadata occupies context *before* the task needs it. A fixed tax paid across eligible tasks.
- **Invoked** — the skill body loads because the current task selected it. Cost is paid only on use.

A good skill can still be a bad resident. A large on-demand catalogue is economically different from the same catalogue loaded permanently.

---

## 3. Context Is a Budget

Permanent context competes with the user's request, conversation history, repo files, code, logs, tool output, and task-specific reasoning — it isn't free just because the context window is large.

```yaml
resident_budget:
  max_files: 5
  max_total_size_kb: 50
warning_threshold:
  files: 4
  total_size_kb: 40
```

These are ceilings, not targets. 3 skills at 24 KB is healthier than 5 skills at 49 KB if both preserve the same behavior.

Rough planning estimate: `1 token ≈ 3.5–4.5 chars` → 50 KB ≈ 11k–15k tokens. Planning arithmetic, not a tokenizer guarantee — use a real tokenizer when exact accounting matters.

---

## 4. Report Two Costs Separately

- **Resident tax** — paid before/across ordinary tasks.
- **Invocation cost** — paid only when a skill is selected.

Don't advertise "N skills only cost X tokens" without saying what's resident vs. on-demand. Don't judge a collection by file count alone. The relevant question is what context is actually present, when, and why.

---

## 5. Three Admission Gates

**Gate 1 — Existence.** *What concrete failure does this prevent, or capability does it reliably provide?* Weak: "helps the agent work more professionally." Strong: "prevents the agent from silently discarding unrelated local Git changes." If removing the skill produces no identifiable behavioral delta, it hasn't justified existence.

**Gate 2 — Residency.** *Why must this be resident rather than on-demand or project-local?* Resident candidates: destructive-operation guardrails, protection of unrelated user work, irreversible external-action constraints, routing rules needed before task selection. On-demand candidates: migrations, releases, framework-specific workflows, incident procedures. Project-local: branch names, CI commands, reviewers, team conventions. Default: **if a skill is only useful once the task category is known, load it on demand.**

**Gate 3 — Claim.** A rule can be admitted on strong risk reasoning alone — "never auto-run `git push --force`" needs no benchmark. A *performance* claim ("reduces mistakes by 37%") needs performance evidence. Never upgrade *plausible → proven* or *used successfully → demonstrated capability* without evidence to match.

---

## 6. Checkable Rules Over Generic Advice

Prefer instructions with a clear trigger, constraint, and violation. "Never auto-run `git push --force`" has all three (trigger: force-push considered; violation: it runs without human intent; failure surface: history rewrite). Compare "write clear, professional commit messages" — plausibly true, but no sharp pass/fail boundary, and it doesn't earn residency just by sounding correct. Generic advice isn't forbidden; it just doesn't earn a permanent seat.

---

## 7. Two Density Metrics — Imperfect on Purpose

- **Primary — checkable rules per resident KB.** Exposes hoarding and duplicated prose. Don't game it by splitting one rule into many bullets; distinct rules must map to distinct failure surfaces.
- **Secondary — prevented failure surface per permanent token.** Rule count alone can't represent severity. (One guardrail against data loss can outweigh ten formatting preferences.) Ask: how costly, how frequent, how reversible, how likely is a competent model to avoid it unaided, does it protect real external state, does it generalize across projects? Don't invent pseudo-precision.

---

## 8. Sentence-Level Deletion Test

For every normative sentence:

```text
delete this sentence → does a concrete agent behavior change?
  no  → delete
  yes → is a competent model already reliable here without it?
          yes → probably delete/demote
          no  → is the failure costly/recurrent enough for its current loading mode?
                  no  → on-demand / project-local
                  yes → resident candidate
```

---

## 9. Evidence Ladder

Describes maturity, not prestige. Not every rule needs the top level, and don't claim a higher one than the repo actually has.

0. **Rationale** — concrete failure + explicit mechanism, no track record yet.
1. **Observed use** — came from a real failure, near-miss, or correction.
2. **Repeated use** — proved useful across multiple tasks, repos, or users.
3. **Benchmark** — controlled comparison vs. a competent baseline, declared tasks/metrics/conditions.
4. **Retention evidence** — survives serious reduction or ablation without losing the capability that justified it.

---

## 10. Benchmark Discipline

State the claim, baseline, treatment, task set, conditions, metrics, and limitations. Use a competent baseline. Don't cripple the control to flatter the skill. Count *work avoided* **and** *work caused*: improving one local metric while increasing total work isn't automatically progress. Negative results are valid and should survive publication, not get tuned away.

Context-shape measurements ("descriptions got 63% smaller") are not performance evidence on their own — keep structural measurements and task-performance claims separate.

---

## 11. Skill Classes

- **CORE** — broadly reusable safety behavior; costly if absent. Usually resident. Should be rare.
- **DEFAULTS** — reusable, override-able workflow preferences. Resident only if genuinely always-needed.
- **ON-DEMAND** — task-specific, loaded when relevant. May exceed the resident ceiling since it isn't resident — but invocation cost, routing cost, and maintenance still matter.
- **PROJECT-LOCAL** — specific to one repo, org, or environment. Keep it local; don't upstream just because it's useful elsewhere.

The resident ceiling applies to the **resident surface**, not total repo file count, but a large on-demand catalogue still isn't free: routing metadata, discovery cost, naming collisions, and human navigation cost all scale with it. Don't use on-demand loading as an excuse to build a skill landfill; every skill still passes Gate 1. A selection mechanism can itself be wrong or costly, so skill usefulness and skill-selection quality are separate claims.

---

## 12. Warning Signs (Pressure to Reduce)

- **Generic virtue language** — "be thoughtful," "use best practices," "write clean code." May be true; truth alone doesn't earn residency.
- **Model-baseline repetition** — instructing the model to do what it already does reliably, absent evidence of recurring failure.
- **Tutorial inflation** — a skill isn't a textbook; include only what the agent must not forget mid-task.
- **Defensive duplication** — repeating one guardrail across sections to make it feel stronger. Prefer one normative owner.
- **Completeness pressure** — padding because the document looks short. Short can be complete.
- **Category completion** — adding a skill just because the repo "lacks one for that category." Real failure outranks taxonomy symmetry.

Prefer mechanism over prose: "before pushing, inspect `git remote -v`" beats "exercise care to ensure operations target the correct remote." Use prose only for scope, exceptions, rationale, or precedence — not to sound authoritative.

---

## 13. Reduction and Removal

Reduce a skill when two sections repeat an invariant, an explanation can become a direct rule, baseline model capability improves, a rule proves too vague to check, project-local material leaked in, or a smaller formulation preserves the same protection. **Delete instructions, not capability.**

Remove a skill entirely when its failure no longer recurs, models handle it unaided, tooling or permissions now enforce it, another skill subsumes it, it's too project-specific, or its cost exceeds its value. Deletion is not repository failure because a repo that sometimes shrinks is healthier than one that only grows.

---

## 14. Measurement Without Fake Precision

Useful: resident skill count, resident bytes/approx tokens, invocation bytes/tokens per skill, distinct checkable rules, distinct failure classes, evidence level. Avoid decorative pseudo-metrics ("wisdom density: 91%", "alignment coefficient: 0.94") — numbers exist for budgeting and falsifiability, not to dress up opinion.

Optional lightweight per-skill record, worth adding once it solves a real maintenance problem, not before:

```yaml
name: git-steward-core
class: core
loading: resident
size_kb: 9
approx_tokens: 2300
evidence_level: 1
failure_surface:
  - destructive repository mutation
  - unrelated work loss
  - shared history rewrite
```

---

## 15. Budget Review & Contribution Test

At **4 files / 40 KB** resident, run a reduction review before admitting another. At **5 files / 50 KB**, a new resident skill requires reduce, replace, demote, or remove — or an explicit, experience-backed constitutional change. Don't raise the ceiling just because a new skill is interesting.

A proposed skill should be able to answer:

- What failure or capability does it address?
- What behavioral delta does it create?
- What loading mode — resident, on-demand, defaults, or project-local — and why, if resident?
- What evidence supports it today, and what exactly is being claimed?
- What is its context cost, and what was cut before admission?
- Why aren't tooling, project docs, permissions, or ordinary model behavior enough?

Not every contribution needs a formal template; the questions matter more than the form.

---

## 16. Repository Health

**Healthy:** few resident skills, bounded resident bytes, a high proportion of operational rules, real failure provenance, on-demand loading where it fits, project-local details staying local, skills that occasionally shrink or get deleted, claims that match evidence, negative benchmarks that get published.

**Unhealthy:** skill count grows by default, generic advice dominates, routing exists only because the taxonomy exploded, the same rule appears everywhere, every topic becomes a skill, nothing is ever removed, context measurements get marketed as task gains, benchmarks appear only after success, the ceiling rises whenever it's inconvenient.

---

## 17. Audience & Governance

Built for indie developers, small teams, and anyone letting coding agents touch real repos and tools without heavy enterprise policy enforcement already in place. It doesn't need to be universal to be reusable: **general enough to reuse, specific enough to prevent a real failure.**

This constitution is operational policy, not sacred text. It may change when real use exposes a false assumption, an unhelpful metric, a missing distinction, a stale budget, a better evidence model, or an unnecessary rule (not merely to accommodate growth.) Prefer learning from real use before adding governance; the repo shouldn't grow a bureaucracy whose job is governing its own anti-bureaucracy rules.

---

## 18. Starting Values (v1)

```yaml
resident_budget: { max_files: 5, max_total_size_kb: 50 }
warning_threshold: { files: 4, total_size_kb: 40 }
economic_model: [installed, resident, invoked]
admission_gates: [existence, residency, claim]
evidence_levels: [rationale, observed_use, repeated_use, benchmark, retention_evidence]
```

Deliberately simple starting assumptions — not natural law.
