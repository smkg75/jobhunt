# Teamtailor

## Recognize

`<slug>.teamtailor.com/jobs`, or a company-branded `careers.<company>.com/jobs/<id>-<slug>` — the
`/jobs/<id>-<slug>` path is the signature that survives the branding.

## Reach the form

The apply button on the posting opens the form on the same page.

## Read a company's openings

No keyless API. Read the listing page `https://<slug>.teamtailor.com/jobs`.

## Fill

A short identity form followed by the company's own questions. The whole toolbox works:
`form_input` for the fields, one click per checkbox.

## Traps

"Postuler via LinkedIn" / "Connect with LinkedIn" sits at the top of the identity block. Try it
first: it pre-fills the identity fields from the LinkedIn profile, and the resume PDF still goes into
"Importer un CV" afterwards — the import does not replace that field. If it opens a LinkedIn sign-in
the session cannot complete, leave it and fill the fields by hand rather than staying stuck. On
2026-09-02 a pass declined the button on the strength of the old note here, and the candidate had to
click it himself.

The form is a full-screen modal on the posting's own URL: `tabs_context_mcp` shows the posting URL,
not a form URL. Reloading the page drops the modal and everything typed or imported into it.

**The modal can hang on "Loading application form".** On 2026-09-03 it opened, showed the
posting header and a spinner, and never rendered a field, through a scripted click and a real one
alike. The form has its own address: **`<posting-url>/applications/new`** renders the same form as
a full page, complete, and the browser tools work on it normally. Reach for that URL as soon as the
spinner outlives a few seconds, and note that nothing is lost by navigating while the modal is
still empty.

**A question's field name depends on its type.** The suffix is `answers_attributes][N][text]`,
`[textarea]`, `[choice]`, `[boolean]` or `[date]`. A verification pass written on `[choice]` alone
reports a filled boolean question as unanswered, which is how a correctly ticked "Have you worked in
a SaaS company before?" was read back as empty. Query `[name*="answers_attributes][N]["]` and let the
element tell you its type.

**A date question is a native date input**, not a free-text field: "What could be your potential
start date?" only takes an ISO value. A sentence such as "available immediately" cannot go in.

**A successful upload empties `input[type=file]`.** Teamtailor ships the file, then clears the
input, so `input.files.length` reads 0 on a resume that is correctly attached. The attachment shows
as a chip carrying the file name, and `candidate[resume_remote_url]` is filled once the upload lands:
check those two, never the input.

**The cookie banner ignores a `ref` click.** The click reports success and the banner stays,
covering the form. Click "Decline all non-necessary" at its coordinates, before anything is typed.

**The consent checkbox sits behind a Rails hidden twin.** `input[name="candidate[consent_given]"]`
matches a hidden input with value `0` first, so `querySelector(...).checked` returns false while the
real box is ticked. Filter on `input[type=checkbox]`. Two consents exist and they are not the same:
`candidate[consent_given]` is required to apply, `candidate[consent_given_future_jobs]` is a talent
pool opt-in and stays unticked.

**La soumission peut exiger une verification par e-mail, et la candidature n'est pas enregistree
sans elle.** Le 2026-09-09 un formulaire soumis n'a pas rendu `/thanks` mais
`/applications/email_verification_needed` : « Verify your email. We've sent you an email. Please
click the verification link in the email to complete your application. » Le mail part dans la
minute, depuis `no-reply@<slug>.teamtailor-mail.com`, objet « Complete the application for <role> ».
Tant que le lien n'est pas clique, rien n'est arrive chez le recruteur. Deux consequences : lire
l'URL apres le clic sur Submit avant de conclure quoi que ce soit, `/thanks` et
`/applications/email_verification_needed` etant deux issues differentes ; et ne pas journaliser
`sent` sur la seconde, sans quoi le compteur J+7 tourne sur une candidature qui n'existe pas.
**Cliquer ce lien n'est pas le geste de l'agent** : il vit dans la boite mail du candidat, et
c'est a lui de le faire, comme une signature electronique. La veille, une soumission sur le meme
site carriere avec la meme adresse etait passee directement a `/thanks` : la verification n'est
donc pas systematique, elle ne se devine pas, elle se lit.

## Last tested

2026-09-09 — une soumission de bout en bout sur `/applications/new` (domaine brande) : upload,
lettre en textarea, deux consentements, clic Submit. Piege de la verification par e-mail trouve la.
Earlier: 2026-09-06 — three applications submitted end to end on `/applications/new` (branded domains),
cover letter as a textarea with no maxlength, upload and cookie-banner traps found. Earlier:
2026-09-03, 2026-09-02.
