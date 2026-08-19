# Mark Pease

I run day-to-day operations for the [Veteran Loan Fund](https://www.veteranloanfund.com/), a national program that has deployed over **$100 million to 1,350 veteran and military spouse-owned businesses** through twelve CDFIs across twelve states. I also built the platform it runs on.

Both of those are the job. I handle the partner network, the application pipeline, compliance, and the reporting that goes to funders and the board. I also wrote the roughly 15,000-line system that produces that reporting, because the alternative was a week of retyping spreadsheet totals every quarter.

## What I'm like to work with

If I find a problem, I fix it so it can't come back.

I audited our reporting pipeline expecting to find arithmetic errors. Every figure reconciled to the cent. The problems were all in the definitions — percentages that double-counted anyone recorded in two categories, a rolling twelve-month window that quietly stretched to fifteen whenever a partner had a quiet quarter, a missing column rendering as `0%` when what it meant was "nobody has this data."

Most of what's below came out of that audit. Not write-ups of the findings. Tools that make those specific mistakes impossible to repeat.

## Selected work

**[reporting-platform-case-study](https://github.com/Ark2027/reporting-platform-case-study)** — How four unrelated data sources became one reporting system, and the five architectural decisions that mattered. No code, just the reasoning and what I'd do differently.

**[entity-match-pipeline](https://github.com/Ark2027/entity-match-pipeline)** — Matching business records across two systems that share no key. Deterministic scoring first, then an LLM adjudicator with schema-constrained tool use, grounding checks that reject invented IDs, and an evaluation harness measuring precision, error rate, and cost per call.

**[lending-portfolio-dashboard](https://github.com/Ark2027/lending-portfolio-dashboard)** — A ten-page analytics dashboard with no framework and no build step, running on generated data. [See it running.](https://ark2027.github.io/lending-portfolio-dashboard/)

**[statement-extract](https://github.com/Ark2027/statement-extract)** — Pulls figures out of financial statement PDFs, then checks them against the accounting identities they have to satisfy. It only corrects a balance sheet when exactly one value can be the wrong one; otherwise it says so and changes nothing.

**[impact-data-quality](https://github.com/Ark2027/impact-data-quality)** — Audits submitted spreadsheets before their numbers reach a published report. Refuses to certify rather than guessing at what a blank means.

**[heart-explorer](https://github.com/Ark2027/heart-explorer)** — Interactive 3D heart anatomy in the browser, 47 structures, no build step. I built it after my dad's brain aneurysm, to understand what I was looking at. [See it running.](https://ark2027.github.io/heart-explorer/)

## How I got here

Economics at UT Austin. Before that, a hedge fund internship analyzing South American cell tower operators and Irish homebuilders.

I took this job as a financial analyst straight out of school and started writing code because the quarterly reporting took a week and I wanted that week back. Everything here is self-taught, built on real problems where being wrong had consequences.

## Reach me

[LinkedIn](https://www.linkedin.com/in/mark-pease-97063023a/)
