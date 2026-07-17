# Airtable Required-Field Setup Guide

**Phase:** 2  
**Goal:** Build and validate the required schema with Spring Games 1–7  
**Excluded:** optional fields, CSV templates, formulas, automations, scripts, dashboards, and interfaces

> Do not invent missing details or Player IDs.

## Before starting

Have these documents open:

1. docs/project-plan.md
2. docs/field-definitions.md
3. docs/linking-rules.md
4. docs/overwrite-rules.md

Use a new or clearly identified Airtable base for the validation. Do not test in a production base containing irreplaceable records.

## Step 1: Create the eight tables

Create these tables in this order:

1. Colorado Rockies Base
2. Daily Baseball
3. Source Reports
4. Hitter Zone Reports
5. Pitcher Zone Reports
6. Acquisitions
7. Front Office/Financials
8. Player Development

Rename each table's first field and keep it as Single line text:

| Table | Primary field |
| --- | --- |
| Colorado Rockies Base | Player Record |
| Daily Baseball | Game Record ID |
| Source Reports | Source Report ID |
| Hitter Zone Reports | Hitter Report ID |
| Pitcher Zone Reports | Pitcher Report ID |
| Acquisitions | Acquisition Record ID |
| Front Office/Financials | Financial Record ID |
| Player Development | Player Development Record |

Do not make Player Name a primary field or a formula during Phase 2.

## Step 2: Add direct-entry fields

Using docs/field-definitions.md, add all Single line text, Long text, Number, Currency, Date, Single select, Multiple select, and Checkbox fields.

For every Single select field:

- use only the approved choice spelling,
- do not create a second spelling variant,
- use Pending, Missing, or Insufficient Evidence instead of guessing.

Do not add lookup fields yet.

## Step 3: Add player links

Create Player Name or Linked Players links to Colorado Rockies Base as listed in docs/linking-rules.md.

- Turn off multiple links where the rules say one player.
- Allow multiple links only for Daily Baseball.Linked Players and Source Reports.Linked Players.
- Confirm Airtable creates reciprocal backlinks.
- Rename backlinks for clarity; do not create duplicate manual links.

Populate links only after the verified Colorado Rockies Base player exists.

## Step 4: Add game links

Create:

- Player Development.Latest Game Record
- Hitter Zone Reports.Game Record
- Pitcher Zone Reports.Game Record
- Source Reports.Game Record

Each allows one Daily Baseball record version.

## Step 5: Add source and workflow links

Create the five domain-specific Player Development links to Source Reports, then create Source Reports links in the zone, acquisition, and financial tables.

Use the reciprocal Daily Baseball.Source Reports backlink created by Source Reports.Game Record.

Create Front Office/Financials.Acquisition Record as a one-record link to Acquisitions.

## Step 6: Add correction self-links

In each historical table, create Supersedes Record as a self-link allowing one earlier record.

Rename the reciprocal backlink Superseded By.

Do not populate these fields for version 1 records.

## Step 7: Add lookups

Only after links exist, create the lookup fields in docs/linking-rules.md.

Test each lookup by linking one verified record and confirming that the expected value appears. If a lookup is blank, first verify that the linked-record cell contains an actual link; creating a link field alone does not connect records.

## Step 8: Create Phase 2 audit views

Create grid views only; these are not dashboards.

| Table | View | Filter or review purpose |
| --- | --- | --- |
| Colorado Rockies Base | Duplicate Player ID Review | Sort/group by Player ID and inspect duplicates or blanks. |
| Daily Baseball | Coverage Review | Coverage Status is not Complete. |
| Daily Baseball | Version Review | Sort by Game ID, then Version. |
| Source Reports | Unprocessed Sources | Processing Status is Unprocessed or Incomplete. |
| Hitter Zone Reports | Hitter Corrections | Report Status is Corrected or Superseded. |
| Pitcher Zone Reports | Pitcher Corrections | Report Status is Corrected or Superseded. |
| Acquisitions | Unlinked Targets | Player Name is empty and Unlinked Player Name is not empty. |
| Player Development | Missing Provenance | Any populated domain summary lacks its domain Source Report or update date. |

No formula is required for these views.

## Step 9: Load the minimum verified player sample

Choose only players with verified Player IDs in Colorado Rockies Base.

For each player:

1. Enter Player Record as Player ID – Player Name.
2. Enter verified baseline fields.
3. Create one Player Development record as PD-Player ID.
4. Link Player Development.Player Name to the existing Colorado Rockies Base record.
5. Confirm Player ID, Position, Level, Overall Rating, and Potential lookups.

Do not create a Player Development record for an unresolved Player ID.

## Step 10: Enter Spring Games 1–7

Use the system Game IDs in docs/project-plan.md.

For each game:

1. Create Game Record ID with V01.
2. Enter Game ID, date, phase, number, opponent, and only verified result context.
3. Enter exact inning coverage.
4. Select the supported Coverage Status.
5. Mark unverified details Pending or Missing.
6. Create and link Source Reports.
7. Create zone reports only when evidence supports them.
8. Route reports to Daily Baseball, zone workflows, Player Development, and Source Reports as required.
9. Add Acquisitions or Front Office/Financials routing only when supplied evidence requires it.

Spring Game 5 and Spring Game 7 must demonstrate partial or missing coverage handling unless a newly supplied complete source verifies otherwise.

## Step 11: Run a correction test

Choose one non-destructive test record:

1. Preserve its V01 record.
2. Create V02 with a new versioned primary ID.
3. Link V02.Supersedes Record to V01.
4. Enter exact Correction Notes.
5. Mark V01 Superseded.
6. Verify the logical Game ID remains unchanged.
7. Update Player Development provenance only if V02 is the qualified current source.

The test fails if V01 is deleted or rewritten.

## Step 12: Run duplicate tests

- Confirm Player ID uniqueness in Colorado Rockies Base.
- Confirm every versioned primary report ID is unique.
- Confirm multiple Daily Baseball versions share Game ID only through a valid correction chain.
- Enter one deliberately duplicated test ID, confirm the review detects it, then correct the test record without deleting historical evidence that must be retained.

## Step 13: Run overwrite tests

Apply Spring Games 1–7 sequentially using docs/overwrite-rules.md.

Verify:

- one current Player Development record per verified player,
- each workflow updates only its fields,
- each populated domain field retains its source link and update date,
- missing input does not erase supported current evidence,
- final advice and Watch Status change only through qualified Player Development synthesis,
- Games 1–7 alone do not produce a batting-stance recommendation.

## Phase 2 completion checklist

- [ ] Eight tables created.
- [ ] All required fields created with correct types.
- [ ] All primary fields are single line text.
- [ ] Player and game links tested.
- [ ] Lookups return expected verified values.
- [ ] Reciprocal backlinks renamed and duplicate links avoided.
- [ ] Correction version chain tested.
- [ ] Duplicate ID review tested.
- [ ] Spring Games 1–7 entered with explicit coverage.
- [ ] Source-specific overwrite tests passed.
- [ ] Missing details and Player IDs were never invented.
- [ ] No optional fields, CSV templates, formulas, automations, scripts, dashboards, or interfaces were created.

Record failures and required schema changes before Phase 3. Do not fix a failed design by silently adding optional fields.

## References

- Airtable: The primary field — https://support.airtable.com/docs/the-primary-field
- Airtable: Linking records — https://support.airtable.com/docs/linking-records-in-airtable
- Airtable: Lookup field overview — https://support.airtable.com/docs/lookup-field-overview
