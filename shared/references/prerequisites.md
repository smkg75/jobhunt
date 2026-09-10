# Prerequisites

Check the files a skill needs before its first step. A required file missing outside `setup` stops the
skill on: "`<file>` is missing. Run `/jobhunt:setup` first."

| File | setup | job-search | tailor-resume | cover-letter | apply | check-replies | network-scan | run |
|---|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| `DATA_DIR/index.md` | creates | — | required | — | required | — | — | required |
| `DATA_DIR/state.md` | creates | — | — | — | — | required | — | required |
| `DATA_DIR/profile.md` | creates | required | required | required | required | — | required | required |
| `DATA_DIR/preferences.md` | creates | required | — | — | required | required | required | required |
| `DATA_DIR/application-data.md` | creates | — | — | — | created if missing | required | — | required |
| `DATA_DIR/job-history.md` | creates | required | — | — | required | required | required | required |
| `DATA_DIR/companies.md` | creates | required | — | — | — | optional | required | required |
| `DATA_DIR/resume/` | creates | — | required | — | required | — | — | required |
| `DATA_DIR/cover-letter/` | creates | — | — | optional | optional | — | — | optional |
| `DATA_DIR/linkedin-contacts.csv` | offers | optional | — | — | — | — | required | optional |

The column is also the reading list. A skill run as a sub-agent reads the files marked for it and no
others — `state.md` is the orchestrator's, never `apply`'s, and `job-history.md` is written by the
side of `apply` that dispatches, never by its application agent — and inside a file, the section its
step names rather than the file. The directory outgrows the pass it serves: `job-history.md` alone passed
90 kB in three weeks, and each sub-agent that swallows it whole pays for it twice.

Two files bend the rule:

- **`profile.md` missing** in `tailor-resume` or `cover-letter` — say the text will rest on the resume
  alone and will take more corrections, recommend `/jobhunt:setup interview`, and continue when the
  candidate asks for it.
- **`linkedin-contacts.csv` missing** in `network-scan` — stop on: "No LinkedIn contacts. Run
  `/jobhunt:setup` and import them first."
