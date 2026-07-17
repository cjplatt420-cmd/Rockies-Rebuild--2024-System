# Required Airtable Field Definitions

**Phase:** 2  
**Status:** Required-field-first implementation specification  
**Source of authority:** docs/project-plan.md version 2.1

## Scope

Create only the fields in this document. Do not create optional fields, CSV import templates, formulas, automations, scripts, dashboards, or interfaces during Phase 2.

> Do not invent missing details or Player IDs.

Player IDs must come from verified project evidence. System Game IDs and report IDs follow the approved internal formats and do not function as Player IDs.

## Airtable primary-field clarification

Airtable primary fields support directly editable types such as single line text, but not Link to another record. Therefore:

- Every table uses a single line text record ID as its primary field.
- Player Name is a separate Link to another record field in downstream tables.
- No formula field is required in Phase 2.
- Linked records display the linked table's primary field. The primary IDs below must therefore be readable and unique.

## Controlled choices

Use these exact Phase 2 choices. Do not add near-duplicate spelling variants during testing.

| Field | Choices |
| --- | --- |
| Level | MLB; AAA; AA; High-A; Low-A; Complex; DSL |
| Roster Status | Active; Injured List; Optioned; Released; Free Agent; Other Verified Status |
| Bats | R; L; S |
| Throws | R; L |
| Potential | A; B; C; D |
| Position | C; 1B; 2B; 3B; SS; LF; CF; RF; DH; SP; RP; CP; Two-Way |
| Organizational Role | Core; MLB Starter; MLB Regular; MLB Bench; MLB Rotation; MLB Bullpen; Upper-Minors Depth; Lower-Minors Development; Prospect; Bridge; Trade Candidate; Release/DFA Risk; Insufficient Evidence |
| Rebuild Fit | Long-Term Fit; Short-Term Bridge; Trade Chip; Depth; Watch; No Fit; Insufficient Evidence |
| Workflow Destinations | Colorado Rockies Base; Player Development; Daily Baseball; Hitter Zone; Pitcher Zone; Acquisitions; Front Office/Financials; Source Reports; Archive |
| Coverage Status, game | Complete; Partial; Result-Only; Missing Innings; Pending |
| Coverage Status, hitter/pitcher zone | Pitch-by-Pitch; Partial; Result-Only; Missing; Pending |
| Coverage Status, source | Complete; Partial; Result-Only; Missing; Pending |
| Report Status | Draft; Complete; Incomplete; Corrected; Superseded |
| Correction Status | None; Correction Pending; Corrected; Superseded |
| Processing Status | Unprocessed; Routed; Complete; Incomplete; Corrected; Superseded |
| Latest-Game Status | Positive; Negative; Mixed; Neutral; No Grade |
| Current Trend | Up; Down; Stable; Volatile; Insufficient Evidence |
| Game Development Signal | Up; Down; Stable; Mixed; No Grade |
| Watch Status | Priority; Normal; None; Insufficient Evidence |

## 1. Colorado Rockies Base

Primary purpose: verified player identity and baseline source of truth.

| Field | Type | Required rule |
| --- | --- | --- |
| Player Record | Single line text, primary | Enter Player ID – Player Name. Do not create without a verified Player ID. |
| Player ID | Single line text | Verified stable unique player key. |
| Player Name | Single line text | Readable verified name. |
| Organization | Single select | Current verified organization. |
| Level | Single select | Use controlled Level choices only. |
| Roster Status | Single select | Use controlled Roster Status choices only. |
| Position | Single select | Use controlled Position choices only. |
| Age | Number, integer | Verified age. |
| Bats | Single select | R, L, or S. |
| Throws | Single select | R or L. |
| Overall Rating | Number, integer | Current supplied rating. |
| Potential | Single select | A, B, C, or D. |
| Organizational Role | Single select | Use controlled Organizational Role choices only. |
| Contract Summary | Long text | Verified terms and control notes. |
| Baseline Report | Long text | Foundation scouting and organizational read. |
| Baseline Version | Number, integer | Begin at 1. |
| Baseline Updated Date | Date | Latest verified baseline update. |

## 2. Player Development

Primary purpose: one current record per verified player.

| Field | Type | Required rule |
| --- | --- | --- |
| Player Development Record | Single line text, primary | Enter PD-Player ID. Create only for a verified Player ID. |
| Player Name | Link to Colorado Rockies Base | Allow one linked record only. |
| Player ID | Lookup | Player Name → Player ID. |
| Position | Lookup | Player Name → Position. |
| Level | Lookup | Player Name → Level. |
| Overall Rating | Lookup | Player Name → Overall Rating. |
| Potential | Lookup | Player Name → Potential. |
| Latest Game Record | Link to Daily Baseball | Allow one linked game-record version only. |
| Latest Game ID | Lookup | Latest Game Record → Game ID. |
| Latest Game Date | Date | Most recent qualified game evidence. |
| Latest Game Source Report | Link to Source Reports | Allow one current source record. |
| Latest Game Source Updated Date | Date | Date game-owned fields were applied. |
| Latest-Game Status | Single select | Use controlled choices. |
| Current Trend | Single select | Final value owned by Player Development synthesis. |
| Recent Positives | Long text | Latest qualified game-derived positives. |
| Recent Negatives | Long text | Latest qualified game-derived negatives. |
| Current Development Advice | Long text | Final value owned only by qualified Player Development synthesis. |
| Hitter Trend Summary | Long text | Latest qualified hitter synthesis. |
| Latest Hitter Source Report | Link to Source Reports | Allow one current source record. |
| Latest Hitter Source Updated Date | Date | Date hitter-owned fields were applied. |
| Pitcher Trend Summary | Long text | Latest qualified pitcher synthesis. |
| Latest Pitcher Source Report | Link to Source Reports | Allow one current source record. |
| Latest Pitcher Source Updated Date | Date | Date pitcher-owned fields were applied. |
| Acquisition Impact | Long text | Owned by Acquisitions. |
| Roster Impact | Long text | Owned by verified acquisition/transaction evidence. |
| Latest Acquisition Source Report | Link to Source Reports | Allow one current source record. |
| Latest Acquisition Source Updated Date | Date | Date acquisition-owned fields were applied. |
| Financial/Control Impact | Long text | Owned by Front Office/Financials. |
| Latest Financial Source Report | Link to Source Reports | Allow one current source record. |
| Latest Financial Source Updated Date | Date | Date financial-owned fields were applied. |
| Current Organizational Role | Single select | Player Development synthesis only; use controlled Organizational Role choices. |
| Current Rebuild Fit | Single select | Player Development synthesis only; use controlled Rebuild Fit choices. |
| Watch Status | Single select | Player Development synthesis only. |
| Last Updated Date | Date | Latest applied current-record update. |
| Current Record Version | Number, integer | Increment whenever current fields change. |

## 3. Daily Baseball

Primary purpose: historical game records, including partial and incomplete games.

| Field | Type | Required rule |
| --- | --- | --- |
| Game Record ID | Single line text, primary | Game ID-VNN, such as COL-2024-SPR-G01-V01. |
| Game ID | Single line text | Stable logical game ID shared by corrected versions. |
| Game Date | Date | Verified date. |
| Game Phase | Single select | Spring; Regular Season; Postseason; Other Supplied Phase. |
| Game Number | Number, integer | Supplied number within phase. |
| Opponent | Single line text | Verified opposing club. |
| Home/Away | Single select | Home; Away. |
| Final Score | Single line text | May remain pending. |
| Game Result | Single select | Win; Loss; Tie; Incomplete; Pending. |
| Inning Coverage | Long text | Exact supplied innings or half-innings. |
| Coverage Status | Single select | Use controlled game choices. |
| Complete Game Analysis | Long text | Evidence-based analysis; may be incomplete when coverage is incomplete. |
| Player Development Summary | Long text | Game-level conclusions. |
| Linked Players | Link to Colorado Rockies Base | Allow multiple; verified identities only. |
| Hitter Zone Reports | Reciprocal link | Airtable-created backlink from Hitter Zone Reports.Game Record. |
| Pitcher Zone Reports | Reciprocal link | Airtable-created backlink from Pitcher Zone Reports.Game Record. |
| Source Reports | Reciprocal link | Airtable-created backlink from Source Reports.Game Record. |
| Workflow Destinations | Multiple select | Approved workflow destinations only. |
| Report Status | Single select | Use controlled choices. |
| Correction Status | Single select | Use controlled choices. |
| Correction Notes | Long text | Exact supported correction. |
| Version | Number, integer | Begin at 1. |
| Supersedes Record | Link to Daily Baseball | Allow one earlier record version only. |

## 4. Hitter Zone Reports

| Field | Type | Required rule |
| --- | --- | --- |
| Hitter Report ID | Single line text, primary | HZR-Game ID-NNN-VNN. |
| Game Record | Link to Daily Baseball | Allow one exact game-record version. |
| Game ID | Lookup | Game Record → Game ID. |
| Report Date | Date | Verified report date. |
| Player Name | Link to Colorado Rockies Base | Allow one verified player only. |
| Player ID | Lookup | Player Name → Player ID. |
| Coverage Status | Single select | Use controlled zone/source choices. |
| Plate-Appearance Evidence | Long text | Supplied pitch, zone, contact, and result evidence. |
| Zone Findings | Long text | Supported findings only. |
| Game Development Signal | Single select | Use controlled choices. |
| Development Recommendation | Long text | Report input; does not directly overwrite final Player Development advice. |
| Player Development Qualified | Checkbox | Check only when evidence is sufficient. |
| Source Reports | Link to Source Reports | Allow multiple supporting sources. |
| Workflow Destinations | Multiple select | Approved destinations only. |
| Report Status | Single select | Use controlled choices. |
| Correction Notes | Long text | Exact supported correction. |
| Version | Number, integer | Begin at 1. |
| Supersedes Record | Link to Hitter Zone Reports | Allow one earlier version only. |

## 5. Pitcher Zone Reports

| Field | Type | Required rule |
| --- | --- | --- |
| Pitcher Report ID | Single line text, primary | PZR-Game ID-NNN-VNN. |
| Game Record | Link to Daily Baseball | Allow one exact game-record version. |
| Game ID | Lookup | Game Record → Game ID. |
| Report Date | Date | Verified report date. |
| Player Name | Link to Colorado Rockies Base | Allow one verified player only. |
| Player ID | Lookup | Player Name → Player ID. |
| Coverage Status | Single select | Use controlled zone/source choices. |
| Batter/Pitch Evidence | Long text | Supplied pitch, zone, velocity, count, and result evidence. |
| Zone and Arsenal Findings | Long text | Supported findings only. |
| Game Development Signal | Single select | Use controlled choices. |
| Development Recommendation | Long text | Report input; does not directly overwrite final Player Development advice. |
| Player Development Qualified | Checkbox | Check only when evidence is sufficient. |
| Source Reports | Link to Source Reports | Allow multiple supporting sources. |
| Workflow Destinations | Multiple select | Approved destinations only. |
| Report Status | Single select | Use controlled choices. |
| Correction Notes | Long text | Exact supported correction. |
| Version | Number, integer | Begin at 1. |
| Supersedes Record | Link to Pitcher Zone Reports | Allow one earlier version only. |

## 6. Acquisitions

| Field | Type | Required rule |
| --- | --- | --- |
| Acquisition Record ID | Single line text, primary | ACQ-YYYY-NNN-VNN. |
| Record Date | Date | Verified evaluation or transaction date. |
| Player Name | Link to Colorado Rockies Base | Allow one; optional until identity is verified. |
| Player ID | Lookup | Player Name → Player ID. |
| Unlinked Player Name | Single line text | Temporary readable name when verified link is unavailable. |
| Acquisition Type | Single select | Target; Trade; Free Agent; Waiver; Extension; DFA; Release; Other Verified Type. |
| Acquisition Status | Single select | Evaluating; Recommended; Completed; Passed; Lost; Pending. |
| Evaluation | Long text | Evidence and organizational analysis. |
| Cost/Return | Long text | Verified or clearly labeled projected cost. |
| Roster Impact | Long text | Acquisition-owned effect. |
| Recommendation | Single select | Acquire; Monitor; Hold; Trade; Pass; Release; Pending. |
| Source Reports | Link to Source Reports | Allow multiple supporting sources. |
| Workflow Destinations | Multiple select | Approved destinations only. |
| Report Status | Single select | Use controlled choices. |
| Correction Notes | Long text | Exact supported correction. |
| Version | Number, integer | Begin at 1. |
| Supersedes Record | Link to Acquisitions | Allow one earlier version only. |

## 7. Front Office/Financials

| Field | Type | Required rule |
| --- | --- | --- |
| Financial Record ID | Single line text, primary | FIN-YYYY-NNN-VNN. |
| Record Date | Date | Verified date. |
| Player Name | Link to Colorado Rockies Base | Allow one; optional for team-level records. |
| Player ID | Lookup | Player Name → Player ID. |
| Record Type | Single select | Contract; Payroll; Transaction Cost; Budget; Extension; Option; Other Verified Type. |
| Payroll Year | Number, integer | Relevant season. |
| Amount | Currency | Verified or clearly labeled projection. |
| Contract/Control Summary | Long text | Verified terms and control information. |
| Budget Impact | Long text | Team financial effect. |
| Decision Status | Single select | Proposed; Approved; Completed; Rejected; Pending. |
| Acquisition Record | Link to Acquisitions | Allow one related acquisition record. |
| Source Reports | Link to Source Reports | Allow multiple supporting sources. |
| Workflow Destinations | Multiple select | Approved destinations only. |
| Report Status | Single select | Use controlled choices. |
| Correction Notes | Long text | Exact supported correction. |
| Version | Number, integer | Begin at 1. |
| Supersedes Record | Link to Front Office/Financials | Allow one earlier version only. |

## 8. Source Reports

| Field | Type | Required rule |
| --- | --- | --- |
| Source Report ID | Single line text, primary | SRC-Game ID-NNN-VNN or SRC-YYYY-NNN-VNN. |
| Source Date | Date | Verified source or observation date. |
| Source Type | Single select | Dictated Game Feed; Box Score; Screenshot; Foundation Report; Transaction Update; Manual Correction; Other Supplied Type. |
| Game Record | Link to Daily Baseball | Allow one exact game-record version; optional. |
| Game ID | Lookup | Game Record → Game ID. |
| Linked Players | Link to Colorado Rockies Base | Allow multiple; verified identities only. |
| Source Content/Summary | Long text | Supplied evidence or faithful summary. |
| Inning Coverage | Long text | Exact supplied innings or half-innings. |
| Coverage Status | Single select | Complete; Partial; Result-Only; Missing; Pending. |
| Missing Information | Long text | Explicit gaps. |
| Correction Notes | Long text | Exact correction and source. |
| Supersedes Record | Link to Source Reports | Allow one earlier version only. |
| Version | Number, integer | Begin at 1. |
| Workflow Destinations | Multiple select | Approved destinations only. |
| Processing Status | Single select | Use controlled choices. |

## Phase 2 completion check

- All eight tables exist.
- Every primary field is single line text.
- Every lookup has its required linked-record source field.
- No linked-record field is configured as a primary field.
- No unverified Player ID exists.
- No formula, automation, script, dashboard, interface, or CSV template has been created.
