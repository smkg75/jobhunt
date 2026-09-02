# LinkedIn Jobs

## Access

Browser only, signed-in session required.

## Search

`https://www.linkedin.com/jobs/search/?keywords=<role>&location=<region>&f_TPR=r604800&sortBy=DD`

`f_TPR=r604800` keeps the last seven days, `sortBy=DD` sorts by date. Add `f_AL=true` to isolate Easy
Apply postings.

The result list is virtualized: targeted extraction sees only the rendered rows. Two passes separated
by ten scroll notches cover about fifteen postings.

## Read a posting

Clicking a result title opens the detail panel, which reads as a single posting — `get_page_text`
applies here.

## Apply on this board

Easy Apply is the native flow: contact, resume, questions, review, `Submit`. Pick the PDF resume
rather than the LinkedIn profile, and read the review page before submitting.

For any other posting the Apply button serves one purpose: naming which ATS the company runs. Reach
that form from the company's careers page instead — `ats/index.md`.

## Export the connections

The connections export is what lets a pass flag a company where the candidate already knows someone.

1. Open linkedin.com/mypreferences/d/download-my-data
2. Pick "Connections" and request the download
3. LinkedIn mails a link, usually within minutes
4. Unzip it and find `Connections.csv`
5. Give the path here

## Traps

**The Apply button stays unclicked when it leads to an external ATS.** It opens a tab outside the MCP
group, invisible and undrivable, and the application is then lost from view.

LinkedIn rate-limits fast: run one search at a time, and stop at the first captcha or "unusual
activity" notice rather than retrying.

**`location=<text>` is silently dropped; only `geoId` filters.** A search on `location=Ile-de-France,
France` returned a page of Lyon roles. Île-de-France is `geoId=104246759`, and the parameter replaces
`location` rather than joining it.

`f_WT=1,3` keeps on-site and hybrid and drops the remote-only postings that otherwise fill a
region search — at the cost of the "France - Remote" ones, which `preferences.md` says not to skip.
Run at least one query without it.

The cookie banner reappears after a navigation and swallows clicks near the bottom of the window.
Decline the non-essential cookies each time it comes back.

Reading a posting through `/jobs/view/<id>` renders the header and nothing else — the description
stays empty. The description only renders inside the search page's detail panel: reach it with
`/jobs/search/?…&currentJobId=<id>`, scroll to the top, click "Voir plus", then read `#job-details`.

The virtualized list only fills the cards it has rendered: unrendered rows return an id and an empty
body, so a card with no text is a card not yet scrolled to, not an empty posting.

## Last tested

2026-09-02
