# Ashby

## Recognize

`jobs.ashbyhq.com/<slug>`.

## Reach the form

The apply button on the posting opens the form, which runs in steps.

## Read a company's openings

`https://api.ashbyhq.com/posting-api/job-board/<slug>` — keyless.

## Fill

Uploading the resume pre-fills several fields at once. Re-read every pre-filled field before moving
on: the parser guesses, and a wrong guess is submitted as an answer.

On a multi-step form, clicking continue on an empty step makes the form list its required fields.

## Traps

**That empty-continue probe belongs to multi-step forms only.** On a single-page Ashby form the same
button is the submit button, so count the steps before using it.

## Last tested

never
