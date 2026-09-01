# Workday

## Recognize

`<company>.wd<N>.myworkdayjobs.com/…`. A branded careers page such as
`jobs.<company>.com/jobs/job/<id>-<slug>/` redirects there on "Apply Now".

## Reach the form

"Apply Now" opens a landing page offering three routes: Autofill with Resume, Apply Manually, Use My
Last Application. Take **Apply Manually** — its URL is the apply URL with `/applyManually` appended,
where `/autofillWithResume` reaches the first route.

An account is required before the wizard starts. **The agent never creates an account**: an auth gate
that the session cannot pass ends the work here, with the gate named.

Signed in, the wizard runs over six pages: My Information, My Experience, Application Questions,
Voluntary Disclosures, Self Identify, Review. "Save and Continue" at the bottom of each page moves to
the next.

## Read a company's openings

No keyless API. Read the company's Workday listing page.

## Fill

`read_page` returns only what the viewport shows, so a long page is scrolled notch by notch and
re-read at each position before anything is written.

Radio buttons never come back from `read_page`, whatever is on screen: `find` locates them and a
`computer` click checks them.

Dropdowns are buttons that open a popup panel — click the button, `find` the option, click it. A
hierarchical dropdown such as "How did you hear about us?" is filtered through the search box inside
its popup.

Clicking "Save and Continue" on an empty page makes the form list every required field it holds,
with a link to each. That is the cheapest way to enumerate a page.

## Traps

Fields are discovered page by page: a wizard page reveals nothing about the next one, so the form is
scouted six times rather than once.

## Last tested

never
