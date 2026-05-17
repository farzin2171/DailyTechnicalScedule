---
stepsCompleted: [1, 2, 3, 4, 5, 6, 7, 8]
lastStep: 8
status: 'complete'
completedAt: '2026-05-17'
inputDocuments: ['_bmad-output/brainstorming/brainstorming-session-2026-05-16-213515.md']
workflowType: 'architecture'
project_name: 'NexArch'
user_name: 'Farzin'
date: '2026-05-17'
---

# Architecture Decision Document — NexArch

_This document builds collaboratively through step-by-step discovery. Sections are appended as we work through each architectural decision together._

---

## Project Context Analysis

### Requirements Overview

**Functional Requirements:**
- Orchestrate 2–3 microservices locally via .NET Aspire AppHost (single-command startup)
- Own IDP using .NET Core Identity with OAuth2/OIDC support (v1); Duende / Azure B2C pluggable (v2)
- Tenant middleware applied to every service — multi-tenancy is a first-class concern
- Typed HTTP connectors pattern for all external system calls
- Service-to-service communication with secure token propagation
- Minimal ReactJS frontend to demonstrate auth flows
- Publishable as GitHub template for the .NET community
- MicroForge-compatible structure — new services scaffold directly in

**Non-Functional Requirements:**
- Single-command local startup via .NET Aspire AppHost
- Clean, opinionated, learnable structure
- Azure deployment target (containerised)
- README + architecture diagrams included
- Runs on Windows, Mac, Linux

**Scale & Complexity:**
- Complexity level: **Medium** (multi-tenancy + identity + service mesh, deliberately small)
- Primary domain: Backend API + minimal ReactJS frontend
- Team: Solo developer, learning project

### Technical Constraints & Dependencies
- .NET 8 or 9
- ReactJS frontend
- Azure deployment target (ACA or AKS — decision needed)
- .NET Core Identity in v1; Duende / Azure B2C in v2
- .NET Aspire for local orchestration and observability dashboard

### Cross-Cutting Concerns Identified
- **Multi-tenancy** — tenant resolution, tenant context propagation across all services
- **Authentication/Authorization** — OAuth2/OIDC flows, token validation, tenant-aware claims
- **Observability** — .NET Aspire dashboard (tracing, logs, metrics out of the box)
- **Service-to-service auth** — how services call each other securely
- **Configuration** — per-environment config (local dev vs Azure)
- **Connector pattern** — standardised typed HTTP clients for external system calls

---

## Starter Template Evaluation

### Primary Technology Domain

.NET multi-service backend + ReactJS frontend, orchestrated by Aspire 13 (v13.3.1)

### Selected Approach: Custom Solution Initialization

NexArch is a multi-project solution — no single starter covers it. Initialization sequence:

```bash
# Solution
dotnet new sln -n NexArch

# Aspire orchestration
dotnet new aspire-apphost -n NexArch.AppHost
dotnet new aspire-servicedefaults -n NexArch.ServiceDefaults

# Microservices
dotnet new webapi -n NexArch.CatalogApi
dotnet new webapi -n NexArch.NotificationApi
dotnet new webapi -n NexArch.IdentityService

# ReactJS frontend
npm create vite@latest frontend -- --template react-ts
```

**Wire ReactJS into AppHost:**
```csharp
var react = builder.AddNpmApp("frontend", "../frontend", "dev")
    .WithExternalHttpEndpoints();
```

### Architectural Decisions Provided by Starter

**Orchestration:** .NET Aspire AppHost — single-command startup, auto service discovery, no hardcoded URLs

**Observability:** Aspire Dashboard included — unified tracing, logs, metrics for all services locally (zero config)

**ServiceDefaults:** OpenTelemetry, health checks, resilience patterns applied to every service by convention

**Frontend build tooling:** Vite + React + TypeScript — fast HMR, modern build pipeline

**Development experience:** Hot reload across all services, environment variable injection, service-to-service URLs auto-resolved by Aspire

### What Gets Built On Top
- .NET Core Identity in `IdentityService` (OAuth2/OIDC server)
- Tenant middleware registered in `ServiceDefaults` (applied to all services)
- Typed HTTP connectors pattern per external integration
- ReactJS PKCE auth flow against IdentityService

**Note:** Project initialization using the sequence above is the first implementation story.

---

## Core Architectural Decisions

### Decision Priority Analysis

**Critical Decisions (Block Implementation):**
- Database engine and isolation strategy
- OAuth2 flows and tenant identification
- Service-to-service communication pattern

**Important Decisions (Shape Architecture):**
- ORM and migration approach
- Frontend state management and component library
- API versioning and error handling standards

**Deferred Decisions (Post-MVP / v2):**
- Caching (Redis via Aspire — trivial to add)
- Multi-IDP switching (Duende / Azure B2C)
- Advanced authorization policies

---

### Data Architecture

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Database engine | **PostgreSQL** (via `Aspire.Hosting.PostgreSQL`) | First-class Aspire support, free, Azure Flexible Server in prod, excellent EF Core support |
| Database isolation | **One database per service** | Microservices best practice — CatalogApi, NotificationApi, IdentityService each own their DB |
| ORM | **EF Core 9, code-first migrations** | Standard .NET, per-service migrations applied at startup |
| Caching | **Deferred to v2** | Redis via Aspire is trivial to add; don't complicate v1 |

---

### Authentication & Security

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Frontend → IdentityService flow | **Authorization Code + PKCE** | Correct and secure for SPA clients |
| Service → Service flow | **Client Credentials** | Machine-to-machine, standard OAuth2 |
| Tenant identification | **HTTP header `X-Tenant-Id`** resolved by tenant middleware in ServiceDefaults | Simple, explicit, easy to debug; subdomain routing is overkill for sandbox |
| Token storage (React) | **Memory only** (never localStorage), refresh token rotation | Secure by default; PKCE flow handles cleanly |

---

### API & Communication Patterns

| Decision | Choice | Rationale |
|----------|--------|-----------|
| External API style | **REST (JSON over HTTP)** | OpenAPI auto-generated — feeds MicroForge + ContractForge later |
| Service-to-service | **Typed HttpClient via Aspire service discovery** | Aspire resolves URLs by name; typed clients = connector pattern enforced by MicroForge |
| API versioning | **URL versioning** (`/api/v1/`) | Simple, explicit, from day one |
| Error handling | **Problem Details (RFC 7807)** | Standardised error responses across all services |

---

### Frontend Architecture

| Decision | Choice | Rationale |
|----------|--------|-----------|
| State management | **Zustand** | Lightweight, no boilerplate, perfect for sandbox scale |
| Component library | **shadcn/ui** (Tailwind-based) | Portfolio-quality visuals with minimal effort |
| Routing | **React Router v6** | Standard, well-documented |

---

### Infrastructure & Deployment

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Azure hosting | **Azure Container Apps (ACA)** | Simpler than AKS, scales to zero (cost-friendly), Aspire has built-in ACA support via `azd up` |
| CI/CD | **GitHub Actions** | Free, integrates directly with repo |
| Secrets | **Azure Key Vault** (prod) + **Aspire local secrets** (dev) | Secure and standard |

---

### Decision Impact Analysis

**Implementation Sequence:**
1. AppHost + ServiceDefaults + 2 services running locally (Aspire foundation)
2. PostgreSQL containers via Aspire, EF Core in each service
3. IdentityService with .NET Core Identity + PKCE endpoint
4. Tenant middleware in ServiceDefaults, propagated via `X-Tenant-Id`
5. Typed HttpClient connectors pattern (first inter-service call)
6. ReactJS frontend with Zustand + shadcn/ui wired to PKCE auth flow
7. GitHub Actions CI, ACA deployment with `azd up`

**Cross-Component Dependencies:**
- Tenant middleware depends on IdentityService issuing tenant claims
- Typed connectors depend on Aspire service discovery being configured
- ReactJS auth flow depends on IdentityService PKCE endpoint being live
- ACA deployment depends on GitHub Actions + Key Vault being configured

---

## Implementation Patterns & Consistency Rules

### Naming Patterns

**Database (PostgreSQL + EF Core):**
- Tables: `snake_case` plural — `users`, `tenant_configs`, `notification_events`
- Columns: `snake_case` — `created_at`, `tenant_id`, `user_email`
- Primary keys: `id` (UUID — never integer)
- Foreign keys: `{entity}_id` — `user_id`, `tenant_id`

**API Endpoints:**
- Always plural nouns: `/api/v1/users`, `/api/v1/notifications`
- Route params: `{id}` — `/api/v1/users/{id}`
- Query params: `camelCase` — `?tenantId=x&pageSize=20`
- Tenant header: `X-Tenant-Id` (required on all protected endpoints)

**C# Code:**
- Classes/Methods: `PascalCase`
- Private fields: `_camelCase`
- Typed connectors: `{System}Connector` — `SendGridConnector`
- Middleware: `{Name}Middleware` — `TenantMiddleware`

**React/TypeScript Code:**
- Components: `PascalCase.tsx` — `UserCard.tsx`
- Hooks: `useX.ts` — `useAuth.ts`
- Zustand stores: `use{Name}Store.ts` — `useAuthStore.ts`
- API functions: `camelCase` — `getUsers()`, `createNotification()`

---

### Structure Patterns

**.NET Service Layout (applied to every microservice):**
```
NexArch.{ServiceName}/
├── Controllers/       ← thin, no business logic
├── Services/          ← business logic (interface + implementation)
├── Repositories/      ← EF Core data access
├── Models/            ← domain entities
├── DTOs/              ← request/response contracts
├── Middleware/        ← service-specific middleware
├── Connectors/        ← typed HttpClient connectors
└── Program.cs         ← minimal, wires DI only
```

**React Frontend Layout:**
```
frontend/src/
├── components/        ← reusable UI (shadcn/ui wrappers)
├── features/          ← feature folders
│   └── {feature}/
│       ├── components/
│       ├── hooks/
│       └── store.ts
├── lib/               ← utilities, API client
└── App.tsx
```

---

### Format Patterns

**API success response:**
```json
{ "data": { ... }, "meta": { "page": 1, "total": 42 } }
```

**API error response (Problem Details RFC 7807):**
```json
{
  "type": "https://nexarch.dev/errors/validation",
  "title": "Validation failed",
  "status": 400,
  "detail": "Email is required",
  "traceId": "00-abc123..."
}
```

- Dates: ISO 8601 UTC always — `2026-05-17T10:30:00Z`
- JSON field names: `camelCase` in all API responses
- IDs: UUID strings — never integers

---

### Communication Patterns

**Typed Connector pattern (mandatory for all external calls):**
```csharp
// ✅ Always
public class SendGridConnector(HttpClient http) : IEmailConnector
{
    public Task SendAsync(EmailMessage message) { ... }
}
// ❌ Never raw HttpClient
```

**Tenant context propagation:**
- Resolved in `TenantMiddleware` from `X-Tenant-Id` header
- Stored in scoped `ITenantContext` service — never passed as method parameter

---

### Process Patterns

**Error handling in controllers:**
```csharp
return result.IsSuccess
    ? Ok(result.Value)
    : Problem(result.Error.Message, statusCode: result.Error.StatusCode);
```

**React auth flow:**
1. Check `useAuthStore` (Zustand) first
2. Token expired → silent refresh via PKCE refresh token
3. Refresh fails → redirect to `/login`
4. Never read tokens from `localStorage`

---

### Enforcement Rules — All Agents MUST

1. Use `X-Tenant-Id` header on every protected API call
2. Use typed connectors — never raw `HttpClient`
3. Register services via `IServiceCollection` extension methods
4. Return Problem Details for all errors — never custom error shapes
5. Apply `ServiceDefaults` extension in every service's `Program.cs`
6. Use UUID for all IDs — never auto-increment integers

---

## Project Structure & Boundaries

### Complete Project Directory Structure

```
NexArch/                                    ← Solution root
├── NexArch.sln
├── README.md
├── .gitignore
├── azure.yaml                              ← azd deployment config
├── .github/
│   └── workflows/
│       ├── ci.yml                          ← build + test on PR
│       └── deploy.yml                      ← deploy to ACA on main
│
├── src/
│   ├── NexArch.AppHost/                    ← Aspire orchestrator
│   │   ├── NexArch.AppHost.csproj
│   │   └── Program.cs                      ← wires all services + DBs
│   │
│   ├── NexArch.ServiceDefaults/            ← shared Aspire config
│   │   ├── NexArch.ServiceDefaults.csproj
│   │   ├── Extensions.cs                   ← AddServiceDefaults()
│   │   ├── TenantMiddleware.cs             ← resolves X-Tenant-Id
│   │   └── TenantContext.cs               ← ITenantContext (scoped)
│   │
│   ├── NexArch.IdentityService/            ← .NET Core Identity + OIDC
│   │   ├── Program.cs
│   │   ├── Controllers/AuthController.cs
│   │   ├── Services/ITokenService.cs + TokenService.cs
│   │   ├── Models/ApplicationUser.cs + TenantUser.cs
│   │   ├── DTOs/LoginRequest.cs + TokenResponse.cs
│   │   └── Data/IdentityDbContext.cs + Migrations/
│   │
│   ├── NexArch.CatalogApi/                 ← Sample domain service #1
│   │   ├── Program.cs
│   │   ├── Controllers/CatalogItemsController.cs
│   │   ├── Services/ICatalogService.cs + CatalogService.cs
│   │   ├── Repositories/ICatalogRepository.cs + CatalogRepository.cs
│   │   ├── Models/CatalogItem.cs
│   │   ├── DTOs/CatalogItemRequest.cs + CatalogItemResponse.cs
│   │   ├── Connectors/NotificationConnector.cs
│   │   └── Data/CatalogDbContext.cs + Migrations/
│   │
│   ├── NexArch.NotificationApi/            ← Sample domain service #2
│   │   ├── Program.cs
│   │   ├── Controllers/NotificationsController.cs
│   │   ├── Services/INotificationService.cs + NotificationService.cs
│   │   ├── Repositories/INotificationRepository.cs + NotificationRepository.cs
│   │   ├── Models/Notification.cs
│   │   ├── DTOs/SendNotificationRequest.cs + NotificationResponse.cs
│   │   ├── Connectors/ExternalEmailConnector.cs
│   │   └── Data/NotificationDbContext.cs + Migrations/
│   │
│   └── frontend/                           ← Vite + React + TypeScript
│       ├── package.json
│       ├── vite.config.ts
│       ├── tsconfig.json
│       └── src/
│           ├── App.tsx
│           ├── components/ui/              ← shadcn/ui wrappers
│           ├── features/
│           │   ├── auth/
│           │   │   ├── components/LoginForm.tsx
│           │   │   ├── hooks/useAuth.ts
│           │   │   └── store.ts            ← useAuthStore (Zustand)
│           │   └── catalog/
│           │       ├── components/
│           │       ├── hooks/
│           │       └── store.ts
│           └── lib/
│               ├── apiClient.ts
│               └── utils.ts
│
└── tests/
    ├── NexArch.IdentityService.Tests/
    ├── NexArch.CatalogApi.Tests/
    └── NexArch.NotificationApi.Tests/
```

### Architectural Boundaries

**API Boundaries:**
- `IdentityService` → `/api/v1/auth/*` — PKCE authorize, token, refresh
- `CatalogApi` → `/api/v1/catalog-items/*` — protected, requires `X-Tenant-Id`
- `NotificationApi` → `/api/v1/notifications/*` — protected, requires `X-Tenant-Id`
- All services listen on Aspire-assigned ports (no hardcoded ports)

**Service-to-Service:**
- `CatalogApi` → `NotificationApi` via `NotificationConnector` (typed HttpClient)
- Resolved via Aspire service discovery: `http://nexarch-notificationapi`

**Data Boundaries:**
- Each service owns its own PostgreSQL database — no cross-DB queries ever
- `IdentityService` owns `identity_*` tables; other services store only `user_id` UUID references

**Tenant Boundary:**
- `TenantMiddleware` runs on every request in every service (wired via `ServiceDefaults`)
- Resolves `X-Tenant-Id` header → populates scoped `ITenantContext`
- All repository queries filter by `TenantId` automatically

---

## Architecture Validation Results

### Coherence Validation ✅

All technology choices are compatible: .NET 9 + Aspire 13.3.1 + EF Core 9 + PostgreSQL + React + Vite + Zustand + shadcn/ui — no version conflicts. Problem Details is built into ASP.NET Core. Typed connectors align with Aspire service discovery. `ServiceDefaults` pattern follows Aspire's own recommendations exactly.

**Minor note:** EF Core requires `Npgsql.EntityFrameworkCore.PostgreSQL` + `UseSnakeCaseNamingConvention()` — include in Story 1.

### Requirements Coverage ✅

| Requirement | Covered By |
|-------------|-----------|
| .NET Aspire orchestration | AppHost/Program.cs |
| .NET Core Identity + OIDC | IdentityService |
| Tenant middleware | ServiceDefaults/TenantMiddleware.cs |
| Typed HTTP connectors | Connectors/ in each service |
| ReactJS frontend + PKCE | frontend/ + AuthController |
| Per-service PostgreSQL DBs | Aspire + EF Core per service |
| GitHub Actions CI/CD | .github/workflows/ |
| Azure ACA deployment | azure.yaml + deploy.yml |
| Aspire observability | ServiceDefaults OpenTelemetry |
| MicroForge-compatible structure | Patterns + folder conventions documented |

### Gap Analysis

**Minor gaps (non-blocking):**
- `appsettings.Development.json` examples not specified — add to Story 1 scope
- Integration test project not in structure — add `NexArch.IntegrationTests/` alongside unit tests
- Localization infrastructure deliberately deferred to v2

**No critical gaps.**

### Architecture Completeness Checklist

- [x] Project context thoroughly analysed
- [x] Scale and complexity assessed
- [x] Technical constraints identified
- [x] Cross-cutting concerns mapped
- [x] Critical decisions documented with versions
- [x] Technology stack fully specified
- [x] Integration patterns defined
- [x] Performance considerations addressed
- [x] Naming conventions established
- [x] Structure patterns defined
- [x] Communication patterns specified
- [x] Process patterns documented
- [x] Complete directory structure defined
- [x] Component boundaries established
- [x] Integration points mapped
- [x] Requirements to structure mapping complete

### Architecture Readiness Assessment

**Overall Status: ✅ READY FOR IMPLEMENTATION**
**Confidence Level: High**

**Key strengths:**
- Aspire handles service discovery, observability, and local dev — you focus on business logic
- `ServiceDefaults` is the right home for cross-cutting concerns — scales as services grow
- NexArch's structure IS MicroForge's future template blueprint

**v2 enhancements (post Day 90):**
- Multi-IDP switching (Duende / Azure B2C)
- Redis caching via Aspire
- Localization system
- Full integration test suite
