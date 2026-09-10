---
name: help
description: The flow of the eight commands, what each produces and which ones need the browser, and where this candidate stands.
disable-model-invocation: true
---

# Help

Prints the flow, then where the candidate stands. Writes nothing.

## Step 1 — The flow

Print this, as is:

```
setup           once per machine; the data directory, the resume, the letter template     files
job-search      one pass over the tracked companies and the boards, a folder per High     browser
tailor-resume   the canonical resume bent to one posting, and its Match                   files
cover-letter    the letter for one posting, with the proof map it rests on                files
apply           one posting end to end: form filled, answers, applied.md, journal line    browser
inbox           the boxes swept for answers, the journal brought up to date               mail
network-scan    the companies where a contact already works, and what they are hiring     browser
run             job-search, then apply on every High in series, then the report           browser
```

Done when: the eight lines are printed.

## Step 2 — Where the candidate stands

Resolve `DATA_DIR` per `shared/references/data-directory.md`. No marker: say the machine is fresh
and that `/jobhunt:setup` comes first; stop.

Found: read `DATA_DIR/state.md` and `DATA_DIR/job-history.md`. Print, in this order:

- the last run — its date and what it found, from `state.md` § Last run
- every application whose last § Journal event is `ready` or `blocked`, one line each with the
  folder and the event, and the next command it waits on
- what `state.md` § Awaiting the candidate holds, one line each

Then name the next command in the flow. Nothing waiting: `/jobhunt:job-search`.

Done when: the four blocks are printed and no file was written.
