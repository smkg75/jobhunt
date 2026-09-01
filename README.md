# jobhunt

A Claude Code plugin that carries a job search from the first query to the submitted form. It reads
the candidate's own files, searches the boards, scores each posting, tailors the resume and the cover
letter to that posting, fills the application form in Chrome through the Claude in Chrome extension,
and logs what went out. The plugin holds the method; every fact about the candidate lives in a data
directory outside it.

## Skills

| Command | Argument | Produces |
|---|---|---|
| `/jobhunt:setup` | `interview` to jump straight to the questions | the data directory, its files filled in from `shared/templates/`, the resume in `resume/` |
| `/jobhunt:job-search` | keywords, optional | a pass table in `job-history.md` § Runs, and `posting.md` under `jobs/` for every High |
| `/jobhunt:tailor-resume` | a posting URL, `last` or `current` | `posting.md` § Match and the built resume in `tailored-resume/` |
| `/jobhunt:cover-letter` | a posting URL, `last` or `current` | `cover-letter/cover-letter.md`, plus a PDF when the form wants a file |
| `/jobhunt:apply` | a posting URL, `last` or `current` | the form filled, `answers.md`, `applied.md`, a line in `job-history.md` § Applications |
| `/jobhunt:network-scan` | a contact count, or `all` (default 25) | openings at the companies of the candidate's LinkedIn contacts, logged in `job-history.md` § Runs |
| `/jobhunt:run` | keywords, optional | a search pass, then an application for every High, and a four-block report in `state.md` § Last run |

## Install

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

## The data directory

```
DATA_DIR/
├── index.md                    map of this folder, resume build command
├── state.md                    last run, what waits on the candidate, open questions
├── profile.md                  roles, evidence, metrics bank, letter rules, open gaps
├── preferences.md              what to look for, what to skip, how far to go
├── application-data.md         form sheet, answers already written
├── job-history.md              applications, search runs, earlier rejections
├── companies.md                companies followed, their ATS and careers URL
├── linkedin-contacts.csv       LinkedIn connections export (optional)
├── resume/                     canonical resumes, one per language, flat
│   ├── <resume>.pdf
│   ├── <resume>.<ext>            the source, when there is one
│   └── images/
└── jobs/
    └── <company>-<date>/       one folder per application
        ├── posting.md          header, Brief, Posting, Match, Form
        ├── tailored-resume/    the chosen source and its built output
        ├── cover-letter/       cover-letter.md, plus .tex and .pdf when a file is wanted
        ├── answers.md          question, answer, source
        └── applied.md          date, channel, send mode, status, replies
```

## Boards and ATS covered

| Board | File |
|---|---|
| Indeed | `job-boards/indeed.md` |
| Hiring.cafe | `job-boards/hiring-cafe.md` |
| Welcome to the Jungle | `job-boards/wttj.md` |
| LinkedIn | `job-boards/linkedin.md` |

| ATS | File |
|---|---|
| Greenhouse | `ats/greenhouse.md` |
| Lever | `ats/lever.md` |
| Workday | `ats/workday.md` |
| Teamtailor | `ats/teamtailor.md` |
| Ashby | `ats/ashby.md` |
| Workable | `ats/workable.md` |

## Adding a board or an ATS

Write one file on the skeleton its index defines, then add its row there — `job-boards/index.md`
§ Adding a board, `ats/index.md` § Adding an ATS. The skills read the index and pick the new file up.

## Credit

Forked from Proficiently's Claude skills plugin (MIT).

## License

MIT
