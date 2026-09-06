# House form

The employer's own form, on the employer's own domain. Not an ATS, and the fallback when no pattern
of `ats/index.md` matches.

## Recognize

No ATS pattern matches and the form is served by the company's own host. Two shapes met so far: a
WordPress form, which gives itself away by `form.wpcf7-form` and its `_wpcf7*` hidden inputs, and a
hand-built form posting to the same host.

## Reach the form

An "Envoyez votre candidature" / "Apply" button on the posting page, which opens the form in place or
on a page of the same site. No account, no auth gate.

## Read a company's openings

The careers page listing. There is no API and no feed: the page is the only source, and its posting
URL is the durable one.

## Fill

Plain inputs and textareas, `form_input` throughout. A hidden honeypot field sits among them — a
`website` input on a form that already asks for LinkedIn, or the `_wpcf7*` block — and it stays
empty. The resume input often caps the file size in its label alone.

## Traps

**The confirmation is a class, not a page.** A WordPress form neither redirects nor reloads: it adds
`sent` to the form's class list and prints its message in place, and `wpcf7-not-valid-tip` marks a
field it refused. Those two are the only proof the application left.

**An invisible reCAPTCHA v3 fires on submit** with nothing to solve. It is not a blocker.

**A 2 MB cap on the resume is common**, stated in the label and nowhere else. A canonical resume
heavier than that is rebuilt with its images recompressed into `tailored-resume/images/`, which the
source reads before `resume/images/`.

## Last tested

2026-09-06 — two house forms, one WordPress (seven fields, 2 MB resume cap), one hand-built (six
fields, honeypot)
