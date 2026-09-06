# Job boards

One file per board, all built on the same skeleton. A skill that searches reads this index to know
which boards are active and in which order — it never names a board itself.

## Order of use

**MCP or API before the browser.** A board with a tool or a keyless endpoint answers faster, returns
structured fields, and never hits a captcha. The browser serves the boards that expose nothing else.

| Board | File | Access | Order of use |
|---|---|---|---|
| Indeed | `job-boards/indeed.md` | MCP `mcp__claude_ai_Indeed__*` | 1 |
| hiring.cafe | `job-boards/hiring-cafe.md` | Browser, no account | 2 |
| Welcome to the Jungle | `job-boards/wttj.md` | Browser, sign-in required, sub-agent | 3 |
| LinkedIn Jobs | `job-boards/linkedin.md` | Browser, sign-in required | 4 |

## Skeleton

Every board file is `# <Board>` followed by these six headings.

**Access** — MCP, API or browser; whether an account is required; how to tell the session is signed
in. A board that needs an account and shows none is a source that is down: name it and move on.

A browser board whose listings are heavy, or whose entry points have moved, says **sub-agent** here
and is worked through `skills/job-search/scripts/browse-board.md`. The sub-agent returns rows and a
board report; its probing never enters the pass's context.

**Search** — the queries the board accepts, its URL patterns, its date and location filters, and any
extraction the list needs. Reading a result list is the same gesture everywhere: `javascript_tool`
pulls the rows and nothing else, `read_page` is the fallback.

```javascript
Array.from(document.querySelectorAll('[class*="job"], [class*="listing"], [class*="card"], tr, [role="listitem"]'))
  .slice(0, 50)
  .map(el => el.innerText.trim())
  .filter(t => t.length > 20 && t.length < 500)
  .join('\n---\n')
```

When the selector misses, fall back as `shared/references/browser-setup.md` § Context safety
describes.

**Read a posting** — how one posting opens and where its text is read.

**Apply on this board** — the board's native flow when it has one. When the posting hands off to the
company's own form instead, that form is an ATS: `ats/index.md`.

**Traps** — what breaks on this board and the gesture that avoids it.

**Last tested** — the date the file was last exercised against the live site, or `never`, and what
was exercised. Never the employer, the role or the posting URL: this file records the board, not
where the candidate applied. Those belong in `DATA_DIR`.

## Adding a board

Write `job-boards/<board>.md` on the skeleton above, then add its row to the table: name, file,
access, and the rank that puts MCP and API ahead of the browser.
