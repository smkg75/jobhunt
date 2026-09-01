# hiring.cafe

## Access

Browser, no account. A meta-board that aggregates the ATS feeds of many companies, so it surfaces
postings that never reach the large boards.

## Search

`https://hiring.cafe` — search by role, then apply the date and location filters. Read the result
list by targeted extraction.

## Read a posting

A result row is a pointer, not the posting. Click through to the employer page and read the posting
there, then extract its body with `javascript_tool` on the description container rather than the
whole page.

## Apply on this board

None. The employer page is an ATS: `ats/index.md`.

## Traps

Only the employer URL is recorded — in `posting.md`, in `DATA_DIR/job-history.md`, and in anything
shown to the candidate. A hiring.cafe link is a search artefact and goes nowhere durable.

## Last tested

never
