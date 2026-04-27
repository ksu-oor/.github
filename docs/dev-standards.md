
NOTE IN DEVELOPMENT AND NOTHING FINALIZED HERE - LOTS TO TWEAK AND MAKE LESS COMPLICATED - OVERKILL RIGHT NO IMO


**Owner:** Office of Research, Kennesaw State University **Maintainer:** OOR Data/Dev Team (Tyra, Raj, Willie) **Status:** Living document — review quarterly **Location:** `ksu-oor/dev-standards` (proposed)

---

## 1. Purpose & Scope

This document defines how the Office of Research (OOR) builds, maintains, and operates software at KSU. It applies to:

- OOR staff developers
- Student workers contributing code
- Faculty collaborators on OOR projects
- Contractors and short-term collaborators
- AI agents (Claude, Cursor, Copilot, Linear Agents, etc.) acting on our behalf

"Development" here means anything we build or run: production applications, internal tools, prototypes, vibe-coded experiments, scripts, data pipelines, and one-off automations. The bar applies to all of it, scaled to risk.

The "why" behind each rule is included on purpose. The rules will change; the reasons usually won't.

---

## 2. Development Scope & Authorization

These four expectations come from Karin (EVPR) and frame everything below.

### 2.1 Development outside OOR or for non-research faculty

Before pursuing development outside (1) the Office of Research or (2) for research faculty, discuss it with leadership first.

**Why:** There is potential for unforeseen political challenges and/or perceived risk (fiscal, student data, FERPA, IRB).

### 2.2 Development within OOR

Coordinate with the data/dev team (Tyra, Raj, Willie) before starting.

**Why:** Minimize duplication of effort and let Tyra maintain visibility across all OOR efforts. Vercel and GitHub will provide much of this signal automatically — the goal is visibility, not bureaucracy.

### 2.3 Lightweight "intent to build"

Before spinning up a new repo, open a Linear issue (or post in the team channel) describing what you're about to build and why. One paragraph is enough. This makes coordination automatic instead of something Tyra has to ask for.

### 2.4 Rapid prototyping is welcome

The team builds fast and sometimes creates dozens of repos. That's a feature, not a problem. The standards below are designed to make repos cheap to create _and_ cheap to discover — not to slow you down.

---

## 3. Team Structure: Builders vs. Maintainers

We operate with two postures, sometimes worn by the same people at different times:

- **Development** — new features, vibe-coded experiments, faculty requests requiring net-new work
- **Maintenance** — bugs, tech debt, on-call, dependency upkeep, faculty-facing fixes on existing tools

People can move between roles, but at any given moment they're operating in one mode. Linear reflects this with two teams (see §6).

**Why separate them:** Maintenance work gets buried under feature sprints when they share a backlog. Splitting them lets us measure each fairly, set different review bars, and protect maintainers from being treated as a feature-dev overflow queue.

Neither role is lower-status. Builders depend on maintainers; maintainers depend on builders writing things that are maintainable.

---

## 4. Source Control Standards

### 4.1 Where code lives

All code developed for KSU lives in the **`ksu-oor`** GitHub organization. If you don't have access, contact Willie — he can grant admin access to the org.

**Why:** Redundancy and elimination of single points of failure or delayed response.

**Personal-repo exception:** If a personal repo makes more sense (open-source collaboration, individual notoriety, conference work, etc.), coordinate with leadership so we maintain visibility. Mirror or link from `ksu-oor` where reasonable.

### 4.2 Naming conventions

Repos should be scannable at a glance:

- `oor-<project>` — production OOR tools
- `prototype-<name>` — experiments, proofs of concept
- `faculty-<lastname>-<project>` — faculty-collaborator work
- `internal-<name>` — internal scripts, automations
- `archived-<original-name>` — optional rename when archiving

### 4.3 Required GitHub topics

Every repo gets at least one status topic and any relevant flags:

- **Status (required):** `prototype`, `production`, or `archived`
- **Flags (as applicable):** `ai-assisted`, `faculty-<name>`, `data-pipeline`, `student-data`, `public`

Topics make the org filterable. Use them.

### 4.4 Branch protection

Anything tagged `production` requires:

- Protected `main` branch
- At least one human review on PRs
- Passing CI before merge
- No direct pushes to `main`

Prototypes can be looser, but if a prototype starts being used by anyone outside the dev team, promote it to `production` rules.

### 4.5 Archiving, not deleting

When a prototype dies, archive it on GitHub and add a one-line note to the README explaining why and what (if anything) replaced it. Don't delete — future-you will want the context.

---

## 5. README & Documentation Standards

Every repo includes a README at the root covering:

- **Purpose** — what this is and who it's for
- **Status** — prototype / production / archived
- **Owner** — primary human contact
- **Setup** — how to get it running locally
- **Usage** — how to actually use it
- **Deployment** — URL and where it deploys from (if applicable)
- **AI assistance** — if Claude/Cursor/Copilot/an agent wrote substantial portions, note it. Not as a stigma — as calibration for future readers.

A template README lives in `ksu-oor/dev-standards/templates/README.md` (proposed). Copy it.

**Why:** Quick path to understanding the purpose and intent of the source.

---

## 6. Linear Workflow (AI-Augmented)

Linear workspace: **`ksu-oor`**

### 6.1 Two teams, shared projects

- **Development team** — features, vibe-coded experiments, new faculty requests
- **Maintenance team** — bugs, tech debt, on-call, dependency upkeep

Both teams contribute issues to the same **Projects**, but each gets its own cycles, triage queue, and statuses. This keeps maintainer work from getting buried under feature sprints and lets us measure them separately.

### 6.2 Workflow statuses

Use an extended status set so AI-generated work is visible:

```
Backlog → Todo → In Progress (Human) / In Progress (Agent) → In Code Review → In QA → Ready to Deploy → Done
```

The split "In Progress" matters because PRs from AI agents need a different review bar than human PRs — reviewers should know upfront.

**Starting point:** use a **label** (`ai-assisted` or `agent-authored`) rather than separate statuses. Labels are lighter weight and easier to evolve. Promote to statuses if the label isn't enough.

### 6.3 Where AI inserts, by role

**Solo / vibe-coding devs:** Linear Agents are the natural fit. Workflow:

1. Developer triages an issue
2. Decides "this is vibe-able"
3. Assigns to the agent with the `ai-assisted` label
4. Reviews the PR when it comes back

Good agent candidates: well-scoped bugs, small refactors, test coverage, dependency bumps, boilerplate.

**Maintenance team:** Lean on AI for the unglamorous parts:

- Auto-triage of incoming bugs (Linear can route, label, and prioritize from customer feedback and Slack)
- Sentry-to-Linear automation so production errors become triaged issues without human relay

**Everyone:** Use Linear's AI for project updates and status reporting. This is where the dev/maintenance split usually creates friction — leads end up chasing both groups for updates. Project updates and dashboards summarize what each team shipped without anyone writing a status doc.

### 6.4 Three guardrails for AI work

1. **Label every AI-significant issue or PR** with `ai-assisted` or `agent-authored` so reviewers calibrate accordingly and we can later measure quality differences.
2. **Required human code review on all agent PRs.** No auto-merge, even for trivial changes.
3. **Off-limits for AI agents:** authentication code, payments, data migrations, anything touching production secrets, anything touching student or HR data, anything FERPA- or IRB-relevant.

### 6.5 Day in the life — Maintenance

A bug comes in from Sentry → auto-creates a Linear issue in the Maintenance team's triage queue → maintainer labels it, decides it's vibe-able, assigns to agent with `ai-assisted` label → agent opens a PR with a fix and tests → maintainer reviews, requests one tweak → merges → Linear auto-closes the issue and the deploy pipeline picks it up.

### 6.6 Day in the life — Development

The Dev team is in a cycle building a new feature. Sub-issues get split:

- Test scaffolding, boilerplate components → agent-assigned with `ai-assisted` label
- Core logic, anything touching auth or data → human-only

Reviewer treats agent PRs with extra scrutiny on edge cases and test coverage.

---

## 7. Vercel & Deployment

Vercel team: **`ksu-research`** at https://vercel.com/ksu-research/

### 7.1 Standards

- All KSU deployments live under the `ksu-research` Vercel team
- Vercel project name should match (or clearly map to) the GitHub repo name
- Environment variables live **only** in Vercel — never committed to the repo, never pasted into Slack
- Preview deployments on every PR; production deploys from `main` (or a designated branch)

### 7.2 Domains

- Custom domains are added by the Vercel team admins (currently the data/dev team)
- DNS records are documented in the domain inventory (see §9)
- SSL/TLS auto-renew is monitored

### 7.3 Agent-authored deploys

Previews of agent-authored code are fine and encouraged — that's how reviewers see what an agent built. Production deploys of agent-authored code require human approval at the PR stage, not at the Vercel stage.

---

## 8. Credentials & Secrets Management

### 8.1 Single source of truth

The team uses **one** password manager as the single source of truth for shared credentials. **Recommendation: 1Password Teams** unless we already have something else standardized. Pick one, document it here, and stop using anything else.

### 8.2 Hard rules

Credentials, API keys, tokens, and secrets are **never**:

- Committed to a repo (even a private one)
- Posted in Slack, email, or Linear comments
- Pasted into AI chat sessions, copilots, or agent prompts
- Stored in plaintext on local disk outside the password manager

### 8.3 Service accounts vs. personal accounts

- **Personal accounts** for individual access to shared tools (GitHub, Linear, Vercel)
- **Service accounts** for automation, CI/CD, deployments, and anything that needs to outlive a single person
- Service-account credentials live in the password manager with a named owner

### 8.4 API keys

- Scoped to the minimum permissions needed
- Named so the owner and purpose are obvious
- Logged in the password manager with creation date and rotation date
- Rotated when the owner leaves the team or when an AI session may have seen the key

### 8.5 Rotation triggers

Rotate immediately when:

- A team member leaves
- A credential is suspected of exposure (committed, pasted, screenshot, etc.)
- An AI session was given a credential as context
- A vendor reports a breach

---

## 9. Domain & DNS Management

### 9.1 Inventory

The data/dev team maintains an inventory of OOR-owned domains, including:

- Domain name
- Registrar
- Renewal date
- DNS host
- Primary contact
- What it points at

### 9.2 Access

- Registrar credentials live in the team password manager
- At least two team members have registrar access at all times
- New domains: open a Linear issue requesting one, with intended use

### 9.3 SSL/TLS

- Auto-renew enabled wherever possible
- Expiration monitoring on production domains
- Mixed-content and HTTPS enforcement on production sites

---

## 10. Data Handling & Risk

### 10.1 Sensitive data categories

Extra scrutiny applies to:

- **Student data (FERPA)** — names, IDs, grades, enrollment, anything tied to a student
- **Research data (IRB-relevant)** — human-subjects data, especially identifiable
- **Fiscal data** — budgets, salaries, grant financials
- **HR data** — personnel records of any kind

### 10.2 AI-specific data rules

The following must **never** be pasted into a chat, copilot context, agent prompt, or AI tool of any kind:

- Student PII or any FERPA-protected data
- Identifiable IRB data
- Fiscal data tied to identifiable individuals
- Production secrets, credentials, or API keys
- HR records

When in doubt, redact or ask before pasting.

### 10.3 Escalation

Escalate to leadership / Karin / KSU IT before proceeding when:

- A project will touch any of the categories above
- A faculty request sits outside OOR scope (see §2.1)
- A vendor wants data we don't normally share
- A breach or potential breach is suspected

### 10.4 Backups

Anything not in GitHub gets a documented backup plan. "It's on my laptop" is not a backup.

---

## 11. Team Norms & Working Together

These aren't decoration. They're how we work.

- **Default to transparency.** Share what you're building, even rough work. Other people knowing about your project is almost always better than them not knowing.
- **Assume good intent.** Before assuming someone duplicated your work or stepped on your project, ask. Most of the time it's a coordination gap, not malice.
- **Code reviews are about the code, not the person.** Critique the diff, not the dev. AI-authored code gets the same review rigor — not less, not more.
- **Disagree in writing, decide together, document the decision.** It's fine to disagree. It's not fine to relitigate the same decision every two weeks.
- **Pair on hard things.** Don't suffer alone for days. If you've been stuck for half a day, ask. If you've been stuck for a full day, you should already have asked.
- **Say "I'm stuck" within a day, not a week.** Nobody is judged for getting stuck. People are judged for hiding it until it's a fire.
- **Credit collaborators visibly.** Commit co-authors, README acknowledgments, Linear comments, demos. Credit AI honestly when it did substantial work — that's calibration, not weakness.
- **Async-first.** Respect time zones and working hours. Default to written communication so people can catch up on their own time.
- **Builders and maintainers respect each other.** Neither role is lower-status. Neither group dumps on the other. Maintainers aren't a feature-dev overflow queue; builders aren't a future tech-debt machine.
- **Onboarding is paired.** Every new dev gets a buddy for their first two weeks.

---

## 12. Onboarding & Offboarding Checklists

### 12.1 New dev — Day 1

- [ ] GitHub: invited to `ksu-oor` org with appropriate role
- [ ] Linear: invited to `ksu-oor` workspace, assigned to Dev or Maintenance team
- [ ] Vercel: invited to `ksu-research` team
- [ ] Password manager: account created, shared vaults granted
- [ ] AI tooling: access to Claude / Cursor / Copilot / Linear Agents as applicable
- [ ] Read this document
- [ ] Paired with a buddy for two weeks
- [ ] Walked through the active Linear projects

### 12.2 Departing dev

- [ ] GitHub access revoked from `ksu-oor` org
- [ ] Linear access removed
- [ ] Vercel access removed
- [ ] Password manager access removed
- [ ] AI tooling access removed
- [ ] Repos owned by them transferred to a new owner
- [ ] Personal-named branches archived or deleted
- [ ] Open Linear issues reassigned
- [ ] Any credentials they touched are rotated
- [ ] Service accounts they owned are reassigned

---

## 13. Review Cadence

- **This document:** reviewed quarterly. Anyone can propose changes via PR to the standards repo.
- **AI-related sections (§6, §8.5, §10.2):** reviewed every 1–2 months. The tooling is moving fast and our practices should too.
- **Domain inventory (§9.1):** reviewed at every quarterly review.
- **Onboarding/offboarding checklists (§12):** reviewed whenever someone joins or leaves — that's the moment we find out what's missing.

---

_Last updated: [date]_ _Next review: [date]_
