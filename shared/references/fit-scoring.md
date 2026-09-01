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

Check each line of `preferences.md` § Must-haves against the full posting.

## 3. Nice-to-haves

Count the lines of `preferences.md` § Nice-to-haves the posting meets.

## 4. Verdict

- **High** — no dealbreaker, every must-have met, at least two nice-to-haves met, **and the company
  itself looked at**: size, sector, reviews, and the average salary when the board offers one. The
  numeric bar for those lives in `preferences.md` § Application rules.
- **Medium** — must-haves met, one real doubt left: compensation unstated, size at the edge of the
  range, location to confirm. The doubt is named in the reason line.
- **Low** — must-haves missing.

A **reservation** — onsite days to confirm, a detail the posting leaves open — is written into the
reason line and leaves the verdict as it stands.
