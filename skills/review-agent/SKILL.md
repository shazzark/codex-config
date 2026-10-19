---
name: review-agent
description: Review completed project work against approved requirements and acceptance criteria. Use for a focused, evidence-based review of a change or delivery phase.
metadata:
  short-description: Review completed work against its requirements
---

# Review Agent

Assess whether the reviewed work meets its approved requirements and acceptance criteria. Focus on actionable defects supported by evidence. Keep the review read-only: do not modify code, apply fixes, approve your own changes, commit, push, deploy, delete files, or install dependencies. Never include secret values in findings.

## Workflow

1. Establish scope, project context, and the approved requirements and acceptance criteria. If these are missing or ambiguous, identify the gap and limit the verdict to what can be assessed.
2. Inspect relevant requirements, existing code context, and Git status and diffs. Do not assume all changed files are in scope or that uncommitted work is disposable.
3. Choose review dimensions that apply to the change. Use [the review checklist](references/review-checklist.md) as a menu, not a requirement to inspect every dimension.
4. Investigate potential defects in the surrounding code and available evidence. Separate confirmed defects from plausible risks and optional improvements; do not manufacture issues or present speculation as fact.
5. Run relevant non-destructive checks when practical and within the user's authorization. Report the exact checks run and results. Do not install dependencies or use production data or systems.
6. Report each substantive finding with severity, type, file and line, evidence, practical impact, reproduction details where possible, and a corrective action. Prioritize by severity and impact. Use [the report template](references/review-report-template.md).
7. Summarize verification results and unresolved risks, then state whether this phase meets its defined acceptance criteria. Passing tests do not guarantee correctness.

If you authored or fixed code in the reviewed scope, disclose that and do not present your review as independent approval or use it to satisfy an independent-review gate.

## Agent delegation

Check whether the active Codex runtime exposes a subagent capability, such as a `spawn_agent` tool; CLI version or session-browsing commands alone do not prove that task delegation is available. If supported and useful, delegate a bounded, read-only review by giving the reviewer the approved criteria and exact scope, asking for evidence and file locations, and explicitly prohibiting edits. Independently assess delegated findings before including them. Delegation does not transfer the lead reviewer's responsibility. Do not claim delegation occurred unless an agent actually ran. If delegation is unavailable or not useful, use the normal Codex review workflow.

## Fit with other skills

This skill owns review scope, evidence, findings, and the acceptance verdict. It complements [engineering-workflow](../engineering-workflow/SKILL.md), which guides implementation and delivery; [debugging-workflow](../debugging-workflow/SKILL.md), which diagnoses and fixes observed failures; [testing-engineering](../testing-engineering/SKILL.md), which designs and evaluates tests; and domain skills for deeper technical criteria. See the [skill integration guide](references/skill-integration.md) when a review crosses these boundaries.

## Validation

Use the official skill-creator validator as described in [validation instructions](references/validation.md). Also check the README entry, Markdown links, report template completeness, and whether the guidance stays complementary to the existing skills.
