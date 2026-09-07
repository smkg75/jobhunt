# Greenhouse

## Recognize

`boards.greenhouse.io/<slug>`, `job-boards.greenhouse.io`, or a company page that holds an element
with `id="grnhse_iframe"`.

## Reach the form

The form lives in a cross-origin iframe, invisible to `read_page`, `find` and `form_input`. Only
`javascript_tool` reaches the host page. Read the two tokens from the iframe source — the security
filter blocks a full URL, so pull the parameters one by one:

```javascript
const iframe = document.getElementById('grnhse_iframe');
const url = new URL(iframe.src);
JSON.stringify({
  boardToken: url.searchParams.get('for'),
  jobToken: url.searchParams.get('token')
});
```

Then navigate to `https://job-boards.greenhouse.io/embed/job_app?for=<board>&token=<job>`. The form
loads as a top-level page, where the browser tools work normally.

## Read a company's openings

`https://boards-api.greenhouse.io/v1/boards/<slug>/jobs?content=true` — keyless, and `content=true`
returns the full description of each opening. The only date is `updated_at`: there is no creation
date, so a board-wide update stamps every opening with the same day.

## Fill

Single page. On the direct form URL, `form_input` handles text fields and the privacy policy
checkbox. Dropdowns are custom components, not `<select>`: see § Traps.

Fields it usually carries: first name, last name, email, phone, resume upload, cover letter upload,
city, country code dropdown, LinkedIn profile, "How did you hear about us?" dropdown, work
authorization questions, an EEO block, a privacy policy checkbox, and the submit button.

## Traps

The "Apply for this job" button sits on the host page, outside the iframe: clicking it changes
nothing that the browser tools can read. The token extraction above replaces it.

**A `job-boards.greenhouse.io/<slug>/jobs/<id>` URL already is the form.** The posting and the form
share that one page, the form sitting below it under "Apply for this job"; its "Apply" button is a
plain anchor. No iframe, no token extraction: that recipe is for a form embedded in a company site.

**`read_page` returns only what the viewport holds.** On that page the first read gave back the
"Apply" button alone while the fifteen form fields were already in the DOM. Scroll to the block, then
read again, or reach the field with `find`.

**"Autofill my application" opens a MyGreenhouse sign-in in a new tab**
(`my.greenhouse.io/users/sign_in?initiator=autofill`), asking for an email and an account. It cannot
run without the candidate: close that tab and fill by hand.

**A custom-question field can be a single-line `input` capped at 255 characters, even when its
question calls for a paragraph.** It renders as `class="input__single-line"` with `maxlength="255"`,
and anything typed past the limit is dropped silently — no error, no truncation warning. Before
writing a long answer, read the field's `maxlength` and class and measure the answer against it; if it
overruns, compress it under the limit and carry the full version into an "Additional Information"
field, which is a real, unlimited textarea.

**The "Country" field beside "Phone" is the dialing code, not the country of residence** (residence
comes from "Location (City)"), **and that Country+Phone block is an `intl-tel-input` widget, not a
react-select** — it refuses any value set from JavaScript. The screen can show the right flag and the
right number while the submission still fails on "Select a country" and "Phone is required". Drive it
by hand instead: **click** the country selector, **type** the country name, click the matching option,
then **type** the phone number on the keyboard.

**That same widget swallows the `+` and any spaces.** Typing the number in international format
(`+33 6 …`) comes out with the `+` stripped and the dial code re-prefixed, producing a wrong number.
Type the **national format** instead — the local digits with the leading `0` — and the widget
reformats it correctly under the chosen flag.

**A validation error does not clear itself once the field is fixed.** "Select a country" or "Phone is
required" can still show on screen after the correction: it is only re-evaluated on the next submit
attempt. Don't read that as the fix having failed, and don't loop on re-fixing an already-correct
field.

**"Location (City)" and single-select custom questions are react-select comboboxes.** `form_input`
does not settle them, and no coordinate click is needed either: call `element.focus()` in
`javascript_tool`, then send **Down** (opens the menu and highlights the first option) and **Return**
(commits it) — `Down Down Return` for the second option. Afterwards the value is **not** in
`input.value`, which stays empty: read it in the sibling `[class*="single-value"]` node instead (e.g.
`Paris, France`). "Location (City)" can list the same city twice, and its selection fills a hidden
latitude and longitude.

**A plain text field driven from `javascript_tool` needs the native setter, not `element.value =
…`.** React ignores a direct assignment and keeps its own state regardless. Set it with
`Object.getOwnPropertyDescriptor(window.HTMLInputElement.prototype, 'value').set.call(element, value)`
(`HTMLTextAreaElement.prototype` for a textarea), then dispatch a bubbling `input` event — that
combination is what React's `onChange` actually picks up.

**The fields carry an `id`, not a `name`.** `document.querySelector('input[name=first_name]')`
returns nothing on this render; `#first_name`, `#last_name`, `#email`, `#phone`, `#country`,
`#candidate-location` and `#question_<id>` all resolve. A verification script written on `name`
reports every field as missing while the form is in fact filled.

**A successful attach removes the file input from the DOM.** Greenhouse replaces
`<input type=file name=resume>` with a chip carrying the file name and a remove cross, so
`input.files[0]` cannot be read back afterwards. Confirm an upload by the chip text, not by the
input: the file names appear as leaf nodes ending in `.pdf` inside the form.

**An invisible reCAPTCHA sits in the footer.** Nothing to solve, it only fires on submit.

## Last tested

2026-09-07 — application submitted after a first rejected attempt: the intl-tel-input country+phone
block and a 255-character custom-question cap were the two blockers; react-select now settles by
keyboard focus, not a coordinate click.
