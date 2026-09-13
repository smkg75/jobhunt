# 💼 jobhunt

A Claude Code plugin that carries a job search from the first query to the submitted form. It reads
the candidate's own files, searches the boards, scores each posting, tailors the resume and the cover
letter to that posting, fills the application form in Chrome through the Claude in Chrome extension,
and logs what went out. The plugin holds the method; every fact about the candidate lives in a data
directory outside it.

## 🧾 Skills

| Command | Argument | Produces |
|---|---|---|
| `/jobhunt:setup` | `interview` to jump straight to the questions | the data directory, its files filled in from `shared/templates/`, the resume in `resume/`, the letter template in `cover-letter/` |
| `/jobhunt:job-search` | keywords, optional | a pass table in `job-history.md` § Runs, and `posting.md` under `jobs/` for every High |
| `/jobhunt:tailor-resume` | a posting URL, `last` or `current` | `posting.md` § Match and the built resume in `tailored-resume/` |
| `/jobhunt:cover-letter` | a posting URL, `last` or `current` | `cover-letter/cover-letter.md`, plus the letter on the candidate's template as a PDF when the form wants a file |
| `/jobhunt:apply` | a posting URL, `last` or `current` | the form filled, `answers.md`, `applied.md`, a line in `job-history.md` § Applications and one in § Journal |
| `/jobhunt:check-replies` | how far back to read, such as `36h` or `7d` | one `ack`, `reply`, `stage-booked` or `rejected` line in `job-history.md` § Journal per answer received, the replies in `applied.md`, and what the mail asks for in `state.md` |
| `/jobhunt:network-scan` | a contact count, or `all` (default 25) | openings at the companies of the candidate's LinkedIn contacts, logged in `job-history.md` § Runs |
| `/jobhunt:run` | keywords, optional | a search pass, then an application for every High, and a four-block report in `state.md` § Last run |
| `/jobhunt:help` | | the flow, and where this candidate stands; writes nothing |

## 📦 Install

```bash
# from a local checkout
claude plugin marketplace add /path/to/jobhunt
# or from the repository
claude plugin marketplace add https://github.com/smkg75/jobhunt.git

claude plugin install jobhunt@jobhunt
```

Then run `/jobhunt:setup`. It creates the data directory, writes its path into `~/.claude/CLAUDE.md`,
and fills the first files. Every other skill reads that path
(`shared/references/data-directory.md`).

Browser work needs Chrome running with the Claude in Chrome extension active.

## 🛠️ Editing the plugin

The install is a copy, refreshed only when the version changes. `git config core.hooksPath .githooks`
once in the checkout: every commit then bumps the patch version, and `claude plugin update
jobhunt@jobhunt` picks the new copy up.

[`DECISIONS.md`](DECISIONS.md) holds the arbitrages the method rests on, each with the reason that
settled it, and the holes left open on purpose. Read the entry before changing what it settles.

## 🗂️ The data directory

```
DATA_DIR/
├── index.md                    map of this folder, resume build command
├── state.md                    last run, what waits on the candidate, open questions
├── profile.md                  roles, evidence, metrics bank, letter rules, open gaps
├── preferences.md              what to look for, what to skip, how far to go
├── application-data.md         form sheet, answers already written
├── job-history.md              applications, the journal, search runs, earlier rejections
├── companies.md                companies followed, their ATS and careers URL
├── linkedin-contacts.csv       LinkedIn connections export (optional)
├── resume/                     canonical resumes, one per language, flat
│   ├── <resume>.pdf
│   ├── <resume>.<ext>            the source, when there is one
│   └── images/
├── cover-letter/               letter template, one per language, source and built sample flat
│   ├── <letter>.tex              sender block filled, the rest in <angle brackets>
│   └── <letter>.pdf
└── jobs/
    └── <company>-<date>/       one folder per application
        ├── posting.md          header, Brief, Posting, Match, Form
        ├── tailored-resume/    the chosen source and its built output
        ├── cover-letter/       cover-letter.md, plus the filled template and its PDF when a file is wanted
        ├── answers.md          question, answer, source
        └── applied.md          date, channel, send mode, confirmation, replies
```

## 🧭 Boards and ATS covered

| Board | File |
|---|---|
| Indeed | `job-boards/indeed.md` |
| Hiring.cafe | `job-boards/hiring-cafe.md` |
| LinkedIn | `job-boards/linkedin.md` |

| ATS | File |
|---|---|
| Greenhouse | `ats/greenhouse.md` |
| Lever | `ats/lever.md` |
| Workday | `ats/workday.md` |
| Teamtailor | `ats/teamtailor.md` |
| Ashby | `ats/ashby.md` |
| Workable | `ats/workable.md` |
| Rippling | `ats/rippling.md` |
| Deel | `ats/deel.md` |
| Recruitee | `ats/recruitee.md` |
| Taleez | `ats/taleez.md` |

## ➕ Adding a board or an ATS

Write one file on the skeleton its index defines, then add its row there — `job-boards/index.md`
§ Adding a board, `ats/index.md` § Adding an ATS. The skills read the index and pick the new file up.

## 🙏 Credit

Forked from Proficiently's Claude skills plugin (MIT) —
[proficientlyjobs/proficiently-claude-skills](https://github.com/proficientlyjobs/proficiently-claude-skills).

## 📄 License

MIT
