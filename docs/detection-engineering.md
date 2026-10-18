# SOC-004 Detection Engineering

## 1. Detection objective
Detect Kerberos pre-authentication failures (Event ID 4771) for investigation, with single-event and correlation-level analytics. No automatic attack classification.

## 2. Telemetry source
DC1 Windows Security log, Event ID 4771 (Kerberos Authentication Service failures), enabled after audit policy change (Success and Failure).

## 3. Validated event
SOC004-Test01, Failure Code 0x18, source IP 192.168.56.102 (WS001), TimeCreated 22:36:10, RecordId not captured in filtered extraction.

## 4. Single-event analytic
Match EventID=4771. Severity low/informational. One 4771 does not establish password spraying or compromise.

## 5. Correlation analytic
Correlation A: Same source IP -> >=3 distinct TargetUserName values -> within 5 minutes (PROPOSED ENGINEERING HYPOTHESIS ONLY / NOT VALIDATED / DISABLED)
Correlation B: Same TargetUserName -> >=3 Event ID 4771 failures -> within 5 minutes (PROPOSED ENGINEERING HYPOTHESIS ONLY / NOT VALIDATED / DISABLED)

Alert text: "Repeated Kerberos authentication failures requiring investigation." (hypothetical)

These thresholds were not empirically validated by the available dataset and must not be interpreted as industry-standard or production-validated thresholds. Correlation rules are DISABLED until empirically validated with sufficient telemetry.

## 6. Threshold rationale
Engineering hypotheses for SOC triage only. Require empirical validation with sufficient baseline telemetry before enabling.

## 7. False positives
Incorrect password entry, cached credential mismatch, clock skew, service misconfiguration, shared source IPs (NAT, terminal server).

## 8. Investigation pivots
TargetUserName, IpAddress, WorkstationName, FailureCode, TimeCreated, source IP/account correlation, 4624/4768/4769 surrounding events.

## 9. ATT&CK mapping
T1110 — Brute Force (Status: CANDIDATE ONLY / NOT CONFIRMED). T1110 was considered as a possible technique but is not confirmed by the available evidence.

## 10. Detection limitations
Single event is informational. Correlation thresholds are proposed engineering hypotheses only and are NOT VALIDATED by the available dataset; they remain DISABLED/EXPERIMENTAL. NTLM/SMB telemetry blind spot exists (earlier failed attempts at 22:29:54/22:30:13/22:30:16 did not produce 4625/4771/4776 for test accounts in queried windows). Endpoint telemetry (WS001) limited/partially inspected. Audit-policy dependency (failure auditing for Kerberos Authentication Service required). Small 24h baseline.

## 11. Validation evidence
Validated detection/rule match: 4771 event for SOC004-Test01 at 22:36:10 from 192.168.56.102, Failure Code 0x18. Single-event analytic correctly matched the observed event. The underlying activity was BENIGN / EXPECTED (controlled telemetry-validation exercise). Correlation thresholds not empirically validated.

## 12. What the detection does NOT prove
Does not prove password spraying, brute force, credential compromise, or persistence. Requires additional evidence and analyst review. A single 4771 is an investigation trigger only.
