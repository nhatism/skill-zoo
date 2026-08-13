# Skill Zoo Constitution

**Version:** 1.0  
**Status:** Active  
**Scope:** Repository policy for reusable AI-agent skills

---

## 1. Purpose

Skill Zoo keeps a deliberately small collection of skills that address concrete, recurring, or expensive failures.

It is not a prompt dump, a skill-count competition, or a place to preserve every useful instruction.

> **Every resident must earn its cage.**  
> **Every always-loaded instruction pays context rent.**

---

## 2. Zoo Status Is Not Runtime Loading

`resident` is a **Zoo admission status**, not a context-loading mode.

```text
Zoo:
incubating ≠ resident

Runtime:
installed ≠ loaded ≠ invoked
```

- **Incubating** — candidate not yet endorsed by the Zoo.
- **Resident** — admitted artifact the project is willing to stand behind, within explicit evidence boundaries.
- **Installed** — available to a user or agent.
- **Loaded** — present in current context.
- **Invoked** — selected for the current task.

A resident may be always-loaded, on-demand, project-local, user-selected, or not installed. **Zoo residency never implies always-loaded context.**

---

## 3. Context Still Has a Price

Admission does not erase runtime economics.

Reference budget for an always-loaded surface:

```yaml
always_loaded_reference_budget: { max_files: 5, max_total_size_kb: 50 }
warning_threshold: { files: 4, total_size_kb: 40 }
```

These are planning ceilings, not Zoo admission limits.

Report separately:

- **Always-loaded tax** — paid across eligible tasks.
- **Invocation cost** — paid only when selected.

Say what is actually loaded, when, and why. Approximate token arithmetic is planning data, not precision.

---

## 4. Three Admission Gates

### Gate 1 — Existence

> **What concrete failure does this prevent, or what capability does it reliably provide?**

Identify the trigger, expected behavior, failure surface, and why ordinary model behavior is unreliable enough to matter. If removing the skill produces no identifiable behavioral delta, it has not justified existence.

### Gate 2 — Residency

> **Does this artifact have enough bounded, honest evidence for the Zoo to stand behind it?**

A resident should be reusable beyond one project, coherent enough to maintain, reduced enough to justify its surface, preserve known negative evidence, and state unsupported areas plainly.

Residency is project endorsement. It is not a deployment recommendation.

### Gate 3 — Claim

> **What exactly are we claiming, and what evidence supports it?**

A safety rule may rest on strong asymmetric-risk reasoning. A performance claim requires performance evidence.

Never upgrade:

```text
plausible → proven
observed → guaranteed
one evaluated behavior → whole-skill validation
```

---

## 5. Evidence and Benchmark Discipline

Evidence levels describe maturity, not prestige:

0. **Rationale** — concrete failure/capability + mechanism.
1. **Observed use** — real failure, near-miss, or correction.
2. **Repeated use** — useful across multiple tasks, repos, or users.
3. **Benchmark** — controlled comparison against a competent baseline.
4. **Retention evidence** — survives serious reduction, ablation, replacement, or competing-policy tests.

Not every rule needs Level 3 or 4. Evidence coverage belongs to the claims it actually supports.

For benchmarks: state claim, baseline, treatment, task set, conditions, metrics, decision rule, and limitations. Use a competent baseline. Count both *work avoided* and *work caused*. Preserve negative results.

When delivery mode matters, record it. Benefit or cost observed under one carrier does not automatically transport to another.

---

## 6. Loading Recommendations Are Separate Claims

For an admitted skill, runtime guidance may distinguish:

```yaml
runtime_loading:
  always-loaded:
  on-demand:
  project-local:
  user-choice:
```

State where benefit and cost were observed, what loading mechanism was tested, and what transport remains unknown.

`resident` is not shorthand for `always-loaded`.

---

## 7. Prefer Checkable Rules; Reduce Aggressively

Prefer instructions with clear triggers, constraints, and violations over generic virtue language.

Pressure to reduce includes generic advice, model-baseline repetition, tutorial inflation, defensive duplication, project-local leakage, and multiple sentences expressing one invariant.

> **Delete instructions, not capability.**

A resident may return to incubation or be removed when models handle the failure reliably unaided, tooling supersedes it, another artifact subsumes it, the failure no longer matters, or maintenance cost exceeds value.

Zoo residency is revisable, not tenure.

---

## 8. Contribution Test

Before admission, answer:

- What failure or capability is addressed?
- What behavioral delta exists?
- What evidence supports it today?
- What exact claims are supported, and what remains unsupported?
- Why is it reusable rather than project-local?
- What was reduced before admission?
- Under which loading modes were benefits and costs observed?
- What runtime recommendation, if any, follows from that evidence?

The questions matter more than the form.

---

## 9. Repository Shape

```text
incubator/
  candidates not yet admitted

skills/
  Zoo residents

evaluations/
  public evidence and limitations
```

`skills/` is an admitted surface, not an always-loaded bundle.

Healthy repositories keep few deliberate residents, bounded claims, visible negative evidence, competent baselines, and artifacts that can shrink or leave.

Unhealthy repositories admit by vibes, infer whole-skill validation from one tested behavior, hide loading assumptions, or grow taxonomy for its own sake.

---

## 10. Governance and Starting Values

This constitution is operating policy, not sacred text. Revise it when real use exposes a false assumption, missing distinction, stale budget, bad metric, or unnecessary rule — not merely to accommodate growth.

```yaml
zoo_status: [incubating, resident]
runtime_loading: [always-loaded, on-demand, project-local, user-choice, not-installed]

always_loaded_reference_budget:
  max_files: 5
  max_total_size_kb: 50

warning_threshold:
  files: 4
  total_size_kb: 40

admission_gates: [existence, residency, claim]
evidence_levels: [rationale, observed_use, repeated_use, benchmark, retention_evidence]
```

Deliberately simple starting assumptions — not natural laws.
