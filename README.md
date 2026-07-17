# Rockies Rebuild 2024 System

This repository defines the Airtable-ready workflow system for the Rockies Rebuild 2024 project.

## Workflow tables

### Seven main workflow tables

1. Colorado Rockies Base
2. Player Development
3. Daily Baseball
4. Hitter Zone Reports
5. Pitcher Zone Reports
6. Acquisitions
7. Front Office/Financials

### Supporting evidence table

8. Source Reports

Source Reports preserves the evidence, coverage, routing, correction, and version history behind the seven decision-making workflows.

## Permanent evidence rule

> Do not invent missing details or Player IDs.

- Store only supported information.
- Mark missing, incomplete, partial, result-only, pending, and unresolved information explicitly.
- Never infer or generate a Player ID.
- Preserve earlier evidence when a correction creates a new version.

## Core data model

- Colorado Rockies Base is the verified player source of truth.
- Player ID is the stable player key.
- Player Name is the readable linked-record field.
- Every table uses a directly editable record ID as its Airtable primary field.
- Downstream player fields link to Colorado Rockies Base and obtain Player ID and baseline details through lookups.
- A missing Player ID stops the player link and any automatic Player Development update.
- Player Development maintains one current record per verified player.
- Daily Baseball, zone reports, acquisitions, financial records, and source reports remain historical and additive.

## Player Development ownership

Each workflow may update only the current fields it owns:

- Daily Baseball owns latest-game evidence.
- Hitter Zone Reports owns hitter-trend evidence.
- Pitcher Zone Reports owns pitcher-trend evidence.
- Acquisitions owns acquisition and transaction impact.
- Front Office/Financials owns financial and control impact.
- Qualified Player Development synthesis owns final trend, advice, organizational role, rebuild fit, and Watch Status.
- Colorado Rockies Base owns permanent identity and verified baseline information.

Blank, missing, incomplete, or no-grade input never erases supported current evidence.

## Correction model

Corrections are immutable versions:

1. Preserve the earlier record.
2. Create the next VNN record.
3. Link the new record through Supersedes Record.
4. Record the exact correction.
5. Mark the earlier record Superseded.
6. Never delete or silently rewrite historical evidence.

## Current phase

Phase 1 approved and merged the required-field-first project plan.

Phase 2 creates documentation for the required Airtable fields and links and validates the design with Spring Games 1–7. It does not create CSV templates, formulas, automations, scripts, dashboards, or interfaces.

## Documentation

- [Project plan](docs/project-plan.md) — approved scope, record ID formats, table schemas, and Spring Games 1–7 validation matrix
- [Required field definitions](docs/field-definitions.md) — exact Phase 2 fields, Airtable types, and controlled choices
- [Linking rules](docs/linking-rules.md) — link direction, cardinality, reciprocal backlinks, and lookups
- [Overwrite rules](docs/overwrite-rules.md) — source ownership, qualification gates, corrections, and Player Development updates
- [Airtable setup guide](docs/airtable-setup-guide.md) — step-by-step required-field build and validation procedure

## Approval gates

1. Review and approve the Phase 2 documentation.
2. Create only the required Airtable fields and links.
3. Validate with Spring Games 1–7.
4. Record defects and required schema changes.
5. Approve the validated required schema.
6. Add optional fields and build templates, formulas, automations, scripts, dashboards, or interfaces only in later, separately reviewed phases.
