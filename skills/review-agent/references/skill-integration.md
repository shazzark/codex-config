# Integration with Existing Skills

`review-agent` coordinates a review and reports evidence, severity, verification, and an acceptance verdict. It does not take over implementation, diagnosis, test design, or specialist design guidance.

| Existing skill | Use it for | Review-agent contribution |
| --- | --- | --- |
| `engineering-workflow` | Implementing and delivering substantial changes | Review the completed phase against its approved requirements and inspect its relevant diff. |
| `debugging-workflow` | Reproducing symptoms and finding root causes, usually with a fix | Report confirmed behavior and evidence; do not silently turn review into a repair task. |
| `testing-engineering` | Test strategy, test authoring, and test maintenance | Assess relevant test evidence and report exactly which checks ran. |
| `security-engineering` | Threat modeling and broad security controls | Flag evidence-backed security findings; use the specialist skill for deeper security criteria. |
| `backend-architecture` | Backend boundaries and system design | Assess changed contracts and architecture where relevant; leave design work to the specialist. |
| `database-engineering` | Data models, queries, migrations, and database operations | Check affected data behavior; use specialist guidance for detailed database criteria. |
| `language-engineering` | Idiomatic, correct, maintainable implementation in supported languages | Identify concrete code risks; use it for language-specific review guidance. |
| `devops-infrastructure` | CI/CD, deployment, observability, and reliability | Check operational impact where the change touches it; use specialist guidance for detailed operations. |

Combine skills only when the changed work crosses their domains. A review may cite a specialist dimension without loading every specialist skill for every change.
