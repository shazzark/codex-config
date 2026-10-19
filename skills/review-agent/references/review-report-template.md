# Review Report

Use this template for a completed review. Remove unused prompts, but keep the distinction between defects, risks, and optional improvements.

## Scope

- **Project and reviewed phase/change:**
- **Requirements and acceptance criteria reviewed:**
- **Files, commits, or diff range inspected:**
- **Review dimensions selected:**
- **Limitations:**

## Findings

List in descending order of severity and practical impact. Use one entry per distinct issue.

### [SEVERITY] [CONFIRMED DEFECT | PLAUSIBLE RISK] Short title

- **Location:** `path/to/file.ext:line`
- **Evidence:** What was observed and how it supports the finding.
- **Impact:** Who or what is affected and under what conditions.
- **Reproduction:** Minimal steps or input, when reproducible; otherwise state what remains unverified.
- **Recommended action:** A concrete corrective action.

If there are no supported defects or risks, say so. Do not add findings to fill space.

## Optional improvements

- **Location (if applicable):**
- **Suggestion and value:**

## Verification

- **Checks run and results:**
- **Checks not run and reason:**
- **Unresolved risks or evidence gaps:**
- **Independent agent review:** State whether an agent ran; identify its bounded scope, or say unavailable/not used.

## Acceptance verdict

- **Result:** Meets / Does not meet / Cannot determine
- **Basis:** Relate the result to the defined acceptance criteria. Explain any unmet criterion or evidence gap. Passing checks alone do not establish correctness.

## Severity

- **Critical:** Severe exploitation risk, destructive data loss, or system-wide failure.
- **High:** Significant correctness, security, or reliability defect.
- **Medium:** Meaningful but bounded defect or maintainability risk.
- **Low:** Minor actionable issue.
- **Informational:** Observation or optional improvement, clearly separated from defects.
