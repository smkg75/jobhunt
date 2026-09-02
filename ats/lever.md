# Lever

## Recognize

`jobs.lever.co/<slug>/<job-id>`.

## Reach the form

Append `/apply` to the posting URL. The "APPLY FOR THIS JOB" button on the posting does the same.
No iframe: the form renders natively on the page.

## Read a company's openings

`https://api.lever.co/v0/postings/<slug>?mode=json` — keyless.

## Fill

Single page, and the whole toolbox works: `read_page` returns every field, `find` locates any of
them, `form_input` writes to all of them.

Fields it usually carries: location combobox, resume upload, full name, pronouns checkboxes, email,
phone, current location, current company, LinkedIn, Twitter, GitHub, portfolio and other website
URLs, company-specific sections such as visa sponsorship or acknowledgements, an "Additional
Information" textarea, an EEO survey, and the submit button.

**"Additional Information" is the cover letter field.**

## Traps

The location combobox stores GUID values behind its labels: write the text with `form_input`, then
pick the matching suggestion so the value is bound.

**"Current location" is that combobox, and it clears itself on blur.** The field reads as
`input[name=location]` and takes typed text, but leaving it without clicking a suggestion wipes the
value and the form goes back empty. Type the city, click the suggestion, and read the hidden
`input[name=selectedLocation]` back: it holds a JSON object with the label and its id. The list
offers administrative labels rather than the country form, so "Paris, France" is not on it: the
options are "Paris, Île-de-France, FRA" and "Paris, Département de la Savoie, FRA".

`createdAt` in the API is the date the posting was created, not the date it was last published: a
posting created eighteen months ago can still sit on the board and be listed as fresh by a job
board. Freshness read from `createdAt` is a floor, not the truth; a posting the board still lists is
noted as such in the reason line.

## Last tested

2026-09-03 — a single-page form on `jobs.lever.co/<slug>` with six
custom question cards, filled end to end, not submitted.
