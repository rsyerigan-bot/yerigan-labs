# ENG-006 — Infrastructure as Code Migration

## Context

During LAB-006 we migrated Docker service definitions from the live server into the Yerigan Labs repository.

The objective was to establish Git as the source of truth for infrastructure configuration while separating configuration from runtime state.

---

## Challenges

Initially it was tempting to simply copy the Docker directory into Git.

After reviewing the directory structure we recognized that runtime data, databases, logs, secrets and application state should remain outside version control.

Determining what belonged in Git required identifying the difference between configuration and state.

---

## Decisions

Configuration files (Docker Compose files, Caddyfile, Homepage configuration) belong in Git.

Runtime state (databases, logs, certificates, Home Assistant database, Docker volumes) remains on the server and is backed up independently.

Sensitive values are represented using .env.example files while actual secrets remain local.

---

## Observations

Infrastructure becomes significantly easier to reason about when configuration and state are intentionally separated.

Git should describe the desired environment rather than capture its runtime condition.

---

## Engineering Principles Reinforced

- Configuration Is Intentional; State Is a Consequence
- Evidence Over Assumptions
- Keep It Simple

---

## Future Improvements

Automate deployment directly from the repository.

Create backup and restore documentation for runtime state.

Expand Infrastructure as Code to additional services.