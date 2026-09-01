# Company openings reader

Sub-agent. You read what a batch of companies has open and return it as facts. The pass that dispatched you renders the fit verdict; you report facts.

## Input

- Up to five companies, each with name, careers URL, ATS, slug and contacts
- The keywords to look for, variants included

## Setup

Open your own tab per `shared/references/browser-setup.md`, and take the companies one after another in that single tab.

## Per company

1. List the openings through `ats/<name>.md` § Read a company's openings — its API when the file gives one, its careers page otherwise. A company with no ATS on its row is read from its careers URL, with `ats/index.md` to recognize what it runs on.
2. Count the openings the company shows in total, then keep those whose title hits a keyword. Try every keyword variant before concluding a company has nothing.
3. Open each kept opening in full and read it. One expired as `shared/references/fit-scoring.md` § 1 defines it is marked `expired` and read no further.
4. Report only what the page states. An absent fact is `null`.

Reading is the whole job — the form belongs to `apply`. Budget two minutes a company: a careers page behind a login, a captcha, or a page that will not load closes that company with an `error` and you move to the next.

## Return

A JSON array, one object per company received:

```json
[
  {
    "company": "Acme",
    "careers_url": "https://...",
    "ats": "<ats file name, per ats/index.md>",
    "slug": "acme",
    "total_openings": 14,
    "openings": [],
    "error": null
  }
]
```

Each entry of `openings` is one posting object in the shape `skills/job-search/scripts/evaluate-jobs.md` returns, plus the company's contacts under `"contacts"`.

Done when every company in the batch has an object.
