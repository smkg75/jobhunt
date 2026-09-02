# Applicant tracking systems

One file per ATS, all built on the same skeleton. A posting's apply link lands on one of these; the
URL says which, and the file says how its form behaves.

| ATS | File | URL pattern |
|---|---|---|
| Greenhouse | `ats/greenhouse.md` | `boards.greenhouse.io/<slug>`, `job-boards.greenhouse.io`, any page holding `grnhse_iframe` |
| Lever | `ats/lever.md` | `jobs.lever.co/<slug>/<job-id>` |
| Workday | `ats/workday.md` | `<company>.wd<N>.myworkdayjobs.com`, and branded careers pages that redirect there |
| Teamtailor | `ats/teamtailor.md` | `<slug>.teamtailor.com/jobs`, `careers.<company>.com/jobs/<id>-<slug>` |
| Ashby | `ats/ashby.md` | `jobs.ashbyhq.com/<slug>` |
| Workable | `ats/workable.md` | `apply.workable.com/<slug>` |
| Recruitee | `ats/recruitee.md` | `<slug>.recruitee.com/o/<job-slug>` |

## Skeleton

Every ATS file is `# <ATS>` followed by these six headings.

**Recognize** — the URL patterns that identify this ATS, including the branded domains it hides
behind.

**Reach the form** — the navigation from the posting to the fillable form, and the auth gate if
there is one.

**Read a company's openings** — the keyless API when the ATS publishes one, otherwise the careers
page listing. This is what fills `DATA_DIR/companies.md`. The `<slug>` is read from the careers URL:
it does not follow from the company name, and a wrong slug answers 404 on every one of these APIs.
Slugs already seen live in `DATA_DIR/companies.md`.

**Fill** — the tool that works for each field type here, radio buttons and custom dropdowns
included: a form filled without reading this section fights the platform. One rule holds on every
ATS: the resume goes in through the `<input type=file>` — `find` or `read_page` locates it,
`file_upload` takes its `ref`. The upload button itself stays unclicked, since it opens a native
picker the browser tools cannot see. `file_upload` reads only paths shared with the session, so a
refused path means starting the session from `DATA_DIR`.

**Traps** — what breaks on this ATS and the gesture that avoids it.

**Last tested** — the date the file was last exercised against a live form, or `never`.

## Adding an ATS

A form met on an ATS with no file here earns `ats/<name>.md` on the skeleton above, plus a row in
each table. An inherent trap of that ATS belongs in that file, where the next application finds it.
