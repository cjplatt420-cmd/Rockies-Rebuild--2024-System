# Airtable Linking Rules

**Phase:** 2  
**Status:** Required-field-first implementation specification

> Do not invent missing details or Player IDs.

## Airtable behavior used by this design

- A linked-record field creates the relationship structure but does not automatically connect existing records.
- When a linked-record field is created, Airtable creates a reciprocal backlink in the other table.
- A lookup field requires an existing linked-record field and reads values from the linked record.
- Primary fields do not support the Link to another record type, so every table uses a single line text record ID as its primary field.
- In Phase 2, create and verify links manually. Do not create automations.

## Table creation order

Create all eight tables and their primary ID fields first:

1. Colorado Rockies Base — Player Record
2. Daily Baseball — Game Record ID
3. Source Reports — Source Report ID
4. Hitter Zone Reports — Hitter Report ID
5. Pitcher Zone Reports — Pitcher Report ID
6. Acquisitions — Acquisition Record ID
7. Front Office/Financials — Financial Record ID
8. Player Development — Player Development Record

This order reduces circular-link confusion. It does not change table authority.

## Player links

| From field | To table | Multiple? | Rule |
| --- | --- | --- | --- |
| Player Development.Player Name | Colorado Rockies Base | No | Exactly one verified player per current development record. |
| Daily Baseball.Linked Players | Colorado Rockies Base | Yes | Link only players supported by the game evidence. |
| Hitter Zone Reports.Player Name | Colorado Rockies Base | No | Exactly one verified hitter. |
| Pitcher Zone Reports.Player Name | Colorado Rockies Base | No | Exactly one verified pitcher. |
| Acquisitions.Player Name | Colorado Rockies Base | No | Optional until the external identity is verified. |
| Front Office/Financials.Player Name | Colorado Rockies Base | No | Optional for team-level financial records. |
| Source Reports.Linked Players | Colorado Rockies Base | Yes | Verified identities only. |

If a Player ID is missing, leave the link empty. An Acquisitions record may use Unlinked Player Name temporarily. No other workflow may create or guess a player link.

## Game links

| From field | To table | Multiple? | Rule |
| --- | --- | --- | --- |
| Player Development.Latest Game Record | Daily Baseball | No | Link the exact qualified game-record version. |
| Hitter Zone Reports.Game Record | Daily Baseball | No | Link the exact game-record version used. |
| Pitcher Zone Reports.Game Record | Daily Baseball | No | Link the exact game-record version used. |
| Source Reports.Game Record | Daily Baseball | No | Optional exact game-record version. |

After each Game Record link exists, create the Game ID lookup from Daily Baseball.Game ID. Do not type a second manual Game ID into zone or source tables.

## Source-evidence links

Create these fields as links to Source Reports:

- Player Development.Latest Game Source Report — one record
- Player Development.Latest Hitter Source Report — one record
- Player Development.Latest Pitcher Source Report — one record
- Player Development.Latest Acquisition Source Report — one record
- Player Development.Latest Financial Source Report — one record
- Hitter Zone Reports.Source Reports — multiple records
- Pitcher Zone Reports.Source Reports — multiple records
- Acquisitions.Source Reports — multiple records
- Front Office/Financials.Source Reports — multiple records

Daily Baseball.Source Reports is the Airtable-created reciprocal backlink from Source Reports.Game Record. Rename the backlink; do not create a duplicate manual link.

## Acquisition and financial link

Create Front Office/Financials.Acquisition Record as a one-record link to Acquisitions. Rename the reciprocal backlink in Acquisitions to Financial Records.

## Correction self-links

Create one self-link in each historical table:

- Daily Baseball.Supersedes Record
- Hitter Zone Reports.Supersedes Record
- Pitcher Zone Reports.Supersedes Record
- Acquisitions.Supersedes Record
- Front Office/Financials.Supersedes Record
- Source Reports.Supersedes Record

Allow one linked earlier version. Airtable will create a reciprocal backlink; rename it Superseded By. Never link a record to itself.

## Required lookups

Create lookups only after their source links exist.

| Lookup table | Link source | Lookup fields |
| --- | --- | --- |
| Player Development | Player Name | Player ID; Position; Level; Overall Rating; Potential |
| Player Development | Latest Game Record | Latest Game ID |
| Hitter Zone Reports | Player Name | Player ID |
| Hitter Zone Reports | Game Record | Game ID |
| Pitcher Zone Reports | Player Name | Player ID |
| Pitcher Zone Reports | Game Record | Game ID |
| Acquisitions | Player Name | Player ID |
| Front Office/Financials | Player Name | Player ID |
| Source Reports | Game Record | Game ID |

## Link matching and safety rules

1. Create the destination record before linking it.
2. Select the existing record from the linked-record picker.
3. Do not convert unscreened free text into a linked-record field; Airtable may create unintended records for unmatched values.
4. Verify the displayed primary ID and readable player name before saving a player link.
5. Creating the link field alone does not populate links; connect test records manually.
6. Use Airtable reciprocal backlinks instead of creating duplicate two-way fields.
7. If a link points to a superseded version, replace it only after the correction has been verified.
8. Missing or ambiguous identity stops the player link and any automatic Player Development update.

## Duplicate audit

Before accepting a record:

- Sort or group Colorado Rockies Base by Player ID and confirm uniqueness.
- Sort or group Daily Baseball by Game Record ID and Game ID.
- Sort or group each report table by its primary report ID.
- Reject any duplicate primary report ID.
- Multiple Daily Baseball versions may share Game ID only when their Game Record IDs differ and Supersedes Record correctly forms the version chain.

## References

- Airtable: Linking records — https://support.airtable.com/docs/linking-records-in-airtable
- Airtable: Lookup field overview — https://support.airtable.com/docs/lookup-field-overview
- Airtable: The primary field — https://support.airtable.com/docs/the-primary-field
