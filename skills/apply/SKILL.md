---
name: apply
description: Fill one job application end to end, from the posting to the record.
argument-hint: "job URL, 'last' for the most recent job folder, or 'current' for the open tab; add 'hands-off' to be asked nothing"
---

# Apply

One application = a folder, a form read whole before anything is written, a tailored resume, a letter when the form asks for one, answers drawn from those two, an event, a record. In that order; each step ends before the next begins.

The work of one application runs in a sub-agent, `scripts/apply-one.md`, one per application and one at a time. This file is the dispatcher's side: the target, the dispatch, the review before the last gesture, the record. The dispatcher's context holds the agent's reports — never the form, the resume source or the pages.

**Hands-off** means nobody is asked, at any step: under `run`, or when the candidate said so at launch — `hands-off` after the argument, or in their own words. Otherwise one grouped question may reach the candidate, only where this file says so.

Conflicts between rules: `shared/references/priority-hierarchy.md`.

## Step 1 — Target

Resolve the data directory per `shared/references/data-directory.md`, then check prerequisites per `shared/references/prerequisites.md`.

`$ARGUMENTS`:

- a URL → the job folder that matches it in `DATA_DIR/jobs/` (company slug, or the `URL` line of `posting.md`), otherwise a new job folder named as `shared/templates/posting.md` names it, created empty: the agent writes `posting.md`.
- `last` → the most recently modified job folder.
- `current` → the form already open in the active tab; match its URL against the job folders to load the context, and keep the tab id for the dispatch.

`DATA_DIR/application-data.md` `## Form sheet` absent: build it from the canonical resume, and ask the candidate one grouped question covering what the resume leaves open — work authorisation, sponsorship, notice, salary expectation, EEO — then save it. Hands-off, there is nobody to ask: a field the resume leaves open surfaces at the review or suspends the application.

Done when: the folder exists, and `## Form sheet` is on disk.

## Step 2 — Dispatch

One sub-agent through the `Agent` tool, following `scripts/apply-one.md`, on the model its frontmatter names. Its input: `DATA_DIR`, the folder, the posting URL, and the tab id when the argument was `current`. It reads its own list and no other file — `job-history.md` and `state.md` are written here and never opened there.

**One at a time.** Two application agents share one Chrome and fight over its tabs: the next starts once the previous has returned `stage: done`.

A correction the candidate says while the agent works goes to it at once by `SendMessage`, verbatim, whatever its stage. The agent writes it back to the data file it belongs to, then redoes what it touches.

Done when: the agent has returned `stage: review` or `stage: done`.

## Step 3 — Review

Skipped when the agent came back `done` without a review — it suspended the application first.

Before the last gesture — submit, send, or the close under `draft` — the agent stops and returns `stage: review`: the answers, the resume's summary block and the bullets it rewrote, the letter, and the form as read back from the DOM field by field, or the mail as composed. It waits in its tab. The tab is perishable (`ats/<name>.md` § Traps says which forms survive nothing), so the answer goes back without delay.

Check, with `DATA_DIR/profile.md` read once per pass:

- every figure, client, date and claim traces to the profile, the canonical resume or a correction the candidate said;
- what the candidate lacks stays unnamed in free text, and a closed question gets the true answer (`preferences.md` `## Application rules`);
- the characters `## Application rules` bans are absent from every value read back;
- every value read back is the value decided — a field that came back different is named with the text it must hold;
- the answers, the letter and the resume tell one story.

Answer by `SendMessage`: `go`, or the corrections, each naming the field, the bullet or the paragraph and the text to put there. The agent applies them, replaces the attachment when the resume changed, reads the form back again and returns a new `review`; the loop ends on `go`.

A doubt the files cannot settle: one grouped question to the candidate, saying the tab is waiting. Hands-off, nobody is asked — a doubt on a fact suspends the application (`blocked: <the question>` sent to the agent), a doubt on wording is settled here.

Done when: `go` has gone back and the agent has returned `stage: done`.

## Step 4 — Record

Written the moment the report is in, never at the end of the run:

- one line in `DATA_DIR/job-history.md` `## Journal`: `date | folder | event | detail`, the event the agent returned. This is the only place a status is ever written.
- one line in `DATA_DIR/job-history.md` `## Applications`: `date | company | role | channel | fit | folder` — the candidature's identity, never its status.
- `DATA_DIR/state.md`: § Awaiting the candidate for what now waits on the candidate, § Open questions for the blocker when the event is `blocked`.

`applied.md` is the agent's: it wrote it in the folder before returning.

Nothing is paid for and no paid account is opened, at any step.

Done when: the journal carries the event, `## Applications` carries the identity line, and `state.md` holds only what the candidate must still decide or answer. Report the company, the role, the channel, the event and the folder, with the letter and the answers as they went out and every reservation the agent named.
