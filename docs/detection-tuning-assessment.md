# SOC-004 Detection Tuning Assessment

## Project identity
SOC-004 — Windows Identity & Authentication Detection Engineering & Investigation
Subtitle: Kerberos authentication telemetry, Event ID 4771, detection validation and evidence-driven investigation.

## Proposed consistent specification (lab guidance only)

- Correlation A (source-based): Same source IP → >=3 distinct TargetUserName values → within 5 minutes. (PROPOSED ENGINEERING HYPOTHESIS ONLY / NOT VALIDATED / DISABLED)
- Correlation B (account-based): Same TargetUserName → >=3 Event ID 4771 failures → within 5 minutes. (PROPOSED ENGINEERING HYPOTHESIS ONLY / NOT VALIDATED / DISABLED)

These thresholds were not empirically validated by the available dataset and must not be interpreted as industry-standard or production-validated thresholds. Correlation rules remain DISABLED until empirically validated with sufficient telemetry.

## Detection status

- EventID 4771 single-event detection: VALIDATED (validated rule match)
- Single 4771 severity: LOW / LOG-ONLY
- Correlation A: PROPOSED / NOT VALIDATED / DISABLED / EXPERIMENTAL
- Correlation B: PROPOSED / NOT VALIDATED / DISABLED / EXPERIMENTAL
- 4740 escalation: PROPOSED / NOT VALIDATED / DISABLED / EXPERIMENTAL
- T1110 (Brute Force): CANDIDATE ONLY / NOT CONFIRMED
- Detection status: EXPERIMENTAL

## Evidence preserved

- Validated detection/rule match: SOC004-Test01 4771 at 22:36:10, Failure Code 0x18, source 192.168.56.102 (SOC002-WS001), MachineName SOC002-DC1.soc002.local (SOC-004-detection-validation.txt). The rule correctly matched the observed event; activity was BENIGN / EXPECTED (controlled telemetry-validation exercise).
- Audit-policy dependency: the audit-policy change enabled failure auditing for Kerberos Authentication Service, allowing the subsequent Kerberos pre-authentication failure to be recorded as Event ID 4771 (SOC-004-audit-policy-change.txt).
- Telemetry gaps: 4625/4776 not observed for test accounts in queried windows; NTLM/SMB telemetry blind spot; WS001 endpoint telemetry limited/partially inspected (SOC-004-telemetry-gap-analysis.txt).
- Controlled scenario: three failed attempts at 22:29:54, 22:30:13, 22:30:16 (client-side) with no corresponding 4625/4771/4776 for test accounts observed in queried windows; successful Test01 Kerberos auth at 22:30:11/22:30:55 (SOC-004-controlled-auth-scenario.txt, SOC-004-evidence-review.txt).
- Single-event analytic matches the validated 4771 (SOC-004-detection-specification.yml, SOC-004-detection-engineering.md, SOC-004-detection-validation.txt).

## Limitations

- Single event is informational only; does not establish brute force, spraying, credential compromise, or persistence.
- Correlation thresholds are proposed engineering hypotheses only and are NOT VALIDATED by existing telemetry; remain DISABLED/EXPERIMENTAL.
- No new evidence generated; no new authentication activity performed.
- Evidence constraints: only one 4771 exists (22:36:10) from controlled post-policy validation; earlier failed attempts did not produce 4771 in queried windows.
- Audit-policy dependency and NTLM/SMB telemetry blind spot preserved.
