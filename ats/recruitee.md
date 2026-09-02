# Recruitee

## Recognize

`<slug>.recruitee.com/o/<job-slug>`.

## Reach the form

The posting page carries two tabs, "Job details" and "Application". The form lives under the second
one and has its own URL: the posting URL suffixed with `/c/new`, reachable directly, no click
needed. The offers API names it too, in `careers_apply_url`.

No auth gate, no account, no consent banner on the form page.

**Read the form from the API before opening it.** `https://<slug>.recruitee.com/api/offers/<job-slug>`
describes the whole form ahead of the browser:

- `options_cv` — `required`, `optional` or `off`.
- `options_cover_letter` — same three values. **`off` means there is no letter field at all**, text
  or file.
- `options_phone`, `options_photo`, `options_salutation`, `options_title` — same shape.
- `open_questions` — the custom questions, verbatim, each with its `kind` (`string`, `boolean`,
  `multi_choice`…) and its `required` flag.

That answers step 4 of `skills/apply/SKILL.md` before a single screenshot, and the browser pass only
confirms it.

## Read a company's openings

`https://<slug>.recruitee.com/api/offers/` — keyless, JSON, one entry per open offer. One offer:
`https://<slug>.recruitee.com/api/offers/<job-slug>`. Each entry carries the office address, a
`remote` boolean and the publication date. Slugs seen: `<slug>`, `<slug>`.

## Fill

A short identity block, then a file field, then the company's own questions. One page, one submit
button labelled **"Send"**.

- Text, email and phone fields take `form_input`.
- **The phone field arrives pre-filled with the country dialling code** (`+33` with the country list
  already on France for a French posting). Write the whole number over it, dialling code included,
  then read the value back.
- The resume goes in through the `<input type=file>`: `read_page` on the form container exposes it as
  `button "CV or resume" type="file"`, and `file_upload` takes its `ref`. The "Upload a file" text
  and the drop zone stay unclicked. Once uploaded, the zone shows the file name plus "Change file"
  and a delete button.
- Radio buttons need a coordinate click, not a `ref` click. See § Traps.
- Field names are readable in the DOM (`candidate.name`, `candidate.email`, `candidate.phone`,
  `candidate.cv`, `candidate.openQuestionAnswers.<id>.content` for a string question,
  `candidate.openQuestionAnswers.<id>.flag` for a boolean). A `javascript_tool` one-liner over
  `document.querySelectorAll('input, textarea, select')` reads every value back in one call, which is
  faster than screenshotting each field.

## Traps

**Radio buttons swallow a `ref` click.** `computer` `left_click` with a `ref` on a radio option
reports "Clicked on element" and `input[type=radio].checked` stays `false`. The same click at the
element's coordinates takes. Read the value back before moving on: a required radio left unset blocks
the submit. Same trap as Ashby.

**An hCaptcha sits in the form.** The accessibility tree carries a node "Widget containing checkbox
for hCaptcha security challenge" while nothing renders on screen. Nothing to solve at fill time; the
challenge can surface when the submit button is clicked, so a `draft` run must warn the candidate
that it may appear under their own click.

**`remote: true` next to a city is not a contradiction the API resolves**: an employer lists
Paris as the city, `Remote job` as the location and `remote: true` on the same offer, and the page
header shows "Paris (France)". The real location is a question for the first exchange, not a fact the
row settles. `remote: false` with a foreign address does settle it: the an employer role that
hiring.cafe listed in Paris was a London on-site role on this API.

**No "Apply with LinkedIn" button and no profile import**, at least on the forms met so far: the
identity fields are filled by hand.

**Required fields carry a red asterisk on the label.** The accessibility tree does carry the hint,
each required field holding a sibling "This field is required and can not be left empty."

## Last tested

2026-09-02 — an employer, Account Executive Enterprise France
(`<slug>.recruitee.com`), form reached at `/c/new` and filled end to end, six fields plus the
resume upload, not submitted.
