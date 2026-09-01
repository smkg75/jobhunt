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

## Last tested

never
