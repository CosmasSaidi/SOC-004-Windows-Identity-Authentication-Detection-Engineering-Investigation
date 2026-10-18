# SOC-004 Evidence Index

| filename | purpose | category | validation status |
|---|---|---|---|
| SOC-004-account-candidates.txt | Candidate account names identified for test identities | Identity/Enumeration | CANDIDATE / NOT VALIDATED |
| SOC-004-audit-policy-change.txt | Record of audit policy changes (failure auditing enabled for Kerberos Authentication Service/Credential Validation) at 22:35:09 | Policy/Audit | OBSERVED / NOT VALIDATED by telemetry dataset alone |
| SOC-004-baseline-contamination-analysis.txt | Analysis of potential baseline contamination | Baseline | CANDIDATE / INFERRED |
| SOC-004-baseline.txt | General baseline | Baseline | OBSERVED |
| SOC-004-clean-precollection-baseline.txt | Clean pre-collection baseline | Baseline | OBSERVED |
| SOC-004-controlled-auth-scenario.txt | Controlled authentication scenario report (Phase A-D, timestamps, outcomes) | Scenario/Evidence | OBSERVED (client-side actions); DC1 representation gaps noted; NTLM/SMB blind spot |
| SOC-004-detection-engineering.md | Detection engineering doc (single-event + correlation engineering hypotheses, ATT&CK T1110 CANDIDATE ONLY) | Detection | EXPERIMENTAL; correlation thresholds PROPOSED/NOT VALIDATED/DISABLED |
| SOC-004-detection-specification.yml | Detection specification (single-event + correlation analytic structure; T1110 marked candidate/confirmed=false) | Detection | EXPERIMENTAL; correlation conditions PROPOSED/NOT VALIDATED/DISABLED |
| SOC-004-detection-tuning-assessment.md | Tuning assessment with consistent proposed engineering hypotheses (>=3); detection status | Tuning/QC | PROPOSED / NOT VALIDATED / DISABLED / EXPERIMENTAL |
| SOC-004-detection-validation.txt | Detection validation confirming validated detection/rule match for single-event 4771 | Validation | VALIDATED (validated rule match; correlation NOT VALIDATED) |
| SOC-004-evidence-review.txt | Forensic evidence review of failed/successful auth, correlation, gaps, audit-policy dependency, NTLM/SMB blind spot | Evidence Review | OBSERVED; INFERRED/NOT VERIFIED clearly distinguished |
| SOC-004-final-investigation-report.md | Final investigation report: BENIGN / EXPECTED — CLOSED; evidence-driven, conservative ATT&CK, limitations documented | Report | FINAL (disposition preserved) |
| SOC-004-identity-baseline-analysis.txt | Identity baseline analysis; lists candidates/inferences; conservative | Identity/Baseline | CANDIDATE / INFERRED / NOT VERIFIED |
| SOC-004-normal-ws001-auth.txt | Normal WS001 authentication telemetry (baseline) | Baseline/Telemetry | OBSERVED |
| SOC-004-precollection-baseline.txt | Pre-collection baseline | Baseline | OBSERVED |
| SOC-004-pre-scenario-check.txt | Pre-scenario checks | Validation/Control | OBSERVED |
| SOC-004-rich-auth-baseline.txt | Rich auth baseline | Baseline | OBSERVED |
| SOC-004-telemetry-gap-analysis.txt | Telemetry gap analysis: NTLM/SMB blind spot, audit-policy dependency, endpoint telemetry limitations; missing 4625/4776/4771 for earlier attempts | Telemetry/Gaps | OBSERVED; INFERRED/NOT VERIFIED distinguished |
| SOC-004-telemetry-validation.txt | Post-policy telemetry validation confirming validated detection/rule match for SOC004-Test01 4771 at 22:36:10 | Validation | VALIDATED (validated rule match) |
| SOC-004-test-identities.txt | Test identities definition (controlled) | Identity/Control | CONTROLLED |
