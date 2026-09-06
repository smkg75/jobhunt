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
Resume field below. **It parses the PDF but does not fill the Resume field: upload twice**, once on
the autofill input, once on the Resume input. See § Traps.

Text fields, text areas and date fields take `form_input`. A date field with the `Pick date...`
placeholder accepts an ISO value and renders it back in US order, so `2026-09-02` shows as
`09/02/2026`: read the rendered value before trusting it.

Yes/No toggles and radio buttons need a coordinate click, not a `ref` click. See § Traps.

On a multi-step form, clicking continue on an empty step makes the form list its required fields.

## Traps

**"Autofill from resume" does not attach the resume.** It parses the PDF into the text fields and
leaves the Resume `<input type=file>` empty, so a form filled that way fails on a required field
nothing flags. Upload the same file a second time on the Resume input, whose id is
`_systemfield_resume`, and read `input.files[0]` back before moving on.

**What the parser fills, it fills wrong as often as right.** On a French resume it wrote the surname
in capitals into "Preferred First & Last Name" ("First LAST") and left "Phone Number" empty, while
email and LinkedIn came back correct. Re-read every field it touched, and every field it did not.

**Required is a CSS class, not an attribute.** The red asterisk lives in the label's class list, so
the whole form reads in one call, no scrolling and no screenshots:

```javascript
Array.from(document.querySelectorAll('div[data-field-path]')).map(d => {
  const lab = d.querySelector('label'), ctl = d.querySelector('input,textarea,select');
  return [d.getAttribute('data-field-path'), lab && lab.innerText.trim(),
          lab && /_required_/.test(lab.className) ? 'REQ' : 'opt',
          ctl && ctl.tagName + ':' + ctl.type].join(' ~ ');
});
```

**A field the DOM calls `input[type=text]` can still be a combobox.** The location field and single
select custom questions render as text inputs and open a suggestion list: `form_input` types into
them, then the option has to be clicked at its coordinates. The location list repeats the same city
twice, and offers the American homonyms below it, so "Paris, France" sits at rows one and two with
Paris, Texas underneath.

**Typing filters a closed list down to what looks like free text.** "How did you hear about us?"
took the word "Indeed" and showed a single option, "Job Board (Indeed, LinkedIn Jobs, etc.)". The
value that registers is the option, never the typed text.


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

**The Phone field takes `form_input` and drops it.** The value appears in the input, the DOM shows
it, and the submit fails on "Missing entry for required field: Phone". React never registered the
change. Type that field on the keyboard instead, and read it back from the DOM before submitting —
the other text fields do accept `form_input`, so this one looks like it worked.

**An invisible reCAPTCHA sits in the footer.** Nothing to solve, it only fires on submit.

## Last tested

2026-09-06 — application sent, single-page form, Phone trap found
