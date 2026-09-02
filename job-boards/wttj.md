# Welcome to the Jungle

## Access

Browser, sign-in required. The French startup and scale-up board.

Without a signed-in session the site renders no list at all: `/fr/jobs?query=…` shows a matching
landing page with a result count and nothing to read. Check the header first — a banner without a
"Se connecter" link means the session is signed in. A signed-out session makes this source
unavailable: say so and continue with the other boards.

## Search

`https://www.welcometothejungle.com/fr/jobs?query=<role>`, then the date and location filters of the
result page. Read the list by targeted extraction.

The public Algolia index behind the search carries its credentials in the page's JS bundle. Re-read
them on the page before any `curl`, and treat a 403 as the answer: back to the browser, without
insisting.

## Read a posting

Open each retained card and read the posting page.

## Apply on this board

"Postuler" opens one of two things. The native WTTJ form: the free-text message field is the cover
letter field. Or the company's own form, which is an ATS: `ats/index.md`.

## Traps

A signed-out session fails silently — a result count with an empty list reads like "no matches"
rather than "no access".

**A signed-in session fails the same way, and for another reason.** As of 2026-09-02 `/fr/jobs` no
longer renders a list at all: signed in, with no "Se connecter" link in the header, the page answers
"4908 jobs trouvés. Oui, on a compté." and offers to build a matching profile instead of showing the
results. The search box on that page produces the same screen. So the count-with-no-list symptom does
not diagnose the session — check the header for the sign-in link before blaming the login, and read
the emptiness as a product change on WTTJ's side.

The Algolia credentials are not in the page's inline scripts, only in the bundled chunks: a scan of
`document.querySelectorAll('script')` finds nothing.

## Last tested

2026-09-02 — no list rendered, source unavailable
