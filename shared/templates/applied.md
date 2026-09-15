# Applied

The shape of `applied.md`, one per application, inside its job folder. Written by `apply` as each
fact comes out, and never rewritten afterwards: everything here is immutable. Its status is not one
of these fields — it is read from `DATA_DIR/job-history.md` § Journal, this folder's last line
there.

```markdown
Date: YYYY-MM-DD
Channel: <board or ATS>
Send mode: draft | auto-submit
Reason: <a note carried by the application, such as `resume language ≠ posting language`>
Confirmation: <what the confirmation screen or mail said>
Resume: tailored | canonical

## Replies

One dated line per reply received, and one per `feedback asked` sent by `check-replies ask-feedback`.
```

A folder archived before it was ever filled may hold no `applied.md` at all — its only record is the
`archived` line in § Journal. No folder is ever deleted.
