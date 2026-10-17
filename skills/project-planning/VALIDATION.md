# Project-planning scenario review

The following six reviews are **design exercises** against the skill and its templates. They are not stakeholder interviews, feasibility proofs, implementations, or user validation. Each scenario uses only the stated prompt and labels missing business details as questions or assumptions.

## 1. Small TypeScript CLI utility

**Mode:** Quick.

**MVP:** One clearly named command that solves the primary task for a defined input/output format, reports invalid input clearly, and has a simple install/run path. Do not assume configuration files, plugins, remote services, telemetry, or a GUI.

**Documents:** `project-brief.md` and `mvp-scope.md`; combine into a concise spec if there is little ambiguity. `implementation-plan.md` with a few phases. No separate TRD unless runtime/platform compatibility, security, packaging, or integration requirements justify it.

**Actionable phases:** Confirm input/output examples and non-goals; implement core command and validation; add tests/docs/package checks and verify supported runtime.

**Review:** Proportionate plan, unknown behavior marked open, no infrastructure assumed, criteria are observable (expected output/exit behavior). Ask for approval of command behavior and target platforms before marking requirements final or coding.

## 2. Full-stack e-commerce web application

**Mode:** Standard initially; move to Advanced only if team/service boundaries, scale, policy, or operational risk warrants it.

**MVP:** One narrow purchase journey, such as browsing a limited catalog and placing an order with a confirmed payment method. Inventory, shipping, refunds, taxes, regions, roles, and compliance are not silently invented; clarify which are launch-critical.

**Documents:** Brief, MVP scope, PRD, app flow, TRD, architecture, database schema, design-system plan, external-integration specs for chosen provider(s), testing/acceptance plan, implementation plan, and tasks. Include a risk register only for material uncertainties; create ADRs for consequential decisions.

**Actionable phases:** Confirm policies and provider contracts; agree data/technical boundaries and acceptance; build a thin vertical purchase path; add payment/security and failure handling; verify integration, accessibility, and release criteria.

**Review:** Documents have distinct product/technical/architecture roles; no provider capability, legal rule, or stock policy is claimed without confirmation. Acceptance needs stakeholder-approved business rules and sandbox evidence. Approval required before selecting provider, finalizing commerce rules, or implementation.

## 3. React Native mobile application

**Mode:** Standard.

**MVP:** The primary mobile journey on explicitly supported platforms with accessible states and a defined sign-in/offline need. Do not assume both app stores, push notifications, offline sync, analytics, or a custom design system are required.

**Documents:** Brief, MVP scope, PRD, app flow, design-system plan, and implementation plan/tasks. Add a TRD for platform/device, privacy, API, offline, or release constraints. Add architecture/database docs only if a backend or data boundary materially affects choices.

**Actionable phases:** Confirm users/platforms and current designs; map the primary journey and states; define native/API constraints and acceptance; implement a thin screen flow; verify target iOS/Android layouts, accessibility, and tests.

**Review:** Web and native behavior are not assumed interchangeable. Figma-derived requirements remain assumptions until source designs are inspected. Approval needed for platform scope, identity/offline decisions, and visual identity.

## 4. Excel-to-database SaaS platform

**Mode:** Standard with Advanced-level risk review if sensitivity, tenant count, scale, or regulatory constraints require it.

**MVP:** Upload one confirmed workbook shape, validate it, preview actionable errors, and import records into an agreed tenant-owned dataset. Do not assume arbitrary spreadsheet support, direct writes into customer databases, scheduled sync, multi-format mapping, or automatic correction.

**Documents:** Brief, MVP scope, PRD, app flow, TRD, architecture, database schema, external-integration spec if a customer/provider system is involved, risk register, testing/acceptance plan, implementation plan/tasks. A design-system plan is optional unless multiple import/review screens need shared patterns. ADRs only for consequential technology choices.

**Actionable phases:** Confirm spreadsheet format, data ownership, tenant access, retention, and import rules; define parsing/validation and failure acceptance; model isolated records and safe retry behavior; build upload/preview/import path; verify permissions, malformed files, duplicates, and cleanup.

**Review:** Critical unknowns (schema, sensitive data, tenant boundaries, duplicate policy, rollback) are explicitly open, not made up. Use database/security skills for detailed design. Approval before production data access, provider credentials, or implementation.

## 5. AI agent with external tools and integrations

**Mode:** Advanced because model/tool behavior, data access, permissions, and external failures require explicit boundaries—not merely because AI is involved.

**MVP:** One bounded user task, a small allowlist of read-only or reversible tools, explicit user confirmation for consequential actions, and a way to inspect or correct results. Do not assume model accuracy, broad account access, autonomous writes, arbitrary web access, or provider permissions.

**Documents:** Brief, MVP scope, PRD, TRD, architecture, external-integrations specs, risk register, testing/acceptance plan, implementation plan/tasks; add app flow/design-system if a user-facing interface exists, database schema if durable state matters, and ADRs for material choices.

**Actionable phases:** Validate use case and data/tool permissions; define trust boundary, allowed actions, refusal/confirmation, privacy and evaluation criteria; build a sandboxed thin path; run scenario/abuse evaluations and integration tests; review telemetry, cost, and deployment controls.

**Review:** Model/provider capabilities are verified from current documentation or listed as unknown; evaluation thresholds require stakeholder agreement. Human confirmation and least-privilege access are planned, not presumed to be implemented. Approval before provider use, credentials, data access, or code.

## 6. Multi-service real-time platform

**Mode:** Advanced.

**MVP:** One user-visible real-time journey with a defined delivery/ordering expectation, a small number of services justified by existing ownership or isolation needs, and explicit behavior during disconnects. Do not assume global scale, exactly-once delivery, multi-region, or a service mesh.

**Documents:** Brief, MVP scope, PRD, app flow, TRD, architecture, external-integration specs as needed, database schema per actual ownership, risk register, testing/acceptance plan, implementation plan/tasks, and ADRs for major communication/consistency decisions. Add design-system where the product has shared UI. Keep detailed transport/deployment controls in networking and DevOps work.

**Actionable phases:** Confirm user journey and delivery semantics; map service/data owners and partial-failure behavior; agree reliability targets and test environments; build one vertical event path; verify reconnect, duplicate, backpressure, and dependency failure behavior; expand only after measured need.

**Review:** System boundaries, message delivery, consistency, and operational cost are explicit. No performance/availability targets are invented. Require approval for service topology, data ownership, and reliability goals before finalizing or coding.

## Overall result and limits

Across all six exercises, mode selection scales with risk and complexity; each MVP excludes unsupported features; assumptions remain labeled; and documents are selected by the decisions they answer. The TRD is included when testable technical constraints warrant one, but can be omitted or combined for small projects. Phases have objectives, dependencies, deliverables, acceptance, verification, risks, and completion checks. These reviews do not validate actual stakeholder needs, provider permissions, cost estimates, feasibility, model quality, or runtime behavior. No application code or external service was used.
