# LinkedIn Jobs

## Access

Browser only, signed-in session required.

## Search

`https://www.linkedin.com/jobs/search/?keywords=<role>&location=<region>&f_TPR=r604800&sortBy=DD`

`f_TPR=r604800` keeps the last seven days, `sortBy=DD` sorts by date. Add `f_AL=true` to isolate Easy
Apply postings.

The result list is virtualized: targeted extraction sees only the rendered rows. Two passes separated
by ten scroll notches cover about fifteen postings.

## Read a posting

Clicking a result title opens the detail panel, which reads as a single posting — `get_page_text`
applies here.

## Apply on this board

Easy Apply is the native flow: contact, resume, questions, review, `Submit`. Pick the PDF resume
rather than the LinkedIn profile, and read the review page before submitting.

For any other posting the Apply button serves one purpose: naming which ATS the company runs. Reach
that form from the company's careers page instead — `ats/index.md`.

## Export the connections

The connections export is what lets a pass flag a company where the candidate already knows someone.

1. Open linkedin.com/mypreferences/d/download-my-data
2. Pick "Connections" and request the download
3. LinkedIn mails a link, usually within minutes
4. Unzip it and find `Connections.csv`
5. Give the path here

## Traps

**The Apply button stays unclicked when it leads to an external ATS.** It opens a tab outside the MCP
group, invisible and undrivable, and the application is then lost from view.

LinkedIn rate-limits fast: run one search at a time, and stop at the first captcha or "unusual
activity" notice rather than retrying.

**`location=<text>` is silently dropped; only `geoId` filters.** A search on `location=Ile-de-France,
France` returned a page of Lyon roles. Île-de-France is `geoId=104246759`, and the parameter replaces
`location` rather than joining it.

`f_WT=1,3` keeps on-site and hybrid and drops the remote-only postings that otherwise fill a
region search — at the cost of the "France - Remote" ones, which `preferences.md` says not to skip.
Run at least one query without it.

The cookie banner reappears after a navigation and swallows clicks near the bottom of the window.
Decline the non-essential cookies each time it comes back.

Reading a posting through `/jobs/view/<id>` renders the header and nothing else — the description
stays empty. The description only renders inside the search page's detail panel: reach it with
`/jobs/search/?…&currentJobId=<id>`, scroll to the top, click "Voir plus", then read `#job-details`.

The virtualized list only fills the cards it has rendered: unrendered rows return an id and an empty
body, so a card with no text is a card not yet scrolled to, not an empty posting.

### Easy Apply, the SDUI modal

**The Easy Apply modal lives in a shadow root, so nothing ref-based can see it.** "Postuler chez
<company>" is not in the main document: it renders in the shadow root of a `div.theme--light`.
`read_page` and `find` return the job page and no dialog, so `form_input` has no ref to work with
and **`scripts/fill-page.md` cannot fill this form at all**. Reach the fields with
`javascript_tool`:

```javascript
const sr = Array.from(document.querySelectorAll('*')).find(e => e.shadowRoot).shadowRoot;
sr.querySelectorAll('input,select,textarea,button')
```

A text value is set with the native setter, then `input` and `change`, or Ember keeps the old one:

```javascript
Object.getOwnPropertyDescriptor(HTMLInputElement.prototype,'value').set.call(el, value);
el.dispatchEvent(new Event('input',{bubbles:true}));
el.dispatchEvent(new Event('change',{bubbles:true}));
```

**The resume cannot be uploaded where the input stands**, since `file_upload` needs a ref and the
shadow root gives none. Move the real `input[type=file]` into `document.body`, stashing its parent
and next sibling; give it an `aria-label`; `find` that label; `file_upload` on the ref; then put the
node back and dispatch `change`. LinkedIn reads the file and empties the input, so `files.length`
returning to 0 after an upload is the normal success state, not a failure.

**Two uploads under the same file name are indistinguishable on screen.** LinkedIn reuses the top
document card: same name, same size rounded to the Ko, same "Importé le" date, and even the same
ember id. After a resume is rebuilt and re-uploaded, nothing in the DOM proves which version is
attached. Two ways out, and the second is the reliable one: read the network line
`POST https://www.linkedin.com/ambry/?…&x-ambry-um-filename=<name>` and check it answers `201` —
but `read_network_requests` only records from its first call, so call it **before** uploading;
otherwise **upload the rebuilt file under a different file name**, which makes the selected card
unambiguous.

**The phone field wants the national number.** Its id ends in `phoneNumber-nationalNumber` and the
country code is a separate select, so the number goes in without its international prefix,
which the select already carries.

**The email field is a closed list** of the addresses verified on the LinkedIn account. An address
the account does not carry cannot be typed in, and adding one means touching account settings.
Read the value back and record the mismatch rather than trying to fix it.

**A click outside the modal closes it** and raises "Enregistrer cette candidature ?", whose only
buttons are Supprimer and Enregistrer. Its X returns to the form with every field intact.

**After the send, LinkedIn offers #OpenToWork.** Answer "Non merci": it changes the profile.

The confirmation to read is "Votre candidature a été envoyée à <company> !", and the posting itself
then carries "Statut de la demande : Candidature envoyée".

## Last tested

2026-09-09 — Easy Apply run end to end on one posting: modal scouted, phone and resume filled through the shadow root, application submitted and the confirmation read.
