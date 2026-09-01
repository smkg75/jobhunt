# Data directory

`DATA_DIR` is the path that follows the marker `JobHunt data:` in `~/.claude/CLAUDE.md`. Backticks
around the path are accepted, and `~` expands to the home directory.

When the marker is absent:

- `setup` — no directory exists yet: create one, and write the marker line.
- every other skill — stop on "Run /jobhunt:setup first".

`setup` offers `~/jobhunt/` as the default location and accepts any path the candidate names.

Its layout is in `README.md`, and every skill instruction writes `DATA_DIR/` for whichever directory
the marker points at.

## Ephemeral sessions

A working directory under an ephemeral session path (`/sessions/...`) loses its files when the session
ends. Stop there and tell the candidate:

> "Select a folder first so your data survives the session. Click 'Work in a folder', pick your home
> directory, then run this again."
