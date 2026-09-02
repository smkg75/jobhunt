# Prerequisites

Check the files a skill needs before its first step. A required file missing outside `setup` stops the
skill on: "`<file>` is missing. Run `/jobhunt:setup` first."

| File | setup | job-search | tailor-resume | cover-letter | apply | network-scan | run |
|---|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| `DATA_DIR/index.md` | creates | — | required | — | required | — | required |
| `DATA_DIR/state.md` | creates | — | — | — | — | — | required |
| `DATA_DIR/profile.md` | creates | required | required | required | required | required | required |
| `DATA_DIR/preferences.md` | creates | required | — | — | required | required | required |
| `DATA_DIR/application-data.md` | creates | — | — | — | created if missing | — | required |
| `DATA_DIR/job-history.md` | creates | required | — | — | required | required | required |
| `DATA_DIR/companies.md` | creates | required | — | — | — | required | required |
| `DATA_DIR/resume/` | creates | — | required | — | required | — | required |
| `DATA_DIR/cover-letter/` | creates | — | — | optional | optional | — | optional |
| `DATA_DIR/linkedin-contacts.csv` | offers | optional | — | — | — | required | optional |

Two files bend the rule:

- **`profile.md` missing** in `tailor-resume` or `cover-letter` — say the text will rest on the resume
  alone and will take more corrections, recommend `/jobhunt:setup interview`, and continue when the
  candidate asks for it.
- **`linkedin-contacts.csv` missing** in `network-scan` — stop on: "No LinkedIn contacts. Run
  `/jobhunt:setup` and import them first."
