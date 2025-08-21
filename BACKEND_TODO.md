# FootballIQ Backend – Project Tasks & Subtasks

This document breaks the backend architecture into actionable work items. Each task includes subtasks, deliverables, acceptance criteria, and dependencies. Owners and estimates are placeholders.

Legend: [ ] = To do, [~] = In progress, [x] = Done

---

## 0. Project Foundation

- [ ] 0.1 Repository layout and standards
  - Subtasks:
    - [ ] Create monorepo structure:
      - `backend/` (API + services)
      - `jobs/` (background workers)
      - `infra/` (IaC)
      - `packages/` (shared: `types`, `sdk`, `config`)
    - [ ] Add base tooling: ESLint, Prettier, EditorConfig, Husky + lint-staged
    - [ ] Set Node.js LTS and TypeScript baseline
  - Deliverables: Monorepo skeleton, tooling config
  - Acceptance: Repo boots with `pnpm i && pnpm -r build` without errors
  - Dependencies: None
  - Owner: TBC

- [ ] 0.2 Runtime & framework
  - Subtasks:
    - [ ] Scaffold NestJS app in `backend/`
    - [ ] Set up module structure (feature modules per API)
    - [ ] Global validation pipe, exception filter, request context
  - Deliverables: Running NestJS app with health endpoint
  - Acceptance: `GET /health` returns 200 OK with app metadata
  - Dependencies: 0.1
  - Owner: TBC

- [ ] 0.3 Config & secrets
  - Subtasks:
    - [ ] Typed config service (runtime schema via zod)
    - [ ] `.env` per env (local, dev, staging, prod)
    - [ ] Integrate AWS SSM/Secrets Manager (prod/staging)
  - Deliverables: Config module, secret provider
  - Acceptance: App boots in all envs; secrets not hardcoded
  - Dependencies: 0.2
  - Owner: TBC

---

## 1. Data Layer

- [ ] 1.1 Database selection & ORM
  - Subtasks:
    - [ ] Choose Postgres (RDS) and Prisma
    - [ ] Configure Prisma schema and generator
    - [ ] Add migration workflow (`prisma migrate`), seeding
  - Deliverables: `prisma/schema.prisma`, migrations, seed script
  - Acceptance: `pnpm prisma migrate dev` applies cleanly; seed runs
  - Dependencies: 0.1, 0.2

- [ ] 1.2 Core tables (Users, Teams, Coaches, Players)
  - Subtasks:
    - [ ] Model entities and relations (team membership, roles)
    - [ ] Indexes (by teamId, email, updatedAt)
  - Deliverables: Prisma models + migrations
  - Acceptance: CRUD scripts validate constraints; FK integrity enforced
  - Dependencies: 1.1

- [ ] 1.3 Football data tables (Plays, Signals, Formations, Concepts, Routes)
  - Subtasks:
    - [ ] Define models and relations (Play→Concepts, Signal→Combinations)
    - [ ] Add JSON fields for structured metadata
  - Deliverables: Prisma models + migrations
  - Acceptance: Integrity of relations; sample seed loads
  - Dependencies: 1.1

- [ ] 1.4 Learning data tables (Questions, Scenarios, Progress, Sessions, Analytics)
  - Subtasks:
    - [ ] Model learning artifacts and session telemetry
    - [ ] Partition/indices for analytics queries
  - Deliverables: Prisma models + migrations
  - Acceptance: Aggregation queries performant (<200ms on seed)
  - Dependencies: 1.1

- [ ] 1.5 Philosophy data (Philosophies, Terminologies, Systems)
  - Subtasks:
    - [ ] Create team-level customization models
  - Deliverables: Prisma models + migrations
  - Acceptance: Team-specific overrides queryable
  - Dependencies: 1.1

- [ ] 1.6 Seed & indices
  - Subtasks:
    - [ ] Minimal seed (1 team, 1 coach, 3 players, 2 plays, 2 signals)
    - [ ] Add essential indices and constraints validation
  - Deliverables: `prisma/seed.ts`
  - Acceptance: `pnpm seed` idempotent; indices verified
  - Dependencies: 1.2–1.5

---

## 2. Auth Layer

- [ ] 2.1 Auth0 integration & JWT validation
  - Subtasks:
    - [ ] JWKS client, caching, audience/issuer checks
    - [ ] Global auth guard
  - Deliverables: Auth module with `JwtAuthGuard`
  - Acceptance: Protected routes reject without valid JWT
  - Dependencies: 0.3

- [ ] 2.2 Role guard & mapping
  - Subtasks:
    - [ ] Define roles (coach, player, admin)
    - [ ] `RoleGuard` decorator + policy checks
  - Deliverables: Role-based access examples
  - Acceptance: RBAC enforced on sample endpoints
  - Dependencies: 2.1

- [ ] 2.3 Session cache (optional)
  - Subtasks:
    - [ ] Redis-backed session claims cache
  - Deliverables: SessionCache service
  - Acceptance: Hit ratio tracked; fallbacks safe
  - Dependencies: 6.x (Redis)

---

## 3. API Gateway & Edge

- [ ] 3.1 Edge concerns
  - Subtasks:
    - [ ] Rate limiting middleware (IP/user scope; stricter on AI)
    - [ ] CORS policy per env
    - [ ] Request ID, logging, versioning `/v1`
  - Deliverables: Global middleware
  - Acceptance: RL blocks as expected; CORS passes whitelisted origins
  - Dependencies: 0.2

- [ ] 3.2 Error & response envelope
  - Subtasks:
    - [ ] Consistent error DTO; exception filter mapping
    - [ ] Pagination, sorting, filtering contracts
  - Deliverables: Response spec, DTOs
  - Acceptance: OpenAPI shows uniform types
  - Dependencies: 0.2, 8.1

---

## 4. API Controllers (skeleton → MVP)

- [ ] 4.1 AuthAPI
  - Subtasks:
    - [ ] `/me` profile (mock from JWT)
  - Deliverables: Auth routes
  - Acceptance: Returns decoded profile
  - Dependencies: 2.1

- [ ] 4.2 TeamsAPI
  - Subtasks:
    - [ ] Teams CRUD, members list
    - [ ] Invites (create token, accept)
  - Deliverables: Teams routes + DTOs
  - Acceptance: E2E tests for membership lifecycle
  - Dependencies: 1.2, 2.2

- [ ] 4.3 PlaysAPI
  - Subtasks:
    - [ ] CRUD plays
    - [ ] Attach diagrams/videos by file key
  - Deliverables: Plays routes
  - Acceptance: CRUD green; file refs validated
  - Dependencies: 1.3, 7.1

- [ ] 4.4 SignalsAPI
  - Subtasks:
    - [ ] CRUD signals
    - [ ] Manage combinations metadata
  - Deliverables: Signals routes
  - Acceptance: CRUD + combinations update
  - Dependencies: 1.3

- [ ] 4.5 FilesAPI
  - Subtasks:
    - [ ] Presigned upload URL, finalize callback
    - [ ] Type/size validation, virus scan stub
  - Deliverables: Files routes
  - Acceptance: Client uploads succeed; keys persisted
  - Dependencies: 7.1

- [ ] 4.6 LearningAPI
  - Subtasks:
    - [ ] Fetch questions/scenarios (by team/player)
    - [ ] Record progress, sessions
  - Deliverables: Learning routes
  - Acceptance: Progress writes stored; queries filtered by team
  - Dependencies: 1.4

- [ ] 4.7 AnalyticsAPI
  - Subtasks:
    - [ ] Team/player aggregates, breakdowns
    - [ ] Time-bucketed series endpoints
  - Deliverables: Analytics routes
  - Acceptance: Dashboard queries <250ms on seed
  - Dependencies: 1.4, 6.2

- [ ] 4.8 AIAPI
  - Subtasks:
    - [ ] Generate questions/scenarios (OpenAI)
    - [ ] Content customization hooks
  - Deliverables: AI routes
  - Acceptance: Rate-limited; deterministic tests via fixtures
  - Dependencies: 5.1, 9.1

---

## 5. Core Services (Business Logic)

- [ ] 5.1 OpenAI client wrapper
  - Subtasks:
    - [ ] Retry, backoff, timeouts, usage logging
    - [ ] Content/size guardrails
  - Deliverables: `OpenAiService`
  - Acceptance: All AI calls go through wrapper; metrics emitted
  - Dependencies: 0.3

- [ ] 5.2 Playbook services
  - Subtasks:
    - [ ] `PlayParser`: diagram/meta extraction (stub)
    - [ ] `PlayMapper`: link to formations/concepts
    - [ ] `ConceptExtractor`, `RouteAnalyzer` stubs
  - Deliverables: Service classes + unit tests
  - Acceptance: Given sample play, returns structured model
  - Dependencies: 1.3

- [ ] 5.3 Signal services
  - Subtasks:
    - [ ] `SignalProcessor`: validate, normalize components
    - [ ] `VideoHandler`: media pipeline hooks
    - [ ] `CombinationBuilder`: build sequence variants
    - [ ] `RecognitionTester`: quiz item generator
  - Deliverables: Service classes + tests
  - Acceptance: Build/test sequences persist and retrieve
  - Dependencies: 1.3, 7.x

- [ ] 5.4 Learning services
  - Subtasks:
    - [ ] `ProgressTracker`: write/read progress
    - [ ] `TeamAnalytics`: aggregate by period
    - [ ] `AdaptiveLearning`, `SpacedRepetition` stubs
  - Deliverables: Service classes + tests
  - Acceptance: Returns expected aggregates on seed
  - Dependencies: 1.4

- [ ] 5.5 Philosophy services
  - Subtasks:
    - [ ] `SystemLearner`, `TerminologyMapper`
    - [ ] `ContentCustomizer`, `PhilosophyTrainer`
  - Deliverables: Service classes + tests (stubs acceptable)
  - Acceptance: Team terminology overrides propagate to outputs
  - Dependencies: 1.5

---

## 6. Caching & Performance

- [ ] 6.1 Redis setup
  - Subtasks:
    - [ ] Client module, health checks
  - Deliverables: Redis module + config
  - Acceptance: App connects; ping health
  - Dependencies: 0.3, 10.x

- [ ] 6.2 Query & session cache
  - Subtasks:
    - [ ] `QueryCache` wrapper with TTL & stampede protection
    - [ ] `SessionCache` for auth claims (optional)
  - Deliverables: Cache services + decorators
  - Acceptance: Cached endpoints show improved latency; hit ratio logged
  - Dependencies: 6.1

---

## 7. Files, Storage, CDN

- [ ] 7.1 S3 buckets & presigned uploads
  - Subtasks:
    - [ ] Buckets: `video`, `diagrams`, `generated`, `temp`
    - [ ] `FilesAPI`: presigned URL, metadata validation
  - Deliverables: AWS config, file routes
  - Acceptance: Uploads via presigned URL; keys stored in DB
  - Dependencies: 10.x (IaC), 4.5

- [ ] 7.2 CDN config
  - Subtasks:
    - [ ] CloudFront/Cloudflare distribution; caching rules
    - [ ] Optional signed URLs for private media
  - Deliverables: CDN setup docs/infra
  - Acceptance: Media cached from CDN; cache headers correct
  - Dependencies: 7.1

- [ ] 7.3 File processing pipeline
  - Subtasks:
    - [ ] `FileProcessor` → `MediaHandler` → `TemplateEngine` stubs
    - [ ] Transcode hooks (future), virus scan stub
  - Deliverables: Worker pipeline skeleton
  - Acceptance: Job processes sample file and updates status
  - Dependencies: 8.2, 9.2

---

## 8. Background Jobs

- [ ] 8.1 Job queue foundation
  - Subtasks:
    - [ ] BullMQ + Redis; base queue module
    - [ ] DLQ strategy, idempotency keys
  - Deliverables: `jobs/` workspace with starter workers
  - Acceptance: Enqueue/dequeue works; retries/backoff tested
  - Dependencies: 6.1

- [ ] 8.2 Processors
  - Subtasks:
    - [ ] `AIProcessor`: generate content (OpenAI)
    - [ ] `VideoProcessor`: media ops (transcode stub)
    - [ ] `AnalyticsProcessor`: daily aggregates
    - [ ] `NotificationProcessor`: push/email (stub)
  - Deliverables: Worker handlers + E2E job tests
  - Acceptance: Each processor handles a sample job to completion
  - Dependencies: 8.1, 5.1, 7.3

---

## 9. Real-time Services

- [ ] 9.1 WebSocket server
  - Subtasks:
    - [ ] Namespaces: `LiveProgress`, `TeamCollaboration`, `SignalPractice`, `Notifications`
    - [ ] JWT handshake, room model (teamId, userId)
  - Deliverables: WS gateway module
  - Acceptance: Clients join rooms; broadcast works; auth enforced
  - Dependencies: 2.1

- [ ] 9.2 Throttling & presence
  - Subtasks:
    - [ ] Rate limit emits; presence tracking in Redis
  - Deliverables: Middlewares + presence service
  - Acceptance: Excess emits dropped; presence list accurate
  - Dependencies: 6.1, 9.1

---

## 10. Monitoring, Security, DevOps

- [ ] 10.1 Observability
  - Subtasks:
    - [ ] Structured JSON logs with correlation IDs
    - [ ] OpenTelemetry metrics/traces; Prometheus export
    - [ ] Error tracking (Sentry)
  - Deliverables: Observability module
  - Acceptance: Logs, metrics, traces visible; errors captured
  - Dependencies: 0.2, 0.3

- [ ] 10.2 Security hardening
  - Subtasks:
    - [ ] DTO validation; sanitization
    - [ ] RBAC checks everywhere
    - [ ] Upload scanning hooks; MIME sniffing; size caps
    - [ ] WAF rules, IP allowlists as needed
  - Deliverables: Security checklist + automated checks
  - Acceptance: OWASP checks pass; CI security gate green
  - Dependencies: 3.x, 4.x

- [ ] 10.3 CI/CD & Infra
  - Subtasks:
    - [ ] GitHub Actions: lint, typecheck, test, build, docker
    - [ ] Dockerfiles for `backend/` and `jobs/`
    - [ ] Terraform in `infra/`: VPC, RDS, Redis, S3, CDN, ECS/EKS or Fargate, Secrets
    - [ ] Blue/green or rolling deploys
  - Deliverables: Pipelines + Terraform stacks
  - Acceptance: One-click deploy to dev/staging; rollbacks tested
  - Dependencies: 0.1, 0.2, 6.1, 7.1

---

## 11. API Contracts & SDK

- [ ] 11.1 OpenAPI documentation
  - Subtasks:
    - [ ] Auto-generate spec from decorators
    - [ ] Document auth, pagination, errors, rate limits
  - Deliverables: `/docs` with Swagger UI
  - Acceptance: Spec validated; examples present
  - Dependencies: 4.x

- [ ] 11.2 TypeScript SDK (`packages/sdk`)
  - Subtasks:
    - [ ] Generate typed client from OpenAPI
    - [ ] Provide auth helpers and retries
  - Deliverables: Published `sdk` package (local)
  - Acceptance: Next.js app consumes SDK without manual types
  - Dependencies: 11.1

---

## 12. Frontend Integration Touchpoints

- [ ] 12.1 Env wiring in Next.js
  - Subtasks:
    - [ ] Add API base URL, Auth0 domains, CDN URLs
  - Deliverables: Updated `.env.local` example and config
  - Acceptance: Local app can hit protected endpoints
  - Dependencies: 2.1, 4.x

- [ ] 12.2 WebSocket client hooks
  - Subtasks:
    - [ ] Connect to namespaces; auth handshake
    - [ ] Demo live updates in coach dashboards
  - Deliverables: Minimal client + sample components
  - Acceptance: Live updates render in UI
  - Dependencies: 9.1

---

## 13. Data Migration & Seeding

- [ ] 13.1 Seed datasets
  - Subtasks:
    - [ ] Demo teams/plays/signals/questions/scenarios
  - Deliverables: Seed scripts with anonymized data
  - Acceptance: Dashboards show meaningful data
  - Dependencies: 1.x

- [ ] 13.2 Backfill scripts (future)
  - Subtasks:
    - [ ] CLI for imports and anonymization
  - Deliverables: `backend/scripts/`
  - Acceptance: CLI runs locally; docs included
  - Dependencies: 1.x

---

## 14. Milestones & Phasing

- [ ] M1 (MVP Platform)
  - Scope: Auth + Teams + Files (upload) + Plays/Signals CRUD
  - Depends on: 0,1,2,3,4.3–4.5,7.1,10.3 (dev infra)

- [ ] M2 (Learning & Analytics)
  - Scope: LearningAPI + ProgressTracker + basic AnalyticsAPI
  - Depends on: 4.6,5.4,4.7,6.2

- [ ] M3 (AI + Jobs)
  - Scope: AIAPI + OpenAI wrapper + AIProcessor + caching
  - Depends on: 5.1,4.8,8.1–8.2,6.2

- [ ] M4 (Realtime)
  - Scope: WebSocket services + Notifications + UI hooks
  - Depends on: 9.1–9.2,12.2

- [ ] M5 (Hardening & Scale)
  - Scope: Observability, WAF, perf, CDN tuning, load tests
  - Depends on: 10.1–10.2,7.2

---

## 15. Launch Checklist

- [ ] Load/perf baseline and capacity plan
- [ ] OpenAI usage quotas & monitoring
- [ ] S3 lifecycle rules (temp retention); backups & restore drills
- [ ] WAF rules + rate-limit policies per tier
- [ ] Runbooks, on-call rotation, alerting
- [ ] Error budgets & SLOs; API limits documented

---

## Appendix

- Owners: Assign per team/engineer
- Estimates: Add story points or t-shirt sizes
- Risks: OpenAI limits, media costs, realtime scale, PII handling
- Non-functional goals: p95 < 250ms for core reads on seed; 99.9% uptime targets for coach dashboards 