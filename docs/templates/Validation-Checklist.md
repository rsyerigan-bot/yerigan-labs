# Validation Checklist — [LAB / PROJECT ID]

**System / Change:** [Name]
**Related Ticket:** `[path]`
**Validator:** [Name / role]
**Date:** [YYYY-MM-DD]

---

## Purpose

Provide a repeatable validation record showing that an engineering change meets its intended requirements without introducing obvious operational or security regressions.

Not every validation item applies to every project. Mark non-applicable items as **N/A** rather than manufacturing unnecessary tests.

---

## Pre-Change Validation

- [ ] Scope confirmed
- [ ] Intended change understood
- [ ] Current state recorded where necessary
- [ ] Dependencies identified
- [ ] Recovery or rollback method identified where applicable
- [ ] Sensitive data / secrets considerations reviewed
- [ ] Expected result defined before implementation

---

## Functional Validation

| Test | Expected Result | Actual Result | Pass / Fail / N/A |
|---|---|---|---|
| [Test] | [Expected] | [Actual] | [Result] |
| [Test] | [Expected] | [Actual] | [Result] |

---

## Security Validation

- [ ] Authentication / authorization impact reviewed
- [ ] Network exposure reviewed
- [ ] Sensitive information exposure reviewed
- [ ] Least-privilege considerations reviewed
- [ ] Logging / audit impact reviewed where applicable
- [ ] New credentials or secrets handled appropriately
- [ ] Security controls independently validated where practical

---

## Operational Validation

- [ ] Service health verified
- [ ] Dependencies remain functional
- [ ] Monitoring impact reviewed where applicable
- [ ] Logging impact reviewed where applicable
- [ ] User / family impact reviewed where applicable
- [ ] Restart / reboot behavior considered where applicable
- [ ] Recovery procedure considered or tested where appropriate

---

## Independent Verification

Record commands, dashboards, logs, application checks, or other evidence used to verify the result independently.

Examples:

    systemctl status <service>
    docker ps
    ss -tulpn
    curl <endpoint>

Use only commands appropriate to the system being validated.

---

## Regression Check

Confirm that unrelated functionality was not unintentionally disrupted.

| Check | Result | Notes |
|---|---|---|
| Existing services | [Pass / Fail / N/A] | |
| Network access | [Pass / Fail / N/A] | |
| Authentication | [Pass / Fail / N/A] | |
| Monitoring | [Pass / Fail / N/A] | |
| Logging | [Pass / Fail / N/A] | |

---

## Deferred Items

Document anything discovered during validation that is intentionally deferred.

- [ ] [Deferred item / N/A]

Deferred work should be recorded in the appropriate ticket, roadmap, or documentation-debt register when necessary.

---

## Final Result

**Validation Status:** [PASS / PASS WITH DEFERRED ITEMS / FAIL]

**Summary:**
[Briefly state whether the intended change was successfully validated and identify any important limitations.]

---

## Evidence Integrity

Validation records should describe what was actually observed.

Do not convert assumptions into successful tests. If a control or capability was not tested, record it as **Not Tested**, **Deferred**, or **N/A** as appropriate.
