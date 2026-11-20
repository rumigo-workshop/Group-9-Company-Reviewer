# Contribution guide

Every team of 5 or 6 builds one research agent over two days. Day 1 is the engineering foundation, with no AI agents. Day 2 adds AI tools for the intelligence behind the product.

Instructors give the team leader admin access to this repo. The leader's first task is to add the rest of the team as collaborators.

1. Leader, first issue of the day: invite the other 4 or 5 members as collaborators with the Write role, not Admin. Each member accepts the invite, and the leader checks that everyone is listed under Settings, Collaborators and teams.
2. Leader, same milestone: protect `main` so changes arrive only through a pull request with one approval. Required status checks are added in Day 1 Milestone 7.

Teams of 5 or 6 use these seats. In a team of 5, the Lead also covers QA and docs.

| Seat | Day 1 focus | Day 2 focus |
| --- | --- | --- |
| Lead (admin, acts as coordinator) | Board, planning, release PRs, unblocking | AI plan, review of AI-written PRs, demo owner |
| Backend A | API contract, endpoints, database | Claude analysis, scoring |
| Backend B | Clerk token checks, async job and worker | Jev classifier, Crawl4AI scraper |
| Frontend | Login screens, query form, history | Recharts report, Stitch and Lovable designs |
| DevOps and CI | Docker, Compose, GitHub Actions | Reliability, security pass, deploy |
| QA and docs (6th seat) | Tests, README, troubleshooting | Evals, citations check, demo script |

The coordinator role is temporary and rotates every milestone. Nobody stays a permanent frontend, backend, QA or DevOps specialist. Everyone opens pull requests and everyone reviews someone else's. The author never approves their own pull request.

## How we write and track issues

Every issue in `docs/issues/` becomes a GitHub issue in this repo, attached to its milestone, with the template below. The Lead creates them in Day 1 Milestone 3 and the team estimates them together.

```
Title: Add GET /health

User story: As a teammate, I want a quick process check so I can see the API is up.
Scope: minimal liveness response. Out of scope: dependency readiness.
Acceptance (Given / When / Then):
Given the API is running
When I call GET /health
Then I get 200 and {"status":"ok"}
Size: XS / S / M / L
Owner: @name
Reviewer: @name
Merge mission: 19
Labels: core, backend
```

## Conventions

- Labels: `core` or `stretch`, plus one of `backend`, `frontend`, `devops`, `qa`, `docs`, `ai` (Day 2), and this repo's project label.
- Sizes: XS is a copy change, S is one endpoint, M is UI plus API plus tests, L should be split. Sizes are relative, not hours.
- Branches: `feat/<issue-number>-short-name`. Each branch is short-lived and holds one focused change.
- Commits: small, with a subject that names the change. No secrets, tokens or `.env` files, ever.
- Pull requests: the description answers what changed, why, how it was tested and which issue it closes, and links evidence such as test output or a `curl -i` result. Request one independent reviewer.

## Definition of done

Definition of Done applies to every issue, on top of its own acceptance criteria: tested, reviewed by someone other than the author, CI green, merged to `main`, and the board card moved to Done.

Issues marked core must finish. Issues marked stretch are dropped first if time runs short.
