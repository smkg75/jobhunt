# Applicant tracking systems

One file per ATS, all built on the same skeleton. A posting's apply link lands on one of these; the
URL says which, and the file says how its form behaves.

The ATS is read off the page that serves the form, never inferred from a domain that happens to
resolve: a company whose name answered on a careers-platform subdomain was recorded as filing through
that platform, and files through a house form.

| ATS | File | URL pattern |
|---|---|---|
| Greenhouse | `ats/greenhouse.md` | `boards.greenhouse.io/<slug>`, `job-boards.greenhouse.io`, any page holding `grnhse_iframe` |
| Lever | `ats/lever.md` | `jobs.lever.co/<slug>/<job-id>` |
| Workday | `ats/workday.md` | `<company>.wd<N>.myworkdayjobs.com`, and branded careers pages that redirect there |
| Teamtailor | `ats/teamtailor.md` | `<slug>.teamtailor.com/jobs`, `careers.<company>.com/jobs/<id>-<slug>` |
| Ashby | `ats/ashby.md` | `jobs.ashbyhq.com/<slug>` |
| Workable | `ats/workable.md` | `apply.workable.com/<slug>` |
| Rippling | `ats/rippling.md` | `ats.rippling.com/<slug>/jobs` |
| Deel | `ats/deel.md` | `jobs.deel.com/<slug>` |
| Recruitee | `ats/recruitee.md` | `<slug>.recruitee.com/o/<job-slug>` |
| Taleez | `ats/taleez.md` | `taleez.com/apply/<job-slug>`, `<slug>.taleez.com` |
| House form | `ats/house-form.md` | no pattern above matches; the employer's own domain serves the form |

## Finding the ATS of a company

A company whose `ATS` and `slug` columns are blank in `DATA_DIR/companies.md`: probe the keyless
endpoints of the § Read a company's openings sections against its slug, all at once. Only a `200`
answers, and it names the ATS.

```bash
for s in acme othercorp; do
  for u in "https://boards-api.greenhouse.io/v1/boards/$s/jobs" \
           "https://api.ashbyhq.com/posting-api/job-board/$s" \
           "https://api.lever.co/v0/postings/$s?mode=json" \
           "https://$s.teamtailor.com/jobs"; do
    [ "$(curl -s -o /dev/null -w '%{http_code}' -L --max-time 8 "$u")" = 200 ] && echo "$s -> $u"
  done
done
```

`--max-time` is not optional: one endpoint that hangs holds the whole pass. The list of URLs is
whatever the ATS files carry — an endpoint added to one of them belongs in this loop too, and an ATS
reachable several ways lists them all in its own file.

A slug that answers nowhere proves nothing about the ATS: the slug is usually the wrong guess, and
the careers page is what settles it. What the probe finds fills both columns in passing.

## Replacing a file already attached

A resume rebuilt from the same source keeps its file name, and often its byte size to the byte: on
2026-09-03 nine tailored resumes were recompiled with a single word changed and every PDF came out
the exact size it was before. So neither the name shown on screen nor `files[0].size` proves that a
replacement went through. Whatever the ATS, the only proof is the transition:

1. read the current state,
2. detach the file and **confirm the `input[type=file]` came back and is empty**,
3. upload,
4. confirm the file name is displayed again — and, where the ATS swallows the input after an
   upload, that the input has disappeared once more.

Steps 2 and 4 together are what proves a fresh upload happened. On Ashby and Lever the input
survives the upload and holds the `File` object, so reading `files[0].name` back is enough. On
Greenhouse and Teamtailor the input is removed and replaced by a pill, and the detach-then-reattach
dance is the only way to be sure.

Never leave a form with no resume attached: if a detach succeeds and the re-upload fails, say so
first and plainly.

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

**Last tested** — the date the file was last exercised against a live form, or `never`, and what
was exercised. Never the employer, the role or the posting URL: this file records the ATS, not where
the candidate applied. Those belong in `DATA_DIR`.

## Adding an ATS

A form met on an ATS with no file here earns `ats/<name>.md` on the skeleton above, plus a row in
each table. An inherent trap of that ATS belongs in that file, where the next application finds it.
