# Job history

## Applications
_One line per application sent or prepared: its identity, never its status. `folder` is the `jobs/`
folder holding it. A folder's current status is its last event in § Journal below._

| date | company | role | channel | fit | folder |
|---|---|---|---|---|---|
| | | | | | |

## Journal
_Append-only. One line per event, written the moment it happens. **Never edit or remove an existing
line.** A folder's current status is its last event here, and it is written nowhere else._

Closed vocabulary, ten events:
`ready` (filled, not submitted) · `blocked` (suspended, `apply` step 10) · `sent` · `sent-unconfirmed` ·
`ack` (an ATS's automated acknowledgement) · `reply` (a human reply) · `stage-booked` (an interview
landed) · `rejected` · `withdrawn` (application pulled) · `archived`

| date | folder | event | detail |
|---|---|---|---|
| | | | |

## Runs
_The newest block only, headed `## YYYY-MM-DD — run`. `Source` names the pass that wrote it,
`Queries` the queries it used; the table holds every posting seen, Skips included with their reason.
Earlier blocks live one per file in `runs/`, and a table at the head of this section lists them._

## YYYY-MM-DD — run

_Source:_ `job-search` or `network`
_Queries:_

| title | company | location | posted | salary | link | fit | notes |
|---|---|---|---|---|---|---|---|
| | | | | | | | |

## Rejected before <YYYY-MM>
_Rejections that predate the first run, kept so fit scoring can recognise them._

| date | company | role | reason |
|---|---|---|---|
| | | | |
