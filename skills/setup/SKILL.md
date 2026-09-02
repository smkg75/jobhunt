---
name: setup
description: One-time onboarding - install the data directory, take in the resume, grill the candidate, fill the data files.
argument-hint: "'interview' to jump straight to the grill"
disable-model-invocation: true
---

# Setup

Puts the data directory in place and fills it. The skill carries the method, `DATA_DIR` carries the
facts.

`$ARGUMENTS` = `interview` — run Step 0 to resolve `DATA_DIR`, then Step 3 alone; a resume must
already sit in `DATA_DIR/resume/`.

Each step ends on its own criterion. The next one starts after it.

## Step 0 — Data directory

Resolve `DATA_DIR` with `shared/references/data-directory.md`. No marker line means a fresh
install: propose the default path that reference names, accept whatever path the candidate names
instead, create the directory.

Write the `JobHunt data:` marker into `~/.claude/CLAUDE.md`, in the form
`shared/references/data-directory.md` reads back; create that file when it is missing.

Copy into `DATA_DIR` every data file of `shared/templates/` that it lacks: `index.md`, `state.md`,
`profile.md`, `preferences.md`, `application-data.md`, `job-history.md`, `companies.md`.
`posting.md`, `applied.md` and `answers.md` are shapes a skill writes inside a job folder, not files
to copy; `cover-letter.tex` is the template Step 1 fills.

Wiki: when the candidate keeps a personal knowledge base, offer one pointer line to `DATA_DIR` in
it — ask for its path, never guess one.

Then read `resume/`, `profile.md`, `preferences.md` and `application-data.md` to see which already
hold real content rather than template text, and run only the steps still empty. Everything filled
already — say so, list the commands available, stop here.

Done when: `DATA_DIR` resolves from the marker, holds every template file, and each of Steps 1-4 is
marked run or skipped in the plan for this session.

## Step 1 — Resume

One canonical resume per target language, flat in `DATA_DIR/resume/`.

Ask for a path. A PDF alone lands as `resume/<name>.pdf`. A source (`.tex`, `.md`, `.docx`) lands in
`resume/` beside its output, flat; its images go to `resume/images/`.

Ask which language each resume is written in. For every language the candidate's target postings use
that has no resume yet, search the machine first (`mdfind`) and offer to write one only when that
search comes back empty.

Fill `DATA_DIR/index.md`: the `build:` line and one `## Resumes` row per canonical, as that template
describes. Run the build once, from `resume/`, to confirm the command produces the PDF.

Then the letter template, one per resume language: copy `shared/templates/cover-letter.tex` to
`DATA_DIR/cover-letter/<name>.tex` and fill its sender block — name, postal address, mobile, email,
the city of the date line — from the resume, asking for what the resume lacks. A language other
than French takes its fixed strings — subject line, salutation, sign-off, date line — in that
language. The other placeholders stay: each letter fills them. Add its `## Cover letters` row to
`index.md` and build it once, from `cover-letter/`.

Done when: every canonical resume sits flat in `resume/` with its images under `resume/images/`,
each has its `## Resumes` row, the `build:` line has produced a PDF once, and `cover-letter/` holds
one filled template per resume language with its `## Cover letters` row and its built PDF.

## Step 2 — Pre-fill from the resume

Read the resume. Fill `profile.md`, `preferences.md` and `application-data.md` from their templates
with what it states: roles, dates, achievements that already carry a figure, tools, and the
`Form sheet` fields it answers. Every hole it leaves reads `?`.

Done when: the three files hold every fact the resume states, and each remaining hole reads `?`.

## Step 3 — Grill

Follow `scripts/grill.md`. Come back when its criterion is met.

## Step 4 — Close

The grill wrote each answer as it came. Read the three files once end to end: every answer landed,
every hole still open reads `?`, and the ones worth a later pass are lines of `profile.md`
§ Open gaps.

Offer the contacts export — it is what lets a pass flag a company where the candidate already knows
someone. The steps are in `job-boards/linkedin.md` § Export the connections.

Save it as `DATA_DIR/linkedin-contacts.csv` and say how many contacts came in. Declined, it can come
any later day.

What the candidate still owes - that export, a resume for a target language, a `?` only they can
close - goes to `DATA_DIR/state.md` § Awaiting the candidate.

Close with a short summary: the resumes and their languages, the target roles, the send mode, how
many roles the profile covers, and the commands now available.

Done when: the summary is rendered and § Awaiting the candidate names everything left open.
