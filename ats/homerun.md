# Homerun

## Recognize

`<slug>.homerun.co/<job-slug>`. The posting and the form share that host; the form is the posting URL
suffixed with `/apply`, and the step lives in the query string as `?&step=1..4`.

## Reach the form

The "Apply" link on the posting, in the banner and again at the foot of the page, both point at
`<posting-url>/apply`. No account, no auth gate. Navigating straight to that URL works.

**The step parameter is not a way in.** Loading `?&step=2` before step 1 is filled bounces back to
step 1 and reveals a hidden "Applying for this job is currently not possible." string that belongs to
the error block, not to the form: it says nothing about the posting being closed.

## Read a company's openings

The careers page listing on the same host. There is no keyless API.

## Fill

**The whole form is in the DOM from the first load**, every step included, the ones that are not
current being hidden rather than absent. One `read_page` on step 1 therefore reads the entire form,
custom questions and submit button included, before a single field is written. Scout it that way
rather than filling a page to discover the next.

Plain inputs throughout, `form_input` on each. The resume is an `<input type=file>` that `file_upload`
takes by its `ref`; the "Select file" button beside it opens a native picker and stays unclicked.
A successful upload leaves the file name displayed as a chip with a "remove" control, and the same
name appears in the step 3 Preview block.

Yes/No questions are a pair of `<button type="button">`, not radios. The selected one fills with the
brand colour; the Preview block prints the chosen word, which is the reading that counts.

## Traps

**Two unlabelled `textbox` elements open the `<form>`, before the page heading: they are honeypots.**
They sit above "First name" in the accessibility tree and look like ordinary text inputs. Filling
either one is what a bot does. Leave them strictly empty.

**The step buttons ignore a `ref` click.** "Next: Questions" reported a successful click and the form
stayed on step 1; the same click at the button's coordinates advanced it. The URL's `step=` parameter
is what confirms the move.

**The cookie banner has no accept control** — one "Learn more" link and a dismiss cross, nothing to
consent to. It floats over the lower third of the viewport and can cover a field: scroll rather than
dismiss it, since the cross is the only thing to click and clicking it consents to nothing either way.

**Step 3 is a real review page.** It reprints identity, resume file name and every answer, and an
unanswered required question shows there as "no answer". It is the cheapest place to verify a form
before submitting.

## Last tested

2026-09-08 — one application submitted end to end, four steps, seven identity fields plus one custom
question. Found: the whole form ships in the DOM at first load, the two honeypot inputs, and the step
buttons that only answer to a coordinate click.
