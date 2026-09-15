---
name: network-scan
description: Check whether the companies where the candidate already knows someone are hiring.
argument-hint: "number of contacts (default 25) or 'all'"
---

# Network scan

A warm introduction beats a cold form. This pass walks the candidate's LinkedIn connections back to their companies, reads what those companies have open, and ranks it.

Instructions that conflict resolve through the plugin root's `shared/references/priority-hierarchy.md`
(`shared/` sits beside `skills/`, not inside it).

## Step 0 — Load the data directory

Resolve `DATA_DIR` per `shared/references/data-directory.md`, then check prerequisites per `shared/references/prerequisites.md`. This pass runs on `DATA_DIR/linkedin-contacts.csv`, the candidate's LinkedIn connections export.

Read `DATA_DIR/preferences.md`, `DATA_DIR/profile.md`, `DATA_DIR/companies.md` and `DATA_DIR/job-history.md`.

## Step 1 — Pick the contacts

`$ARGUMENTS` sets how many: a number takes that many, `all` takes every contact — say so first when there are more than 200, the pass is long — and an empty argument takes 25.

Sort `linkedin-contacts.csv` by `Connected On`, most recent first, and take the first N. Group them by company; a contact with a blank company drops out. Each company keeps its people: name, position, profile URL.

Done when the pass holds a company list with its contacts attached, and has told the candidate how many companies came out of how many contacts.

## Step 2 — Give every company a careers page

For each company, in `DATA_DIR/companies.md`:

- **Row present** — it gives `careers URL`, `ATS` and `slug`. A row whose `last visit` is within seven days keeps all three as they stand.
- **Row present, noted `ignored`** — the company drops out of the pass.
- **No row** — open the company's own site per `shared/references/browser-setup.md`, follow its careers or jobs link, recognize the platform against `ats/index.md`, and add the row with its contact in the `contact` column.
- **Careers page out of reach** — add the row with `careers page not found` in `note`, and name the company in the "what broke" block.

Done when every company of the list has a row in `companies.md` carrying a careers URL, or sits in the "what broke" block.

## Step 3 — Read the openings

Split the companies that have a careers URL into batches of five. One sub-agent per batch follows `scripts/evaluate-company.md`, five running at once at most, each in its own tab.

Each batch receives its companies (name, careers URL, ATS, slug, contacts) and the keywords built from `preferences.md` § Target roles, variants included.

A batch that fails or times out is logged with the companies it held, and the pass moves on — those companies come back on the next run.

Done when every batch has returned or been logged as failed with the companies it held.

## Step 4 — Rank

Score every opening returned with `shared/references/fit-scoring.md`. One verdict, one line of reason, dated.

Done when every opening returned carries a verdict and its dated reason.

## Step 5 — Record the pass

- **`companies.md`** — for every company scanned: `last visit` set to today, `last role seen` refreshed, `contact` carrying the connection, `ATS` and `slug` completed by what the sub-agent read.
- **`job-history.md` § Runs** — one run block in the format `skills/job-search/SKILL.md` Step 8 defines, with `Source: network` and, in place of queries, the contact count and company count. Every company scanned takes at least one row; a company with nothing open takes a single row carrying `no opening` in `notes`, and every opening's row names its contact in `notes`.
- **`jobs/`** — each `High` gets its folder and its `posting.md` exactly as `skills/job-search/SKILL.md` Step 7 defines, `Source` naming the company.

Done when `companies.md`, `job-history.md` § Runs and the `jobs/` folders agree on this pass.

## Step 6 — Report

Return, in this order:

1. **Matches** — grouped by fit: company, role, location, link, the contact to ask for the introduction, the folder for each High
2. **Scanned, nothing matching** — company with the number of openings it showed
3. **No careers page** — the companies of Step 2 that stayed out of reach
4. **What broke** — batches that failed and the companies they held

Done when the four blocks are rendered.

Close as `skills/job-search/SKILL.md` Step 9 closes.

## Step 7 — Feed the results back

At the moment the candidate reacts:

- "Skip this company" → `ignored` in that row's `note` in `companies.md`; later passes leave it alone.
- A corrected careers URL, ATS or slug → the row, right away.
- Anything about roles, package, sectors or size lands as `skills/job-search/SKILL.md` Step 10 says.

Done when every reaction voiced is written into its file.
