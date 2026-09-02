---
name: apply
description: Fill one job application end to end, from the posting to the record.
argument-hint: "job URL, 'last' for the most recent job folder, or 'current' for the open tab"
---

# Apply

One application = a folder, a form read whole before anything is written, a tailored resume, a letter when the form asks for one, answers drawn from those two, a status, a record. In that order; each step ends before the next begins.

Conflicts between rules: `shared/references/priority-hierarchy.md`. Browser tab and session safety: `shared/references/browser-setup.md`.

## Step 1 — Target

Resolve the data directory per `shared/references/data-directory.md`, then check prerequisites per `shared/references/prerequisites.md`.

`$ARGUMENTS`:

- a URL → the job folder that matches it in `DATA_DIR/jobs/` (company slug, or the `URL` line of `posting.md`), otherwise a new job folder named as `shared/templates/posting.md` names it.
- `last` → the most recently modified job folder.
- `current` → the form already open in the active tab; match its URL against the job folders to load the context.

`posting.md` absent or without `## Brief`: read the posting and write the file on `shared/templates/posting.md` — header, `## Brief`, `## Posting`.

Done when: the folder exists and `posting.md` carries its header, `## Brief` and `## Posting`.

## Step 2 — Application data

Read `DATA_DIR/application-data.md` `## Form sheet`. Absent: build it from the canonical resume, and ask the candidate one grouped question covering what the resume leaves open — work authorisation, sponsorship, notice, salary expectation, EEO — then save it. Under `run` there is nobody to ask: a field the resume leaves open surfaces at step 7 or step 10.

Done when: the form sheet is in context.

## Step 3 — Reach the form

Match the URL against `ats/index.md` and `job-boards/index.md`:

- ATS recognised → follow `ats/<name>.md`, sections **Recognize** and **Reach the form**.
- Board with its own application flow → follow `job-boards/<board>.md` § Apply on this board.
- Both offered on the same posting → the board's native flow goes first.
- Only an email address in the posting → the mail channel; step 9 handles the send.

Unknown ATS, the generic path: navigate, screenshot, read the page, and work the form as it presents itself. A trap you meet there that belongs to the ATS itself, not to this posting, earns its file per `ats/index.md` § Adding an ATS.

Done when: the application form is open in a tab of the MCP group, or the mail channel is chosen.

## Step 4 — Scout the whole form

Read the entire form before writing a single field. Scroll top to bottom, reading at each position, and record in `posting.md` `## Form`:

- the resume field,
- the cover letter field and its shape — text area or file,
- every required field,
- the custom questions, verbatim,
- any block the candidate alone can decide, such as EEO.

Done when: `## Form` lists every field the form asks for, each marked required or optional.

## Step 5 — Tailor the resume

Always, on every application. Run `skills/tailor-resume/SKILL.md` inline on this folder, passing `## Form` as extra requirements: the custom questions say what the company wants to see. It writes `posting.md` `## Match` and `tailored-resume/`.

Done when: `tailored-resume/` holds the file to upload.

## Step 6 — Cover letter

Only when `## Form` holds a cover letter field. Run `skills/cover-letter/SKILL.md` inline; the shape recorded at step 4 decides whether a PDF is built. No letter field, no letter.

Done when: `cover-letter/` holds the letter in the shape the form takes, or the form has no letter field.

## Step 7 — Answers to the custom questions

For each question of `## Form`:

1. `application-data.md` `## Reusable answers` — the same question answered for another company.
2. Otherwise, from the letter and the tailored resume: the same evidence, the same figures, three to five sentences. `profile.md` feeds those two documents, and they feed the answers; it is not an answer source itself.
3. A question whose honest answer needs a fact held by neither `profile.md` nor `application-data.md` suspends the application (step 10).

A fresh answer goes into `## Reusable answers` (`question | answer | where, date`) the moment it is written. `answers.md`, on `shared/templates/answers.md`, is written question by question as the form goes, not afterwards.

Done when: every question of `## Form` has its line in `answers.md`.

## Step 8 — Fill

One pass, through the `scripts/fill-page.md` subagent: the tab id, the field → value mapping built at steps 2, 5, 6 and 7, and the paths of the files to upload. The method approves that mapping — no per-field question reaches the candidate.

Multi-page form: fill the page, advance the way `ats/<name>.md` describes, scout the new page as at step 4, fill again, until the review page.

A field the subagent returns as failed after its two tries: its question and its answer go to `answers.md`, ready to paste, and the status stays `ready to submit`. A missed gesture is not a missing fact.

Done when: every field of `## Form` is filled, uploaded, or written into `answers.md`.

## Step 9 — Send mode

`DATA_DIR/preferences.md` `## Send mode` decides the last gesture:

- `draft` — everything filled, nothing submitted, tab closed. Status `ready to submit`. Mail channel: a Gmail draft (`create_draft`) — subject `Application - <role> - <candidate name>`, written in the posting's language (`Candidature - <poste> - <nom>` for a French posting), plain hyphens only; the letter as body, the tailored resume attached.
- `auto-submit` — submit without asking. Read the confirmation on screen (`get_page_text`) or in the confirmation mail: status `sent`. No confirmation read: status `sent (unconfirmed)`. Mail channel: `send_message`, same subject, body and attachment.

Done when: the application carries one of `sent`, `sent (unconfirmed)`, `ready to submit`.

## Step 10 — What suspends

Status `to validate`, the reason named, the folder kept as it stands so the candidate resumes where it stopped:

- technical or personality test,
- video to record,
- imposed salary range below the floor of `preferences.md`,
- account to create with a password,
- captcha, 2FA, "unusual activity",
- a question needing a fact absent from both `profile.md` and `application-data.md`,
- a must-have with no evidence, not even transferable,
- a required field that step 4 did not put in `## Form`, a required EEO block left undecided included,
- an attachment that cannot be uploaded,
- a signature asked for under `auto-submit`.

Done when: no line of this list applies, or the status is `to validate` with one line naming the blocker.

## Step 11 — Record

Written the moment each fact comes out, never at the end of the run:

- `applied.md` in the folder, on `shared/templates/applied.md`, which carries its fields and the full status set: `sent`, `sent (unconfirmed)`, `ready to submit`, `to validate`, `archived`.
- one line in `DATA_DIR/job-history.md` `## Applications`: `date | company | role | channel | fit | status | folder`.
- `DATA_DIR/state.md`: § Awaiting the candidate for what now waits on the candidate, § Open questions for the blocker when the status is `to validate`.

Nothing is paid for and no paid account is opened, at any step.

Done when: the folder, `job-history.md` and `state.md` agree on one status. Report the company, the role, the channel, the status and the folder.
