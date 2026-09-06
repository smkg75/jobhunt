# Indeed

## Access

MCP, `mcp__claude_ai_Indeed__*` — no browser and no sign-in gesture, the server carries the account.
`get_resume()` returns the profile Indeed holds for the candidate (preferred titles, minimum salary):
compare it with `DATA_DIR/preferences.md` whenever either side changes.

## Search

`search_jobs(search, location, country_code)` — one call per target role, with the location and
country of `DATA_DIR/preferences.md` § Location. The call returns a list of results with job ids.

## Read a posting

`get_job_details(job_id)` gives the description, the salary, the publication date and the apply link.

`get_company_data(companyName, …)` gives employee reviews, headcount and salary ranges — the look at
the company that a `High` requires.

## Apply on this board

No apply tool. The apply link from `get_job_details` opens either the native Indeed Apply flow in the
browser — attach the PDF resume rather than the Indeed profile, and answer the screening questions
from the posting — or the company's own form, which is an ATS: `ats/index.md`.

## Traps

Results carry no age of their own, so a search alone cannot tell a fresh posting from a stale one:
call `get_job_details` on every result before scoring, its publication date is what § Freshness reads.

A search returns at most ten results and answers "No job results found" on any phrasing it does not
recognize — the same role in another wording returns a full list. Two phrasings per role, not one.

`location` is matched loosely: "Paris" and "Paris, Île-de-France" return different lists, and neither
is a region filter. Every result's own location is read again before scoring.

The `job_id` is a search-session handle (`JOBSEARCH_<n>`), not a stable id: it is only valid for
`get_job_details` inside the same pass, and the `to.indeed.com` short link changes between calls on
the same posting.

The "Apply now" button of a posting page opens **Indeed Apply, which asks for a signed-in Indeed
account**: the browser session carries none, and the page offers "Sign in to Indeed with Google"
instead of a form. The employer's own form is then the only channel, and the posting page does not
link it: reach it from the company's website, whose careers link ("On recrute !", "Nous rejoindre")
lands on the ATS. That ATS URL, not the Indeed one, is the durable URL of the posting.

A posting's publication date on Indeed can be later than the one its ATS shows, the board reposting
an older offer: one posting read 25 August 2026 on Indeed and 20 July 2026 on its ATS. The
ATS date is the one § Freshness reads.

`get_company_data` matches on the name alone and answers on the first company that carries it
anywhere in the world: asked about a Paris RegTech, it returned a New Zealand peer-to-peer lender of
the same name, reviews and salaries included. Check the country and the sector of what comes back
before the company look a `High` requires rests on it.

## Last tested

2026-09-06 — twelve searches, two returning nothing, one homonym on `get_company_data`
