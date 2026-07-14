# Rockies Rebuild 2024 Project Plan

This plan defines the first Airtable-ready structure for the Rockies Rebuild 2024 system. It is intentionally schema-first: no formulas, automations, scripts, or import templates should be built until these tables, links, lookup fields, and overwrite rules are approved.

## Recommended Repository Folder Structure

```text
Rockies-Rebuild--2024-System/
├── README.md
├── docs/
│   ├── system-overview.md
│   ├── project-plan.md
│   ├── airtable-schema.md
│   ├── linking-rules.md
│   ├── player-development-rules.md
│   └── report-history-rules.md
├── airtable/
│   ├── tables/
│   │   ├── colorado-rockies-base.md
│   │   ├── player-development.md
│   │   ├── daily-baseball.md
│   │   ├── hitter-zone-reports.md
│   │   ├── pitcher-zone-reports.md
│   │   ├── acquisitions.md
│   │   └── front-office-financials.md
│   ├── field-maps/
│   │   ├── base-field-map.md
│   │   ├── development-field-map.md
│   │   ├── lookup-field-map.md
│   │   └── report-field-map.md
│   ├── import-templates/
│   └── automations/
├── reports/
│   ├── foundation/
│   ├── complete-game-analysis/
│   ├── hitter-zone/
│   ├── pitcher-zone/
│   └── acquisitions/
└── exports/
    ├── csv/
    └── airtable-schema/
```

## Airtable Table List

1. Colorado Rockies Base
2. Player Development
3. Daily Baseball
4. Hitter Zone Reports
5. Pitcher Zone Reports
6. Acquisitions
7. Front Office/Financials

## Core Linking Model

- **Player ID** is the stable player key across every table.
- **Player Name** is the readable linked-record display field.
- **Colorado Rockies Base** is the source of truth for permanent player identity, roster, role, contract, and baseline scouting information.
- Every player-centered workflow table links back to **Colorado Rockies Base**.
- Workflow tables should use lookup fields for baseline information instead of manually duplicating it.
- Report tables preserve historical records. They should not overwrite old reports.
- **Player Development** keeps one current record per player and receives latest-game overwrite updates from qualified reports.

## Table Schemas

### 1. Colorado Rockies Base

Primary purpose: permanent player master table and baseline information source.

| Field name | Airtable field type | Notes |
| --- | --- | --- |
| Player Name | Single line text, primary field | Readable player name. |
| Player ID | Single line text | Stable unique player key. Required for linking and imports. |
| Organization | Single select | MLB organization, default Colorado Rockies when applicable. |
| Level | Single select | MLB, AAA, AA, High-A, Low-A, Complex, DSL, injured list, free agent, target, etc. |
| Position | Single select | Primary defensive position. |
| Secondary Positions | Multiple select | Optional defensive flexibility. |
| Age | Number | Baseline age. |
| Bats | Single select | R, L, S. |
| Throws | Single select | R, L. |
| Overall Rating | Number | Current overall evaluation. |
| Potential | Number | Future projection. |
| Organizational Role | Single select | Core, regular, depth, prospect, lottery ticket, trade target, DFA risk, etc. |
| Contract Status | Single select | Pre-arb, arbitration, guaranteed, minor league, free agent, etc. |
| Forty-Man Status | Checkbox | Whether the player is on the 40-man roster. |
| Service/Control Notes | Long text | Contract control context. |
| Acquisition Source | Single select | Draft, international, trade, waiver, free agency, Rule 5, inherited, etc. |
| Acquisition Date | Date | Date player entered the organization. |
| Current Roster Status | Single select | Active, injured, optioned, suspended, released, target, etc. |
| Rebuild Fit Baseline | Single select | Long-term fit, short-term bridge, trade chip, depth, watch, no fit. |
| Baseline Notes | Long text | Static scouting and organizational notes. |
| Player Development Link | Link to another record | Links to Player Development. |
| Daily Baseball Links | Link to another record | Links to Daily Baseball records. |
| Hitter Zone Report Links | Link to another record | Links to Hitter Zone Reports. |
| Pitcher Zone Report Links | Link to another record | Links to Pitcher Zone Reports. |
| Acquisition Links | Link to another record | Links to Acquisitions. |
| Financial Links | Link to another record | Links to Front Office/Financials. |

### 2. Player Development

Primary purpose: one current development record per player, updated by the latest qualified game/report inputs.

| Field name | Airtable field type | Notes |
| --- | --- | --- |
| Player Name | Link to another record | Link to Colorado Rockies Base; readable linked field. |
| Player ID | Lookup | Lookup from Colorado Rockies Base. |
| Position | Lookup | Lookup from Colorado Rockies Base. |
| Level | Lookup | Lookup from Colorado Rockies Base. |
| Age | Lookup | Lookup from Colorado Rockies Base. |
| Bats | Lookup | Lookup from Colorado Rockies Base. |
| Throws | Lookup | Lookup from Colorado Rockies Base. |
| Overall Rating | Lookup | Lookup from Colorado Rockies Base. |
| Potential | Lookup | Lookup from Colorado Rockies Base. |
| Contract Status | Lookup | Lookup from Colorado Rockies Base. |
| Forty-Man Status | Lookup | Lookup from Colorado Rockies Base. |
| Baseline Organizational Role | Lookup | Lookup from Colorado Rockies Base. |
| Current Development Trend | Single select | Overwritten by latest qualified report. |
| Trend Direction | Single select | Up, down, stable, volatile, unknown; overwritten by latest qualified report. |
| Latest-Game Status | Single select | Strong, acceptable, concern, injured, no data, etc.; overwritten. |
| Recent Positives | Long text | Overwritten summary from latest qualified report. |
| Recent Negatives | Long text | Overwritten summary from latest qualified report. |
| Development Advice | Long text | Overwritten current recommendation. |
| Current Organizational Role | Single select | Current role assessment, can differ from baseline. |
| Rebuild Fit | Single select | Current fit assessment. |
| Recommendation | Single select | Promote, hold, demote, acquire, sell high, monitor, release, etc. |
| Watch Status | Single select | Priority watch, normal watch, no watch, urgent. |
| Acquisition Impact | Long text | Current acquisition or roster impact summary. |
| Roster Impact | Long text | Current roster movement impact summary. |
| Last Updated Date | Date | Date latest overwrite was applied. |
| Latest Source Report | Single line text | Name or ID of report that supplied current fields. |
| Latest Game Number | Number | Game number from latest qualified input. |
| Linked Daily Baseball Records | Link to another record | Historical game records. |
| Linked Hitter Zone Reports | Link to another record | Historical hitter reports. |
| Linked Pitcher Zone Reports | Link to another record | Historical pitcher reports. |
| Linked Acquisition Records | Link to another record | Historical acquisition records. |

### 3. Daily Baseball

Primary purpose: game-by-game historical log for team and player observations.

| Field name | Airtable field type | Notes |
| --- | --- | --- |
| Game Record | Autonumber or single line text, primary field | Unique daily/game record label. |
| Game Date | Date | Date of game. |
| Game Number | Number | Season or save-file game number. |
| Opponent | Single line text | Opposing team. |
| Rockies Level | Single select | MLB, AAA, AA, etc. |
| Player Name | Link to another record | Link to Colorado Rockies Base when player-specific. |
| Player ID | Lookup | Lookup from Colorado Rockies Base. |
| Position | Lookup | Lookup from Colorado Rockies Base. |
| Level | Lookup | Lookup from Colorado Rockies Base. |
| Game Role | Single select | Starter, bench, reliever, opener, closer, pinch hitter, defensive replacement, etc. |
| Box Score Summary | Long text | Historical game summary. |
| Key Result | Single select | Positive, negative, mixed, neutral, injury, transaction. |
| Development Signal | Single select | Up, down, stable, no signal. |
| Player Development Link | Link to another record | Link to current Player Development record. |
| Source Report Type | Single select | Complete game analysis, daily note, manual observation, etc. |
| Source Notes | Long text | Historical source details. |

### 4. Hitter Zone Reports

Primary purpose: historical hitter-specific zone analysis reports.

| Field name | Airtable field type | Notes |
| --- | --- | --- |
| Hitter Report ID | Autonumber or single line text, primary field | Unique hitter report record. |
| Report Date | Date | Date of report. |
| Game Number | Number | Game number tied to report. |
| Player Name | Link to another record | Link to Colorado Rockies Base. |
| Player ID | Lookup | Lookup from Colorado Rockies Base. |
| Position | Lookup | Lookup from Colorado Rockies Base. |
| Level | Lookup | Lookup from Colorado Rockies Base. |
| Bats | Lookup | Lookup from Colorado Rockies Base. |
| Plate Discipline Trend | Single select | Improving, declining, stable, unknown. |
| Contact Quality Trend | Single select | Improving, declining, stable, unknown. |
| Power Trend | Single select | Improving, declining, stable, unknown. |
| Chase Zone Notes | Long text | Historical zone analysis. |
| Damage Zone Notes | Long text | Historical zone analysis. |
| Weakness Zone Notes | Long text | Historical zone analysis. |
| Adjustment Needed | Long text | Historical recommendation from this report. |
| Current Development Signal | Single select | Up, down, stable, volatile. |
| Player Development Link | Link to another record | Link to one current development record. |
| Should Update Player Development | Checkbox | Marks report as qualified to overwrite latest fields. |
| Source File/Report | Single line text | File, report name, or source identifier. |

### 5. Pitcher Zone Reports

Primary purpose: historical pitcher-specific zone and arsenal analysis reports.

| Field name | Airtable field type | Notes |
| --- | --- | --- |
| Pitcher Report ID | Autonumber or single line text, primary field | Unique pitcher report record. |
| Report Date | Date | Date of report. |
| Game Number | Number | Game number tied to report. |
| Player Name | Link to another record | Link to Colorado Rockies Base. |
| Player ID | Lookup | Lookup from Colorado Rockies Base. |
| Position | Lookup | Lookup from Colorado Rockies Base. |
| Level | Lookup | Lookup from Colorado Rockies Base. |
| Throws | Lookup | Lookup from Colorado Rockies Base. |
| Command Trend | Single select | Improving, declining, stable, unknown. |
| Stuff Trend | Single select | Improving, declining, stable, unknown. |
| Pitch Mix Trend | Single select | Improving, declining, stable, unknown. |
| Strike Zone Notes | Long text | Historical zone analysis. |
| Chase/Whiff Zone Notes | Long text | Historical zone analysis. |
| Damage Allowed Zone Notes | Long text | Historical zone analysis. |
| Arsenal Adjustment Needed | Long text | Historical recommendation from this report. |
| Current Development Signal | Single select | Up, down, stable, volatile. |
| Player Development Link | Link to another record | Link to one current development record. |
| Should Update Player Development | Checkbox | Marks report as qualified to overwrite latest fields. |
| Source File/Report | Single line text | File, report name, or source identifier. |

### 6. Acquisitions

Primary purpose: track targets, roster moves, acquired players, trade candidates, and transaction rationale.

| Field name | Airtable field type | Notes |
| --- | --- | --- |
| Acquisition Record | Autonumber or single line text, primary field | Unique acquisition/transaction record. |
| Player Name | Link to another record | Link to Colorado Rockies Base for known players. |
| Player ID | Lookup | Lookup from Colorado Rockies Base when linked. |
| Position | Lookup | Lookup from Colorado Rockies Base. |
| Age | Lookup | Lookup from Colorado Rockies Base. |
| Current Organization | Single line text | External org or Rockies level at time of move. |
| Acquisition Type | Single select | Trade, free agency, waiver, draft, international, Rule 5, extension, release, DFA, target. |
| Transaction Date | Date | Date of move or evaluation. |
| Acquisition Status | Single select | Target, active pursuit, acquired, passed, lost, completed, rejected. |
| Cost/Return | Long text | Assets sent or expected cost. |
| Scouting Summary | Long text | Player-specific acquisition evaluation. |
| Roster Fit | Single select | MLB help, prospect upside, depth, financial move, no fit, etc. |
| Rebuild Fit | Single select | Strong, moderate, weak, no fit, unknown. |
| Risk Level | Single select | Low, medium, high. |
| Recommendation | Single select | Acquire, monitor, pass, sell, hold, release. |
| Player Development Link | Link to another record | Connects acquisition impact to current development record. |
| Financial Link | Link to another record | Connects to financial/contract impact record. |
| Source Notes | Long text | Historical rationale and evidence. |

### 7. Front Office/Financials

Primary purpose: financial commitments, payroll planning, roster control, and front-office decision tracking.

| Field name | Airtable field type | Notes |
| --- | --- | --- |
| Financial Record | Autonumber or single line text, primary field | Unique financial/planning record. |
| Player Name | Link to another record | Link to Colorado Rockies Base when player-specific. |
| Player ID | Lookup | Lookup from Colorado Rockies Base. |
| Position | Lookup | Lookup from Colorado Rockies Base. |
| Contract Status | Lookup | Lookup from Colorado Rockies Base. |
| Forty-Man Status | Lookup | Lookup from Colorado Rockies Base. |
| Payroll Year | Number | Season/year. |
| Salary/Commitment | Currency | Salary, bonus, or projected commitment. |
| Arbitration Estimate | Currency | If applicable. |
| Option Status | Single select | Team option, player option, mutual option, no option, unknown. |
| Control Years Remaining | Number | Approximate years of club control. |
| Roster Decision | Single select | Keep, trade, non-tender, extend, DFA, release, protect, expose. |
| Financial Priority | Single select | High, medium, low. |
| Budget Impact | Single select | Adds payroll, reduces payroll, neutral, unknown. |
| Acquisition Link | Link to another record | Links financial impact to Acquisitions. |
| Player Development Link | Link to another record | Links financial decision to current player evaluation. |
| Front Office Notes | Long text | Planning notes. |

## Required Links Between Tables

| From table | Field | To table | Purpose |
| --- | --- | --- | --- |
| Player Development | Player Name | Colorado Rockies Base | One current development record per player. |
| Daily Baseball | Player Name | Colorado Rockies Base | Historical daily/game records by player. |
| Daily Baseball | Player Development Link | Player Development | Connect daily signals to current trend record. |
| Hitter Zone Reports | Player Name | Colorado Rockies Base | Historical hitter reports by player. |
| Hitter Zone Reports | Player Development Link | Player Development | Qualified reports can update current development. |
| Pitcher Zone Reports | Player Name | Colorado Rockies Base | Historical pitcher reports by player. |
| Pitcher Zone Reports | Player Development Link | Player Development | Qualified reports can update current development. |
| Acquisitions | Player Name | Colorado Rockies Base | Transaction and target records by player. |
| Acquisitions | Player Development Link | Player Development | Acquisition impact on player evaluation. |
| Acquisitions | Financial Link | Front Office/Financials | Transaction impact on payroll/control. |
| Front Office/Financials | Player Name | Colorado Rockies Base | Player-specific financial planning. |
| Front Office/Financials | Player Development Link | Player Development | Connect roster decisions to current evaluation. |

## Recommended Lookup Fields

Use these baseline lookups in workflow tables whenever the table links to Colorado Rockies Base:

- Player ID
- Position
- Level
- Age
- Bats
- Throws
- Overall Rating
- Potential
- Contract Status
- Forty-Man Status
- Organizational Role
- Current Roster Status
- Rebuild Fit Baseline

## Player Development Latest-Game Overwrite Fields

Only these current-status fields should be overwritten by the newest qualified report:

- Current Development Trend
- Trend Direction
- Latest-Game Status
- Recent Positives
- Recent Negatives
- Development Advice
- Current Organizational Role
- Rebuild Fit
- Recommendation
- Watch Status
- Acquisition Impact
- Roster Impact
- Last Updated Date
- Latest Source Report
- Latest Game Number

## Historical Information That Should Not Be Overwritten

These records should remain historical and additive:

- Every Daily Baseball game record
- Every Hitter Zone Report
- Every Pitcher Zone Report
- Every Acquisition record or transaction evaluation
- Every Front Office/Financials planning record by player/year/decision
- Source notes, scouting summaries, zone notes, box score summaries, and report IDs

## Approval Gate

After this structure is approved, the next phase should create Airtable import templates and field maps only. Automations, formulas, scripts, dashboards, or generated code should wait until the Airtable schema has been validated with sample records.
