# Security Verification and Vulnerability Response

## Verify the control at the real boundary

- Add focused unit tests for policy/helpers and integration tests for the boundary that enforces the control. Include denied and malformed requests, wrong tenant/object, privilege escalation, and failure behavior where relevant.
- Test the application through direct routes and server entry points, not only through a UI flow. For Next.js include Server Actions, Route Handlers, server data access, and cache behavior that matter to the change.
- Use isolated test accounts, synthetic data, test credentials, and authorized provider/broker/database environments. Do not point destructive or intrusive tests at production.
- Test parser and upload limits, injection-shaped inputs, authorization at resource/property level, CSRF where cookie credentials apply, SSRF destination restrictions, unsafe redirects, rate limits, and safe errors according to the changed surface.
- Test dependency and container findings with lockfile/image context; a version match alone may not show exploitability, and a clean scanner result does not establish that the application is secure.
- Keep security regression tests stable and targeted. Avoid embedding real secrets or attack payloads that would affect live systems.

## Security checks and their limits

- **Static analysis** finds code patterns; it may miss runtime configuration, business logic, and paths it cannot model.
- **Dependency scanning** reports known vulnerable components; it cannot prove a vulnerability is reachable or detect unknown flaws.
- **Dynamic testing** exercises observable behavior in a configured environment; it does not cover every route, role, tenant, or state unless tests explicitly do so.
- **Configuration review** checks the reviewed files and values only; record the environment and version inspected.
- **Penetration testing** must have explicit authorization, scope, safety limits, and a stop condition. Never infer permission from a URL, credential, or repository access.

## Triage and remediation

For each finding, record the affected component and version, preconditions, trust boundary, observed evidence, impact, confidence, and recommended fix. Separate confirmed exploitable behavior from a plausible concern or general hardening suggestion.

Prioritize by exploitability, exposure, affected data/operations, and blast radius. Prefer the smallest correction that closes the path while preserving existing controls. Add a regression test that fails before the fix when practical. If exploitability cannot be confirmed safely, state what evidence is missing and recommend an authorized verification path.

For a vulnerable dependency, inspect the advisory, affected range, installed dependency path, available fixed releases, compatibility, and whether the affected code path is used. Upgrade to a supported fixed version or apply a documented mitigation; verify lockfile changes and relevant tests. Do not suppress an alert or force a broad upgrade without explaining its impact.

## Reporting

State precisely which source paths, commands, versions, environments, tests, and scans were checked. Distinguish implemented and tested controls from untested assumptions, proposed mitigations, and out-of-scope areas. A passing test suite or scan is evidence about those checks only, not a security guarantee.

## References

- [OWASP Application Security Verification Standard](https://owasp.org/www-project-application-security-verification-standard/)
- [OWASP Web Top 10:2025](https://top10.owasp.org/2025/)
- [OWASP API Security Top 10:2023](https://api-security.owasp.org/editions/2023/en/0x00-header/)
- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
- [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
- [OWASP Threat Modeling Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html)
