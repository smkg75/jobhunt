# Taleez

## Recognize

`taleez.com/apply/<job-slug>` for a posting, and `<slug>.taleez.com` for a company's career page.
The footer of both reads "Propulsé par taleez". A company site often links it as "On recrute !".

## Reach the form

The career page `https://<slug>.taleez.com/` lists the open jobs; each title links to
`https://taleez.com/apply/<job-slug>?utm_source=<slug>&utm_medium=career`. The durable URL is that
link stripped of its `utm_*` parameters.

The posting page carries three "POSTULER" buttons (banner, job details, footer). Any of them opens
the form on the posting URL suffixed with **`/applying`**, reachable directly, no click needed.

No auth gate, no account, no consent banner.

## Read a company's openings

The career page listing, read with `read_page` after scrolling: each row gives the title, the city,
the contract type and the publication date, and the title carries the apply link. No keyless API is
known. A spontaneous application lives at `https://taleez.com/apply/sp/<slug>`.

## Fill

One page, an identity block then the company's own questions, closed by a single button labelled
**"Envoyer ma candidature"**. The CGU are accepted implicitly by submitting: there is no consent
checkbox to tick.

- Text, email, textarea and single-line question fields take `form_input`.
- The phone field sits next to a dialling-code button already set to the posting's country
  (`+33` for France) and its placeholder shows the national format, `06 12 34 56 78`. Write the
  national number with its leading zero, not the international form.
- The resume goes in through the `<input type=file>`: `find` exposes it as `button "(no name)"
  (file)`, and `file_upload` takes its `ref`. The "Déposer ou choisir un fichier" zone stays
  unclicked. Once uploaded the zone shows the file name, and hovering it reveals a red delete badge
  in the middle of the zone — do not click there.
- The date question is a text input with a calendar icon: clicking it opens a month picker with the
  current day highlighted. Clicking the day writes `JJ/MM/AAAA` into the field.
- A dropdown question is a custom `TZ-SELECT-LIST-V2` component. See § Traps.

## Traps

**`form_input` refuses every dropdown**: the answer is `Element type "TZ-SELECT-LIST-V2" is not a
supported form input`, and there is no `<select>` in the DOM at all
(`document.querySelectorAll('select').length === 0`). Click the field, type a few characters into the
"Rechercher" box it turns into, then click the option. After the click the field shows the option's
full label with no search icon: that is the selected state, and it survives a blur.

**A dropdown's options are invisible until it is opened.** Neither `read_page` nor a DOM query
exposes them, so the only way to know a closed list is to open it and screenshot. Type in its search
box to narrow a long list rather than scrolling it.

**A "Lien linkedin" field is not a text field.** It opens a network picker (Linkedin, X, Behance,
Github, Dribbble, Viadeo, Skype, Site perso). Pick "Linkedin" first: a second row appears, labelled
with the network and prefixed `https://`, and that row takes the URL **without its scheme**
(`www.linkedin.com/in/<handle>`). The prefix completes it.

**No "Apply with LinkedIn" button and no profile import** on the forms met so far: the identity
fields are filled by hand.

**`javascript_tool` is refused on anything that returns HTML or a URL with a query string**
(`[BLOCKED: Cookie/query string data]`). Reading `input.value`, `input.type` and label text works;
`outerHTML` and `a.href` do not. Read links with `read_page` on a `ref_id` instead.

**Required fields carry a red asterisk on the label**, and the form heads with "* Champs requis".
An optional field carries no asterisk at all.

**The submit is not the end.** « Envoyer ma candidature » lands on `/apply/thanks`, « Vous y êtes
presque ! » : Taleez e-mails a confirmation link to the candidate's address and calls that click
« obligatoire pour valider votre candidature ». Until it is clicked the status is `sent (unconfirmed)`;
the mail is the next thing to open (an employer, 2026-09-02).

## Last tested

2026-09-02 — an employer, Team Leader Sales B2B
(`<slug>.taleez.com`), form reached at `/applying` and filled end to end: five identity
fields, three custom questions, a resume upload, a date and a closed dropdown, not submitted.
