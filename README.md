# Mark Pease

I build data systems where being wrong is expensive, and the controls that catch it when they are.

Most of that has been at the [Veteran Loan Fund](https://www.veteranloanfund.com/), a national program that has deployed over **$100 million to 1,350 veteran and military spouse-owned businesses** through twelve CDFIs across twelve states. I wrote the platform it runs on, about **15,000 lines** across ingestion, reconciliation, quality control, and the front end. I also run the program's day-to-day operations, which is why the requirements were never a guess.

## The platform

Four sources that agreed on nothing: partner Excel workbooks, a CRM Postgres database behind an SSH hop, a document store of PDFs and Word files, and an advertising API. Each one slow, each one able to fail on its own schedule.

The architecture that made it work:

**Precompute to a file rather than query live.** The dashboard reads one payload and never touches a database or an API. Live-querying four sources means the page is only as available as the least available one, and a network share that goes unreachable during a board meeting is not hypothetical.

**Every source fails independently, and a failure is never a zero.** When the CRM query fails, the pipeline serves the last good snapshot and marks it stale with the timestamp of when it was actually fresh. Failing the whole refresh makes the dashboard frequently unavailable; substituting zero makes it confidently wrong. Stale data with its age attached is the only option of the three that never lies.

**Publish a relational mirror alongside the JSON.** Every refresh writes 25 SQLite tables so analysts can connect their own tools instead of routing questions through me. It took an afternoon and removed most of the ad-hoc requests.

Written up in full, with what I'd do differently, in **[reporting-platform-case-study](https://github.com/Ark2027/reporting-platform-case-study)**.

## Selected work

**[entity-match-pipeline](https://github.com/Ark2027/entity-match-pipeline)** — Matching business records across two systems that share no key. A deterministic pass sorts every record into auto-accepted, deferred, or discarded, and an LLM adjudicator only ever sees the deferred band. It can't touch an auto-accept or resurrect a discard, so it can add automation but can't damage a decision that was already made.

The evaluation harness runs against 92 labeled records, eight of which are traps where the right answer is "none of these," included specifically to see whether a model asked to find a match will invent one. The adjudicator took automation from 95.2% to **100%** at **100% precision, 0% error, and 100% trap survival**, resolving four deferrals and correctly declining all eight traps. Cost and latency are measured per call. 69 tests.

**[statement-extract](https://github.com/Ark2027/statement-extract)** — Pulls figures out of financial statement PDFs and checks them against the accounting identities they have to satisfy. A parser that crashes gets fixed; one that returns `2026` where total assets belong does not, because that's a number and every report downstream renders it without complaint.

It corrects a balance sheet only when exactly one value can be the wrong one. When two of the three could be, it says so and changes nothing, because guessing trades a visible problem for an invisible one. 62 tests, most of them regressions.

**[lending-portfolio-dashboard](https://github.com/Ark2027/lending-portfolio-dashboard)** — Ten pages, no framework, no build step, including a US choropleth built by hand from Albers-projection GeoJSON. Every figure derives from two generated ledgers rather than being written to a target, which is what keeps the totals, partner splits, and KPI tiles from disagreeing with each other. [See it running.](https://ark2027.github.io/lending-portfolio-dashboard/)

**[impact-data-quality](https://github.com/Ark2027/impact-data-quality)** — Audits submitted spreadsheets before their numbers reach a published report. It refuses to certify rather than guessing what a blank means, and CI asserts that it refuses when handed a defective workbook.

**[snapshot-diff](https://github.com/Ark2027/snapshot-diff)** — Diffs two versions of a dataset with no ID column, so a corrected typo stops looking like a row being deleted and re-added. The pairing was quadratic until I timed it: 1,600 rows a side took 1.5 seconds. Reframing it as an index lookup rather than a search got the same answers in **34ms**.

**[heart-explorer](https://github.com/Ark2027/heart-explorer)** — Interactive 3D heart anatomy in the browser, 47 structures, no build step. I built it after my dad's brain aneurysm, to understand what I was looking at. [See it running.](https://ark2027.github.io/heart-explorer/)

Everything above with code in it has a test suite running in CI. Also here: **[google-ads-connector](https://github.com/Ark2027/google-ads-connector)**, which pulls ten advertising reports into one payload and scrubs credentials out of anything it logs, and **[wp-runtime-content](https://github.com/Ark2027/wp-runtime-content)**, a WordPress plugin that lets non-technical staff edit an app's copy without a deploy.

## Where the tooling came from

If I find a problem, I fix it so it can't come back.

I audited our reporting pipeline expecting arithmetic errors. Every figure reconciled to the cent. The problems were all in the definitions — percentages that double-counted anyone recorded in two categories, a rolling twelve-month window that quietly stretched to fifteen whenever a partner had a quiet quarter, a missing column rendering as `0%` when what it meant was "nobody has this data."

The audit itself is written up in **[reporting-audit-case-study](https://github.com/Ark2027/reporting-audit-case-study)**. Three of the repos above are that audit turned into tooling: not write-ups of the findings, but code that makes those specific mistakes impossible to repeat.

## How I got here

I grew up around politics, and economics always struck me as the part of it that actually decides things. So I studied it at UT Austin. Building software was not the plan.

From 2022 into 2023 I did equity research at Hewes Fund, building three-statement models on Latin American and European companies to work out why a market had something priced wrong. The European side meant sitting down with the management teams at Cairn Properties and Glenveagh in Dublin to understand capital structure and how they were allocating funds for buybacks.

The lesson that stuck: pulling the numbers is the easy part. Knowing whether a number means what its label says is the actual job, and that turned out to be the same problem I've spent three years solving in code.

[PeopleFund](https://peoplefund.org/) hired me as a Fund Analyst in May 2023 to build the financial models for spinning the Veteran Loan Fund out into its own entity, working with the CEO, CFO, and outside counsel on the structure. It was a corporate finance job. I had never written software.

I knew R and Stata from economics, so I wasn't starting from scratch. The first thing I built was an AWS Lambda endpoint that took what applicants uploaded to our site and turned it into a dashboard. I learned the rest from Coursera, documentation, reference books, and friends who were incredibly patient.

About six months in my manager left, and I ended up reporting to the CEO. Everything arrived at once: the website, the reporting, compliance, architecture, and the SQL.

The shift came down to one specific thing. I'd been downloading Excel exports off our Postgres server and cleaning them up in Python, which was already an improvement on doing it by hand. Then it occurred to me that I didn't have to download anything at all. I could tunnel into the server over SSH and pull live numbers instead. That collector is still running today, and the platform described above grew out of it.

## Reach me

[LinkedIn](https://www.linkedin.com/in/mark-pease-97063023a/)
