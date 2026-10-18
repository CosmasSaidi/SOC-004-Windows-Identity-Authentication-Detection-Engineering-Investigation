# SOC-004 Final Investigation Report

## Project identity
SOC-004 — Windows Identity & Authentication Detection Engineering & Investigation
Subtitle: Kerberos authentication telemetry, Event ID 4771, detection validation and evidence-driven investigation.

## Final Disposition
BENIGN / EXPECTED — CLOSED

## Investigation summary
Evidence-driven Windows/Active Directory investigation focused on Kerberos authentication telemetry, Event ID 4771, authentication baselining, detection validation, cross-event correlation and conservative ATT&CK assessment. Evaluates what authentication telemetry can and cannot establish, including audit-policy dependencies and NTLM/SMB visibility gaps. Correlation thresholds are treated as engineering hypotheses rather than validated production detections.

Controlled test scenario using dedicated SOC004 test accounts (Test01/02/03) from SOC002-WS001 against SOC002-DC1. Audit policy change enabled failure auditing for Kerberos Authentication Service, allowing the subsequent Kerberos pre-authentication failure to be recorded as Event ID 4771 at 22:36:10. After policy change, a controlled failed authentication for SOC004-Test01 produced a validated Event ID 4771 (Kerberos pre-authentication failed) at 22:36:10 with Failure Code 0x18, source 192.168.56.102 (SOC002-WS001).

## Key findings (preserved)

- 4771 at 22:36:10: SOC004-Test01, source 192.168.56.102 / SOC002-WS001, Failure Code 0x18, MachineName SOC002-DC1.soc002.local (validated rule match).
- SOC004-Test01 successful Kerberos auth represented by 4768/4769/4624 from 192.168.56.102 at ~22:30:11; SYSVOL access succeeded at 22:30:55.
- audit policy change at 22:35:09: enabled failure auditing for Kerberos Authentication Service (and Credential Validation Failure), allowing the subsequent Kerberos pre-authentication failure to be recorded as Event ID 4771.
- controlled-test context: dedicated test accounts, known source IP (192.168.56.102), known workstation (SOC002-WS001).
- no successful Test01 authentication after the 4771 (validated 4771 is the post-policy controlled failed attempt; successful Test01 auth occurred earlier at 22:30:11 as part of scenario; no successful Test01 auth observed after 22:36:10 in collected windows).
- no evidence establishing brute force, spraying, credential compromise, or persistence.
- telemetry limitations: NTLM/SMB telemetry blind spot (earlier three failed attempts 22:29:54, 22:30:13, 22:30:16 did not produce 4625/4771/4776 for test accounts in queried windows; WS001 endpoint telemetry limited/partially inspected; audit-policy dependency; only one 4771 exists in available data).

## ATT&CK assessment
T1110 — Brute Force: CANDIDATE ONLY / NOT CONFIRMED. Considered as a possible technique but not confirmed by available evidence.

## Detection/validation (evidence)
- Telemetry validation (post-policy): 4771 observed for SOC004-Test01 at 22:36:10 from 192.168.56.102 — validated detection/rule match (SOC-004-telemetry-validation.txt). The rule correctly matched the observed event; underlying activity was BENIGN / EXPECTED (controlled telemetry-validation exercise).
- Single-event detection validated as rule match (SOC-004-detection-validation.txt).
- Detection engineering: single-event LOW/LOG-ONLY; correlation thresholds proposed engineering hypotheses only (NOT VALIDATED / DISABLED / EXPERIMENTAL) (SOC-004-detection-engineering.md, SOC-004-detection-specification.yml).

## Conclusion
Activity is BENIGN / EXPECTED under controlled test conditions. No indicators of attack confirmed (no brute force, password spraying, credential compromise, or malicious post-authentication activity). Case CLOSED.

## Evidence references (existing artifacts)
- SOC-004-audit-policy-change.txt
- SOC-004-controlled-auth-scenario.txt
- SOC-004-detection-engineering.md, SOC-004-detection-specification.yml, SOC-004-detection-validation.txt
- SOC-004-telemetry-validation.txt, SOC-004-telemetry-gap-analysis.txt
- SOC-004-evidence-review.txt
- SOC-004-normal-ws001-auth.txt, SOC-004-baseline*.txt, SOC-004-test-identities.txt
