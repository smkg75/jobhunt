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

**The location and the date shown are the board's own enrichment, not the posting's.** hiring.cafe
attributes the employer's offices to every one of its postings, so a role open in one city surfaces
under a filter on another: a role open only in London (`remote:false` on the employer's own ATS) is
listed "Paris, Île-de-France" because Paris is where that employer is headquartered. Its
`datePosted` drifts too — one posting read 2026-07-07 here and 2026-08-18 on the employer's ATS. Every retained line is re-read on the employer's ATS before it is scored, and
the ATS is what the row records.

The apply button is a script, not a link: the employer URL never appears in an `href`. It is read out
of the page's own scripts, or by opening the employer page the button leads to.

**The URL carries the whole search.** `?searchState=<url-encoded JSON>` holds `searchQuery` and
`locations`, so a query runs by navigation alone, no typing. A hand-built `place_id` answers "Failed
to load jobs": set the location once through the UI, then reuse the `searchState` the site writes and
swap only `searchQuery`. Leaving `options.flexible_regions` filled pulls in remote roles from the
whole country and continent — empty it to keep the region.

A natural-language query is parsed into filters, and an over-specified one filters everything out:
a role title followed by a qualifier returned zero in Île-de-France where a neighbouring title alone
returned sixty-four. Drop the qualifier before concluding the board is empty.

## Last tested

2026-09-02
