# Browser setup

Sequence for every skill that drives Chrome through the Claude in Chrome tools:

1. `tabs_context_mcp` — read the browser state. No tab, or an error: ask the candidate to confirm
   Chrome is open with the extension active.
2. `tabs_create_mcp` — open a tab inside the MCP group.
3. `navigate` — go to the URL.
4. Extract.

## Context safety

`get_page_text` returns the whole page. On a listing, a search result or a dashboard it fills the
context window and the session becomes unrecoverable. Reserve it for one posting, one confirmation
screen, one short page.

Read every other page with `javascript_tool` and a selector, or with `read_page` for element refs.
The extraction recipe for a board listing is in `job-boards/index.md`.

A page that fails twice is a page to leave: move on, and ask the candidate for its content.
