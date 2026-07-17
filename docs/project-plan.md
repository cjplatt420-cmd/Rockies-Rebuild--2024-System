# Rockies Rebuild 2024 Project Plan

**Plan version:** 2.0  
**Status:** Approved required-field-first design  
**Validation phase:** Spring Games 1–7  
**Scope:** Planning documentation only

This plan defines the first Airtable-ready build for the Rockies Rebuild 2024 system. The first build is intentionally small: create and validate required fields before adding optional fields, import templates, formulas, automations, scripts, or dashboards.

## Permanent Evidence Rule

This rule applies to every table, report, handoff packet, import, correction, and future automation:

> Do not invent missing details or Player IDs.

- Store only information supported by the supplied source.
- Mark missing innings, pitch data, game details, and identifiers as missing, incomplete, pending, or unresolved.
- Never infer or manufacture a Player ID from a player name.
- A record without a verified Player ID may remain unlinked until the correct player is identified.
- Corrections must preserve the earlier record or version and clearly identify what changed.
- Limited or result-only coverage must not be presented as complete pitch-by-pitch coverage.

## Approved Repository Structure

This is the approved planning structure. Empty templates, formulas, and automations are not part of PR #2.

~~~text
Rockies-Rebuild--2024-System/
├── README.md
├── docs/
│   ├── system-overview.md
│   ├── airtable-setup-guide.md
│   ├── linking-rules.md
│   ├── overwrite-rules.md
│   ├── field-definitions.md
│   └── project-plan.md
├── airtable-templates/
│   ├── 01-colorado-rockies-base/
│   ├── 02-player-development/
│   ├── 03-daily-baseball/
│   ├── 04-hitter-zone-reports/
│   ├── 05-pitcher-zone-reports/
│   ├── 06-acquisitions/
│   └── 07-front-office-financials/
└── import-templates/
    ├── players-import.csv
    ├── player-development-import.csv
    ├── games-import.csv
    ├── hitter-reports-import.csv
    ├── pitcher-reports-import.csv
    ├── acquisitions-import.csv
    └── financials-import.csv
~~~

## Table Scope

### Seven main workflow tables

1. Colorado Rockies Base
2. Player Development
3. Daily Baseball
4. Hitter Zone Reports
5. Pitcher Zone Reports
6. Acquisitions
7. Front Office/Financials

### Required supporting table

8. Source Reports

Source Reports preserves source evidence, coverage, corrections, versions, and routing. It supports the seven main workflow tables but is not a separate decision-making workflow.

## Core Linking Model

- Colorado Rockies Base is the player source of truth.
- Player ID is the stable unique player key.
- Player Name is the readable name used in linked-record fields.
- The recommended Colorado Rockies Base primary display value is Player ID – Player Name.
- Downstream player links point to Colorado Rockies Base and obtain Player ID through a lookup.
- Player Development contains one current record per player.
- Daily Baseball, zone reports, acquisitions, financial records, and source reports remain historical and additive.
- Airtable-created reciprocal links should be used instead of manually creating duplicate link fields.
- Missing Player IDs remain unresolved; they are never invented.

## Required-Field-First Schemas

Only the fields below belong in the first build. Optional fields may be added after the Spring Games 1–7 test.

### 1. Colorado Rockies Base

Purpose: player identity and verified baseline source of truth.

| Required field | Airtable field type | Rule |
| --- | --- | --- |
| Player Record | Single line text, primary field | Display Player ID – Player Name; populate manually or during import in the first build. |
| Player ID | Single line text | Stable unique key; required only when verified. |
| Player Name | Single line text | Readable player name. |
| Organization | Single select | Current organization. |
| Level | Single select | Playing level only: MLB, AAA, AA, High-A, Low-A, Complex, or DSL. |
| Roster Status | Single select | Active, injured list, optioned, released, free agent, target, or other verified status. |
| Position | Single select | Primary position. |
| Age | Number | Verified age. |
| Bats | Single select | R, L, or S. |
| Throws | Single select | R or L. |
| Overall Rating | Number | Current supplied rating. |
| Potential | Single select | Letter grade such as A, B, C, or D. |
| Organizational Role | Single select | Verified baseline role. |
| Contract Summary | Long text | Verified contract and control information. |
| Baseline Report | Long text | Foundation scouting and organizational read. |
| Baseline Version | Number | Current baseline version. |
| Baseline Updated Date | Date | Date of latest verified baseline update. |

### 2. Player Development

Purpose: one current development record per player with source-owned current fields.

| Required field | Airtable field type | Rule |
| --- | --- | --- |
| Player Name | Link to Colorado Rockies Base, primary field | One Player Development record per linked player. |
| Player ID | Lookup | From Colorado Rockies Base. |
| Position | Lookup | From Colorado Rockies Base. |
| Level | Lookup | From Colorado Rockies Base. |
| Overall Rating | Lookup | From Colorado Rockies Base. |
| Potential | Lookup | From Colorado Rockies Base. |
| Latest Game ID | Link to Daily Baseball | Most recent qualified game update. |
| Latest Game Date | Date | Date of most recent qualified game evidence. |
| Latest-Game Status | Single select | Positive, negative, mixed, neutral, or no grade. |
| Current Trend | Single select | Up, down, stable, volatile, or insufficient evidence. |
| Recent Positives | Long text | Latest qualified game-derived positives. |
| Recent Negatives | Long text | Latest qualified game-derived negatives. |
| Development Advice | Long text | Current evidence-based advice. |
| Hitter Trend Summary | Long text | Latest qualified Hitter Zone synthesis, when applicable. |
| Pitcher Trend Summary | Long text | Latest qualified Pitcher Zone synthesis, when applicable. |
| Acquisition Impact | Long text | Owned by Acquisitions updates. |
| Roster Impact | Long text | Owned by verified transaction or roster updates. |
| Financial/Control Impact | Long text | Owned by Front Office/Financials updates. |
| Current Organizational Role | Single select | Changed only by a qualified Player Development synthesis. |
| Current Rebuild Fit | Single select | Changed only by a qualified Player Development synthesis. |
| Watch Status | Single select | Priority, normal, none, or insufficient evidence. |
| Latest Source Report ID | Link to Source Reports | Source supporting the latest applied update. |
| Last Updated Date | Date | Date the current record was last updated. |
| Current Record Version | Number | Increment when current fields change. |

### 3. Daily Baseball

Purpose: one historical complete-game record per game.

| Required field | Airtable field type | Rule |
| --- | --- | --- |
| Game ID | Single line text, primary field | Stable unique game identifier. |
| Game Date | Date | Game date. |
| Game Phase | Single select | Spring, regular season, postseason, or other supplied phase. |
| Game Number | Number | Supplied game number within the phase. |
| Opponent | Single line text | Opposing club. |
| Home/Away | Single select | Home or away. |
| Final Score | Single line text | Supplied final score; may remain pending. |
| Game Result | Single select | Win, loss, tie, incomplete, or pending. |
| Inning Coverage | Long text | Exact innings or half-innings supplied. |
| Coverage Status | Single select | Complete, partial, result-only, missing innings, or pending. |
| Complete Game Analysis | Long text | Evidence-based team and game analysis. |
| Player Development Summary | Long text | Game-level development conclusions. |
| Linked Players | Link to Colorado Rockies Base | Verified players discussed in the record. |
| Hitter Zone Reports | Link to Hitter Zone Reports | Historical reports produced from the game. |
| Pitcher Zone Reports | Link to Pitcher Zone Reports | Historical reports produced from the game. |
| Source Reports | Link to Source Reports | Evidence used for the game record. |
| Workflow Destinations | Multiple select | Daily Baseball, Player Development, Hitter Zone, Pitcher Zone, Acquisitions, Front Office/Financials, or archive. |
| Report Status | Single select | Draft, complete, incomplete, corrected, or superseded. |
| Correction Status | Single select | None, correction pending, corrected, or superseded. |
| Correction Notes | Long text | Exact corrections without rewriting unsupported details. |
| Version | Number | Increment for each corrected game record. |

### 4. Hitter Zone Reports

Purpose: historical hitter evidence and zone analysis.

| Required field | Airtable field type | Rule |
| --- | --- | --- |
| Hitter Report ID | Single line text, primary field | Stable unique report identifier. |
| Game ID | Link to Daily Baseball | Game that produced the report. |
| Report Date | Date | Report date. |
| Player Name | Link to Colorado Rockies Base | Link only when identity is verified. |
| Player ID | Lookup | From Colorado Rockies Base. |
| Coverage Status | Single select | Pitch-by-pitch, partial, result-only, missing, or pending. |
| Plate-Appearance Evidence | Long text | Supplied pitch, zone, contact, and result evidence. |
| Zone Findings | Long text | Supported chase, damage, weakness, and contact findings. |
| Game Development Signal | Single select | Up, down, stable, mixed, or no grade. |
| Development Recommendation | Long text | Evidence-based current recommendation. |
| Player Development Qualified | Checkbox | Checked only when evidence is sufficient for an update. |
| Source Reports | Link to Source Reports | Supporting evidence. |
| Workflow Destinations | Multiple select | Required routing destinations. |
| Report Status | Single select | Draft, complete, incomplete, corrected, or superseded. |
| Correction Notes | Long text | Exact correction record. |
| Version | Number | Increment for each correction. |

### 5. Pitcher Zone Reports

Purpose: historical pitcher evidence, zone use, and arsenal analysis.

| Required field | Airtable field type | Rule |
| --- | --- | --- |
| Pitcher Report ID | Single line text, primary field | Stable unique report identifier. |
| Game ID | Link to Daily Baseball | Game that produced the report. |
| Report Date | Date | Report date. |
| Player Name | Link to Colorado Rockies Base | Link only when identity is verified. |
| Player ID | Lookup | From Colorado Rockies Base. |
| Coverage Status | Single select | Pitch-by-pitch, partial, result-only, missing, or pending. |
| Batter/Pitch Evidence | Long text | Supplied pitch, zone, velocity, count, and result evidence. |
| Zone and Arsenal Findings | Long text | Supported command, whiff, damage, and pitch-mix findings. |
| Game Development Signal | Single select | Up, down, stable, mixed, or no grade. |
| Development Recommendation | Long text | Evidence-based current recommendation. |
| Player Development Qualified | Checkbox | Checked only when evidence is sufficient for an update. |
| Source Reports | Link to Source Reports | Supporting evidence. |
| Workflow Destinations | Multiple select | Required routing destinations. |
| Report Status | Single select | Draft, complete, incomplete, corrected, or superseded. |
| Correction Notes | Long text | Exact correction record. |
| Version | Number | Increment for each correction. |

### 6. Acquisitions

Purpose: historical targets, transactions, roster moves, and acquisition decisions.

| Required field | Airtable field type | Rule |
| --- | --- | --- |
| Acquisition Record ID | Single line text, primary field | Stable unique evaluation or transaction ID. |
| Record Date | Date | Evaluation or transaction date. |
| Player Name | Link to Colorado Rockies Base | Link only if the player exists with verified identity. |
| Player ID | Lookup | From Colorado Rockies Base; never invented for an external target. |
| Unlinked Player Name | Single line text | Temporary readable name when a verified link is unavailable. |
| Acquisition Type | Single select | Target, trade, free agent, waiver, extension, DFA, release, or other verified type. |
| Acquisition Status | Single select | Evaluating, recommended, completed, passed, lost, or pending. |
| Evaluation | Long text | Evidence and organizational analysis. |
| Cost/Return | Long text | Verified or explicitly projected cost and return. |
| Roster Impact | Long text | Acquisition-owned roster effect. |
| Recommendation | Single select | Acquire, monitor, hold, trade, pass, release, or pending. |
| Source Reports | Link to Source Reports | Supporting report or transaction evidence. |
| Workflow Destinations | Multiple select | Acquisitions, Player Development, Colorado Rockies Base, Front Office/Financials, or archive. |
| Report Status | Single select | Draft, complete, incomplete, corrected, or superseded. |
| Correction Notes | Long text | Exact corrections. |
| Version | Number | Increment for each correction. |

### 7. Front Office/Financials

Purpose: historical payroll, contract, control, and decision records.

| Required field | Airtable field type | Rule |
| --- | --- | --- |
| Financial Record ID | Single line text, primary field | Stable unique financial record ID. |
| Record Date | Date | Date of the financial record. |
| Player Name | Link to Colorado Rockies Base | Optional for team-level records. |
| Player ID | Lookup | From Colorado Rockies Base when linked. |
| Record Type | Single select | Contract, payroll, transaction cost, budget, extension, option, or other verified type. |
| Payroll Year | Number | Relevant season. |
| Amount | Currency | Verified or clearly labeled projected amount. |
| Contract/Control Summary | Long text | Verified terms and control information. |
| Budget Impact | Long text | Team financial effect. |
| Decision Status | Single select | Proposed, approved, completed, rejected, or pending. |
| Acquisition Record | Link to Acquisitions | Related transaction or evaluation. |
| Source Reports | Link to Source Reports | Supporting evidence. |
| Workflow Destinations | Multiple select | Front Office/Financials, Acquisitions, Player Development, Colorado Rockies Base, or archive. |
| Report Status | Single select | Draft, complete, incomplete, corrected, or superseded. |
| Correction Notes | Long text | Exact corrections. |
| Version | Number | Increment for each correction. |

### 8. Source Reports

Purpose: preserve the source, coverage, routing, correction, and version trail behind every derived report.

| Required field | Airtable field type | Rule |
| --- | --- | --- |
| Source Report ID | Single line text, primary field | Stable unique source identifier. |
| Source Date | Date | Date of source or observation. |
| Source Type | Single select | Dictated game feed, box score, screenshot, foundation report, transaction update, manual correction, or other supplied type. |
| Game ID | Link to Daily Baseball | Optional game connection. |
| Linked Players | Link to Colorado Rockies Base | Only verified player identities. |
| Source Content/Summary | Long text | Supplied evidence or faithful summary. |
| Inning Coverage | Long text | Exact innings or half-innings supplied. |
| Coverage Status | Single select | Complete, partial, result-only, missing, or pending. |
| Missing Information | Long text | Explicitly identify gaps. |
| Correction Notes | Long text | Exact changes and their source. |
| Supersedes Source Report | Link to Source Reports | Earlier version when corrected. |
| Version | Number | Increment for a corrected source. |
| Workflow Destinations | Multiple select | All required report destinations. |
| Processing Status | Single select | Unprocessed, routed, complete, incomplete, corrected, or superseded. |

## Required Links

| From | To | Purpose |
| --- | --- | --- |
| Player Development.Player Name | Colorado Rockies Base | One current record per verified player. |
| Daily Baseball.Linked Players | Colorado Rockies Base | Players supported by the game evidence. |
| Hitter Zone Reports.Player Name | Colorado Rockies Base | Verified hitter identity. |
| Pitcher Zone Reports.Player Name | Colorado Rockies Base | Verified pitcher identity. |
| Acquisitions.Player Name | Colorado Rockies Base | Verified target or transaction identity. |
| Front Office/Financials.Player Name | Colorado Rockies Base | Player-specific financial decision. |
| All report tables.Source Reports | Source Reports | Evidence, coverage, corrections, and version trail. |
| Zone reports.Game ID | Daily Baseball | Tie player evidence to the historical game. |
| Daily Baseball zone links | Hitter/Pitcher Zone Reports | Navigate from the game to derived reports. |
| Front Office/Financials.Acquisition Record | Acquisitions | Tie financial impact to the related roster decision. |

## Source-Specific Overwrite Rules

Historical source and report records are additive. Only Player Development current fields may be overwritten, and each source may update only the fields it owns.

### Daily Baseball owns

- Latest Game ID
- Latest Game Date
- Latest-Game Status
- Recent Positives
- Recent Negatives
- general game-derived Development Advice
- Latest Source Report ID
- Last Updated Date
- Current Record Version

### Hitter Zone Reports own

- Hitter Trend Summary
- hitter-specific Development Advice
- hitter evidence contribution to Current Trend and Watch Status
- latest hitter source reference and update date

### Pitcher Zone Reports own

- Pitcher Trend Summary
- pitcher-specific Development Advice
- pitcher evidence contribution to Current Trend and Watch Status
- latest pitcher source reference and update date

### Acquisitions owns

- Acquisition Impact
- transaction-derived Roster Impact
- acquisition source reference and update date

### Front Office/Financials owns

- Financial/Control Impact
- financial source reference and update date

### Colorado Rockies Base owns

- Player ID and permanent identity
- verified organization, level, roster status, ratings, potential, contract baseline, and organizational baseline
- baseline version and baseline update date

### Qualified Player Development synthesis owns

- Current Organizational Role
- Current Rebuild Fit
- final Current Trend
- Watch Status when multiple report types must be reconciled

### Overwrite safeguards

1. A newer version from the same source type may supersede the earlier current value.
2. One source type may not erase or replace a field owned by another source type.
3. Blank, missing, incomplete, or no-grade input does not erase a supported current value.
4. Corrections supersede the affected version and must preserve the correction trail.
5. If dates or game numbers conflict, stop the overwrite and mark the update for review.
6. Missing Player IDs remain unlinked and cannot update a Player Development record automatically.
7. Individual historical reports are never deleted or overwritten by a later game.
8. A batting-stance adjustment may be recommended only after approximately 10–20 games show a repeated pitch-type, zone-location, and contact/result pattern. One game or one at-bat is never sufficient.

## Spring Games 1–7 Validation Plan

The first implementation must be tested with Spring Games 1–7 before optional fields or automation work begins.

### Test sequence

1. Load a small verified player sample into Colorado Rockies Base.
2. Create one Daily Baseball record for each Spring Game 1 through Spring Game 7.
3. Record exact inning coverage for every game, including partial or missing coverage.
4. Link each game to its Source Reports.
5. Create Hitter Zone and Pitcher Zone records only where supplied evidence supports them.
6. Route each completed report to the required workflow destinations.
7. Apply qualified Player Development updates sequentially from Game 1 through Game 7.
8. Confirm that each verified player still has exactly one current Player Development record.
9. Confirm that later qualified games overwrite only source-owned current fields.
10. Confirm that every Daily Baseball and zone report remains available as historical evidence.
11. Enter at least one correction as a new version and confirm that the correction trail is preserved.
12. Test a partial game, a result-only report, a missing inning, and a player without a verified Player ID.
13. Confirm that missing data is visibly marked and never filled by inference.
14. Confirm that acquisition and financial fields are not overwritten by game reports.
15. Confirm that no batting-stance recommendation is produced from Games 1–7 alone.

### Acceptance criteria

The required-field-first design passes when:

- Spring Games 1–7 can be stored without inventing any detail or Player ID.
- Every game has a stable Game ID, coverage status, routing status, correction status, and version.
- Historical reports remain additive and traceable to Source Reports.
- Player Development maintains one current record per verified player.
- Latest-game updates affect only their approved fields.
- Corrections supersede earlier versions without deleting history.
- Partial and missing coverage are clearly distinguishable from complete coverage.
- External acquisition targets can be recorded without forcing an unverified Player ID.
- No optional field, formula, automation, script, or dashboard is required to complete the test.

## Approval Gate

PR #2 documents the approved design and validation plan only.

After this plan is merged:

1. Create only the required Airtable fields and links.
2. Run the Spring Games 1–7 validation.
3. Record defects and necessary field changes.
4. Obtain approval of the validated required schema.
5. Add optional fields only after that approval.
6. Build import templates, formulas, automations, scripts, and dashboards in later, separately reviewed phases.
