# Phase 2

Day 2 issues for Group 9, Company Reviewer.

Day 1 taught the team to build and ship by hand. Day 2 gives them AI tools and asks for much more: a full intelligence pipeline, a polished report, and hardening. The skill being taught is not "let the agent do it" but "plan, delegate, check, and keep the evidence".

Tools introduced on Day 2: Antigravity agents, Supermemory, codegraph, Superpowers, Matt Pocock skills, MCP, Stitch and Lovable, a UI design skill and the Cloudflare audit skills.

| Milestone | Theme | Issues |
| --- | --- | --- |
| D2-M1 | AI toolkit: rules, memory, codegraph, planning | 8 |
| D2-M2 | Intelligence pipeline: Jev, Crawl4AI, Claude, partial and failed states | 9 |
| D2-M3 | Project-specific intelligence (A, B or C) | 6 per project |
| D2-M4 | Experience: designs, Recharts, my-runs, polish | 8 |
| D2-M5 | Reliability and AI safety | 7 |
| D2-M6 | Extended features, two to three per team | 3 to 6 |
| D2-M7 | Security, Cloudflare audit and deployment | 6 |
| D2-M8 | Wrap-up, review and demo | 6 |

Ground rules for every AI-assisted issue

- The human owns the plan. Write the issue's done-when criteria first, then let the agent propose a plan, and read the plan before any code is written.
- One issue, one branch, one PR. The agent never pushes to main, and branch protection from Day 1 still applies.
- Every agent PR gets a human reviewer from a different seat. The PR description says which parts were AI-written and how they were checked.
- Tests come first or alongside. An agent change without a test is not done.
- No secrets in prompts, rules files or memory. Keys live in environment variables and Actions secrets only.
- Treat everything scraped from the web as untrusted text. It is data to analyse, never instructions to follow.
- Keep a token log: model, rough input and output size, and cost per run for each issue that calls an AI service.
- If the agent is stuck after two attempts, stop, restate the problem in smaller steps, or switch to doing it by hand.

Seats on Day 2. Roles rotate again. Add one standing duty: an AI Reviewer, rotating each milestone, who checks agent plans and diffs and owns the evidence of how each AI-written change was verified.

## Milestone 1: AI toolkit

**Goal.** Set the team up to work with AI agents safely and repeatably before any feature work. Day 2 ID format is D2-1.1, D2-1.2 and so on.

| ID | Issue | Seat | Size | Type | Done when |
| --- | --- | --- | --- | --- | --- |
| D2-1.1 | Open the repo in Antigravity, start the agent panel, and run one read-only task: "explain this repo's structure" | Everyone | XS | core | Each member can show the agent's answer and say one thing it got wrong or missed |
| D2-1.2 | Write the project rules file: stack, folder map, commands to run tests and lint, code style, "never do" list (no secrets, no direct pushes, no scraping private data) | Lead | M | core | The rules file is merged by PR and an agent given a task follows at least three of its rules |
| D2-1.3 | Add codegraph so the agent can see how modules connect, and check it on one real question ("what calls the job status update?") | Backend A | M | core | The agent's answer matches what the team finds by reading the code |
| D2-1.4 | Set up Supermemory with project decisions: API contract, job statuses, naming, what was learned on Day 1. No keys, personal data or scraped text stored | DevOps/CI | M | core | A fresh agent session recalls the contract and the status names correctly |
| D2-1.5 | Token tracking: a small helper or log that records model, input and output size and estimated cost for every AI call and agent session | Backend B | M | core | A run shows its token count and cost in one log line or table |
| D2-1.6 | Plan one real feature with Superpowers: brainstorm, write a plan, split into tasks, review the plan before any code | Lead | M | core | A plan file in the repo with tasks, tests per task and a reviewer's sign-off |
| D2-1.7 | Apply a Matt Pocock skill to turn the plan into small tickets and tighten the ticket wording | QA | S | core | At least five issues rewritten so each has a clear done-when and test |
| D2-1.8 | Connect one MCP server (for example GitHub or the database, read-only) and use it for one task | Frontend | M | stretch | The task is done through MCP, and the team notes what permission it needed and why read-only was enough |

What the team learns here: a short, specific rules file beats a long one; memory should hold decisions, not secrets; and a plan the team has read is the cheapest bug fix.

### Project variant

Add one project line to the rules file and to memory:

- C: allegations are phrased as reported or alleged, founder research is public professional information only, and the right company must be matched.

## Milestone 2: Intelligence pipeline

**Goal.** Replace the Day 1 "search only" worker with the full pipeline: search, classify, scrape, analyse, score, and save a structured report. Use agents for the plumbing, and keep a human check on every AI-written piece.

Pipeline per job: SearXNG returns links, Jev labels each link, Crawl4AI scrapes the useful ones, Claude turns the content into a structured report, the score is computed by plain code, and the report is saved with its sources.

| ID | Issue | Seat | Size | Type | Done when |
| --- | --- | --- | --- | --- | --- |
| D2-2.1 | Define the report JSON schema (sections, scores, sources, warnings) and validate every Claude response against it | Lead | M | core | Invalid or missing fields are caught, logged and retried once, never shown to the user |
| D2-2.2 | Jev classifier step: send each link title and snippet to Jev, store the label and confidence, drop irrelevant | Backend A | M | core | A job stores one label per link, and a test covers the irrelevant case |
| D2-2.3 | Crawl4AI scraper step: scrape only the kept links with a timeout and a size cap, store clean text and the URL | Backend B | M | core | A dead link or a slow page is recorded as skipped and does not stop the job |
| D2-2.4 | Claude analysis step: prompt with the scraped text, ask for the schema, include the source id for every claim | Backend A | L | core | The report fills every section from the sources and each claim points to a stored source |
| D2-2.5 | Scoring in plain code from the Claude output, with a short written rule list in the README | Backend B | M | core | The same input gives the same score, and a test proves it |
| D2-2.6 | Job states: queued, running, partial, failed, completed, with a safe user message for each | DevOps/CI | M | core | A stopped Crawl4AI gives partial or failed and never a blank report |
| D2-2.7 | Save and fetch the report: GET /reports/{id} returns the schema, sources and warnings for the owner only | Backend B | M | core | Another user's report id returns 404 |
| D2-2.8 | Eval set: 5 fixed queries with expected labels and expected report fields, run by one command | QA | M | core | The script prints pass or fail per query and is run in CI on a small subset |
| D2-2.9 | Prompt versioning: keep prompts in files, name each version, store the version on every report | Frontend | S | stretch | A report shows which prompt version produced it |

Review rule for this milestone: the AI Reviewer reads every prompt and every schema change. A prompt is code. It gets a PR, a reviewer and a test.

### Project variant

The project-specific rules for labels, scoring and report content are in Milestone 3. The pipeline steps above are the same for this project.

## Milestone 3: Project-specific intelligence

**Goal.** Make each report smart in the way its problem statement needs. Each team uses only its own table. The first issue in each table is for the AI to help write, and the last is for humans to judge.

| ID | Issue | Seat | Size | Type | Done when |
| --- | --- | --- | --- | --- | --- |
| C-3.1 | Jev labels: company_site, funding_or_deal, news_article, customer_review, customer_or_partner_mention, founder_profile, legal_or_regulatory_case, irrelevant, with example links per label | Backend A | M | core | The classifier matches expected labels on at least 16 of 20 eval links |
| C-3.2 | Company matching: show the matched company (name, site, location) and ask for confirmation when the name is ambiguous | Backend B | M | core | A common name returns a choice, not a silent guess |
| C-3.3 | Deals and customers extraction: date, amount, investors, named customers, each linked to a source | Backend A | M | core | The deals timeline can be drawn from the JSON and every item has a source |
| C-3.4 | Legal and allegation handling: phrase as "reported" or "alleged", show the source and date, and separate allegations from court or regulator outcomes | Lead | L | core | No allegation appears as a fact, and a test checks the wording on a sample case |
| C-3.5 | Founder section limited to public professional information (role history, public interviews), with no family, home or private details | Backend B | M | core | The prompt, the schema and a test all exclude private details |
| C-3.6 | Sub-scores and verdict: traction, customers, sentiment, legal, founders, each visible, ending with a verdict plus the line "research summary, not financial advice" | QA | M | core | The disclaimer is in the report and the verdict can be re-calculated from the sub-scores |

Human check for every project: before D2-M4, each team reads two full reports end to end and lists every claim that has no source. Fix the pipeline, not the report.

## Milestone 4: Experience

**Goal.** Turn the report JSON into a clear, good-looking report page and a useful my-runs page, using design tools to explore and agents to build, with the team's own eye as the final check.

| ID | Issue | Seat | Size | Type | Done when |
| --- | --- | --- | --- | --- | --- |
| D2-4.1 | Design three directions for the report page in Stitch, pick one as a team and write down why | Frontend | M | core | A chosen design with a short reason, saved as images or a link in the repo |
| D2-4.2 | Build a first clickable version in Lovable or a similar tool, then compare it with the Stitch design | Frontend | M | core | A list of three differences and which ones the team keeps |
| D2-4.3 | Create a UI design skill (colors, spacing, type scale, component rules) and add it to the repo so the agent builds consistent screens | Lead | M | core | A new screen built by the agent follows the skill without extra prompting |
| D2-4.4 | Report page layout: summary on top, score, sections, sources list, warnings and low-evidence notices | Frontend | L | core | A completed report renders from GET /reports/{id} with no hard-coded text |
| D2-4.5 | Recharts visuals from the report JSON, one per chart listed below, each with a text alternative and an empty state | Frontend | L | core | Every chart renders from real data, handles missing data, and has a table fallback |
| D2-4.6 | My-runs page: list of past reports with status, date and query, filter by status, open, delete and re-run | Backend B | M | core | A user sees only their own runs, and delete removes the report and its stored scraped text |
| D2-4.7 | UI polish: loading skeletons, friendly error states, mobile layout, keyboard focus and readable colors | QA | M | core | Passes a manual check at phone width and with keyboard only, and has no overlapping text |
| D2-4.8 | Chart review: the team presents each chart and says what decision it supports and what it could mislead about | Lead | S | core | One sentence per chart in the README, with no chart kept just because it looks nice |

### Project variant

Recharts visuals for this project.

| Charts to build | Data note |
| --- | --- |
| Deals timeline, customer segment bars, sentiment donut with themes, red-flag matrix, sub-score radar | Red flags show "reported" or "alleged" wording and the source date |

Recharts does not draw every shape natively (funnel, gauge, matrix, quadrant). Use the closest chart type or build with simple SVG, and keep the data shape the same.

## Milestone 5: Reliability and AI safety

**Goal.** Make the pipeline survive slow, broken and hostile input, and keep AI cost under control. An intelligent app that fails in silence is worse than a simple one that fails clearly.

| ID | Issue | Seat | Size | Type | Done when |
| --- | --- | --- | --- | --- | --- |
| D2-5.1 | Retries with backoff for SearXNG, Jev, Crawl4AI and Anthropic calls, with a cap on attempts | Backend A | M | core | A forced timeout is retried twice, then the step is marked failed, never an infinite loop |
| D2-5.2 | Rate limits: per-user job limit and a concurrency limit in the worker | Backend B | M | core | A user starting too many jobs gets 429 and a clear message |
| D2-5.3 | Prompt-injection defence: scraped text is wrapped as data, the prompt says to ignore instructions inside it, and the output schema blocks free-form actions | Lead | L | core | A planted page saying "ignore previous instructions" does not change the report, and a test keeps it that way |
| D2-5.4 | Token and cost budget per job: truncate or summarise long pages, stop at a cap, and show "partial" if the cap is hit | Backend A | M | core | The token log from D2-1.5 shows every job under its cap |
| D2-5.5 | Idempotent job pickup: a job is never processed twice if the worker restarts | DevOps/CI | M | core | Killing the worker mid-job and restarting it gives one report |
| D2-5.6 | Failure drills: stop each service one at a time and write what the user sees | QA | M | core | A table of service, failure, user message, and log line, with no stack traces shown to users |
| D2-5.7 | Output safety check: a second pass scans the report for private data, unsupported claims and banned content for this project | QA | M | stretch | Eval fails if a banned item appears |

### Project variant

The banned content and the highest-risk failure per project:

- C: unfair statements about people and companies. Block allegations written as facts, private personal details, and a verdict that ignores the evidence strength.

## Milestone 6: Extended features

**Goal.** Each team picks two or three features that fit its project and its pace, so faster teams are not bored and slower teams are not buried. Instructors recommend; the team chooses and writes the choice in an issue. Teams of 5 or 6 can take three; teams that fall behind on D2-M2 take one.

| Feature | Issues the team writes | Size | Best fit |
| --- | --- | --- | --- |
| SendGrid email when a report finishes | X-1 Send a plain email with a link to the report; X-2 store the key in an environment variable and add a template; X-3 test with a sandbox address and handle a failed send without failing the job | M | A, C |
| Chrome desktop notification when a report finishes | X-4 ask permission only after a user action; X-5 show the notification on status change, and open the report on click; X-6 fall back quietly when permission is denied | M | All |
| Live progress updates | X-7 worker writes steps (searching, classifying, scraping, analysing); X-8 UI shows a progress bar from the steps, polling or server-sent events; X-9 a stuck step shows a time-out hint | M | All |
| Async hardening, retries and rate limits | X-10 dead-letter state for jobs that keep failing; X-11 cancel a running job; X-12 admin view of queue length | L | B |
| Source citations in the report | X-13 numbered citations inside each claim; X-14 a sources panel with label, domain and date; X-15 a check that fails if a claim has no source | M | All, strongly recommended |
| Security pass and deployment | See D2-M7 below, which is the required version of this feature | L | All |
| PDF export and report comparison | X-16 export a report to PDF with charts and sources; X-17 compare two reports of the same query side by side; X-18 highlight what changed | L | A, B |
| TOTP recovery and lockout | X-19 recovery codes for a lost authenticator; X-20 lockout after repeated failed sign-ins; X-21 an audit log of sign-in events. Done with Clerk settings where possible | M | All who finished M5 early |
| OpenAI as a second AI provider | X-22 an AIProvider interface with Anthropic and OpenAI adapters; X-23 a settings switch for the provider; X-24 run the eval set on both and write the differences | L | A, C |

Rules for extended features:

- Each feature needs its own issue list, written by the team before building, using the same table format as above.
- A feature is done only when it has a test, a line in the README, and a short demo in the Day 2 wrap-up.
- No feature may bypass the safety rules from D2-M5 (prompt-injection defence, rate limits, banned content).
- Using a second AI provider means the evals from D2-2.8 must pass on both, and the token log must record the provider.

### Project variant

Suggested starting picks, not rules: citations and a second AI provider, to compare how each phrases allegations.

## Milestone 7: Security, Cloudflare audit and deployment

**Goal.** Check the app the way an outsider would, fix what is found, and deploy it with secrets handled properly. The Cloudflare audit skills guide the review; the team still decides what to fix.

| ID | Issue | Seat | Size | Type | Done when |
| --- | --- | --- | --- | --- | --- |
| D2-7.1 | Secret scan: check the repo, the history, the rules file and Supermemory for keys, then rotate anything found | DevOps/CI | S | core | A scan report with zero findings, and any rotated key noted without printing its value |
| D2-7.2 | Run the Cloudflare audit skills on the app (headers, caching, bot and rate-limit settings, TLS) and list each finding with severity | Lead | M | core | A findings table with at least five items, each marked fix, accept or defer, with a reason |
| D2-7.3 | Fix the top findings: security headers, CORS limited to the app origin, cookies and session settings, input size limits | Backend A | M | core | The re-run audit shows the chosen findings closed |
| D2-7.4 | Authorization review: try to read another user's job, report and run from the API with a valid token | QA | M | core | Every attempt returns 403 or 404, and the attempts are saved as tests |
| D2-7.5 | Dependency and container check: update vulnerable packages, drop the root user in the Dockerfile, pin image versions | DevOps/CI | M | core | The audit command is green or has a written exception |
| D2-7.6 | Deploy to the training environment (Cloudflare or the instructors' target) with environment variables, health check, logs and the Clerk production keys kept out of the repo | DevOps/CI | L | core | A public URL works end to end, and a failed deploy can be rolled back to the previous tag |

Safety line for the audit. The team audits only its own app and the training environment. Nobody scans a system they do not own, and the audit skill's output is kept inside the team repo.

### Project variant

Add a data-retention note: how long scraped pages and reports stay stored, and how a user deletes them.

## Milestone 8: Wrap-up and demo

**Goal.** Review what AI helped with and what it cost, leave a repo another team can use, and show the work.

| ID | Issue | Seat | Size | Type | Done when |
| --- | --- | --- | --- | --- | --- |
| D2-8.1 | Token and cost review: total tokens, cost per report, the three most expensive steps, and one change that would cut cost | Backend B | M | core | A one-page table in the repo with real numbers from the token log |
| D2-8.2 | Memory review: read everything stored in Supermemory, delete anything stale, personal or secret, and keep the decisions that helped | Lead | S | core | A short list of what was kept and what was removed |
| D2-8.3 | AI-use report: for five merged PRs, say what the agent wrote, what the human changed and what bug the review caught | QA | M | core | Five entries, including at least one place where the agent was wrong |
| D2-8.4 | Final README: what it does, architecture picture, how to run, how to test, API list, report charts, limits and the safety lines for this project | Frontend | M | core | A new reader runs the app from the README only |
| D2-8.5 | Demo script and recording: sign in, submit a query, show progress, open the report, show sources, show one failure handled | Everyone | M | core | A three to five minute demo that any member can present |
| D2-8.6 | Team retro and next steps: three things to build next, three risks, one thing each person learned | Lead | S | core | Posted as an issue and linked from the README |

Tag the final commit v1.0.0, and close every milestone only when all its core issues are closed. Stretch issues left open are listed in the README as future work.
