# Teamtailor

## Recognize

`<slug>.teamtailor.com/jobs`, or a company-branded `careers.<company>.com/jobs/<id>-<slug>` — the
`/jobs/<id>-<slug>` path is the signature that survives the branding.

## Reach the form

The apply button on the posting opens the form on the same page.

## Read a company's openings

No keyless API. Read the listing page `https://<slug>.teamtailor.com/jobs`.

## Fill

A short identity form followed by the company's own questions. The whole toolbox works:
`form_input` for the fields, one click per checkbox.

## Traps

"Postuler via LinkedIn" / "Connect with LinkedIn" sits at the top of the identity block. Try it
first: it pre-fills the identity fields from the LinkedIn profile, and the resume PDF still goes into
"Importer un CV" afterwards — the import does not replace that field. If it opens a LinkedIn sign-in
the session cannot complete, leave it and fill the fields by hand rather than staying stuck. On
2026-09-02 a pass declined the button on the strength of the old note here, and the candidate had to
click it himself.

The form is a full-screen modal on the posting's own URL: `tabs_context_mcp` shows the posting URL,
not a form URL. Reloading the page drops the modal and everything typed or imported into it.

**The modal can hang on "Loading application form".** On 2026-09-03 it opened, showed the
posting header and a spinner, and never rendered a field, through a scripted click and a real one
alike. The form has its own address: **`<posting-url>/applications/new`** renders the same form as
a full page, complete, and the browser tools work on it normally. Reach for that URL as soon as the
spinner outlives a few seconds, and note that nothing is lost by navigating while the modal is
still empty.

## Last tested

2026-09-03 — a form on a branded domain, thirteen custom questions plus the
identity block and a cover letter textarea, filled end to end on `/applications/new`, not
submitted. Earlier: 2026-09-02.
