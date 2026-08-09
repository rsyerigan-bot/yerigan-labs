# Yerigan Labs Engineering Principles

## Purpose

Capture reusable engineering principles discovered through Yerigan Labs work.

These principles should guide technical design, troubleshooting, documentation, operations, and future client work.

---

## EP-001 — Keep It Simple

Prefer the simplest design that safely satisfies the requirement.

Complexity must justify itself.

---

## EP-002 — Verify Before Changing

Gather evidence and validate assumptions before making permanent changes.

---

## EP-003 — Evidence Over Assumptions

Troubleshooting decisions should follow observed behavior, logs, tests, and system state rather than expectation.

---

## EP-004 — Build for Modularity

Design systems so components can be modified, replaced, or repaired independently whenever practical.

---

## EP-005 — Configuration Is Intentional; State Is a Consequence

Configuration describes how a system should operate.

State is created as the system runs.

Configuration belongs in version control where appropriate; runtime state belongs in backups and operational storage.

---

## EP-006 — Automate Outcomes, Not Hardware

Automations should describe the desired function or outcome rather than depend unnecessarily on a specific vendor or device.

---

## EP-007 — Critical Automations Require Local Fallbacks

Safety- or security-critical functions should not depend exclusively on cloud connectivity or a single notification path.

---

## EP-008 — Verify the Interface Before Troubleshooting the Implementation

Confirm that requests are being sent to the correct service, action, host, port, or endpoint before assuming the underlying system is broken.

---

## EP-009 — Raw Signals Are Evidence, Not Decisions

Individual sensors, trackers, and events should be treated as evidence.

Higher-confidence decisions should correlate multiple signals when the consequence warrants it.

---

## EP-010 — Leave Every Environment Better Than You Found It

Engineering work should improve reliability, clarity, security, documentation, or maintainability rather than merely accomplish the immediate task.
