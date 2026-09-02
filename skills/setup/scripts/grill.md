# Grill

Step 3 of `skills/setup/SKILL.md`: pull out of the candidate what the resume does not say and what
a fit verdict or a tailoring depends on.

This is a grilling session. When the `grilling` skill is installed, invoke it and hand it the table
of § What to ask as the guide for its questions; the rules of § How to ask hold either way.

## How to ask

- **One question at a time.** Wait for the answer before the next one. Several questions in one
  message bewilder the candidate and cost the detail you came for.
- **Recommend an answer with every question.** Say what you would put down and why, so the candidate
  corrects a draft instead of composing from nothing.
- **Look facts up, ask for decisions.** A fact the resume, `DATA_DIR` or the filesystem already holds
  is read, never asked. What the candidate wants - a floor, a limit, a refusal - is theirs to say.
- **Open with the fact, then the hole in it.** Quote the line, name what is missing from it:

  > Your <company> bullet says the team went from four to eleven. What did the number it was hired to
  > move do over those same months? My read of the rest of the page says roughly a doubling - is that
  > close?

  A question that starts "tell me about your role at <company>" hands the work back and buys a
  paragraph you already have.
- **Ask only what changes a sort or a tailoring.** A detail that moves no verdict and lands in no
  bullet is not worth a turn.

## What to ask, and where the answer lands

Write each answer into its file as it comes, not at the end of the pass.

| Ask | Because | Lands in |
| --- | --- | --- |
| The figure missing from an achievement | A bullet with no number cannot be ranked against a posting | `profile.md` § Achievements of that role, and § Metrics bank |
| What failed in the role | Form questions ask for it, and a tailoring that only wins reads false | `profile.md` § What failed |
| Why it ended | The letter has to account for the date | `profile.md` § Why it ended |
| The salary floor | It turns a posted range into a `Skip` on its own | `preferences.md` § Compensation |
| Where, how far, on site or remote | Same, and it decides which searches run | `preferences.md` § Location |
| What the candidate refuses outright | First test of every verdict | `preferences.md` § Dealbreakers |
| The form fields neither the resume nor Step 2 answered | Every application asks them again | `application-data.md` § Form sheet |

An answer the candidate does not have stays `?` in its place, plus one line in `profile.md`
§ Open gaps. Nothing is filled in on their behalf.

Done when: for every role of the resume, its achievements carry figures or a `?`, and What failed
and Why it ended are answered or `?`; `preferences.md` § Compensation, § Location and § Dealbreakers
each hold an answer; § Form sheet has no hole the candidate could have closed; and no question was
asked that the files already answered.
