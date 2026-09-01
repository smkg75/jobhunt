---
name: cover-letter
description: Write the cover letter for one job posting, with the proof map it rests on.
argument-hint: "job URL, 'last' for the most recent job folder, or 'current' for the open tab"
disable-model-invocation: true
---

# Cover letter

One letter answers one posting, from evidence already mapped. The proof map is written first: it decides what the letter can claim.

Conflicts between rules: `shared/references/priority-hierarchy.md`.

## Step 1 — Load

Resolve the data directory per `shared/references/data-directory.md`, then check prerequisites per `shared/references/prerequisites.md`.

Resolve the target job folder as `skills/apply/SKILL.md` step 1 specifies. Read `posting.md` `## Brief` and `## Match`, the tailored resume in `tailored-resume/`, and `DATA_DIR/profile.md` — `## Letter rules`, `## Metrics bank`, and the roles the letter will name.

`## Match` missing: run `skills/tailor-resume/SKILL.md` on this folder first, so the letter and the resume rest on the same evidence.

Done when: brief, match, tailored resume and letter rules are in context.

## Step 2 — Write the letter

Write `cover-letter/cover-letter.md` in the job folder: `## Proof map`, then `## Letter`. Both follow `scripts/write-cover-letter.md`.

Done when: `cover-letter.md` holds the two sections and has passed the review that closes `scripts/write-cover-letter.md`.

## Step 3 — PDF, when the form wants a file

`posting.md` `## Form` says whether the cover letter field takes text or a file. Text field: `cover-letter.md` is the deliverable, stop here.

File: build the letter text, and only the letter text.

- `DATA_DIR/index.md` carries a `build:` line: compose with the resume's engine. Write `cover-letter/cover-letter.tex` as a minimal document — A4, the font of the resume source, 11 pt, normal margins, the paragraphs of `## Letter` and nothing else: no header, no address block, no signature block, no template that adds content of its own. Build it in `cover-letter/` the way the `build:` line builds the resume, output `cover-letter.pdf`.
- No `build:` line: use the machine's own converter on the letter text — on macOS, `cupsfilter -o media=A4` on a temporary text file, output `cover-letter.pdf`.

Done when: `cover-letter/` holds `cover-letter.md`, plus `cover-letter.tex` and `cover-letter.pdf` whose only content is the letter.

## Step 4 — Report

The letter itself, the two or three proofs it leans on, and any must-have the proof map left without evidence — that one sets the application to `to validate` in `skills/apply/SKILL.md` step 10.

Done when: the letter, its proofs and any evidence-less must-have have been reported.
