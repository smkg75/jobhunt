# Fit scoring

Four verdicts: `High` · `Medium` · `Low` · `Skip`. Each one is written as a single dated line of
reason, so a later pass knows why.

Run the four steps in order and stop at the first that decides.

## 1. Skip on the first hard exclusion

Reading stops at the first hit; the rest of the posting goes unread.

- Any dealbreaker listed in `DATA_DIR/preferences.md` § Dealbreakers.
- A posting older than `preferences.md` § Freshness.
- An expired posting: the page no longer opens, or it carries no apply button.
- A role already in `DATA_DIR/job-history.md` § Applications or § Rejected before <YYYY-MM>, whether
  applied to or rejected. A rejection's reason says how wide it reaches: when it covers the company,
  every role there is Skip; when it covers that one role, another role at the same company is still
  scored.

## 2. Must-haves

Check each line of `preferences.md` § Must-haves against the full posting. Where the posting's own
words and the ATS fields disagree, the words win: a role the ATS files under Paris whose body says
it will be based in Dublin is based in Dublin.

## 3. Nice-to-haves

Count the lines of `preferences.md` § Nice-to-haves the posting meets.

## 4. Verdict

- **High** — no dealbreaker, every must-have met, at least two nice-to-haves met, **and the company
  itself looked at**: size, sector, reviews, and the average salary when the board offers one. The
  numeric bar for those lives in `preferences.md` § Application rules.
- **Medium** — one real doubt left, named in the reason line: size at the edge of the range, location
  to confirm, the employer not named behind a recruiting firm. A **missing must-have belongs here
  too when it can be said in one sentence** — years of experience, a sector of origin, a tool never
  used. The letter names the gap and the application goes.
- **Low** — the posting describes another job: its day-to-day is not the candidate's, and no sentence
  closes that.

An **unstated package is a reservation, not a doubt**, and so is any detail the posting leaves open,
onsite days included. A reservation is written into the reason line and leaves the verdict as it
stands: the floor lives in `preferences.md` § Compensation and the figure is put at the first
exchange.
