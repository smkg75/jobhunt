# Ashby

## Recognize

`jobs.ashbyhq.com/<slug>`.

## Reach the form

The apply button on the posting opens the form, which runs in steps. The form also has its own URL:
the posting URL suffixed with `/application`, reachable directly, no click needed.

## Read a company's openings

`https://api.ashbyhq.com/posting-api/job-board/<slug>` — keyless. `publishedAt` is the publication
date, `descriptionPlain` the full posting text, `workplaceType` (OnSite / Hybrid / Remote) the
location policy; `compensation` is often null even when the text states a range.

## Fill

Uploading the resume pre-fills several fields at once. Re-read every pre-filled field before moving
on: the parser guesses, and a wrong guess is submitted as an answer.

The "Autofill from resume" box at the top of the form is a separate `<input type=file>` from the
Resume field below. `file_upload` on the autofill input fills both: it parses the PDF and lands the
same file in the Resume field. One upload, not two.

Text fields, text areas and date fields take `form_input`. A date field with the `Pick date...`
placeholder accepts an ISO value and renders it back in US order, so `2026-09-02` shows as
`09/02/2026`: read the rendered value before trusting it.

Yes/No toggles and radio buttons need a coordinate click, not a `ref` click. See § Traps.

On a multi-step form, clicking continue on an empty step makes the form list its required fields.

## Traps

**That empty-continue probe belongs to multi-step forms only.** On a single-page Ashby form the same
button is the submit button, so count the steps before using it.

**Yes/No toggles and radio buttons swallow a `ref` click.** `computer` `left_click` with a `ref`
reports success on the two-button Yes/No control and on a radio option, and nothing is selected. The
same click at the element's coordinates takes. Screenshot after each one: the selected state is the
only proof, and a required toggle left unset blocks the submit silently.

**Required fields carry a red asterisk on the label, and nothing marks the optional ones.** The
accessibility tree does not carry that asterisk, so `read_page` cannot tell required from optional
here: scroll and screenshot the whole form to read them.

**The label text in the accessibility tree is truncated at about 100 characters.** A long custom
question comes back cut mid-word. Read the form container's `innerText` for the full wording before
writing an answer to it.

**An invisible reCAPTCHA sits in the footer.** Nothing to solve, it only fires on submit.

## Last tested

2026-09-02 — an employer, VP Revenue (`jobs.ashbyhq.com/<slug>`), single-page form filled end to
end, not submitted.
