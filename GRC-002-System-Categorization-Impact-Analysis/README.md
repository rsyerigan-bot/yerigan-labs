# GRC-002 — System Categorization and Impact Analysis

## Objective

Evaluate the confidentiality, integrity, and availability requirements of the Yerigan Labs Cyber Range and develop a documented system-impact categorization based on the consequences of loss.

## Learning Goal

Understand how system security requirements are driven by mission, information types, system purpose, and the potential consequences of losing confidentiality, integrity, or availability.

The purpose of this lab is not to assign impact ratings from intuition.

Each rating must be supported by a documented rationale tied to the actual system defined in GRC-001.

## Prerequisite

GRC-001 — System Boundary and Asset Inventory

GRC-001 established:

- the system boundary
- in-scope assets
- external dependencies
- administrative interfaces
- internal and external data flows
- network-isolation evidence

GRC-002 uses that defined system as the basis for impact analysis.

## System

**System Name:** Yerigan Labs Cyber Range

**System Purpose:**
Provide an isolated and authorized environment for cybersecurity training, network reconnaissance, vulnerability validation, and controlled exploitation.

## Analysis Areas

The assessment will examine potential loss of:

- confidentiality
- integrity
- availability

The analysis will consider both information processed by the system and the operational consequences of compromise.

## Deliverables

- information-type inventory
- confidentiality impact analysis
- integrity impact analysis
- availability impact analysis
- proposed system categorization
- rationale for each impact determination
- assumptions and limitations

## Rules

Impact ratings will not be assigned until:

1. relevant information types are identified,
2. potential consequences are described,
3. assumptions are documented, and
4. the reasoning can be defended from evidence.

## Status

Complete.

The cyber-range information types and potential consequences of confidentiality, integrity, and availability loss were evaluated.

Proposed categorization:

- **Confidentiality:** LOW
- **Integrity:** LOW
- **Availability:** LOW

Overall system impact: **LOW**

Integrity was identified as the highest operational priority, while remaining Low impact based on the limited consequences associated with a recoverable, non-production training environment.
