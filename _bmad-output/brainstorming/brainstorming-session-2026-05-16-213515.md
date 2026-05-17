---
stepsCompleted: [1, 2, 3]
inputDocuments: []
session_topic: 'Portfolio project ideas for a 90-day learning plan'
session_goals: 'Generate cool project ideas that leverage microservices/.NET/ReactJS/Duende/Azure expertise while growing into AI integration and spec-driven design, building a visible portfolio'
selected_approach: 'progressive-flow'
techniques_used: ['What If Scenarios']
ideas_generated: [4]
context_file: ''
status: 'in-progress — paused after Phase 1, to continue tomorrow'
---

# Brainstorming Session Results

**Facilitator:** Farzin
**Date:** 2026-05-16

## Session Overview

**Topic:** Portfolio project ideas for a 90-day learning plan
**Goals:** Generate cool project ideas that leverage microservices/.NET/ReactJS/Duende/Azure expertise while growing into AI integration and spec-driven design, building a visible portfolio

### Session Setup

Farzin is a Tech Lead specializing in microservices architecture (.NET, ReactJS, Duende Identity Provider, Azure). Recently started working with AI and spec-driven design. Already building a **Release Management** tool (helps release new versions of a microservices product). Looking for additional portfolio projects to learn by building over 90 days.

**Key architectural context:**
- Multi-tenant SaaS product
- Typed HTTP connectors for calling external systems
- Tenant middleware (applied to every service)
- Duende Identity Provider for auth
- Authorization currently managed via pipeline (not self-serve)
- Localization system per tenant

## Technique Selection

**Approach:** Progressive Technique Flow

- **Phase 1 - Exploration:** What If Scenarios — maximum idea generation without constraints
- **Phase 2 - Pattern Recognition:** Mind Mapping — organize ideas into portfolio themes
- **Phase 3 - Development:** SCAMPER Method — refine and differentiate top concepts
- **Phase 4 - Action Planning:** Decision Tree Mapping — sequence projects into 90-day plan

---

## Phase 1: Expansive Exploration — What If Scenarios

### Ideas Generated

---

**[Portfolio #1]**: Microservice Narrator / Living Docs AI
*Concept:* An AI agent that reads code, Swagger specs, and runtime telemetry to generate always-current natural language documentation per microservice. Junior-friendly, auto-updating.
*Novelty:* Targets the specific pain of microservice sprawl — 20 services, nobody knows what half of them do anymore.
*Status:* Real pain identified (documentation IS a team challenge) but Farzin is not personally passionate about this direction. Deprioritized.

---

**[Portfolio #2]**: MicroForge CLI ⭐ HOT
*Concept:* A CLI + AI scaffolder that generates production-ready .NET microservices pre-wired with the team's tenant middleware, typed HTTP connectors, Duende auth, and localization — from a plain language description. Example: `scaffold new-service "handles customer notifications, integrates with SendGrid, secured by Duende"` → fully opinionated, standards-compliant project, ready to run.
*Novelty:* Encodes institutional architecture knowledge (connectors, middleware topology, tenant-awareness, auth patterns) that currently lives only in senior devs' heads. Not a generic scaffolder — it's YOUR architecture, driven by AI and natural language.
*Learning value:* Building it forces deep understanding of multi-tenancy, Duende authorization flows, and localization patterns by encoding them. The tool IS the architecture documentation.
*Stack:* .NET CLI, AI/LLM integration, Duende Identity, Azure

---

**[Portfolio #3]**: ArchGuard AI ⭐ HOT
*Concept:* A GitHub Actions bot / PR reviewer that knows YOUR architecture rules — flags raw `HttpClient` usage instead of typed connectors, missing tenant middleware, incorrect auth patterns, localization gaps — and explains WHY it violates patterns, not just that it does.
*Novelty:* Not a generic linter. Trained on your specific architecture decisions. A junior dev gets a senior review on every PR, even at 2am.
*Learning value:* Forces you to articulate architecture rules precisely enough for AI to enforce them — a masterclass in architecture documentation.
*Stack:* GitHub Actions, .NET analysis, AI/LLM, Roslyn or custom rules

---

**[Portfolio #4]**: TenantAuth Studio ⭐⭐ HOTTEST — potential real product
*Concept:* A ReactJS dashboard + backend policy engine that lets tenant admins define, modify, and manage authorization flows dynamically — no pipelines, no code changes. Tenant admins own their security model (role assignment, permission policies, workflow-based auth). Integrates with Duende Identity.
*Novelty:* Solves the gap between "auth lives in code" and "business needs to control access" — a real pain in every multi-tenant SaaS product. AI angle: suggest policies, detect anomalies, flag risky permission combinations.
*Learning value:* Deep dive into dynamic authorization, policy engines, Duende internals, ReactJS admin UI.
*Background:* Has been a long-running internal idea at Farzin's company. Currently auth changes require pipeline deployments — no self-service for tenants.
*Stack:* ReactJS, .NET, Duende Identity Provider, Azure, AI/LLM

---

### Emerging Portfolio Theme

A coherent **"Developer Experience Platform for Microservices"** is emerging:

- **MicroForge** — create a service right, from day one
- **ArchGuard** — keep it right, on every PR
- **TenantAuth Studio** — let tenants own their security, dynamically

These three form a natural trilogy. Release Management (existing) completes the picture: build → guard → secure → release.

---

**[Portfolio #5]**: NexArch — Modern Microservices Sandbox ⭐⭐⭐ FOUNDATION
*Concept:* A from-scratch miniature microservices architecture built with .NET Aspire — a clean sandbox modernizing current architecture patterns. Pluggable identity gateway (switch between .NET Core Identity, Azure B2C, or Duende). The place to experiment, break things safely, and deeply understand authorization, localization, and multi-tenancy.
*Learning value:* Understanding by rebuilding. .NET Aspire orchestration, identity federation, multi-IDP patterns — all in one sandbox.
*Stack:* .NET Aspire, .NET Core Identity, Azure B2C, Duende, ReactJS, Azure

---

**[Portfolio #6]**: ContractForge ⭐
*Concept:* Spec-first workflow enforcer — OpenAPI spec written first, AI generates integration tests, CI blocks merges if code drifts from spec. Introduces contract testing culture through automation, not discipline.

---

**[Portfolio #7]**: DevOnboard AI ⭐
*Concept:* Interactive AI simulation of the microservices system — new developers chat with it, break things safely, explore service connections before touching real code.

---

**[Portfolio #8]**: TeamMind AI ⭐
*Concept:* Team-trained AI pair programmer that knows YOUR codebase, conventions, and past decisions — available in every developer's IDE. Not generic Copilot.

---

**[Portfolio #9]**: DebtRadar ⭐
*Concept:* AI that continuously scans repos and surfaces live technical debt scores with trends and top contributors per sprint.

---

**[Portfolio #10]**: Release Intelligence ⭐
*Concept:* AI co-pilot inside Release Management — analyses signals before deployment and outputs risk score with explanation. Phase 2 of the existing project.

---

## Phase 1 Complete — 10 Ideas Captured

| # | Project | Heat | Status |
|---|---------|------|--------|
| 1 | Microservice Narrator | ❄️ | Deprioritized — real pain, low passion |
| 2 | MicroForge CLI | ⭐ | Selected for 90-day plan |
| 3 | ArchGuard AI | ⭐ | Backlog — future sprint |
| 4 | TenantAuth Studio | ⭐⭐ | Selected for 90-day plan |
| 5 | NexArch Sandbox | ⭐⭐⭐ | Selected — flagship project |
| 6 | ContractForge | ⭐ | Backlog — bake into NexArch later |
| 7 | DevOnboard AI | ⭐ | Backlog |
| 8 | TeamMind AI | ⭐ | Backlog |
| 9 | DebtRadar | ⭐ | Backlog |
| 10 | Release Intelligence | ⭐ | Phase 2 of existing project |

---

## Phase 2: Pattern Recognition — Mind Mapping

**Selected top 3 for 90-day plan:** NexArch + MicroForge + TenantAuth Studio

**Rationale:** Foundation + Tooling + Identity = complete portfolio story. "I built the platform, I built the tool to populate it, and I solved the hardest security problem in multi-tenant SaaS."

**Full ecosystem map:**
```
BUILD IT RIGHT     → NexArch + MicroForge + ContractForge
KEEP IT RIGHT      → ArchGuard + DebtRadar
SECURE IT          → TenantAuth Studio
SHIP IT SAFELY     → Release Management + Release Intelligence
GROW THE TEAM      → DevOnboard AI + TeamMind AI
```

---

---

## Phase 4: Action Planning — 90-Day Decision Tree

### Realistic Constraints
- ~1–2 hrs/weekday, ~4–6 hrs/weekend ≈ 150–180 hours total
- **Rule:** One project at a time, in sequence. Done = runs + is on GitHub.
- If Month 1 runs long, TenantAuth Studio slides to Month 4 — it's self-contained.

### Scope Cuts (Deliberate)
| Project | Cut from v1 | Reason |
|---------|------------|--------|
| NexArch | Multi-IDP switching, localization | .NET Aspire learning curve needs the time |
| MicroForge | LLM natural language input, IDE extension | Templates ARE the value — AI is v2 |
| TenantAuth Studio | Policy engine, simulator, AI anomaly detection | Self-service roles is the real pain to solve |

### Timeline

**Month 1 — Learn and Extract (Days 1–30)**
- Week 1–2: Learn .NET Aspire, build AppHost + 2 services
- Week 3–4: Reverse-engineer existing services → MicroForge templates
- ✅ Deliverable: NexArch runs locally, MicroForge has 1 working template

**⚡ Checkpoint 1 — Day 30**
NexArch runs locally with 2 services? MicroForge has 1 template? → Yes: continue. No: cut localization or CLI, 5 more days.

**Month 2 — Build and Connect (Days 31–60)**
- Week 5–6: NexArch v1 complete + published to GitHub
- Week 7–8: MicroForge CLI ships, generates services into NexArch
- ✅ Deliverable: One command scaffolds a service into NexArch end-to-end

**⚡ Checkpoint 2 — Day 60**
MicroForge generates into NexArch? TenantAuth v1 showing roles? → Yes: sprint. No: cut one feature, focus on demos that work.

**Month 3 — TenantAuth and Polish (Days 61–90)**
- Week 9–10: TenantAuth Studio — Duende wrapper + tenant role dashboard
- Week 11: Connect TenantAuth to NexArch identity layer
- Week 12: Polish, READMEs, 3 demo videos, portfolio write-ups
- ✅ Deliverable: 3 published repos, 3 demos, 1 portfolio story

### The Portfolio Story
> "I'm a tech lead who took my team's architecture, modernized it with .NET Aspire, built the tooling to generate services consistently, and solved the multi-tenant authorization problem that every SaaS company struggles with — all while learning AI integration in practice."

### Backlog (Post Day 90)
ArchGuard AI → ContractForge → DebtRadar → DevOnboard AI → TeamMind AI → Release Intelligence

---

## Phase 3: Idea Development — SCAMPER

### NexArch — Refined by SCAMPER (all lenses accepted)
- **Combine:** Built together with MicroForge — NexArch is the platform MicroForge generates into
- **Adapt:** Published as public GitHub template for .NET community
- **Modify:** Progressive complexity — v1 starts with 2 services, layers added incrementally
- **Eliminate:** Multi-IDP switching dropped from v1 — .NET Core Identity only first
- **Put to other uses:** Doubles as team onboarding sandbox

### MicroForge — Refined by SCAMPER (Reverse lens selected)
- **Reverse:** Scan existing production services, extract real patterns into templates — templates are guaranteed correct because they came from working code

### TenantAuth Studio — Refined by SCAMPER (Adapt lens selected)
- **Adapt:** Wrap Duende admin UI rather than build from scratch — focus purely on the tenant self-service gap, 70% less work
