# Rockies Rebuild 2024 System

This repository is for the Rockies Rebuild 2024 system.

## Project Goal

Build Airtable-ready workflow tables for:

- Colorado Rockies Base
- Player Development
- Daily Baseball
- Hitter Zone Reports
- Pitcher Zone Reports
- Acquisitions
- Front Office/Financials

## Core Linking Rules

- Use **Player ID** as the permanent, stable identifier for every player.
- Use **Player Name** as the readable linked-record field.
- Connect each workflow table to the main **Colorado Rockies Base** player table.
- Use lookup fields to bring baseline player information into workflow tables.
- Do not manually duplicate baseline information when a lookup field can provide it.

Baseline lookup information may include:

- Player ID
- Player Name
- Position
- Organization level
- Age
- Bats
- Throws
- Overall rating
- Potential
- Contract status
- Forty-man roster status
- Organizational role

## Player Development Rules

The Player Development table should maintain one current record per player.

It should accept information from:

- Complete Game Analysis Reports
- Hitter Zone Reports
- Pitcher Zone Reports
- Acquisitions and roster-change reports
- Foundation reports

Latest-game information should overwrite the previous current-status fields instead of creating duplicate player records.

Historical game reports should remain stored in their own report tables so earlier analysis is not lost.

Current Player Development fields may include:

- Current development trend
- Trend direction
- Latest-game status
- Recent positives
- Recent negatives
- Development advice
- Organizational role
- Rebuild fit
- Recommendation
- Watch status
- Acquisition impact
- Roster impact
- Last updated date
- Latest source report
- Latest game number

## Report and History Rules

- Keep permanent baseline information in the Colorado Rockies Base.
- Keep current player evaluations in Player Development.
- Keep individual game records and reports in the appropriate report tables.
- Link reports to players through Player ID and Player Name.
- Preserve historical reports rather than overwriting them.
- Only current-status fields in Player Development should be overwritten by the newest qualified report.

## Project Plan

The first schema-first project plan is documented in [`docs/project-plan.md`](docs/project-plan.md). It covers the recommended repository folder structure, Airtable table list, field names, field types, links, lookups, Player Development overwrite fields, and historical records that should not be overwritten.

## Initial Codex Assignment

Start by providing:

1. The recommended repository folder structure.
2. The complete Airtable table list.
3. The field names for each table.
4. The Airtable field type for every field.
5. The links required between tables.
6. Which fields should be lookups.
7. Which Player Development fields should be overwritten by the latest report.
8. Which information should remain historical.

Do not write code yet.

Explain the proposed structure first and wait for approval before creating import templates, formulas, automations, scripts, or other code.
