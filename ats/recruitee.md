# Recruitee

## Recognize

`<slug>.recruitee.com/o/<job-slug>`.

## Reach the form

Not yet met: no application has run on this ATS. The first one writes this section.

## Read a company's openings

`https://<slug>.recruitee.com/api/offers/` — keyless, JSON, one entry per open offer. One offer:
`https://<slug>.recruitee.com/api/offers/<job-slug>`. Each entry carries the office address, a
`remote` boolean and the publication date. Slugs seen: `<slug>`, `<slug>`.

## Fill

Not yet met.

## Traps

`remote: true` next to a city is not a contradiction the API resolves: an employer lists Paris
as the city and `remote: true` on the same offer, so the real location is a question for the first
exchange, not a fact the row settles. `remote: false` with a foreign address does settle it: the
an employer role that hiring.cafe listed in Paris was a London on-site role on this API.

## Last tested

2026-09-02 — openings API only, no form reached.
