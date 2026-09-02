---
name: run
description: One hands-off pass - search the boards, apply to every High in series, then report.
argument-hint: "optional keywords to steer the search"
---

# Run

One pass end to end, without the candidate at the keyboard. `network-scan` stays its own command and
never runs from here.

`$ARGUMENTS` = optional keywords, handed to the search untouched.

Resolve `DATA_DIR` per `shared/references/data-directory.md`, then check prerequisites per
`shared/references/prerequisites.md`, the `run` column. One missing — stop and name it.

Orchestration is the `Agent` tool: one search agent, then one apply agent per `High`. Where the
`Workflow` tool exists it may carry that same structure.

## Step 1 — Search

Dispatch one sub-agent: follow `skills/job-search/SKILL.md` with `$ARGUMENTS` as its argument.

It comes back with the `High` and `Medium` postings and the folders it created.

Done when: the list is back and every `High` has its folder under `jobs/`.

## Step 2 — Apply, in series

For each `High`, one sub-agent: follow `skills/apply/SKILL.md` with the posting URL as its argument.
It reads the send mode itself and stops where that mode stops.

**One at a time.** Two apply agents share the same Chrome and fight over its tabs; start the next
only once the previous has returned.

No cap - every `High` goes.

An agent that comes back broken leaves its posting for the fourth block below, and the pass carries
on to the next `High`.

Done when: every `High` has come back `sent`, `sent (unconfirmed)`, `ready to submit` or
`to validate`, or is named in the fourth block.

## Step 3 — Report

Four short blocks:

1. **Sent** - one line per application: status (`sent`, `sent (unconfirmed)` or `ready to submit`),
   company, role, channel, folder, and the LinkedIn contact when the search found one.
2. **To validate** - every `Medium`, with the doubt that holds it named, and every `High` an apply
   agent suspended, with the reason it named. One question per line, no application filed for any of
   them.
3. **Notable skips** - the postings dropped that the candidate would want to hear about, each with
   its reason.
4. **What broke** - the board that failed, the agent that came back empty, the export still missing.

Then rewrite `DATA_DIR/state.md` § Last run: the date, postings seen, applications out, and what
awaits the candidate. What the second block leaves open goes to § Open questions, what the candidate
still owes to § Awaiting the candidate.

Done when: the four blocks are rendered and § Last run reads for this pass.
