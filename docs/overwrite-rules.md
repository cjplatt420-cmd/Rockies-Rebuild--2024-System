# Player Development Overwrite Rules

**Phase:** 2  
**Status:** Manual validation rules; no formulas or automation

Phase 2 uses direct field entry, links, and lookups only.

> Do not invent missing details or Player IDs.

## Core rule

Historical records are immutable and additive. Daily Baseball, Hitter Zone Reports, Pitcher Zone Reports, Acquisitions, Front Office/Financials, and Source Reports never overwrite an earlier historical record.

Only the single current Player Development record for a verified player may be updated.

## Eligibility gate

A proposed Player Development update may proceed only when all are true:

1. Player Name links to exactly one Colorado Rockies Base record.
2. Player ID lookup is present and verified.
3. The source record is linked.
4. Source coverage is explicitly labeled.
5. The source is the newest qualified evidence for the fields it owns.
6. Missing or no-grade input will not erase supported current evidence.
7. Any correction points to the earlier immutable version.
8. The update does not cross into fields owned by another workflow.

If any condition fails, stop and mark the update for review.

## Field ownership

| Source | Player Development fields it may update |
| --- | --- |
| Daily Baseball | Latest Game Record; Latest Game ID lookup; Latest Game Date; Latest-Game Status; Recent Positives; Recent Negatives; Latest Game Source Report; Latest Game Source Updated Date |
| Hitter Zone Reports | Hitter Trend Summary; Latest Hitter Source Report; Latest Hitter Source Updated Date |
| Pitcher Zone Reports | Pitcher Trend Summary; Latest Pitcher Source Report; Latest Pitcher Source Updated Date |
| Acquisitions | Acquisition Impact; Roster Impact; Latest Acquisition Source Report; Latest Acquisition Source Updated Date |
| Front Office/Financials | Financial/Control Impact; Latest Financial Source Report; Latest Financial Source Updated Date |
| Qualified Player Development synthesis | Current Trend; Current Development Advice; Current Organizational Role; Current Rebuild Fit; Watch Status |
| Colorado Rockies Base | Player identity, organization, level, roster status, ratings, potential, contract baseline, and baseline organizational role |

Last Updated Date and Current Record Version change whenever an approved Player Development update is applied.

## Fields no source may erase

A blank, missing, pending, incomplete, result-only, or no-grade source must not blank an already supported current value.

A source may explicitly replace a supported value only when:

- it owns the field,
- it is newer or a verified correction,
- the source record is linked, and
- the change is recorded in the update review.

## Final synthesis ownership

Daily Baseball and zone reports provide evidence. They do not directly set:

- Current Trend
- Current Development Advice
- Current Organizational Role
- Current Rebuild Fit
- Watch Status

Those fields require a qualified Player Development synthesis that considers the available game, zone, acquisition, financial, and baseline evidence without allowing one source to overwrite another source's domain.

## Manual update sequence

1. Open the verified Player Development record.
2. Confirm Player ID lookup.
3. Open the proposed source record and verify coverage and version.
4. Compare its source date and game version with the currently linked source for that domain.
5. Update only source-owned fields.
6. Link the new domain-specific Source Report.
7. Enter the domain-specific Source Updated Date.
8. If a qualified synthesis is warranted, update synthesis-owned fields separately.
9. Increment Current Record Version by one.
10. Update Last Updated Date.
11. Recheck that other domain fields did not change.

## Corrections and version chains

For every corrected historical record:

1. Preserve the earlier record unchanged.
2. Create a new primary record ID with the next VNN value.
3. Keep the same logical Game ID where applicable.
4. Link Supersedes Record to the immediately previous version.
5. Describe the exact change in Correction Notes.
6. Set the new record to Corrected or Complete as appropriate.
7. Set the earlier record to Superseded.
8. Move current Player Development source links only after verifying the corrected version.
9. Never delete the earlier evidence.

## Conflict rules

- Conflicting Player IDs: stop; do not link or update.
- Same player and same source date but different content: require correction/version review.
- Older source than the current domain source: preserve historically; do not overwrite current fields.
- Newer but incomplete source: preserve historically; do not erase stronger current evidence.
- Different workflows disagree: preserve both domain summaries and require Player Development synthesis.
- Unverified transaction: keep Acquisitions status Pending; do not change Colorado Rockies Base.
- Unverified final score or inning: mark pending or partial; do not complete the game record.

## Batting-stance rule

A batting-stance test may be recommended only after approximately 10–20 games show a repeated pitch-type, zone-location, and contact/result pattern.

One game, one at-bat, mixed evidence, or Spring Games 1–7 alone is insufficient. Any eventual recommendation is a user-play mechanics test, not a confirmed ratings or simulation improvement.

## Phase 2 audit record

For each manual Player Development test update, record outside the current fields:

- player reviewed,
- source record used,
- source domain,
- fields changed,
- fields intentionally preserved,
- current record version before and after,
- reviewer result: pass, fail, or needs correction.

Phase 2 does not automate this audit. The Spring Games 1–7 validation results will determine the later implementation.
