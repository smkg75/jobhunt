# Board browser

Sub-agent. You work one board through the browser and return its rows. The pass that dispatched you
deduplicates, scores and decides; you report what the board shows.

A board earns this treatment when its file says so under § Access. The reason is context: a listing
page read whole fills the window and the pass becomes unrecoverable, and a board that has changed
shape needs several probes before it yields anything. Both belong in a sub-agent, whose context is
thrown away when it returns.

## Input

- The board file, `job-boards/<board>.md`, which carries its access, its URL patterns and its traps
- The queries to run, already phrased by the pass
- `shared/references/browser-setup.md`

## Work

1. Open the browser per `shared/references/browser-setup.md`, then check the session the way § Access
   describes. Signed out on a board that needs an account: stop and return `unavailable`, with what
   the header showed as the reason.
2. Run each query through the entry points § Search names, in the order it names them. An entry point
   that renders no list is not the board's answer — it is one entry point failing. Move to the next
   before concluding anything.
3. Read every list with `javascript_tool` and a selector, never with `get_page_text`. The recipe is in
   `job-boards/index.md` § Skeleton; `read_page` is the fallback when the selector misses.
4. Stop after the entry points § Search names are exhausted. Do not invent new ones, do not sign in,
   do not accept a consent banner beyond declining non-essential cookies, and do not solve a captcha.

## Return

Two blocks, nothing else.

**Rows** — one line per posting the board listed, `company | role | url | posted | location`. A field
the list does not show is `null`; do not fill it from the posting page, the pass opens those itself.

**Board report** — `ok`, `partial` or `unavailable`; the queries that ran and what each returned; the
entry points tried and what each rendered; and any trap you met that is the board's and not this
pass's. A trap that is new goes back as a line the pass can paste into § Traps.

Done when the rows are listed, or the board is `unavailable` with its reason named.
