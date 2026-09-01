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

## Last tested

never
