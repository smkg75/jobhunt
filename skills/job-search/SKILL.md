---
name: job-search
description: Run one search pass over the tracked companies and the active job boards, and rank what it finds by fit.
argument-hint: "optional search keywords"
---

# Job search

One pass: tracked companies first, then the boards, every posting opened, every posting a verdict.

Instructions that conflict resolve through the plugin root's `shared/references/priority-hierarchy.md`
(`shared/` sits beside `skills/`, not inside it).

## Step 0 — Load the data directory

Resolve `DATA_DIR` per `shared/references/data-directory.md`, then check prerequisites per `shared/references/prerequisites.md`.

Read:

- `DATA_DIR/preferences.md` — `Target roles`, `Location`, `Query rules`, `Sources & accounts`, and the lists the fit rules read
- `DATA_DIR/profile.md`
- `DATA_DIR/job-history.md` — `Runs` for the previous pass's queries; the fit rules read `Applications` and `Rejected before <YYYY-MM>`
- `DATA_DIR/companies.md`
- `DATA_DIR/linkedin-contacts.csv` when it exists

## Step 1 — Re-read the tracked companies

Tracked companies come before any board.

A row whose ATS publishes a keyless API is read **on every pass**: it costs one call, and it is the source that converts. The seven-day rule on `last visit` holds only for the rows that need the browser.

Read the openings through `ats/<name>.md` § Read a company's openings. A row with a blank `ATS` or `slug` is read from its `careers URL`, which is the authority — recognize the platform against `ats/index.md`, § Finding the ATS of a company when nothing is known, and fill both columns in passing.

Each opening joins the raw list with the company as its source. Redate `last visit` to today and refresh `last role seen` on every row you read, whether or not it was hiring.

Done when every row due this pass is read and redated.

## Step 2 — Work the boards

Read `job-boards/index.md` and take the active boards in the order it gives. Each board's own file carries its access, its query syntax and its list extraction; follow it. A board reached through the browser opens per `shared/references/browser-setup.md`.

A board whose § Access says **sub-agent** is dispatched to one, following `scripts/browse-board.md`, with that board's file and this pass's queries. It returns rows and a board report; the rows join the raw list, and the report feeds the "what broke" block. Two such boards never run at once — they share one browser.

Queries are each `Target roles` entry × `Location`, phrased differently from the queries recorded under the previous `## YYYY-MM-DD — run` heading in `job-history.md` § Runs, and bounded by `preferences.md` § Query rules. `$ARGUMENTS`, when the candidate gives one, replaces the role list for this pass.

A raw posting is: company, role, URL, posted date, source.

A board that fails — login lost, captcha, tool down — is named in the "what broke" block, and the pass continues on the next one.

Done when: every active board of `job-boards/index.md` has been queried on every `Target roles` × `Location` pair, or named in the "what broke" block.

## Step 3 — Deduplicate

Same company plus same role is one line. Keep the line from the earliest source: the tracked companies before every board, then the boards in the order of `job-boards/index.md`.

Done when: one line per company-plus-role remains.

## Step 4 — Open every posting

Qualification reads the full posting, never a list row. Open each line through its board file's § Read a posting, or through the ATS file for a company opening.

Past twenty postings on a board whose access is MCP or API, dispatch them in batches of ten to sub-agents following `scripts/evaluate-jobs.md`, each tooled with that board's posting tool alone. They return facts; the verdict is rendered here. A batch that comes back broken is relaunched once, then its postings are opened one by one in the pass.

Done when every line of the raw list carries the facts of its full posting.

## Step 5 — Rank

Score each posting with `shared/references/fit-scoring.md`. One verdict, one line of reason, dated.

Done when: every line carries a verdict and its dated reason.

## Step 6 — Cross the network

For every `High` and `Medium`, look the company up in the `Company` column of `DATA_DIR/linkedin-contacts.csv`, matching loosely ("Acme" matches "Acme Corp"). A hit writes the contact's name and title into the `contact` column of `companies.md` and into the report.

`linkedin-contacts.csv` missing: the line `LinkedIn export to drop` goes to the "what broke" block and the pass continues.

Done when: every High and Medium has been looked up in the CSV, or the missing-export line is in the block.

## Step 7 — Open a folder for each High

Each `High` gets its folder and its `posting.md` on `shared/templates/posting.md`, which names both: header + `## Brief` + `## Posting` here, `## Match` to `tailor-resume`, `## Form` to `apply`.

Done when: every High has its folder with `posting.md` header, `## Brief` and `## Posting`.

## Step 8 — Record the pass

Append to `DATA_DIR/job-history.md` § Runs:

```markdown
## YYYY-MM-DD — run

Source: job-search
Queries: <one line per query>

| title | company | location | posted | salary | link | fit | notes |
```

Every posting seen takes a row, `Skip` included, its reason in `notes`.

§ Runs holds the newest block only: the one it replaces moves to `DATA_DIR/runs/<YYYY-MM-DD>.md`, whole, and takes a line in the table at the head of the section. A run block runs to twenty kilobytes, and every sub-agent of the next pass would otherwise read them all.

Done when: the run block holds one row per posting seen, Skips included, and the block it replaced sits in `runs/`.

## Step 9 — Report

Return, in this order:

1. **High and Medium** — company, role, fit, location, posted date, compensation, link, network contact when found, and the folder for each High
2. **Notable skips** — company, role, the one-line reason
3. **What broke** — boards that failed, batches relaunched, the missing LinkedIn export

Done when: the three blocks are rendered.

Tailoring, letters and forms run as their own skills: point the candidate at `/jobhunt:apply <url>`.

## Step 10 — Feed the preferences back

A reaction to the results is a preference: write it into `DATA_DIR/preferences.md`, under the section it belongs to, at the moment it is said.

Done when: every reaction voiced is written into its section of `preferences.md`.
