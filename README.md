# SOC-004 — Windows Identity & Authentication Detection Engineering & Investigation

An evidence-driven Windows/Active Directory security investigation focused on Kerberos authentication telemetry, Event ID 4771, authentication baselining, detection validation, cross-event correlation and conservative ATT&CK assessment.

## Overview

The investigation examined authentication telemetry in a Windows Server 2022 Active Directory environment (`soc002.local`) consisting of DC1 (domain controller/DNS) and WS001 (domain-joined workstation). The work focused on Kerberos pre-authentication failures (Event ID 4771) and supporting authentication events to evaluate what the telemetry can and cannot establish under controlled conditions.

## What I Investigated

Can authentication telemetry identify and support investigation of Kerberos pre-authentication failures, and what additional evidence would be required before treating such activity as suspicious?

The project evaluates the limits of the telemetry rather than assuming malicious activity.

## Environment

| Component | Role |
|---|---|
| SOC002-DC1 | Domain Controller / DNS |
| SOC002-WS001 | Domain-joined workstation |
| Domain | soc002.local |
| Network | 192.168.56.0/24 |
| Telemetry | Windows Security Events |

## Telemetry

Relevant Windows Security events used in this investigation:

- 4624 — Successful logon
- 4625 — Failed logon
- 4768 — Kerberos TGT requested
- 4769 — Kerberos service ticket requested
- 4771 — Kerberos pre-authentication failure
- 4776 — Credential validation
- 4740 — Account lockout

Not every event produced useful evidence for this case.

## Detection

### Validated detection
- **Event ID 4771 — Kerberos pre-authentication failure.** The single-event rule was validated against an observed 4771 event for `SOC004-Test01` (Failure Code 0x18) at 22:36:10 from source `192.168.56.102` (SOC002-WS001). This constitutes a **validated detection/rule match**. The underlying activity was **BENIGN / EXPECTED** (authorized controlled telemetry-validation exercise).

### Proposed correlation hypotheses
- **Correlation A:** Same source IP → >=3 distinct TargetUserName values → within 5 minutes  
- **Correlation B:** Same TargetUserName → >=3 Event ID 4771 failures → within 5 minutes

These thresholds are **proposed engineering hypotheses only**. They were **not empirically validated** by the available dataset and remain **disabled/experimental**; they must not be interpreted as production-ready or industry-standard thresholds.

## Investigation

Key observations:

- Controlled authentication activity using dedicated lab identities (`SOC004-Test01/02/03`) from SOC002-WS001.
- Validated 4771 for `SOC004-Test01` at 22:36:10 (Failure Code 0x18) from `192.168.56.102` (SOC002-WS001).
- Audit-policy change enabled failure auditing for Kerberos Authentication Service, allowing the subsequent Kerberos pre-authentication failure to be recorded as Event ID 4771.
- No successful `SOC004-Test01` authentication after the observed 4771.
- No evidence established brute force, password spraying, credential compromise, or malicious post-authentication activity.
- **Telemetry limitations:** NTLM/SMB visibility blind spot (earlier failed attempts at 22:29:54/22:30:13/22:30:16 did not produce 4625/4771/4776 for test accounts in queried windows); endpoint telemetry (WS001) was limited/partially inspected; audit-policy dependency; dataset was too small to validate correlation thresholds.

## ATT&CK

**T1110 — Brute Force** — Status: **Candidate only / Not confirmed**

T1110 was considered as a possible technique, but the available evidence was insufficient to confirm it. No indicators supporting confirmed brute force activity were observed.

## Detection Limitations

- Kerberos failure visibility depended on audit-policy configuration (failure auditing for Kerberos Authentication Service).
- NTLM/SMB authentication failures were not reliably represented as 4771 in the observed windows.
- Available dataset was too small to empirically validate correlation thresholds.
- Endpoint telemetry (WS001) was limited for the relevant investigation window.
- A single Event ID 4771 does not establish password spraying, brute force, credential compromise, or persistence.

## Final Disposition

**BENIGN / EXPECTED — CLOSED**

The observed 4771 event was generated during an authorized controlled telemetry-validation exercise. The evidence did not support malicious activity, including brute force, password spraying, credential compromise, or malicious post-authentication activity.

## What This Project Shows

- Windows authentication log analysis
- Active Directory identity investigation
- Kerberos telemetry analysis
- Detection validation (validated rule match)
- Authentication baselining
- Cross-event correlation
- False-positive considerations
- Detection tuning
- Conservative ATT&CK assessment
- Evidence-based analyst disposition

## Documentation

- [Investigation Report](docs/investigation-report.md)
- [Detection Engineering](docs/detection-engineering.md)
- [Detection Tuning Assessment](docs/detection-tuning-assessment.md)
- [Detection Specification](detection/detection-specification.yml)
- [Evidence Index](docs/evidence-index.md)
