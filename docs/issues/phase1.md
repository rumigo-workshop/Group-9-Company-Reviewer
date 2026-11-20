# Phase 1

Day 1 issues for Group 9, Company Reviewer.

Day 1 is the engineering foundation. Core issues must finish. Stretch issues are dropped first if time runs short.

## Milestone 1: Team up and repo access

**Goal.** Everyone has access, one shared understanding of the product, and a first merged pull request. Covers Merge Chapter 1 and Missions 1 to 7.

| ID | Issue | Seat | Size | Type | Done when |
| --- | --- | --- | --- | --- | --- |
| 1.1 | Leader adds every teammate as a collaborator with Write access (Mission 2) | Lead | XS | core | Everyone can open the team repo and create a branch; the access list shows 1 Admin and 4 or 5 Write |
| 1.2 | Reconstruct the assignment: pool requirement clues, write one shared user story and one open question for the client (Mission 1) | All, in pairs | S | core | A user story and a question are posted in the repo README or an issue |
| 1.3 | Connect to GitHub over SSH: check for an existing key, add only the public key, test (Mission 3) | Each member | S | core | ssh -T git@github.com succeeds for every member |
| 1.4 | Clone the repo, check the remote, map the monorepo folders (Mission 4) | Each member | S | core | Each member can name what lives in web, api, shared, db, tests and infra |
| 1.5 | Protect main: pull request required, one approval required | Lead | XS | core | A direct push to main is rejected |
| 1.6 | Onboarding PR: create onboarding/<handle>, add your contributor file, push, open a PR, review someone else's (Missions 5, 6) | Each member | S | core | Everyone has one merged PR and has reviewed one other PR |
| 1.7 | Merge-conflict drill: two members edit the same line, agree on the intent, resolve and run tests | Pair | S | core | Conflict markers are gone, tests pass, both authors confirm the result |

Missions 5 and 7 (switch branches, stash unfinished work) run as short guided drills before issue 1.6.

### Project variant

The shared user story in 1.2 starts differently per project:

- C: As an investor, I want a sourced research summary on a company and its founders so I can judge the opportunity.

## Milestone 2: Make it run

**Goal.** A repeatable, secret-safe local setup that every member can start, test and debug. Covers Merge Chapter 2 and Missions 8 to 15.

| ID | Issue | Seat | Size | Type | Done when |
| --- | --- | --- | --- | --- | --- |
| 2.1 | Follow the README and write a startup checklist: prerequisites, config, start, test (Mission 8) | DevOps | S | core | A checklist another member can follow without help |
| 2.2 | Local settings: copy .env.example to .env, fill the safe values, check that .gitignore keeps .env out of commits (Mission 9) | Each member | S | core | The app reads its config and git status shows no secret files |
| 2.3 | Complete the API Dockerfile and build the image (Mission 10) | DevOps | M | core | The API answers from inside its container |
| 2.4 | Compose stack: web, api and Postgres, with a health check on the database (Mission 11) | DevOps | M | core | docker compose up --build -d brings all three up and the api reaches Postgres by service name |
| 2.5 | Add SearXNG and Crawl4AI to Compose and smoke-test both with curl | Backend B | M | core | A curl against each service returns a valid response from the host |
| 2.6 | Smoke-test the Jev and Anthropic APIs with curl, keys read from .env only | Backend A | S | core | One call to each succeeds; no key appears in any commit, log or screenshot |
| 2.7 | Prove the baseline: use the app, run the starter tests, follow one request through the logs (Missions 12, 13) | QA | S | core | A short note lists the test result and the log line for one request |
| 2.8 | Setup mystery: reproduce a prepared failure, write a hypothesis, fix one cause, verify; then repair one unclear README line (Missions 14, 15) | Pair | S | stretch | Failure, fix and verification are written up; a teammate follows the new README line unaided |

### Project variant

None for setup. All three projects use the same Compose stack. Inside a container, localhost points at that container, so services reach each other by name, for example searxng and crawl4ai.

## Milestone 3: Plan the change

**Goal.** Turn the user need into small, testable work that the team can estimate and share out. Covers Merge Chapter 3 and Mission 16.

| ID | Issue | Seat | Size | Type | Done when |
| --- | --- | --- | --- | --- | --- |
| 3.1 | Write the product brief: who the user is, the problem, and the three most important user stories | Lead | S | core | Brief committed to docs/brief.md and agreed by the team |
| 3.2 | Set scope for each story: must change, not in this task, open question | All | S | core | Every story lists at least one explicit out-of-scope item |
| 3.3 | Write acceptance criteria as Given, When, Then for the three stories, covering login and the async job | QA | M | core | Each criterion describes something observable that a test can check |
| 3.4 | Create the project board (Backlog, Ready, In progress, Review, Done) and file every issue from Milestones 4 to 7 with the issue template (Mission 16) | Lead | M | core | Issues exist with milestone, labels, owner and reviewer |
| 3.5 | Size all issues with planning poker and split any L | All | S | core | No issue is larger than M; disagreements are written down with the unknown behind them |
| 3.6 | Sprint commitment: choose a finishable slice, set owners and reviewers, limit work in progress to 2 per person, plan who rotates across frontend and backend | Lead | S | core | The board shows a committed slice with names on every card |
| 3.7 | Pin the Definition of Done and a PR template in the repo | DevOps | XS | stretch | .github/pull_request_template.md asks what, why, how tested, ticket |

### Project variant

The three user stories to plan:

- C: enter a company and confirm the right company was matched; read a sourced summary of deals, customers, reviews and cases; see a verdict with a clear "research summary, not financial advice" note.

## Milestone 4: Agree on the API

**Goal.** Frontend and backend agree on one written contract before either side writes code. Covers Merge Chapter 4 (API design) and Missions 17 and 18. Nothing here needs a running server; the output is documents, a spec file and a mock.

| ID | Issue | Seat | Size | Type | Done when |
| --- | --- | --- | --- | --- | --- |
| 4.1 | Design the data model: users (Clerk user id), research jobs, sources, reports; write it as a diagram or table in docs/data-model.md | Backend A | M | core | Every field has a type and nullability; no private or internal field is planned for any response |
| 4.2 | Name the resources and build the paths: collection /research-jobs, one item /research-jobs/{id}, report /reports/{id}; choose query options such as ?status= and paging | Backend A, Frontend | S | core | A path table is agreed, with method and purpose for each row |
| 4.3 | Contract for starting a job: request body, 202 Accepted with a job id, and the error responses | Backend B, Frontend | M | core | One success example and at least two error examples are written down |
| 4.4 | Contract for job status: states queued, running, partial, failed, completed, plus what the UI polls | Backend B, Frontend | S | core | The polling response shape is fixed and covers failed and partial runs |
| 4.5 | Contract for history and report: list a user's jobs with paging and filter, and fetch one report with its sources | Backend A, Frontend | M | core | The report JSON lists its top-level keys and where each source link appears |
| 4.6 | Error and access contract: one error shape, and the rules for 400, 401, 403, 404, 409 and 422; a user can only read their own jobs | Lead, QA | S | core | A table maps each failure to a status and a safe message with no stack traces |
| 4.7 | Publish the contract as openapi.yaml and a Postman collection; check a mock with curl -i against every example (Missions 17, 18) | DevOps | M | core | Frontend and backend both sign off in a PR review; every example matches the mock's real response |

The client is not the final authority: the contract states which checks the server must always make, even if the UI already prevents them.

### Project variant

What changes is the request body and the report's top-level keys.

| Request body | Report keys |
| --- | --- |
| company, optional website | entity_match, deals, customers, sentiment with themes, red_flags (worded as reported or alleged), subscores, verdict |

## Milestone 5: Login and auth with Clerk

**Goal.** Only signed-in users reach the product, and every API call checks who is asking and what they may see. Applies the Merge lessons on authentication vs authorization, secret-safe setup and server-side rules.

| ID | Issue | Seat | Size | Type | Done when |
| --- | --- | --- | --- | --- | --- |
| 5.1 | Create the team's Clerk application in a development instance and choose the sign-in methods; put the publishable key in the web config and the secret key only in the server .env, with placeholders in .env.example | Lead, DevOps | S | core | The keys work locally and a diff review shows no real key committed |
| 5.2 | Frontend: wrap the app in Clerk's provider, build sign-up and sign-in pages, redirect signed-out users away from protected pages, send the session token on every API call | Frontend | M | core | A new user can sign up, sign in and sign out, and protected pages are unreachable when signed out |
| 5.3 | Turn on authenticator-app MFA (TOTP) in Clerk and test enrolling and signing in with a code | QA | S | core | A test account enrols an authenticator and cannot finish sign-in without the code |
| 5.4 | Backend: add middleware that verifies the Clerk session token on every protected route, using Clerk's backend SDK for your runtime or its JWKS endpoint | Backend B | M | core | A missing, expired or tampered token gets 401 in the agreed error shape |
| 5.5 | Link Clerk users to the database: create a users row from the Clerk user id on first sign-in, storing only what the product needs | Backend A | M | core | Signing in twice creates one row, not two |
| 5.6 | Authorization on the server: a user can only list, read or delete their own jobs and reports | Backend A | M | core | Requesting another user's job id returns the contract's 404 or 403, never the data |
| 5.7 | Auth tests and Postman cases: no token, bad token, valid token, and user A reading user B's job | QA | M | core | The cases run in CI-ready form and all pass; each failure names its status code |

Why the server must check: a hidden button or a hidden page is not security, because anyone can call the API directly with curl or Postman. Authentication answers who you are, and authorization answers whether you may do this.

### Project variant

Queries are sensitive (a company an investor is looking at), so per-user isolation in 5.6 is a hard requirement, not a stretch.

## Milestone 6: Build the research core

**Goal.** A signed-in user submits a query, gets a job id at once, and sees the job move through its states while a worker runs a real web search. No AI yet. Covers Merge Chapter 5 and Missions 19 to 25.

| ID | Issue | Seat | Size | Type | Done when |
| --- | --- | --- | --- | --- | --- |
| 6.1 | GET /health liveness endpoint with a test (Missions 19, 21) | Backend B | XS | core | Returns 200 and {"status":"ok"}; the test checks status and JSON shape; the docs say it does not prove dependencies are up |
| 6.2 | Database migrations for users, research jobs, sources and reports, following the Milestone 4 data model | Backend A | M | core | A fresh docker compose up creates the tables; no real personal data in seeds |
| 6.3 | POST /research-jobs: validate the body on the server, save the job as queued, return 202 with the job id | Backend A | M | core | Bad input gets 400 or 422 in the agreed error shape; a valid call creates exactly one job for the signed-in user |
| 6.4 | GET /research-jobs/{id} and GET /research-jobs with paging, scoped to the signed-in user | Backend B | M | core | Statuses match the contract and another user's jobs never appear |
| 6.5 | Frontend: query form with clear error feedback, plus a history page that polls job status | Frontend | M | core | Submitting shows the new job in the list, and its status updates without a page reload |
| 6.6 | Worker v1: pick up queued jobs, run a real SearXNG search for the query, store the result links as sources, set the job to completed or failed | Backend B | M | core | A submitted query ends with 10 or more stored source links; a stopped SearXNG gives failed with a safe message |
| 6.7 | Tests: unit tests for validation and status changes, an integration test for API plus Postgres, and one failing-before, passing-after bug test (Mission 21) | QA | M | core | Tests run in one command and the bug test fails on the broken code and passes after the fix |
| 6.8 | PR quality round: three merged PRs checked against the PR template, and one review comment answered by pushing to the same PR (Missions 22 to 25) | Lead | S | core | Each checked PR has what, why, how tested and a ticket link; the update appears in the original PR, not a new one |

Commit habits for every issue: stage only intended files, read the staged diff, write a subject that names the change. Branch from an up-to-date main and pull before you push.

### Project variant

The worker's search queries differ (public information only in all three):

- C: <company> funding, <company> customer reviews, <company> lawsuit or regulatory case, <company> news. Founder research is limited to public professional information.

## Milestone 7: Pass the checks

**Goal.** Every change to main goes through an automatic CI check and a reviewed PR, and the team knows how to read a red build, use a secret safely and merge with evidence. Covers Merge Chapter 6 and Missions 26 to 30.

| ID | Issue | Seat | Size | Type | Done when |
| --- | --- | --- | --- | --- | --- |
| 7.1 | First CI workflow: install dependencies, run lint and the tests on every push and PR (Mission 26) | DevOps/CI | M | core | A green check is linked to your commit and the workflow file is reviewed like code |
| 7.2 | Add the training secret as a GitHub Actions secret and use it in the workflow without printing it (Mission 27) | DevOps/CI | S | core | The run works, the log shows \*\*\* instead of the value, and no secret appears in the repo or in .env.example |
| 7.3 | Make CI run the Milestone 6 tests against a Postgres service container | DevOps/CI | M | core | Integration tests pass in CI the same way they pass locally |
| 7.4 | Repair the prepared failing pipeline: read the error, choose one cause, fix it, re-run (Mission 28) | Backend A | S | core | A fresh passing check for the corrected commit, and a two-line note on what the error message said |
| 7.5 | Turn on required status checks and at least one approval for main, no direct pushes (extends the M1 protection) | Lead | S | core | A test PR with a red check cannot be merged, and the leader's own pushes are blocked too |
| 7.6 | Merge-conflict drill: two people change the same lines, resolve it, keep both intents, re-run the tests | Backend B | M | core | The merge commit passes CI and the conflict markers are gone from every file |
| 7.7 | Approve and merge with evidence: the assigned person checks the latest commit, the tests and open discussions, then merges (Mission 29) | QA | S | core | The merged PR shows a green check, one approval and no open discussions |
| 7.8 | Sync after the merge: return to main, pull, delete the merged branch, confirm the change is local (Mission 30) | Everyone | XS | core | git log shows the merged work and no stale branches remain |
| 7.9 | Add a coverage report or lint badge to the README | QA | S | stretch | The badge reflects the real state of main |

Team habit: nobody merges their own PR. The seat that did not write the code checks it, and the Lead only steps in when the check is red for a reason the team cannot find.

### Project variant

The tests CI must protect differ per project:

- C: a test that a missing or ambiguous company name returns a clear "cannot match" error, not a guess.

## Milestone 8: Ship safely, recover, show what you learned

**Goal.** The team releases a version it can prove is running, handles one planted incident calmly, and hands a clean repo to another team. Covers Merge Chapters 7, 8 and 9 and Missions 31 to 33.

| ID | Issue | Seat | Size | Type | Done when |
| --- | --- | --- | --- | --- | --- |
| 8.1 | Tag a release (for example v0.1.0) and run the training release workflow (Mission 31) | DevOps/CI | M | core | The tag exists, the workflow is green and the release notes list what changed |
| 8.2 | Prove the running version: /health returns the version, one real flow works, and the logs show the matching request | QA | S | core | Screenshot or log lines linked in the release PR as evidence |
| 8.3 | Add structured request logging with a request id, never logging passwords, tokens or full queries with personal data | Backend B | M | core | One request can be followed across API and worker logs by its id |
| 8.4 | Incident drill: use requests and logs to find the planted bug, choose a safe fix and verify it (Mission 32) | Backend A | M | core | A fix PR with a test that failed before and passes after |
| 8.5 | Write the incident update: what users saw, the cause, the fix, what we will check next time (Mission 32) | Lead | S | core | A short message another person can read without asking questions |
| 8.6 | README handover: setup, run, test, env example, API list, known limits, who owns what (Mission 33) | QA | M | core | Another team follows it from a fresh clone and gets a running app without help |
| 8.7 | Hand over to another team: share the PR and instructions, let them try it, show the result and one lesson (Mission 33) | Everyone | S | core | The other team confirms they could run and verify your work |
| 8.8 | Day 1 retro: three things that worked, three that hurt, one habit for Day 2 | Lead | XS | core | Posted as an issue comment and linked from the README |
| 8.9 | Add a rollback note: how to go back to the previous tag | DevOps/CI | S | stretch | A teammate performs the rollback on the training release |

Before Day 2, tag the last Day 1 commit day1-complete. Day 2 work starts from that tag, so a failed AI experiment never costs the team its Day 1 baseline.

### Project variant

The planted incident and the handover limits are worded per project:

- C: the incident is a company matched to a similarly named one. The README states that the output is a research summary, not financial advice.

## Day 1 exit checklist

The instructor checks these before the team moves to Day 2. Every line should be answerable with a link, not a promise.

- [ ] All members are collaborators; the leader is Admin, the others have Write access.
- [ ] main is protected: PR required, one approval, required checks, no direct pushes.
- [ ] docker compose up starts the app, Postgres, SearXNG, Crawl4AI and the Jev connection from a fresh clone.
- [ ] Each of the Anthropic, Jev, SearXNG and Crawl4AI smoke checks has a passing test or script.
- [ ] OpenAPI or Postman contract matches the running API for /research-jobs, /research-jobs/{id} and /reports/{id}.
- [ ] Clerk sign-up, sign-in and authenticator MFA work, and the backend rejects a missing, expired or forged session token.
- [ ] A signed-in user submits a query, gets 202 and a job id, and sees the status change in the history page.
- [ ] The worker runs a real SearXNG search and stores source links.
- [ ] CI is green on main, and a red PR cannot be merged.
- [ ] A tagged release exists with evidence that it runs.
- [ ] The incident drill has a fix PR and a short incident update.
- [ ] Every member has at least one merged PR and one review comment.
- [ ] No secrets in the repo history; .env.example has placeholders only.
- [ ] The tag day1-complete exists.

If a team misses more than three lines, it uses the first part of Day 2 to close the gaps. The Day 2 AI tools work better on a clean baseline.
