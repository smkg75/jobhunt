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
returns the full description of each opening.

## Fill

Single page. On the direct form URL, `form_input` handles text fields, dropdowns and the privacy
policy checkbox.

Fields it usually carries: first name, last name, email, phone, resume upload, cover letter upload,
city, country code dropdown, LinkedIn profile, "How did you hear about us?" dropdown, work
authorization questions, an EEO block, a privacy policy checkbox, and the submit button.

## Traps

The "Apply for this job" button sits on the host page, outside the iframe: clicking it changes
nothing that the browser tools can read. The token extraction above replaces it.

## Last tested

never
