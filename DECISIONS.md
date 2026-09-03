# Decisions

Why the method is shaped the way it is. Each entry is an arbitrage that was settled once, with the
reason that settled it, so it is not re-litigated or silently undone. Several of them reverse the
upstream project or an earlier reading of this fork: those say so, because the reversed version is
the one a contributor is most likely to restore.

What is deliberately left undone sits at the end, under [Deferred](#deferred).

## The plugin

**The plugin holds the method only** · 2026-09-01
The repository carries instructions, skeletons and templates. No fact about the candidate and no
application artefact is ever committed to it: everything a pass produces lives in `DATA_DIR`,
outside the plugin tree. The install is a copy that gets replaced on update, so anything of the
candidate's kept inside would be destroyed by the next one. The fork skeleton stripped everything
that was not method, including the upstream messaging-bot skill and the per-skill asset folders.

**`DATA_DIR` is found by a marker line** · 2026-09-01
The data directory is the path following `JobHunt data:` in `~/.claude/CLAUDE.md`. `setup` writes
that line, offers a default and accepts any path; every other skill stops on "Run `/jobhunt:setup`
first" when the marker is absent. This replaces the upstream algorithm, which probed the working
directory and then the home directory for one hard-coded folder name.

**A personal wiki gets a pointer, never a copy** · 2026-09-01
Where the candidate keeps a knowledge base of their own, `setup` offers to write one line pointing
at `DATA_DIR` into it, asking for its path rather than guessing. No search fact is copied there. A
pointer survives; a copy diverges.

**The checkout is its own marketplace** · 2026-09-01
`marketplace.json` declares the plugin with `"source": "./"`, so `claude plugin marketplace add
<checkout>` then `claude plugin install jobhunt@jobhunt` works from a local clone. This reverses an
upstream fix that had replaced the relative source with an explicit GitHub one. Restoring that
would break installing from a checkout, which is how this fork is used.

**Every commit ships a version** · 2026-09-02
`.githooks/pre-commit` increments the patch in `plugin.json` and `marketplace.json` and stages
them. The install is refreshed only when the version changes, so without a bump `claude plugin
update` sees the same number and the edit never reaches the installed copy. The version is a commit
counter, not semantics.

**The seven commands are model-invocable** · 2026-09-02
`disable-model-invocation` was removed from all seven frontmatters. It had reserved the commands
for an explicit slash command, so a session could neither call them nor reproduce their steps, and
preparatory work stalled at the point of handing the run back. Removing it is also what lets `run`
chain search, tailoring, letter and apply on its own. The corollary is that a session can act
without an explicit keystroke: `run` is launched on request, and what it is about to do is said
first.

**MIT, with the upstream copyright kept** · 2026-09-02
`LICENSE` carries two copyright lines, the upstream project for the original work and the fork
author for the modifications, and the README credits the upstream repository by name and link.
The fork is a derivative of an MIT project and keeps the attribution the licence requires.

**No vendor footer, no permissions block** · 2026-09-01
The upstream call to action and the `## Permissions Required` block that closed every upstream
`SKILL.md` were dropped and never returned. A report ends on what was done and where it was
written; the tools a skill needs are declared in its frontmatter, not recited to the candidate.

**Section titles in English, bodies in any language** · 2026-09-01
Skill instructions address sections by name (`§ Open gaps`, `§ Form sheet`, `§ Runs`), so a
translated heading breaks the read path while a translated body costs nothing.

**Every step ends on a written criterion** · 2026-09-01
Each step of each skill closes on a `Done when:` line, checkable without judgement, and the next
step starts after it. No upstream skill had one. It is what makes a hands-off run auditable and
what stops a step from being half-done.

**One prerequisites table for all seven skills** · 2026-09-01
Kept from upstream and widened with the fork's own files, a `run` column and one uniform stop
message. Two exceptions are stated there once instead of being re-argued per skill: a missing
`profile.md` degrades tailoring and letter writing with a warning rather than blocking, and a
missing contacts export blocks `network-scan` alone.

**An ephemeral working directory stops the skill** · 2026-09-01
A working directory under a session path makes the skill stop and ask for a persistent folder.
Kept from upstream: files written there vanish with the session.

**The platform files never name an employer** · 2026-09-03
Employer names, role titles and posting URLs are banned from `ats/*.md` and `job-boards/*.md`,
`Last tested` and `Traps` included; URLs are written as `<slug>`, `<id>`, `<job-slug>`. The rule is
stated in both indexes. These files record how a platform behaves, not where the candidate applied,
and those facts already live in `DATA_DIR`. The history was rewritten the same day to purge the
names it had already carried, and the remote repository recreated, because a force-push leaves the
old objects served by SHA.

## Setup and the data directory

**Setup is one skill with the grill inside it** · 2026-09-01
Onboarding is five steps in one skill, ending on a grill that delegates to the `grilling` skill
when it is installed and applies its own rules otherwise. Upstream had spread resume upload,
preferences and a per-role interview across several skills and a conduct-interview script. Only the
steps whose files are still template text run again.

**The grill asks one thing at a time, and recommends** · 2026-09-01
One question per turn, each with a recommended answer, facts looked up rather than asked for, and
only what changes a sort or a tailoring. The pattern is to quote the line as it stands and name the
hole in it. Upstream asked for target roles, location, salary and exclusions in a single natural
question, which returns a paragraph nobody can file.

**An unknown fact stays a `?`** · 2026-09-01
Anything neither the resume nor the candidate supplies is written as `?` in its place, plus a line
in `profile.md § Open gaps`. A blank is indistinguishable from an answer and an invented one
poisons every later verdict; the marker makes the gap re-askable.

**A correction is written back before it is used** · 2026-09-01
A factual correction said in session goes into `profile.md` first, then into the document. The
correction outlives this application, and without the write-back the same mistake returns on the
next posting.

**A reaction is a preference, written when it is said** · 2026-09-01
Anything the candidate says about the results goes into its section of `preferences.md` at the
moment it is said, or into the company row for "skip this one", a corrected careers URL, ATS or
slug. A preference voiced and not written is a verdict the next pass gets wrong again.

## Finding and scoring

**Four verdicts, decided at the first hit** · 2026-09-01
`High`, `Medium`, `Low`, `Skip`, one dated line of reason each. The steps run in order and reading
stops at the first hard exclusion, leaving the rest of the posting unread. A verdict without a
reason cannot be reused by the next pass.

**`Medium` is a question, not a weaker score** · 2026-09-01
Upstream graded `Medium` on how many must-haves were met. Here the must-haves are met and one real
doubt is left, that doubt is named on the reason line, and `run` files every `Medium` under
"To validate", one question per line. It is what the candidate is asked, not what the skill
half-decided.

**A `High` also requires looking at the company** · 2026-09-01
No dealbreaker, every must-have met, at least two nice-to-haves, and the company itself examined:
size, sector, reviews, salary range where a board gives one. The numeric bar lives in the
candidate's `preferences.md`, never in the skill, because a threshold is a fact about the candidate.

**Qualification never scores a list row** · 2026-09-01
Every deduplicated line is opened in full before it is scored. A list row carries the board's own
enrichment rather than the posting's: a meta-board attributes the employer's offices to every one
of its postings, so a role open in one country surfaces under another city's filter, and its dates
drift.

**The ATS date is the freshness authority** · 2026-09-02
Every retained line is re-read on the employer's ATS and that date is what the freshness rule uses.
Measured in both directions: one posting read six weeks apart on a board and on its ATS. Where an
ATS exposes only a creation or update stamp, freshness is a floor and the row says so.

**Only the employer URL is durable** · 2026-09-02
The URL recorded in the posting file, in the history and in anything shown to the candidate is the
employer's ATS URL. A board link is a search artefact: one board's id is a session handle and its
short link changes between calls on the same posting.

**Tracked companies before any board** · 2026-09-01
Rows of `companies.md` older than seven days are read first, then the boards in index order.
Deduplication on company plus role keeps the line from the earliest source, so the employer's own
ATS wins over the board copy.

**The index decides which sources run** · 2026-09-01
`job-boards/index.md` holds the active boards and their order, tool- or endpoint-backed ones ahead
of browser-only ones; no skill names a board itself. A board with a tool or a keyless endpoint
answers faster, returns structured fields and never hits a captcha.

**A broken source never stops the pass** · 2026-09-01
A dead board, a relaunched batch or a missing contacts export is named in the "what broke" block
and the pass continues. A board that needs an account and shows none is a source that is down.

**Every posting seen gets a row, `Skip` included** · 2026-09-01
One row per posting in `§ Runs`, with its reason. Those rows are what a later pass reads back to
recognise a posting and to avoid re-running the same query. Only a `High` opens a job folder: a
folder is work, and work is spent on `High`.

**One run log, one block format** · 2026-09-01
Search passes and network passes both write into `DATA_DIR/job-history.md § Runs`, in the block
shape defined once, distinguished by a `Source:` line. Upstream kept a separate history file for
network-scan with its own table shape.

**Sub-agents report facts, the pass scores** · 2026-09-01
Reader sub-agents return structured facts with `null` for anything the page does not state, and
the dispatching pass renders the verdict. Upstream had each sub-agent score its own batch and
return only the good ones. One scoring authority, and gaps stay gaps.

## Applying

**Send mode alone decides the last gesture** · 2026-09-01
`preferences.md § Send mode` is the whole draft-versus-send distinction: `draft` fills everything
and submits nothing; `auto-submit` submits and reads the confirmation back, giving `sent` or, with
nothing read back, `sent (unconfirmed)`. Upstream demanded a human confirmation before every
submit, which a hands-off run cannot carry. `sent (unconfirmed)` exists so an unread confirmation
is never reported as a send.

**The skill answers nothing personal on the candidate's behalf** · 2026-09-01
Upstream defaulted arbitration and terms to Accept, interview and transcription consent to Accept,
the electronic signature to the candidate's full name, and every EEO field to Decline. All of it is
inverted here: EEO rows stay empty until the candidate fills them, a signature is returned as
`for the candidate`, and a required consent or EEO block left undecided suspends the application at
`to validate`. This is the most tempting rule to "improve", and the one that must not move.

**A closed list of what suspends an application** · 2026-09-01
Test, video, salary below the floor, an account to create, a captcha or 2FA, a question whose
honest answer is in no data file, a must-have with no evidence, an unexpected required field, an
unuploadable attachment, a signature under `auto-submit`. Nothing is paid for and no account is
created. The folder is kept as it stands so the candidate resumes where it stopped.

**No approval loop, a report instead** · 2026-09-01
Upstream's present-approve-iterate conversations were removed from both document skills. Each ends
on a report: what moved to the front and why, the proofs the letter leans on, any evidence-less
must-have, the paths written. No per-field question reaches the candidate. A skill that stops to
ask about tone stalls the chain; what the candidate would have corrected is caught by the proof
map, the `to validate` status and the correction write-back.

**The chain runs in one imposed order** · 2026-09-01
Form scouted whole, resume tailored with the form's custom questions as extra requirements, letter
only if there is a letter field, answers drawn from the letter and the tailored resume, fill, send,
record. The custom questions say what the company wants to see, so scouting precedes tailoring;
`profile.md` feeds the two documents and they feed the answers, which keeps one story across all
three.

**Apply knows when nobody is there to ask** · 2026-09-01
Outside a run, a missing form sheet is built by asking one grouped question. Under `run` there is
nobody to ask, so a field the resume leaves open surfaces later as a suspension. It is the one
place a skill changes its contract depending on its caller, and it is written down in the skill.

**Run applies in series** · 2026-09-01
One search agent, then one apply agent per `High`, strictly one at a time and with no cap;
`network-scan` never runs from there. Two apply agents share one Chrome and fight over its tabs.

**The upload is the skill's job, not the candidate's** · 2026-09-01
Upstream told the user the file path and asked them to attach the resume by hand, calling it a
known limitation. Here the resume is attached on every ATS: `find` or `read_page` locates the
`<input type=file>` and `file_upload` takes its `ref`. The visible button stays unclicked because
it opens a native picker the browser tools cannot see. A path is handed back only when
`file_upload` refuses it, which means starting the session from `DATA_DIR`.

**Never sign in, never create an account** · 2026-09-02
An autofill or apply route that opens a vendor sign-in is abandoned and the form filled by hand; an
auth gate the session cannot pass ends the work there, with the gate named. Upstream paused and
asked the user to sign in and say "continue"; a hands-off run has nobody to wait for.

**A folder is never deleted** · 2026-09-01
`archived` closes a folder without an application, the posting withdrawn or the candidate having
dropped it, and the folder stays. It is one of the five statuses that must agree across
`applied.md`, the history and the folder itself.

## The resume and the letter

**One canonical per language, no translation** · 2026-09-01
The `## Resumes` row whose language matches the posting wins; with no row in that language the
first is used as written and the mismatch is recorded in `applied.md`. A machine-translated resume
is a new document nobody has proofread, and recording the mismatch keeps the fact visible instead
of hiding it.

**The tailored resume rewrites the canonical source** · 2026-09-01
The canonical source is copied into the job folder, rewritten in place and built with the `build:`
line of `DATA_DIR/index.md`. Upstream's "write a `resume.md`" survives only as the fallback when
the canonical exists as a PDF alone. The candidate's own build produces the document a recruiter
sees; a markdown rewrite loses the layout it was designed with.

**Nothing is invented, a gap stays a gap** · 2026-09-01
Only what the canonical, `profile.md` or a correction states. Titles, dates and the roster of roles
stay exactly as the canonical has them, gaps included; scope stays as written, and ambiguity gets
conservative wording or the detail is dropped. Written after the first test run inflated scope and
invented business models. Accuracy ranks above workflow, writing and tone, so no later rule trades
it away.

**The letter and the resume rest on one evidence base** · 2026-09-01
The letter reads `posting.md § Match` and the tailored resume, and runs `tailor-resume` on the
folder first when `§ Match` is missing. Two documents built from separate readings of the posting
contradict each other in front of the recruiter.

**The proof map decides what the letter may claim** · 2026-09-01
`cover-letter.md` carries `## Proof map` above `## Letter`: one line per requirement, as
requirement, evidence, source, quality. A must-have with no evidence is written with its hole
named, the application goes to `to validate`, and the letter claims nothing in its place. A gap
becomes a status the candidate can act on rather than a sentence that smooths it over.

**No letter field, no letter** · 2026-09-01
A letter is written only when the form, read top to bottom before anything is filled, holds a
cover letter field, and the shape recorded there decides text or PDF. The resume is tailored on
every application without exception. A letter nobody asked for never reaches a reader.

**The letter is built on the candidate's own template** · 2026-09-02
The PDF is the candidate's LaTeX template, read from `DATA_DIR/index.md`, copied into the job
folder and filled placeholder by placeholder. The skill composes no document of its own. This
reversed two readings in two days, first "no header, no address block, no template that adds
content", then the template with its blocks: the layout is a fact about the candidate, and facts
live in the data directory.

**The Vous / Je / Nous shape, whatever the language** · 2026-09-02
Subject line, salutation, three paragraphs read as Vous, Je, Nous, sign-off. It replaced the fork's
own Hook / Proof / Close of the day before. The letter answers a French market: the shape is what
the reader looks for, and the language of the posting changes the words, not the order.

**Vous is one or two sentences** · 2026-09-02
Enough to show the posting was understood and why the candidate applies. No portrait of the
company, because the recruiter knows where they work. This reversed, the same day, a first reading
that asked for two to four sentences describing the company.

**A text field takes no subject line** · 2026-09-02
A cover letter text area receives the salutation, the three paragraphs and the sign-off only. The
field already sits under the posting. The full block order stays for the template's placeholders
when the form wants a file.

**The addressee is hunted, but only on the posting** · 2026-09-02
The `Contact:` line first, else one look at the posting page, else the generic form. A name found
is written back. Chasing a name off the posting is unbounded work with a real chance of naming the
wrong person.

**A postal address is never invented** · 2026-09-02
The address the posting gives; failing that one search on the company and its head office; failing
that the city alone. The same accuracy rule as the resume, applied to the one block a template
makes it easy to fill with a plausible guess.

**Letters have their own writing rules** · 2026-09-02
On top of the shared list: every sentence sayable aloud in one breath, a subject and a verb in each,
varied length, a fact always paired with why it matters to this company, at most one figure per
sentence, and the finished letter read aloud before it is saved. The resume rules alone let a
letter come out as a row of machine-sounding claims. The ban on em dashes and dash chains belongs
to this list: it governs text generated for the candidate, not the repository's own prose.

**150 to 250 words** · 2026-09-01
Upstream's 250 to 350 was halved. Past that the letter repeats the resume. It is the one thing
about letter length that `preferences.md` may override.

**A line-by-line read replaces the critique step** · 2026-09-01
The sentence rules live in one shared file and close both documents: read line by line against the
list before saving, every failing line rewritten. Upstream's per-skill critique-and-rewrite step
was dropped, because the same check duplicated in two skills drifts and a step can be skipped where
a closing condition cannot.

**The candidate's voice overrides the shape** · 2026-09-01
`profile.md § Letter rules` beats the six-block shape the script describes. How someone wants to be
told is a fact about them, and facts live in the data directory.

**Page count is measured, not estimated** · 2026-09-02
The built resume matches the canonical's page count and the letter is one page, both read with
`pdfinfo`. Over: cut bullets that answer nothing in `§ Match`, or cut paragraphs, and build again.
Growing the margins to fit is the tempting fix and is forbidden for the letter, whose margins and
font are fixed by the template.

**The template staggers its blocks and uses one weight** · 2026-09-02
Sender top-left, recipient below and offset right with the date aligned on it. No bold, no light
face, one ink colour, one `parskip`. Staggering is the French convention, and a single weight stops
the letter reading as a designed page.

**Logistics belong to the form** · 2026-09-01
Availability, notice period and location stay out of the letter. The form asks those questions
anyway, with a field the recruiter can filter on, and the letter's words are scarce.

## ATS and board knowledge

**One file per platform, on one skeleton** · 2026-09-01
Each ATS and each board gets its own file on a fixed set of six headings, plus its row in the
index. A posting's apply link lands on one of these: the URL says which, the file says how its form
behaves. A new platform is one file and one row, picked up with no other change, and only traps
inherent to the platform go in it.

**No comparison matrix, but the URL column stays** · 2026-09-02
The cross-ATS "How the forms behave" table was deleted rather than kept up to date, because it
restated what each file already says and a second copy drifts. The index's URL-pattern column is
the deliberate exception: matching a URL is what the index is for, and doing it without opening ten
files is worth the duplication.

**`Last tested` says what was exercised** · 2026-09-03
A date and what was actually exercised, or the single word `never`. A file written from
documentation and a file exercised against a live form must not read alike, and `never` is what
warns the next contributor that the recipe below is untried.

**The slug is read off the careers URL** · 2026-09-01
Never derived from the company name, which answers 404 on every one of these APIs. Known slugs live
in `DATA_DIR/companies.md`, not in these files.

**Read the form from the API before opening a browser** · 2026-09-02
Where an ATS publishes a keyless offer API describing the form, it answers the scout step before a
single screenshot, custom questions verbatim included. One field there says whether a letter field
exists at all, which the letter step needs before it writes anything.

**A signed-out board is a down source** · 2026-09-02
The sign-in check is positive, by looking for the sign-in link, not inferred from an empty list: a
signed-out session fails silently and a result count with no list reads like "no matches" rather
than "no access". The same symptom later appeared on a signed-in session after a product change, so
the symptom does not diagnose the session.

**A profile import is tried, not declined** · 2026-09-02
Where an ATS offers one, the pass tries it and falls back to filling by hand if it opens a sign-in.
This reverses the file's earlier instruction to decline it: a pass declined the button on the
strength of that note and the candidate had to click it himself. The resume PDF still goes into the
CV field afterwards.

**Proving a replacement takes the whole dance** · 2026-09-03
Read the current state, detach, confirm the input came back empty, upload, confirm the name is
displayed again. A resume rebuilt from the same source keeps its file name and often its byte size
to the byte: nine tailored resumes recompiled with a single word changed all came out the exact
size they were. Neither the name nor the size proves anything, only the transition does. And a form
is never left with no resume attached: a failed re-upload after a successful detach is reported
first and plainly.

## Deferred

Known holes, kept visible on purpose.

**Untested platforms.** Four ATS files are written from documentation and marked
`Last tested: never`, two of them with `?` under Reach the form, Fill and Traps. No ATS file
documents a completed submit: every live exercise so far stopped short of sending. Five ATS have no
keyless openings API and are read from their listing pages.

**Two rules that contradict each other.** `apply` says the board's native flow goes first when a
posting offers both, while two board files say the native flow needs an account the session does
not carry, making the employer's form the only channel. Nothing reconciles the rule with its two
standing exceptions. Likewise, `network-scan` runs five browser readers at once while `run` fills
one form at a time on the grounds that two agents fight over the same Chrome; no commit reconciles
the two.

**A board ranked but unusable.** One board sits at rank 3 in the index and has rendered no result
list since 2026-09-02; another has no usable application channel at all. Neither is demoted,
because the rank records intent rather than current health.

**`preferences.md § Sources & accounts` is read and then used by nobody.** The search pass reads it
at step 0 and no later step consumes it; the index is the real authority on which boards run.

**Onboarding does not ask for everything scoring depends on.** Several preference sections the
verdicts read are never covered by the grill, and `§ Rejected before <YYYY-MM>` has no writer at
all: it is filled by hand or not at all.

**The version hook only fires where it was enabled.** `git config core.hooksPath .githooks` is a
manual step per checkout; without it commits ship no version bump and the installed copy silently
stays behind.

**Out of scope, on purpose.** Translating a canonical resume, tailoring a canonical that exists as
a PDF alone, building a letter with no template or on a machine without the build chain, a letter
for the mail channel, and letter templates in a language other than French.

**The `?` placeholders are the permanent backlog.** Across the data templates and `§ Open gaps`,
they are by design: a fact nobody has supplied yet is a question waiting, not an omission to fix in
the plugin.
