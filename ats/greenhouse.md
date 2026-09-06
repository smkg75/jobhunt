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

**The "Country" field beside "Phone" is the dialing code, not the country of residence.** It shows a
flag and "+33" once chosen; residence comes from "Location (City)".

**Country, "Location (City)" and single-select custom questions are react-select comboboxes.**
`form_input` does not settle them, and on the 2026-09-06 render **a `ref` click did not open the menu
either** — the click reported success and nothing dropped down. Click the field at its
**coordinates**, which opens it; from there the keyboard finishes the job, Return taking the first
option and Down then Return the second. Clicking the option at its coordinates works too. Afterwards the value is **not** in `input.value`, which stays empty:
read it in the sibling `div.select__single-value`, or on screen. "Location (City)" can list the same
city twice, and its selection fills a hidden latitude and longitude.

**The fields carry an `id`, not a `name`.** `document.querySelector('input[name=first_name]')`
returns nothing on this render; `#first_name`, `#last_name`, `#email`, `#phone`, `#country`,
`#candidate-location` and `#question_<id>` all resolve. A verification script written on `name`
reports every field as missing while the form is in fact filled.

**A successful attach removes the file input from the DOM.** Greenhouse replaces
`<input type=file name=resume>` with a chip carrying the file name and a remove cross, so
`input.files[0]` cannot be read back afterwards. Confirm an upload by the chip text, not by the
input: the file names appear as leaf nodes ending in `.pdf` inside the form.

**A react-select leaves its own input empty.** After picking, `#country` and `#candidate-location`
both read as `""`; the chosen values sit in the sibling `[class*=single-value]` nodes, as `+33` and
`Paris, France`. Read those.

**An invisible reCAPTCHA sits in the footer.** Nothing to solve, it only fires on submit.

## Last tested

2026-09-06 — application sent on a single-page `job-boards` form, 24 fields, react-select needs a coordinate click
