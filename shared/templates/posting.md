# Posting

The shape of `posting.md`, one per application, inside its job folder. The folder is
`DATA_DIR/jobs/<company-slug>-<YYYY-MM-DD>/`, dated the day it opens; a slug already used today takes
the suffix `-2`.

`job-search` and `network-scan` write the header, `## Brief` and `## Posting`; `tailor-resume` writes
`## Match`; `apply` writes `## Form`.

```markdown
Source: <board or tracked company>
URL: <posting URL>
Apply URL: <the form, when the posting names it>
Found: YYYY-MM-DD
Posted: YYYY-MM-DD

## Brief

Seven lines, in the posting's own words: what the company does · stage and size · title and level ·
responsibilities · must-have · nice-to-have · keywords.

## Posting

The posting as read, trimmed of navigation and boilerplate.

## Match

One line per requirement: requirement → evidence → source → quality (direct | analogous |
transferable), closed by `Gaps:`.

## Form

Every field the form asks for, each marked required or optional.
```

Both URLs sit in the header because the page may be gone by the time the folder is reread.
