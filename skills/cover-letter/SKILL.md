---
name: cover-letter
description: Write the cover letter for one job posting, with the proof map it rests on.
argument-hint: "job URL, 'last' for the most recent job folder, or 'current' for the open tab"
---

# Cover letter

One letter answers one posting, from evidence already mapped. The proof map is written first: it decides what the letter can claim. The letter has the shape a French recruiter expects — a subject line, then three paragraphs read as "Vous, Je, Nous" — and, as a file, sits on the candidate's own template.

Conflicts between rules: `shared/references/priority-hierarchy.md`.

## Step 1 — Load

Resolve the data directory per `shared/references/data-directory.md`, then check prerequisites per `shared/references/prerequisites.md`.

Resolve the target job folder as `skills/apply/SKILL.md` step 1 specifies. Read `posting.md` `## Brief`, `## Posting` and `## Match`, the tailored resume in `tailored-resume/`, and `DATA_DIR/profile.md` — `## Letter rules`, `## Metrics bank`, and the roles the letter will name.

`## Match` missing: run `skills/tailor-resume/SKILL.md` on this folder first, so the letter and the resume rest on the same evidence.

Done when: brief, posting, match, tailored resume and letter rules are in context.

## Step 2 — Write the letter

Write `cover-letter/cover-letter.md` in the job folder: `## Proof map`, then `## Letter`. Both follow `scripts/write-cover-letter.md`.

Done when: `cover-letter.md` holds the two sections and has passed the review that closes `scripts/write-cover-letter.md`.

## Step 3 — PDF, when the form wants a file

`posting.md` `## Form` says whether the cover letter field takes text or a file. Text field: `## Letter` of `cover-letter.md` is the deliverable, stop here.

File: the letter laid out on the candidate's template, read from the `## Cover letters` table of `DATA_DIR/index.md` (`lang | src | output`).

1. Pick the row whose `lang` is the language of the posting. No row in that language: take the first row; its fixed strings — subject line, salutation, sign-off, date line — are rewritten in the posting's language.
2. Copy the `src` file from `DATA_DIR/cover-letter/` into the folder's `cover-letter/`, same file name.
3. In the copy, replace every `<placeholder>` and nothing else: the recipient block, the date of the day, the role in the subject line as the posting titles it, the paragraphs of `## Letter`. A placeholder the letter has nothing for goes with its line. The sender block is the template's own. Characters the source escapes — `%`, `&`, the currency sign — are escaped as the resume source escapes them.
4. Recipient block: the company; the addressee found for the salutation (`scripts/write-cover-letter.md` § Letter, item 2), with the role the posting gives them; the postal address the posting gives. No address in the posting: one web search on the company's name and its head office, and the address it returns. Still nothing: the city the posting names, alone. An address is never invented.
5. Build in `cover-letter/` with the `build:` line of `DATA_DIR/index.md`, the copied file name in place of `<src>`. Output: the `output` column. `pdfinfo` gives the page count (its `Pages:` line): more than one page, the paragraphs are cut in `## Letter` and the letter built again; margins and font stay.

No `## Cover letters` table: the same blocks — sender, recipient, date, subject line, salutation, paragraphs, sign-off — as plain text, converted by the machine's own converter — on macOS, `cupsfilter -o media=A4` on a temporary text file, output `cover-letter.pdf`.

Done when: `cover-letter/` holds `cover-letter.md`, plus the filled source and a one-page PDF named by the `output` column.

## Step 4 — Report

The letter itself, the two or three proofs it leans on, and any must-have the proof map left without evidence — that one sets the application to `blocked` in `skills/apply/SKILL.md` step 10.

Done when: the letter, its proofs and any evidence-less must-have have been reported.
