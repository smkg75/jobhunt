---
name: tailor-resume
description: Tailor the canonical resume to one job posting and write the match into posting.md.
argument-hint: "job URL, 'last' for the most recent job folder, or 'current' for the open tab"
---

# Tailor resume

Reorganise and rewrite what the candidate has actually done so a recruiter sees the fit in seven seconds. Nothing is invented; the roles, titles and dates of the canonical stay as they are.

Conflicts between rules: `shared/references/priority-hierarchy.md`.

## Step 1 — Load the job

Resolve the data directory per `shared/references/data-directory.md`, then check prerequisites per `shared/references/prerequisites.md`.

Resolve the target job folder as `skills/apply/SKILL.md` step 1 specifies. Read from it `posting.md` — `## Brief`, `## Posting`, and `## Form` when apply has already scouted the form, its custom questions counting as requirements. Read `DATA_DIR/profile.md`.

Done when: `posting.md` carries `## Brief` and `## Posting`, and the profile is in context.

## Step 2 — Pick the canonical

Read the `## Resumes` table of `DATA_DIR/index.md` (`lang | src | output`).

- The row whose `lang` is the language of the posting wins.
- No row in that language: take the first row, and record `resume language ≠ posting language` in the `Reason` field of the folder's `applied.md`. The canonical is used as written; translating it here is out of scope.

Done when: one row is picked, and its `src` and `output` are known.

## Step 3 — Write § Match

For each requirement of `## Posting` and each question of `## Form`, find the strongest evidence in the canonical resume and in `DATA_DIR/profile.md`. Write the result into `posting.md` as `## Match`, one line per requirement:

```
requirement → evidence → source → quality (direct | analogous | transferable)
```

Close the section with `Gaps:` listing the requirements that carry no evidence. A gap stays a gap: adjacent experience is written as adjacent, under its own quality.

Done when: every requirement of the posting holds a line or sits under `Gaps:`.

## Step 4 — Build the tailored resume

The `src` column names a source file in `DATA_DIR/resume/`:

1. Copy that one file into the folder's `tailored-resume/` — flat, same file name, source alone.
2. Rewrite the content of the copy following `scripts/tailor-resume.md`.
3. Run the `build:` line of `DATA_DIR/index.md` inside `tailored-resume/`, with the copied file name in place of `<src>`. The PDF lands beside the source under the name in the `output` column.
4. Read the page count of both PDFs with `pdfinfo <file>` (its `Pages:` line), the built one and the canonical in `DATA_DIR/resume/`. Longer than the canonical: cut the bullets that answer nothing in `## Match`, and build again until the counts are equal.

The `src` column is empty (the canonical exists as a PDF only): write the tailored resume as `tailored-resume/resume.md`, with the sections of the canonical.

Done when: `tailored-resume/` holds the rewritten source plus a PDF of the same page count as the canonical, or `resume.md` when there is no source.

## Step 5 — Record what the candidate corrects

A factual correction said in session goes into `DATA_DIR/profile.md` immediately — business model, scope, responsibilities, figures, dates — and then into the resume. The correction outlives this application.

Report: what moved to the front and why, what was rewritten, the two or three sentence narrative that makes this candidate the obvious one, and the paths written.

Done when: every correction said in session sits in `profile.md`, and the report names what moved and the paths written.
