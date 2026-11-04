# Implementation Plan

## Project

Internal Application Hosting & Deployment Platform (project code: Internal Hosting Platform / IHP). The complete, canonical requirements are in [PROJECT_REQUIREMENTS.md](PROJECT_REQUIREMENTS.md).

**Current assignment (2026-11-04): TASK 042 - M10-DB DB-W0 decision finalization and readiness.** Documentation-only review of W0-01 through W0-14 and operational readiness. W0-12 remains the approved product policy; the TASK 023 first-use and explicit safeguarded replacement requirements are preserved; W0-10's import mode/provenance/location and all other unresolved decisions remain pending. The grouped owner questions remain in PROJECT_REQUIREMENTS.md section 9.1.19; current status and readiness blockers are summarized in section 9.1.22. No code/build/test/database operation is authorized or performed. M10-DB and M7 remain IN PROGRESS; Source Build remains disabled; D00-D29 remain PENDING. See PROGRESS.md for evidence and the exact next task.

**Historical assignment (2026-11-04): TASK 041 - M10-DB Administrator review/rejection of database-mode requests.** Implemented an Administrator-only app-scoped review/rejection workflow for inert mode requests: `REQUESTED` -> `PENDING_REVIEW` -> `REJECTED`, preserving existing cancellation behavior. Focused tests passed 9/9; `npm.cmd run verify` passed 611 checks, 0 failed, 0 skipped, with DLL-only .NET execution and owned loopback smoke cleanup. M10-DB remains IN PROGRESS; M7 remains IN PROGRESS and Source Build disabled; DB-W0 and D00-D29 remain pending. No active database mode, physical path, application database file operation or runtime association exists. See PROGRESS.md and PROJECT_REQUIREMENTS.md section 9.1.21. Do not begin provisioning, attachment, migration, backup/restore, replacement/deletion, runtime integration, or another milestone.

**Historical assignment (2026-11-04): TASK 040 - M10-DB database mode and association-intent metadata.** Implemented inert app-scoped requests for NONE, MANAGED_SQLITE and EXISTING_SQLITE with additive Registry migration, M11 authorization, candidate metadata ownership checks, audit/event linkage, cancellation and hashed idempotency. Focused tests passed 6/6; `npm.cmd run verify` passed 608 checks, 0 failed, 0 skipped, plus owned loopback smoke cleanup. This records request intent only: no active database mode, physical path, file operation or runtime association exists. M10-DB and M7 remain IN PROGRESS; Source Build stays disabled; DB-W0 and D00-D29 remain pending. See PROGRESS.md and PROJECT_REQUIREMENTS.md section 9.1.20. Do not begin provisioning, attachment, migration, backup/restore, replacement/deletion, runtime integration, or another milestone.

**Historical assignment (2026-11-03): TASK 039 - M10-DB product decision package and readiness.** Grouped DB-W0 options, alternatives, impact and exact product-owner confirmations are recorded in PROJECT_REQUIREMENTS.md section 9.1.19. TASK 023's safe first-use, KEEP-on-upgrade and explicit replacement-authorization requirements are preserved; only W0-12 is fully resolved, while import mode/provenance remains pending. The supplementary health/routing decision W0-14 is not approved. DB-W0 is ready for owner review but NOT READY for path-dependent lifecycle/runtime implementation. No application code/tests or database operations occurred. M7/M10-DB remain IN PROGRESS; Source Build stays disabled; D00-D29 remain PENDING.

**Historical assignment (2026-11-03): TASK 038 - M10-DB path compatibility and DB-W0 decision record.** Documentation-only compatibility analysis. The path-pattern matrix, three database-mode contracts, My Work Center source findings and 13-item DB-W0 decision matrix were recorded in PROJECT_REQUIREMENTS.md section 9.1.18. First-use and KEEP policy were preserved; import mode/provenance/location remained pending. No code, app, runtime or database changed; no application tests were run. M7/M10-DB remained IN PROGRESS, Source Build disabled, and D00-D29 PENDING.

**Historical assignment (2026-11-03): TASK 037 - M10-DB prospective per-application storage resolver.** Implemented an explicit-area, prospective path resolver with traversal, reparse/junction, alias, Registry-overlap, profile/repository and cross-application boundary checks. It does not create or open files, attach/provision/modify/migrate/replace/delete databases, or configure runtime behavior. DB-W0 compatibility decisions remain unresolved; the resolver requires explicit `workspace` or `data` selection and chooses no default. Focused resolver tests passed 10/10; API tests passed 159/159; `npm.cmd run verify` passed 602 checks with zero failures/skips and owned smoke cleanup. M10-DB remains IN PROGRESS; M7 remains IN PROGRESS with Source Build disabled; D00-D29 remain PENDING. See PROGRESS.md for evidence. Do not begin lifecycle/runtime integration or another milestone.

**Historical assignment (2026-11-03): TASK 036 - M10-DB Phase 1 candidate metadata inventory.** The existing authenticated Folder/ZIP intake now persists review-only SQLite candidate and WAL/SHM metadata in an additive SQLite Registry migration and exposes three M11 `application.read`-protected app-scoped read endpoints. No database bytes enter Registry; no candidate is attached, modified or executed; no runtime configuration or lifecycle operations are added. Final verification passed 592/592 with zero failures/skips. M10-DB is IN PROGRESS for this metadata slice only. M7 remains IN PROGRESS with Source Build disabled; D00-D29 remain PENDING. See PROGRESS.md for exact evidence. Do not begin runtime attachment, provisioning, backup/restore or another milestone.

**Historical assignment (2026-11-03): TASK 035 — IT/Security approval evidence review and worker readiness.** Workspace/task evidence review found no traceable IT/Security approval, worker provisioning or isolation results. Decision status is 0 APPROVED / 30 PENDING; readiness is NOT READY. Product-owner-reported policy clarifications remain context only. M7 remains IN PROGRESS and Source Build disabled. No reviewer contact, provisioning, source execution, application build/test or security change occurred. Do not start worker integration until written approvals and actual technical gates pass.

**Historical assignment (2026-11-02): TASK 034 — IT/Security approval evidence and Build Worker readiness.** Prepared a per-decision readiness matrix, 16-item checklist and objective implementation gate, and mapped TASK 033 seams by static review. No approval or worker readiness was claimed. All D00–D29 remain PENDING; M7 remains IN PROGRESS and Source Build disabled.

**Historical assignment (2026-11-02): TASK 033 — Non-executing Build Job metadata API.** The authorized Category A subset is implemented and verified. `npm.cmd run verify` passed with 587 checks, 0 failures and 0 skips, including the SQLite/M11 Build Job tests; DLL publish checks and owned loopback smoke passed. Valid API requests stop at `BLOCKED_WORKER_UNAPPROVED`; no worker is provisioned/connected, no uploaded source executes and no artifact is created. This metadata-only work did not change M7 or the D00–D29 approval gate.

**Historical assignment (2026-11-02): TASK 032 — M8 real-browser acceptance.** The existing PROGRESS.md matrix has 53 scenarios; the approved operator procedure remains available. No approved browser-control capability was exposed, no fixture was started and no browser scenario ran: 0 PASS, 0 FAIL, 0 scenario BLOCKED, 53 NOT RUN. M8 stays IN PROGRESS/default-disabled. This preceding task does not change current M7/source-build approval status.

**Latest assignment (2026-11-02): TASK 031 — IT/Security approval review and decision tracking.** Consolidate the TASK 030 package into a human-readable internal review summary and evidence-based decision register covering all controls, threats, checklist items, communication design, worker proposal and templates. The product owner handles reviewer communication; do not send requests, assume approval, provision/connect infrastructure, execute uploaded source/builds or change application behavior. Preserve M7 IN PROGRESS and every other milestone status. Source builds remain disabled. Latest application verification remains TASK 027: 40 package checks and 577 full checks passed; TASK 031 is documentation-only and does not rerun application suites.

**Historical assignment (2026-11-02): TASK 030 — prepare IT/Security approval package.** Documented the proposed isolated Windows builder architecture, trust boundaries/data flow, control matrix, network design, Node/.NET templates, threat model and approval checklist. No reviewer contact or approval occurred; all decisions remained pending. No application tests were run.

**Historical assignment (2026-11-02): TASK 029 — mandatory source-build architecture.** Product owner confirmed source builds remain mandatory. The proposed isolated builder is not approved or provisioned; M7 remains IN PROGRESS and build execution disabled. TASK 030 prepares a review package without changing that gate.

**Historical assignment (2026-10-30): TASK 028 — assess M7 source-build isolation and deployment scope.** Read-only review of M7 package intake, validation, approval, prebuilt runtime, Agent lifecycle, M6 routing, and SQLite candidate handling. TASK 029 superseded its unresolved product-owner decision: source builds are mandatory. No other TASK 028 evidence or security restrictions are waived.

**Previous assignment (2026-10-30): TASK 027 — fix ZIP wrapper-folder validation and restore full regression.** Validate forbidden segments in every original normalized ZIP path before wrapper removal, including explicit directory entries, then retain post-normalization path/type validation. Preserve safe wrappers and all existing package security controls. Focused package verification passed 40/40; full `npm.cmd run verify` passed 577 checks and owned smoke cleanup. Keep all milestone statuses unchanged: M1–M6/M11 COMPLETED, M7/M8 IN PROGRESS, M9/M10/M10-DB/M12 NOT STARTED. M7 source-build isolation remains unavailable; do not enable source builds.

**Previous assignment (2026-10-30): TASK 026 — diagnose M6 Dynamic Routing integration failures.** Inspect process/port ownership, reproduce route-withdrawal and API-readiness failures independently, add actionable diagnostics and lifecycle regressions, and run `npm.cmd run test:routing` followed by `npm.cmd run verify`. Preserve route security and all unrelated functionality. Keep M1–M6/M11 COMPLETED, M7/M8 IN PROGRESS, and M9/M10/M10-DB/M12 NOT STARTED.

**Previous assignment (2026-10-30): TASK 025 — detailed upload validation errors.** Implement bounded, sanitized rejected-file details for authenticated Folder and ZIP uploads, render actionable details in the Dashboard, preserve M11 authorization and all existing limits/security checks, and run focused plus full verification. Do not alter Gateway application routing, source-build policy, deployment behavior, or unrelated milestones. M7/M8 remain IN PROGRESS; M10-DB remains NOT STARTED.

**Previous assignment (2026-10-29): TASK 024 — secure unified folder/ZIP intake with SQLite artifact separation.** Extend only authenticated upload/intake, static candidate classification, private staging, bounded read-only SQLite validation and Dashboard review results. Keep package approval unchanged and code packages free of database bytes. This does not authorize application execution/deployment, DB replacement/restore/provisioning/migrations, runtime path injection, Gateway changes, or M10-DB lifecycle work. M7 remains IN PROGRESS; M10-DB remains NOT STARTED. Preserve existing budgets and run the full approved verification suite.

**Latest assignment (2026-10-29): TASK 023 — My Work Center SQLite compatibility investigation, documentation only.** This supersedes the TASK 022 next-task note for this task. Inspect the available application folder and IHP package/runtime boundaries without executing the app or touching its database; document findings and exact future requirements in the four tracking records. M7/M8 remain IN PROGRESS and M10-DB remains NOT STARTED. No source, app, schema, migration, package-validator, runtime or filesystem behavior is changed.

**Latest assignment (2026-10-29): TASK 022 — Persistent application workspace and SQLite architecture design, documentation only.** This supersedes active implementation assignments for this task. Inspect and document actual package/folder/runtime behavior, define a safe persistent workspace and database-artifact workflow, update only the four tracking documents, and run documentation consistency/format/diff checks. Do not implement, build, test, migrate, execute applications or change milestone states. M7/M8 remain IN PROGRESS; M10-DB remains NOT STARTED. Stop after documentation and wait for explicit authorization before implementation.

This roadmap translates those requirements into independently verifiable milestones. It does not claim that planned functionality already exists.

**Current assigned task (2026-10-29): TASK 021 — M7 folder-upload Dashboard flow.** The default-off React candidate now registers/selects an owned application, selects and previews a folder, enforces the client payload cap, invokes the existing owner-scoped intake/package endpoints, and displays backend-confirmed outcomes. Automated full verification passed; actual-browser acceptance was not run. M7 and M8 remain IN PROGRESS; M10-DB remains NOT STARTED. Source builds and deployment automation remain disabled. Older dated assignment notes are historical when superseded; preserve their evidence and statuses.

**Current M8 continuation (2026-10-28):** The latest user resumes M8 implementation and testing, superseding the earlier documentation-only directive below. Diagnose the recorded API :5266 readiness failure, preserve M11 guards and source-build restrictions, verify the restricted browser boundary and authorized UI operations, and run the complete regression. M8 remains IN PROGRESS until every required criterion including permitted actual-browser evidence passes. M7 remains IN PROGRESS; M10-DB remains NOT STARTED. No M9/M10/M10-DB/M12 work is authorized.

**Current task override (2026-10-27): DOCUMENTATION ONLY.** Define Managed Application Databases in the four official records; do not implement, modify, build or test application code, change schemas/migrations or execute applications. The earlier M8 assignment below is retained as history and is not the active task. M1–M6/M11 remain COMPLETED; M7/M8 remain IN PROGRESS; M9/M10/M12 remain NOT STARTED. Add M10-DB as a future NOT STARTED sub-milestone after M10 without renumbering or changing any existing milestone's status. No implementation of M10-DB or another milestone is authorized by this update.

**Current assignment (2026-10-27):** M8 only, explicitly assigned after the verified M7 prebuilt subset despite unfinished source builds. M7 remains IN PROGRESS; no acceptance waiver/build authorization is implied. Older waits are superseded. M8 is IN PROGRESS with browser authentication unavailable under unchanged M11 controls. Do not start M9 or another milestone.

## Status rules

- `NOT STARTED`: work has not begun.
- `IN PROGRESS`: work is underway and acceptance criteria remain.
- `COMPLETED`: every acceptance criterion for the milestone has been verified, with actual evidence recorded in `PROGRESS.md`.
- `BLOCKED`: work cannot proceed because a documented external decision, permission, or dependency is unavailable. Record the reason and the unblock condition in `PROGRESS.md`.

Do not mark a milestone complete based on code being written alone. Verification must match the acceptance criteria and run in the approved local environment. A test that was not run is not a pass.

## Repository layout

M1 created the four component shells and shared foundations shown below. M2 added Gateway verification; M3 added both trusted samples and managed sample verification. Later milestone projects remain planned. Keep verification commands aligned with actual paths.

```text
src/Gateway/                 ASP.NET Core 8 + YARP
src/Api/                     NestJS + Prisma + SQLite
src/DeploymentAgent/         .NET 8 Worker
src/Dashboard/               React + TypeScript + Vite
src/Foundation/              shared .NET host configuration
src/CredentialProtection/    published current-user DPAPI helper (M4)
src/PackageInspector/        published metadata-only ZIP helper (M7)
packages/config/             shared Node configuration
packages/contracts/          TypeScript contracts
tools/                       local development lifecycle and checks
tests/Foundation.Tests/      published managed Foundation verification program
tests/foundation/            Node foundation tests
samples/DemoNode/            trusted dependency-free Node.js sample
samples/DemoDotnet/          trusted ASP.NET Core 8 sample
tests/Samples.Tests/         published managed sample/process verification
tests/Gateway.Tests/         managed YARP HTTP/WebSocket verification
tests/DeploymentAgent.Tests/ published M5 lifecycle/recovery verification
tests/Platform.Integration.Tests/ published M6/M11 integration suite
tests/Packages.Tests/        published M7 archive/HTTP/trusted-runtime verification
```

M1 commands below refer to existing artifacts. Commands for later milestones remain planned and require their future projects/tests. Use `npm.cmd` from PowerShell.

## Cross-milestone constraints and gates

- The Gateway is fixed to `http://127.0.0.1:5050` for the current authorized scope; alternate ports/hosts are rejected at startup. Do not enable the corporate-network listener or deploy to the corporate network without local verification and explicit authorization.
- Port 5050 is the only intended network-facing entry point. Application processes must bind to loopback, using an internal port in the approved `6100–6999` range. Do not expose runtime ports to the corporate network.
- Do not use Docker or require Administrator privileges for normal operation. Do not alter firewall rules, execution policy, endpoint protection, registry settings, or other IT controls.
- Do not run .exe artifacts. All .NET applications and verification programs must use framework-dependent dotnet publish with UseAppHost=false, followed by dotnet <published DLL>. Do not use VSTest/testhost, native apphosts, or dotnet run. Publish outputs must be checked for forbidden executable artifacts.
- Treat uploads as untrusted. Do not execute arbitrary uploaded code or manifest commands without the approved deployment policy and security boundary. Where available permissions cannot provide adequate isolation, restrict deployment to reviewed, trusted applications and record the limitation.
- Do not modify existing applications. Use only platform-owned storage and sample applications.
- Keep secrets out of browser responses and logs. Validate routes and destinations; proxy only to approved loopback destinations.
- Use bounded restart attempts and backoff. Do not assume unattended startup after logout or reboot without IT approval.
- Authentication and deployment authorization must fail closed. Before their milestone is complete, any development-only management access must remain local and must not enable untrusted or network-accessible deployment.

Managed Application Database requirements are defined in PROJECT_REQUIREMENTS.md section 9.1. The architecture is one dedicated Platform Registry plus optional independent application SQLite databases. M10-DB is IN PROGRESS for TASK 036 candidate metadata, TASK 037 prospective path validation, TASK 040 inert database-mode request metadata and TASK 041 Administrator review/rejection metadata; no active mode, runtime integration or database lifecycle is implemented. The TASK 037 resolver does not change current physical paths or resolve any application path pattern. Registry/application separation, release-independent data, explicit authorization, SQLite-only operations and the same-Windows-identity isolation limitation apply throughout this work.

## Milestones

### M1 — Project Foundation

**Objective:** Establish a maintainable local development foundation and confirm architecture boundaries before application feature work.

**Tasks:**

- Inspect the repository and installed Node.js, npm, and .NET 8 tooling; confirm the proposed layout or document a replacement.
- Record component boundaries and data/control flows for Gateway, NestJS API, .NET Deployment Agent, React dashboard, SQLite registry, and application processes.
- Create the monorepo structure and shared development configuration without requiring administrator access or Docker.
- Define local-only startup and shutdown instructions, configurable storage paths, ports, and environment-specific settings; do not hardcode a Windows username.
- Establish formatting, lint/publish/verification entry points and a safe local verification procedure. Use `npm.cmd` in PowerShell. Publish a managed Foundation assertion program that exercises production code, exits nonzero on failures, and needs no VSTest executable.
- Record security assumptions, deployment approval gates, and the limits of process isolation on Windows without Docker.

**Dependencies:** Documentation review and user approval to begin implementation; local toolchain availability. Later application milestones depend on the foundation created here.

**Acceptance criteria:**

- The agreed source layout and component boundaries are documented and present in the repository.
- Developers can restore/compile and publish the component shells locally without Docker, Administrator privileges, system security changes, or corporate-network access. .NET applications and checks are run from published DLLs; no native executable artifacts are launched.
- Configuration and storage locations are user-configurable; runtime and gateway defaults are loopback-only.
- Local start/stop and verification commands are documented and work on the target Windows environment.
- The repo's baseline checks and their actual results are recorded in `PROGRESS.md`.

**Verification commands (implemented):**

```powershell
npm.cmd ci
npm.cmd run doctor
npm.cmd run build
npm.cmd run check
npm.cmd run test:node
npm.cmd run test:dotnet
npm.cmd run smoke
npm.cmd run verify
```

**Status:** COMPLETED

**Policy reassessment:** The original behavior criteria are unchanged. The 2026-10-26 instruction permits published managed verification DLLs, so VSTest is not a required acceptance dependency. A dedicated verifier now exercises the production Foundation library without loading/rehosting the blocked runner.

| M1 acceptance criterion                                            | Verified evidence                                                                                                                       |
| ------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| Source layout and component boundaries exist                       | Four source shells, shared Foundation/config/contracts, local tooling, and architecture documentation inspected                         |
| Restore/compile/publish without Docker or Administrator privileges | Full npm verification exited 0; three framework-dependent publishes succeeded; current token Administrator check was False              |
| Configurable storage/ports and loopback defaults                   | 27 managed production settings checks, 17 Node foundation checks, and three live managed listener assertions passed                     |
| Local start/stop and verification commands work                    | Published Gateway/Agent DLLs and Node components passed real health/page/module/proxy checks; all four exited 0 and ports were released |
| Baseline results recorded accurately                               | Full command/results, negative control, changes, prior blocked evidence, and resume instructions are recorded in PROGRESS.md            |

**Full verification:** `npm.cmd run verify` passed: API/dashboard compilation, three DLL publishes, publish scans reporting no .exe artifacts, formatting/type checks, **19 Node/API checks + 27 managed Foundation checks**, and local smoke. A negative control on an absent listener returned the expected assertion failure and exit code 1. No legacy VSTest test is reported as executed. Historical error 1260 remains part of the record; no security settings were changed.

**Historical scope at M1 completion:** Only M1 was complete at that point; the user's conditional fallback to M2 was not needed. Current milestone states are recorded below.

### M2 — YARP Gateway

**Objective:** Provide the single local HTTP entry point and a safe proxy foundation.

**Tasks:**

- Implement ASP.NET Core 8 with YARP 2.3.0 and a fixed 127.0.0.1:5050 listener.
- Add GET/HEAD health handling, explicit body/header/line/time/connection budgets, controlled problem responses, and timestamped structured request/error logs.
- Preserve application path prefixes and queries, set trusted forwarded headers, and support HTTP/1.1 WebSocket forwarding.
- Validate route IDs, reserved/conflicting paths, strict startup JSON, and literal approved loopback destinations.
- Keep application route data out of Program.cs using IApprovedGatewayRouteSource and a validated immutable IProxyConfigProvider snapshot; live updates remain M6.
- Publish a dedicated managed Gateway verification program with UseAppHost=false and run only its DLL.

**Dependencies:** M1 — satisfied.

**Acceptance criteria and evidence:**

| Criterion                                                                           | Verified result                                                                                                                                                                                 |
| ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Gateway publishes and starts on loopback port 5050 without Administrator privileges | PASS: real GatewayHost and published-process smoke; alternate host/port rejected before binding; current token Administrator=False                                                              |
| Local health endpoint succeeds                                                      | PASS: GET JSON, HEAD empty body, safe 405 for unsupported methods                                                                                                                               |
| No corporate-facing listener                                                        | PASS: managed TCP inspection confirms only 127.0.0.1 on Gateway port 5050                                                                                                                       |
| Reserved paths, invalid destinations, proxy failures, prefixes, WebSockets covered  | PASS: managed configuration/HTTP/WebSocket checks, including chunked limits, controlled 502/504, interrupted response, forwarded headers, and log omission of supplied credential/query markers |
| Application fixture ports bind only to loopback                                     | PASS: owned runtime-range fixture sockets inspected and stopped; no sample deployments or existing applications were modified                                                                   |

**Verification commands (implemented):**

```powershell
npm.cmd run build
npm.cmd run test:gateway
npm.cmd run verify
npm.cmd run doctor
```

The dedicated command publishes tests/Gateway.Tests with UseAppHost=false and executes artifacts/publish/Gateway.Tests/Gateway.Tests.dll through dotnet. Manual equivalent:

```powershell
dotnet publish .\tests\Gateway.Tests\Gateway.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false -p:UseSharedCompilation=false -p:PublishDir=../../artifacts/publish/Gateway.Tests/
dotnet .\artifacts\publish\Gateway.Tests\Gateway.Tests.dll
```

**Status:** COMPLETED

**Final results:** npm.cmd run verify exited 0: **79 Gateway + 27 Foundation + 21 Node/API checks = 127 passing checks**, plus full platform smoke. All four .NET publish roots were scanned for .exe artifacts. The smoke verified actual production JSON log output, real HTTP/React/API responses, loopback sockets, graceful exit code 0, port release, and temporary-storage cleanup. Final static/format and doctor checks passed.

**Decisions and limitations:** config/gateway.routes.json defaults to an empty list of operator-reviewed startup routes. The immutable provider does not watch files or support registry/runtime updates yet. Request prefixes are preserved; no guessed redirect/cookie/body rewrites are applied. Kestrel parser errors may have native response bodies; committed/upgrade stream failures terminate the stream instead of replacing already-sent headers with JSON. Authentication, package deployments, sample applications, metrics/log retention, and live routing remain later milestones.

**Evidence:** Detailed self-contained results and failed-attempt history are in PROGRESS.md. Additional local-only references are GATEWAY.md and docs/verification/M2-2026-10-26.md. No later milestone was started.

### M3 — Node.js and ASP.NET Core Sample Deployments

**Objective:** Provide trusted, minimal samples to verify the two required runtime families independently before package deployment is introduced.

**Tasks:**

- Add a Node.js sample with a health endpoint and configurable loopback port.
- Add an ASP.NET Core 8 sample with a health endpoint and configurable loopback port.
- Add run instructions and make sample responses identify the sample and release/build.
- Verify each sample directly on localhost; keep these samples separate from existing applications.
- Prepare the samples for later gateway routing and subpath compatibility checks.

**Dependencies:** M1 and M2 for the gateway context; each sample can be tested directly without dynamic routing.

**Acceptance criteria:**

- Both samples start as separate processes and bind only to loopback.
- Both health endpoints return success when each process is healthy.
- Both sample processes can run concurrently on distinct ports and stop cleanly.
- Sample instructions do not require Docker, Administrator privileges, or changes to existing applications.

**Verification commands (implemented):**

```powershell
npm.cmd run test:samples
npm.cmd run verify
npm.cmd run doctor
```

The dedicated task runs Node configuration tests, publishes Gateway/DemoDotnet/Samples.Tests with UseAppHost=false, scans their output for executable artifacts, and runs the managed Samples.Tests DLL. No npm install is needed for DemoNode. Run instructions are in samples/README.md and each sample's README.md. Direct run commands from the repository root:

```powershell
node .\samples\DemoNode\server.mjs
dotnet publish .\samples\DemoDotnet\DemoDotnet.csproj -c Release --no-self-contained -p:UseAppHost=false -p:UseSharedCompilation=false -p:PublishDir=../../artifacts/publish/DemoDotnet/
dotnet .\artifacts\publish\DemoDotnet\DemoDotnet.dll
```

Run the samples in separate terminals. Defaults are 6101/6102; IHP_SAMPLE_PORT accepts only 6100–6999. Both listeners are fixed to 127.0.0.1. Direct health paths default to /health. An optional canonical base path moves root/health under that prefix. Release/build labels are configurable safe tokens.

**Status:** COMPLETED

**Acceptance evidence (2026-10-26):**

| Criterion                                | Verified result                                                                                                                                                                                       |
| ---------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Separate sample processes, loopback-only | PASS: distinct simultaneously live PIDs, real sockets exclusively 127.0.0.1:6411 and :6421; hostile generic URL/host settings did not override binding                                                |
| Both health endpoints succeed            | PASS: real GET/HEAD, status=ok, correct sample/runtime/release/build/port/basePath/healthPath; root identity and 404/405 checks                                                                       |
| Concurrent execution and clean shutdown  | PASS: both direct and prefixed phases; stdin command and EOF yielded exit 0, listeners removed and ports successfully rebound                                                                         |
| Safe run instructions                    | PASS: dependency-free Node and framework-dependent ASP.NET Core DLL published/run in ordinary Windows session; Administrator=False; no Docker, system-security changes, or existing-app modifications |
| Later-prefix foundation                  | PASS: real /demo-node/ and /demo-dotnet/ root/health through published M2 Gateway at 127.0.0.1:5050 using an owned temporary startup routes file; custom release/build metadata preserved             |

**Final results:** npm.cmd run verify exited 0 with **215 passing checks**: 25 Node (including six sample tests), two API, 27 Foundation, 79 Gateway, and **82 managed sample checks**, plus full platform smoke. No skipped/failed checks. All six publish roots passed no-.exe scans; formatting, type checks, and doctor passed. The first dedicated run passed 6+80 checks; the final suite adds two malformed-target regressions requiring HTTP 400 and continued Node health. Detailed commands, actual evidence, and preserved history are in PROGRESS.md; local evidence also lives in docs/verification/M3-2026-10-26.md.

**Scope limits:** Trusted minimal JSON HTTP samples only. No deployment engine/registry, persistent port allocation, dynamic routing, uploads/build execution, authentication, assets/cookies/redirect rewrites, or later milestone was implemented. Shutdown-command and EOF were automated; interactive terminal Ctrl+C was documented but not manually exercised. Exact Node 24 from the specification remains unverified; installed Node 26.12.1 was used. No M3 blocker remains.

### M4 — Platform API and Database

**Objective:** Implement the NestJS management API and persistent SQLite registry using the policy-compatible Prisma workflow.

**Tasks and verified disposition:**

- Assess Prisma against the no-.exe policy. COMPLETED: pinned Prisma 7.10.1 uses guarded WASM validation, in-process TypeScript generation, and a real JS/WASM client with node:sqlite; native engines/Migrate are not executed.
- Define normalized users, applications, deployments, runtime instances, routes, database connections, logs, secrets and approvals. COMPLETED: 11 models including the application/connection assignment join.
- Add versioned migrations, indexes, constraints, FKs and unique slugs. COMPLETED: reviewed SQLite SQL plus a checksummed transactional Node runner; fresh/repeated migrations, rollback, schema/type/FK parity and integrity checks pass.
- Implement validated registry create/read/update, pagination/filtering/search, safe errors, request logs and OpenAPI/Swagger. COMPLETED: real NestJS HTTP and Prisma persistence verification.
- Keep protected SQL Server/PostgreSQL/MySQL reference registration/read/update/assignment as metadata-only APIs. COMPLETED: DPAPI credential protection, rotation, concurrency, restart persistence and redaction pass.
- Enforce SQLite-only real operations. COMPLETED: external connectivity implementation/direct API drivers/protocol fixtures removed; the compatibility test endpoint refuses with 403 database_connectivity_disabled_by_policy without credential loading or reachability claims.
- Keep paths configurable, listeners loopback-only, and destructive/deployment operations disabled. COMPLETED: configuration, guard, real socket and shutdown checks pass.

**Dependencies:** M1; M2/M3 regression context remains satisfied.

**Acceptance criteria under the authoritative 2026-10-27 clarification:**

| Criterion                                                           | Verified disposition                                                                                                                          |
| ------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Prisma validates schema and reviewed migrations create fresh SQLite | PASS: guarded WASM plus real SQLite/client; no native engine                                                                                  |
| Registry validation rejects duplicates and reserved paths           | PASS: real HTTP and SQLite constraints, including concurrent duplicate writes                                                                 |
| Connection-reference registration/read/update/assignment            | PASS for all three metadata type labels; data retained in SQLite and protected vault                                                          |
| SQLite-only policy enforcement                                      | PASS: all external-engine test requests denied before credential loading; no driver/protocol fixture runs                                     |
| Credentials omitted from responses/logs                             | PASS: DPAPI round trips/tampering/swap/rotation, raw registry scans, HTTP/log/OpenAPI checks                                                  |
| API validation/error/documentation                                  | PASS: 33 real HTTP/policy checks plus 13 guarded SQLite checks                                                                                |
| No anonymous deployment or destructive action                       | PASS: management defaults disabled, strong local capability required; deployment absent and DELETE disabled                                   |
| SQL Server/PostgreSQL/MySQL real connectivity verification          | ACCEPTED POLICY GAP: prohibited, not implemented/supported/claimed; excluded from M4 completion criteria by the user's explicit clarification |

**Verification commands (implemented and exercised):**

```powershell
npm.cmd run build --workspace @ihp/api
npm.cmd run test --workspace @ihp/api
npm.cmd run verify
npm.cmd audit --json
```

The build validates/generates the Prisma client in-process. The test task includes guarded migration checks. Supported separate registry steps after building:

```powershell
npm.cmd --workspace @ihp/api run db:migrate
npm.cmd --workspace @ihp/api run db:status
npm.cmd run start:local
```

The underlying Node migrate/status CLI was also verified in a retained, new user-profile test directory, separate from the actual platform registry. Do not run native Prisma CLI/Migrate or any external-engine connectivity test.

**Status:** COMPLETED

**Final evidence (2026-10-27, SQLite-only completion):** npm.cmd run verify exited 0 with **259 passing checks**, zero failures/skips: 25 Node foundation/sample, **33 API/reference/policy**, **13 guarded Prisma/SQLite**, 27 managed Foundation, 79 Gateway, 82 managed Samples; plus real platform smoke. All seven .NET publish roots passed no-.exe scans. Formatting/type/solution checks passed, npm audit reported zero vulnerabilities, ignore-scripts=true, Administrator=False. The current 33 API checks replace the former protocol tests with policy-denial/no-credential-load and ID checks; an unchanged total must not be read as replaying forbidden tests. No SQL Server/PostgreSQL/MySQL connection/protocol/authentication test ran in this completion session.

**Policy decision and historical M4 handoff:** Only SQLite is allowed/available for real operations. The user explicitly accepted missing external-engine connectivity evidence as a company-policy gap and requested M4 completion. The former blocker and instructions to obtain/test those servers are superseded, not satisfied by invented evidence. Their models/reference APIs remain; the execution path is removed. No M4 blocker remains. The original M4 handoff deferred M5; the subsequent explicit M5 authorization and verified completion below supersede that session-specific wait.

**Prisma/operational limits retained:** The custom Node migration runner is not native Prisma Migrate and does not implement automatic diff/shadow DB/history/drift repair. Pinned internal WASM/generator APIs and the published CommonJS entry remain version-sensitive. DPAPI is current-user protection, not same-user isolation; crash-time orphan reconciliation/identity backup and exact Node 24/manual Ctrl+C compatibility remain documented limits, not new completion claims. Details and preserved history are in PROGRESS.md and local API.md/docs/verification/M4-SQLite-only-2026-10-27.md.

### M5 — Deployment Engine

**Objective:** Provide a controlled .NET 8 runtime/process manager for trusted, reviewed releases without enabling untrusted/ordinary-user deployment.

**Tasks — implemented and verified:**

- Pure file/metadata Node ESM and published ASP.NET Core 8 detection; installed runtime checks, reviewed v1 contract, path/reparse/executable/native artifact validation, SHA-256 release fingerprints and canonical startup metadata.
- SQLite-backed unique leases in 6100–6999 with immediate writer transactions, occupied/exclusive socket probes, concurrent allocation, exhaustion handling and select-to-bind conflict cleanup that never kills an occupant.
- Reviewed release approval/selection; start/stop/restart/status, PID/start identity, instance/exit/end-reason persistence, CPU/working-set snapshots, stdout/stderr capture and bounded matching health checks.
- Allowlisted child environment, fixed loopback contract, actual PID-owned IPv4/IPv6 listener inspection, Windows kill-on-close/single-process jobs and fail-closed ownership errors.
- Durable desired state and bounded failure count/exponential backoff; graceful/hard Agent restart recovery, unknown-exit honesty, PID reuse protection and quarantine of matching live prior PIDs.
- Explicit local stdin operator controls only when enabled; no Agent HTTP listener and no new Platform API deployment/control endpoint.
- Managed DeploymentAgent.Tests plus a trusted fault fixture, all published with UseAppHost=false. Operator/setup/isolation instructions in local DEPLOYMENT_ENGINE.md.

**Dependencies:** M1/M3 and M4 registry contracts, all satisfied. M11 is still required before ordinary-user/unreviewed deployment is enabled.

**Acceptance evidence (2026-10-27):**

| Criterion                                                   | Verified result                                                                                                                                                      |
| ----------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Lifecycle/health/conflict/concurrency/failure/exit/recovery | PASS: 27 managed checks; real Node/.NET samples, actual exit 17/23/137, unhealthy/declining health, bounded attempts across restarts, actual Agent hard kill/restart |
| Loopback and durable leases                                 | PASS: real PID-owned socket checks; schema/unique leases; concurrent allocators; occupied/exhausted/racing port cases; reconciliation and port release               |
| Failure logs associated with app/release                    | PASS: persisted deployment logs with instance IDs, fixed failures/exit codes, both streams, redaction and capture budgets                                            |
| Ordinary-user Agent with no network exposure                | PASS: Administrator=False; published DLL processes; no Agent listener; explicit control-disabled check                                                               |
| Detection never executes unreviewed code                    | PASS: inspection sentinel stays absent; unreviewed/changed/traversal/native-artifact/canonical-metadata cases rejected before application launch                     |

**Verification commands (implemented):**

```powershell
npm.cmd run test:agent
npm.cmd run verify
npm.cmd run doctor
npm.cmd audit --json
```

The dedicated command publishes Agent, DeploymentAgent.Fixture, DemoDotnet and DeploymentAgent.Tests, checks for .exe artifacts and runs the managed verification DLL. Equivalent manual publish/run, with the three prerequisite publish roots already prepared:

```powershell
dotnet publish .\tests\DeploymentAgent.Tests\DeploymentAgent.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false -p:UseSharedCompilation=false -p:PublishDir=../../artifacts/publish/DeploymentAgent.Tests/
dotnet .\artifacts\publish\DeploymentAgent.Tests\DeploymentAgent.Tests.dll
```

**Status:** COMPLETED

**Final results:** npm.cmd run verify exited 0 with **286 passing checks** and no failures/skips: 25 Node foundation/sample, 33 API/reference/policy, 13 guarded SQLite/migration, 27 Foundation, 79 Gateway, 82 Samples, **27 Deployment Agent**. Full smoke passed real API/React/Gateway responses, fresh Node SQLite migration read by the Windows SQLite Agent, loopback sockets, four exit codes 0, released ports and removed owned smoke storage. All **nine** publish roots passed no-.exe scans. Static/type/solution checks, doctor and npm audit passed; zero npm vulnerabilities. Windows SQLite 3.43.2 and installed Node/.NET environment verified.

**Schema and workflow:** Appended reviewed migration 202610270002_engine and four normalized Prisma models (15 total): agent_releases, agent_desired, agent_port_leases, agent_instances. Existing migrations/applications are unchanged. Apply the explicit db:migrate step before starting the new Agent. The managed provider uses Microsoft.Data.Sqlite.Core 8.0.33 plus SQLitePCLRaw.provider.winsqlite3 2.1.11 and the OS DLL in-process; no native executable/server is installed. No SQL Server/PostgreSQL/MySQL operation/test was performed.

**Limits and decisions:** Trusted reviewed single-process loopback-v1 releases only; no shell/npm build/upload execution, injected platform credentials, Agent HTTP API, dynamic routing or later-milestone implementation. Jobs/environment checks are not same-user or network isolation. A small Process.Start-to-job-assignment window remains; no atomic suspended launch or orphan-proof guarantee. Hard-kill testing observes cleanup under the job/EOF contract; it does not distinguish which wins that race. Unknown prior exits remain null. Redaction is best-effort; unlabelled secrets can leak if trusted apps log them. No unattended reboot/logout startup, identity isolation, comprehensive crash recovery, exact Node 24 or manual Ctrl+C verification is claimed. Full detail/history is in PROGRESS.md and local DEPLOYMENT_ENGINE.md/docs/verification/M5-2026-10-27.md.

**Historical M5 handoff:** M6 was deferred for separate authorization. The subsequent user instruction and verified M6 completion below supersede that session-specific wait.

### M6 — Dynamic Routing

**Objective:** Connect approved application routes in SQLite to YARP at runtime without restarting the Gateway, preserving M2's static/security boundary.

**Tasks — implemented and verified:**

- Immutable validated static-plus-registry snapshots, atomic replacement and one-shot change tokens; unchanged snapshots do not reload and rejected candidates preserve the exact last known good configuration.
- Fixed loopback API polling with a separate read-only routing capability, strict versioned/nonduplicate JSON, streaming size/depth/header/timeout budgets, no redirect/proxy/cookie use and safe structured state logs.
- Operator-protected route-intent read/publish/update/remove with atomic audits; no caller-supplied destination. Projection follows selected reviewed M5 RUNNING release/instance/app/session/lease state and matching basePath.
- M2 validation/reserved paths and static source retained; configured/narrowed ports plus absolute 6100–6999 boundary; duplicate/static collisions and arbitrary destinations rejected.
- Runtime/release replacement and stopped-state reconciliation; Gateway/API restart recovery; static-only compatibility and source-failure retention.
- Prefix/assets/redirects/cookies and existing/new WebSockets verified. Path-only route boundary remains separate from future hostname support.
- Managed Gateway and Platform.Integration.Tests verification, safe owned fixtures and operator/trust documentation in local DYNAMIC_ROUTING.md.

**Dependencies:** M2–M5, all satisfied. M11 remains required before ordinary-user/unreviewed deployment or process execution APIs.

**Acceptance evidence (2026-10-27):**

| Criterion                                            | Actual result                                                                                                                                                                    |
| ---------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Live add/update/remove                               | PASS: real API intent and live provider changes; published Gateway PID/start ticks unchanged during operations                                                                   |
| Both samples on port 5050                            | PASS: reviewed Node/ASP.NET Core sample copies simultaneously healthy through distinct /demo-node and /demo-dotnet prefixes                                                      |
| Release/destination update                           | PASS: reviewed v2 Node replacement skips an occupied old port; new release/build/port is served through the same Gateway                                                         |
| Invalid/reserved/conflicting/arbitrary routes reject | PASS: M2 regression, operator-input and combined/static validation; malformed/unauthorized/redirect/oversized/timeout source tests                                               |
| Failed update retains last good                      | PASS: exact snapshot/token identity retained, live proxy stays usable, redirect trap receives zero calls; valid source recovery applies                                          |
| Restart reconciliation                               | PASS: deliberate published Gateway restart reloads persisted SQLite routes; API restart/removal reaches unchanged Gateway; stopped runtime withdraws/resumes                     |
| Static and WebSocket compatibility                   | PASS: static-only mode/source retained; existing socket survives changes/removal; new post-update socket negotiates/echoes/closes; preserved-prefix asset/redirect/cookie checks |

**Verification commands (implemented):**

```powershell
npm.cmd run test:routing
npm.cmd run test --workspace @ihp/api
npm.cmd run verify
npm.cmd run doctor
npm.cmd audit --json
```

test:routing builds the guarded API, publishes/runs Gateway.Tests, then publishes Gateway/Agent/DemoDotnet/trusted fixture/Platform.Integration.Tests and runs the managed integration DLL. All .NET publishes use UseAppHost=false. For manual integration execution after its prerequisites:

```powershell
dotnet publish .\tests\Platform.Integration.Tests\Platform.Integration.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false -p:UseSharedCompilation=false -p:PublishDir=../../artifacts/publish/Platform.Integration.Tests/
dotnet .\artifacts\publish\Platform.Integration.Tests\Platform.Integration.Tests.dll
```

**Status:** COMPLETED

**Final results:** npm.cmd run verify exited 0: **340 passing checks**, zero failures/skips — 25 Node foundation/sample, 44 API/reference/policy/routing, 13 guarded SQLite, 27 Foundation, **108 Gateway**, 82 Samples, 27 Agent and **14 managed routing integration**. Real platform smoke, all ten no-.exe publish scans, build/type/format/solution checks, doctor and npm audit passed. Audit reports zero vulnerabilities; Administrator=False. Evidence is in PROGRESS.md, local docs/verification/M6-2026-10-27.md and artifacts/m6-verify.log.

**TASK 026 regression follow-up (2026-10-30):** The two TASK 025-era M6 failures were not reproduced. Before verification, the required Gateway/API/runtime test ports had no listeners; no unrelated process was terminated. The focused `npm.cmd run test:routing` passed with Gateway 114/114 and published SQLite/API/Agent/Gateway integration 19/19. It verified three additional stop/start cycles: each stopped runtime lost its effective Gateway route, retained one route-intent row, and regained the route only after the runtime returned healthy; the Gateway process identity remained unchanged. API health was identity-checked. Measured API readiness was approximately 1.6 seconds in the focused run and up to 3.8 seconds in the full run, with the observed initialization phases recorded in PROGRESS.md. The old 15-second fixture deadlines were retained. No production Gateway, API or Agent routing behavior changed. The full `npm.cmd run verify` passed the M6 routing integration but failed later in Packages.Tests on a wrapper-folder ZIP validation assertion; the failure is outside M6 and is documented for TASK 027. M6 remains **COMPLETED**; no milestone status changed.

**Storage and authorization:** Uses the existing 15 models/two reviewed migrations; no schema/migration/dependency/server was added. Only SQLite performs real operations; the external-engine accepted gap is unchanged. API deployment/process/application-deletion remains disabled. M6 permits only local capability-protected route-intent removal, not application destruction or ordinary-user execution.

**Operational limits:** Opt-in polling/eventual activation; a successful PUT stores intent rather than acknowledging Gateway activation. Static collisions reject at the Gateway and require operator correction. Failed/offline feeds retain last accepted in-memory destinations, which can become stale; no offline revocation or independent API OS ownership authority is claimed. Fresh Gateway startup loads static routes only until the API responds. Registry projection trusts M5 state and same-user operator discipline. Existing WebSockets keep their old selected connection. No arbitrary upload/build, zero-downtime/rollback orchestration, hostname routing, login/RBAC, sandbox, unattended startup or corporate-network access is implemented.

**M6 historical handoff:** M7 was NOT STARTED at M6 completion. The latest attachment now authorizes M7 and integration of completed M11; see the current assignment and M7 status below.

### M7 — ZIP Upload and Approved Source Builds

**Milestone status:** IN PROGRESS. Product-owner decision is confirmed: source build and deployment remain mandatory alongside prebuilt deployment. Approved source-build isolation remains unavailable, so source builds stay disabled and the source-build acceptance criteria remain unmet. TASK 025 upload-reporting and TASK 027 ZIP-validation work do not satisfy or change those criteria.

#### TASK 033 — Non-executing Build Job Metadata API (metadata-only scope COMPLETED)

**Objective:** implement and verify the safe Category A foundation only while D00–D29 remain PENDING. This is an M7 subtask and cannot complete M7 or authorize a worker.

**Scope delivered:** additive SQLite `BuildJob`/append-only event schema and migration; authenticated application-scoped create/list/detail/cancel API; source snapshot ID and validated source fingerprint; target release label (intent only); fixed Node.js, ASP.NET Core 8 and React-static metadata templates; idempotency key hashing; M11 authorization; restricted states `REQUESTED`, `VALIDATED`, `REJECTED_VALIDATION`, `BLOCKED_WORKER_UNAPPROVED` and `CANCELLED`; bounded sanitized views; an inert production status boundary with no endpoint/network/process capability; and a test-only `SIMULATED` adapter that cannot create a deployable artifact. Arbitrary paths, commands/arguments, executable names, environment variables, dependency URLs and credentials are rejected. No Dashboard UI or worker connectivity was added.

**Dependencies:** existing M4 SQLite API/registry, M11 authentication/authorization and existing private staged folder intake. New requests verify the staged reference and source digest; missing, tampered or expired snapshots fail closed. The stored ID/hash are immutable, while the existing same-user staging filesystem is not an OS sandbox and retains its current expiry. D00–D29 approval is not required for this metadata-only slice, but worker integration, source execution and the complete build lifecycle remain gated on written approval and actual provisioning. No generic upload endpoint was added.

**Acceptance criteria:**

- Additive migration is tested only against owned disposable SQLite data; existing migration files/checksums are unchanged.
- Developers create/read requests only for owned applications under M11; Administrators get only existing authorized scope; Viewers cannot mutate; cross-application and unauthenticated access fail closed.
- Snapshot identity/digest is verified against the staged intake and stored immutably; missing/tampered references, invalid template IDs, idempotency conflicts and illegal transitions are rejected.
- The contract has no command/executable field; the production path has no worker endpoint/configuration/process launcher; tests verify no outbound network request or build process path is present.
- Without approved worker configuration, valid requests remain BLOCKED_WORKER_UNAPPROVED; no queue/run/success/artifact/deploy state can be created. Test-only results remain SIMULATED and cannot enter package approval/deployment.
- State/audit history is append-only, bounded and secret-safe; responses expose no credentials, database values, source content, absolute paths or raw exceptions.
- Tests cover schema, static framework detection, authorization/ownership/roles, idempotency, transition graph, snapshot tampering, no-worker failure, absence of process/network activity, audit redaction and separation from exact-artifact approval. Execute focused auth tests, a new `npm.cmd run test:build-jobs` target, then `npm.cmd run verify`; record exact results and retain existing no-`.exe`/published-DLL rules.

**Status:** COMPLETED for the metadata-only TASK 033 scope after focused and full verification passed. This does not change M7, approve a worker, or satisfy any source-build execution, output validation, exact-artifact approval or deployment acceptance criterion.

TASK 033 is a separately scoped, pre-approval metadata-only subset. It does not complete Phase 2, permit queue/dispatch/worker lifecycle transitions, or pass the Phase 1 approval/provisioning gate below. The existing phase dependencies remain authoritative for every operational build capability.

#### TASK 034 — IT/Security approval evidence and Build Worker readiness (documentation-only scope)

**Objective:** prepare a decision-by-decision evidence ledger, final approval checklist and objective implementation-readiness gate before any future worker integration.

**Scope:** review the D00–D29 register and TASK 029–033 records; distinguish product-owner-reported policy clarification from traceable written IT/Security approval and verified technical evidence; map the 16 required approval areas; inspect TASK 033 integration seams read-only; document the work required before dispatch, authentication, status, artifact, failure or audit integration. Do not contact reviewers, provision/connect a service, modify application code, execute source/tools, create executable artifacts, change security settings or begin another milestone.

**Dependencies:** TASK 029–033 documentation, existing Build Job metadata API and the unchanged Phase 1 approval gate. The listed responsible/approver roles remain proposed until authorized people are named. The reported permission for approved tools and an isolated worker is not IT/Security authorization.

**Acceptance criteria:**

- The D00–D29 readiness matrix identifies reported context, missing written evidence, required technical configuration/acceptance and status for all 30 decisions, without inferring approval.
- The final 16-item checklist distinguishes reported information, verified technical evidence, written approval and outstanding requirements.
- The implementation gate requires applicable written decisions plus actual provisioning, independent isolation, binary/policy, network/authentication, dependency, resource, logging/monitoring, recovery, artifact-integrity and exact-fingerprint Administrator approval evidence.
- TASK 033 dispatch/auth/status/fingerprint/failure/audit boundaries are described accurately from static inspection; existing metadata behavior and its fail-closed state are preserved.
- Only the four official tracking documents change; formatting, consistency and `git diff --check` pass. Application build/tests are not run because implementation is out of scope.
- M7 remains IN PROGRESS, Source Build stays disabled and every D00–D29 decision stays PENDING absent written authorization.

**Status:** COMPLETED for TASK 034 approval-readiness documentation only. No milestone or D-register status changes. No approval, worker readiness or isolation success is claimed.

**Next task:** TASK 035 — review traceable written IT/Security responses against D00–D29 and the readiness checklist. Update statuses only with authorized evidence. If approvals and provisioning/isolation evidence are complete, propose a separate bounded worker-integration task; otherwise record clarifications/rejections and keep Source Build disabled.

#### TASK 035 — IT/Security approval evidence review (documentation-only scope)

**Objective:** decide readiness from evidence actually present in the project workspace or explicitly supplied by the product owner; preserve the current execution/approval gate.

**Evidence reviewed:** product-owner policy clarification in TASK 035 (2026-11-03) and its TASK 032 record (2026-11-02); the D00–D29 register/TASK 031, approval package/TASK 030, mandatory source-build/TASK 029 requirements, TASK 034 checklist/readiness matrix, and TASK 033 API/schema/migration/read-only integration boundary. Workspace filename/text inventory review on 2026-11-03 found no traceable IT/Security decision, signed approval, worker service record, provisioned configuration or isolation-test artifact. The source build policy statements are not reviewer evidence.

**Result:** **0 APPROVED / 30 PENDING; Worker Integration NOT READY.** No named authorized approver or decision evidence reference/date was supplied. All 16 checklist items remain outstanding in whole or in part; TASK 033 proves only the non-executing metadata API. No status changes and no worker or application behavior changes are authorized.

**Acceptance criteria:**

- For each D00–D29, record available evidence sources/dates, reported context, missing written authorization and technical acceptance gate; retain PENDING absent authorized evidence.
- Verify all 16 readiness items distinguish product-owner report, observed technical evidence, written approval and outstanding actions.
- Keep written authorization, actual provisioning, independent isolation, approved binaries/hashes, network/authentication, dependency policy, quotas, audit/monitoring, recovery/cleanup and exact-artifact approval as hard prerequisites.
- Preserve no uploaded/source-generated `.exe`, arbitrary unapproved executable, M11 authorization, exact-fingerprint Administrator approval, loopback-only IHP, SQLite-only, no-Docker/no-Administrator and no-security-policy-change requirements.
- Documentation checks pass; do not run application tests/builds because no application code changes are in scope.
- Keep M7 IN PROGRESS, Source Build disabled and D00–D29 PENDING until supported by written evidence.

**Status:** COMPLETED for the evidence-review documentation only. Worker Integration remains **NOT READY**. No approval is fabricated and no source execution/provisioning occurred.

**Next task:** TASK 036 — the product owner supplies traceable written D00–D29 responses and outstanding service/configuration evidence from authorized owners. Update only decisions supported by that evidence. If all approvals are granted and the worker is provisioned and independently verified, propose a separate bounded integration implementation assignment; otherwise record unresolved questions and keep Source Build disabled.

#### TASK 031 — Human review summary and decision register (prepared; all decisions PENDING)

PROJECT_REQUIREMENTS.md now contains the concise reviewer summary, explicit mandatory-vs-optional choices, five no-`.exe` policy questions, and decision IDs D00–D29. The register maps the 22 existing controls, 14-item checklist, 11 threats, architecture/network/toolchain choices, artifact approval and security acceptance. Each decision includes a proposed solution, responsible role, allowed status, written-evidence requirement, dependencies and implementation impact. All statuses remain PENDING; the product owner will handle communication. No approval or infrastructure is assumed. Do not start Phase 1 implementation until all applicable decisions have traceable written approval and the approved service is actually provisioned and verified. If any decision needs clarification or is rejected, source builds remain disabled and M7 remains IN PROGRESS.

#### TASK 030 — IT/Security approval package (prepared; approval PENDING)

Documented the proposed component diagram and trust boundaries, API-initiated authenticated communication, source/artifact integrity rules, complete control matrix, Node.js/.NET template proposals, threat matrix and 14-item approval checklist in PROJECT_REQUIREMENTS.md. This prepares material for an authorized human review; it does not contact reviewers or represent approval. All infrastructure, tool execution, identity, network, dependency source, quota, retention and operational approvals remain **PENDING**. No infrastructure or build execution is authorized. M7 remains **IN PROGRESS**. The existing TASK 029 ten-phase roadmap is unchanged and remains gated at Phase 1.

#### TASK 029 — Mandatory source-build workstream roadmap (future; implementation not started)

**Objective:** Deliver fixed-template Node.js and ASP.NET Core source builds through a company-approved isolated builder, then validate, record and independently approve the exact output before existing Deployment Agent deployment. Upload is inert; a separately authorized build request is required. Build completion never auto-approves or deploys.

**Current gate:** Product requirement is resolved as mandatory. **IT/Security architecture approval and builder provisioning are BLOCKED/pending.** The Platform host has no approved isolation boundary. Do not implement or enable build execution, run uploaded install/build/restore logic, or provision/connect to an infrastructure option before written approval. Product-owner permission for source builds does not substitute for IT/Security authorization. If no approved boundary can be provided, source builds remain disabled and M7 remains IN PROGRESS.

| Phase                                  | Scope and required output                                                                                                                                                                                                                                                                                                                       | Dependencies and gate                                                                                                                                                                     | Acceptance evidence before advancing                                                                                                                                                                                                                                               |
| -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1. Architecture and security approval  | Select worker/service; document threat model, identities, OS isolation, network/dependency policy, resources, secrets, provenance, logs, retention, cleanup, operations, cost and exact IHP communication flows.                                                                                                                                | TASK 029 recommendation; written IT/Security and service-owner approval; no unapproved elevation, Docker, policy change or IHP network listener. **BLOCKED; implementation not started.** | Written approval names service/owner, allowed data and network flows, dependency sources, enforced controls, incident/retention owner and acceptable execution policy. Independent review confirms the design is a real isolation boundary.                                        |
| 2. Build request API and job lifecycle | Add application-scoped, authorized build-request creation, immutable source snapshot reference/hash, supported-template/toolchain identifiers, queued/running/succeeded/failed/cancelled states, bounded retry/cancel/expiry and audit linkage. No generic command field.                                                                       | Phase 1; existing M11 session, role, ownership, upload validation and SQLite-only Registry.                                                                                               | Managed tests prove authorization, ownership, idempotency, valid state transitions, cancellation/timeout and no build start from upload alone; schemas/migrations reviewed and tested only on owned SQLite.                                                                        |
| 3. Isolated builder integration        | Implement narrow authenticated job/artifact exchange with the approved service; transfer only necessary source/metadata; enforce job identity separation and quarantine output. No shared writable filesystem or direct Registry/runtime access.                                                                                                | Phases 1–2; provisioned IT-owned builder, endpoint/identity/flows explicitly approved.                                                                                                    | Integration tests prove authentication, replay protection, tenant/job separation, no Registry/secret exposure, bounded transfer, rejection of spoofed/stale jobs and cleanup on success/failure.                                                                                   |
| 4. Supported Node.js templates         | Define fixed Node templates and pinned toolchain/dependency inputs; the template may invoke its approved package/lifecycle/build steps only inside the isolated boundary. Preserve Node ESM/prebuilt runtime output contract.                                                                                                                   | Phases 1–3; approved Node toolchain and dependency mirrors; explicit IT decision on allowed build executables/processes on the worker.                                                    | Disposable isolated jobs build approved fixture apps; incompatible project shapes, dependency sources or runtime fail closed; lifecycle/build code cannot escape the worker boundary; the Platform host never runs uploaded build code. Logs and output are bounded and sanitized. |
| 5. Supported ASP.NET Core templates    | Define fixed ASP.NET Core templates, supported SDK/target framework and approved restore/build behavior within the isolated boundary; MSBuild targets/tasks and restore dependencies are treated as untrusted code and must remain contained. Produce framework-dependent runtime files compatible with the existing Deployment Agent contract. | Phases 1–3; approved .NET SDK/dependency sources and execution policy on worker.                                                                                                          | Isolated fixture build produces required DLL, `.deps.json` and `.runtimeconfig.json`; unsupported targets/native requirements fail closed; restore/build code cannot escape the worker boundary; host does not run uploaded restore/build logic.                                   |
| 6. Dependency and artifact validation  | Validate toolchain/template identity, dependency provenance, output path/type/count/size, required runtime files, secrets, links/reparse points, package metadata, hashes and provenance/attestation. Store accepted results in quarantine.                                                                                                     | Phases 3–5; fixed artifact format and approved storage/retention.                                                                                                                         | Positive/negative tests reject traversal, links, unexpected files, secret material, missing/oversized/ambiguous output and modified provenance; fingerprints are stable and bound to source, template, toolchain and dependency record.                                            |
| 7. Administrator artifact approval     | Create a distinct review request tied to the exact immutable output fingerprint and sanitized build details. Enforce reviewer separation and current M11 authority.                                                                                                                                                                             | Phase 6; existing approval/audit interfaces and authorized reviewer roles.                                                                                                                | Requester cannot self-approve; changed artifact/source invalidates prior approval; approval/rejection/audit are recorded; approval alone does not start or deploy.                                                                                                                 |
| 8. Deployment integration              | Submit only approved validated artifacts to the existing Deployment Agent workflow under existing trusted-execution and deployment permissions. Register/update the dynamic route only after runtime health succeeds.                                                                                                                           | Phase 7; M5/M6 workflows and current opt-ins; no M10 rollback or M10-DB behavior.                                                                                                         | Integration tests prove unapproved/changed artifacts are rejected, deployment is not automatic, health failure leaves route absent, healthy runtime gets validated route, and stop/failure preserves existing guards.                                                              |
| 9. Build status, logs and audit        | Expose authorized job status, bounded sanitized build logs, artifact fingerprints, requester/reviewer and state history; define retention, quotas and operator health for the builder.                                                                                                                                                          | Phases 2–8; approved log/retention access policy.                                                                                                                                         | RBAC/ownership tests prevent cross-application/user disclosure; tests assert secrets/raw environment/unsafe exception output are absent; retention and cleanup are bounded. This is build-job visibility only, not M9 runtime monitoring.                                          |
| 10. Security and acceptance testing    | Test isolation boundaries and complete build-to-approved-deployment flow, plus all existing M1–M6/M11 regressions and M7 package controls.                                                                                                                                                                                                      | Phases 1–9; IT/Security test authorization and approved isolated test service.                                                                                                            | Positive and adversarial tests verify identity/filesystem/process/network/resource/secret controls, artifact provenance, tamper resistance, logs, cleanup and all mandatory source-build templates. Only then may M7 be considered for completion.                                 |

**Phase 1 exit gate:** all applicable D00–D29 register items must be **APPROVED** with traceable written evidence, and the selected service must actually be provisioned and verified against the approved design. Any PENDING, NEEDS_CLARIFICATION or REJECTED item blocks source-build implementation. Approval status alone does not prove provisioning or passing isolation tests.

**Architecture recommendation:** evaluate an IT-managed Windows build worker first for Windows toolchain/output compatibility and company-controlled dependency sources. It must be an isolated managed host/VM or equivalently strong service with per-job disposable workspaces; a second process/user on the existing platform host is insufficient. Prefer an already-approved remote or other company builder instead if it proves all controls and has better support, cost or isolation. No option is currently provisioned or authorized. If company policy prohibits the required tool execution even on a separate service and no approved service can meet both policy and product scope, report the conflict and keep M7 blocked; do not work around it.

**React source-build scope:** Existing M7 requirements include React builds. Phase 4 must define a distinct React static-output profile and obtain an approved artifact/serving contract compatible with the existing Deployment Agent/Gateway before it can be accepted. No new runtime format or Gateway behavior is assumed by this roadmap.

**Commands:** There are no executable implementation/verification commands for this documentation-only planning task. After Phase 1 is approved and later implementation is separately authorized, add exact builder/API tests and safe commands to the corresponding phase record; do not improvise or run build commands against uploaded source on the current host.

**Verified ZIP cap refinement (2026-10-28):** compressed ZIP uploads now allow **50 MiB / 52,428,800 bytes inclusive**, Dashboard label **Maximum ZIP size: 50 MB**. Valid one-byte-below/exact-cap archives passed native API and signed Gateway inspection into pending approval; one-byte-above is rejected in the client before hashing/network and at server ingress with 413. Expanded total/per-file/count/ratio/CRC/path/type/fingerprint/approval/source-build/ordinary-proxy policies are unchanged. Additive fifth migration preserves old checksums/rows/indexes/foreign keys/immutable-state guards; configured data was not migrated and requires an explicit authorized operator step before updated components run. Final npm.cmd run verify exited **0 with 529 passing checks, zero failures/skips, plus owned local smoke**, with all twelve no-.exe publish scans/build/type/format checks passing. Focused package 37/37, API 84 + SQLite 14, Dashboard 43/43, signed protocol 51/51 and Agent 27/27 passed. PROGRESS.md records files, memory review, corrected HTTP-fixture/stale migration-count failures and operator-resolved DLL lock. This completes only the cap refinement: **M7 remains IN PROGRESS** (source-build isolation/templates unfulfilled), **M8 remains IN PROGRESS/default-off with 53 browser scenarios NOT RUN**; all later statuses are unchanged.

**TASK 025 — detailed upload validation errors (2026-10-30):** Implemented sanitized, bounded rejected-file details and applicable limits for Folder and ZIP intake/package validation; the Dashboard renders safe paths/reasons, truncation and remediation guidance. Focused folder/ZIP intake tests pass 36/36; the full API suite passes 120/120 plus 14 SQLite checks, Dashboard 47/47, and signed browser-boundary tests 54/54 within `npm.cmd run verify`. That full command exited 1 at the pre-existing published M6 routing integration gate: 12 checks passed and 6 failed after effective-route withdrawal timed out, with dependent health/authorization/socket assertions failing. `npm.cmd run test:routing` also exited 1 on an API readiness timeout. Package tests and final smoke were not reached. These failures are unresolved and are not attributed to upload changes; the full regression gate is therefore not passing. M7/M8 remain IN PROGRESS; M10-DB remains NOT STARTED. Actual-browser acceptance remains separate and unverified.

**TASK 027 — ZIP wrapper-folder validation (2026-10-30):** Fixed a validation-order defect where `SplitIntake` removed a common wrapper before checking forbidden path segments. Intake now checks every normalized original archive path (including explicit directory entries) before optional wrapper removal and checks file type/package-relative paths after normalization. A forbidden directory cannot be hidden as the wrapper or nested at any depth. Safe wrapper ZIP intake remains accepted. Existing traversal/canonicalization, duplicate, link, CRC/integrity, 50 MiB compressed, 32 MiB expanded, 8 MiB per-file, 256-entry, ratio, SQLite candidate/sidecar separation, package fingerprint, Administrator approval, M11 authorization/ownership and deployment restrictions remain unchanged. Regression cases cover `node_modules` and `.git` at root-wrapper, within an allowed wrapper, and deeper nested levels, plus explicit forbidden directory entries; existing safe-wrapper, path, alias/duplicate, SQLite, type/size/entry/malformed, package and real API/Agent checks remain in the gate.

**TASK 027 files and verification:** Changed local source `src/DeploymentAgent/PackageArchive.cs` and managed verifier `tests/Packages.Tests/Program.cs`. `npm.cmd run test:packages` exited 0: **40 passed, 0 failed**. `npm.cmd run verify` exited 0: **577 passed, 0 failed, 0 skipped**, then owned smoke verified Gateway 127.0.0.1:5050 and disposable loopback API/Dashboard readiness, safe API/browser behavior and cleanup. Breakdown: Node readiness/config 33; API/auth/routing/intake 120; SQLite 14; Dashboard 47; signed browser boundary 54; Foundation managed DLL 27; Gateway 114; samples 82; Deployment Agent 27; routing integration 19; Packages 40. All .NET verification artifacts used `UseAppHost=false`, had no `.exe` artifacts, and ran through `dotnet <published DLL>`. No application build/upload execution, My Work Center changes, DB migration, external database, Docker, Administrator privilege, security setting or corporate listener was used.

**TASK 028 — source-build isolation and deployment scope assessment (2026-10-30, investigation only):** Code review confirms authenticated ZIP/Folder intake, SQLite candidate separation, strict package inspection/fingerprints, distinct exact-fingerprint Administrator approval, and supported Node ESM/framework-dependent ASP.NET Core 8 trusted-prebuilt preparation are implemented. Managed package and routing integration fixtures verify selected prebuilt lifecycle scenarios. API and Agent trusted execution flags default off and must be opted in independently; approval is not proof of code safety. The child runs under the same ordinary Windows identity, so process/job/environment controls do not sandbox it. Source-only folder uploads report BUILD_REQUIRED; no source build, package hook or user-provided build command is run. Current API/Dashboard actual-browser management remains default-disabled pending 53 real-browser scenarios; this is an M8 verification gap, not evidence from the 54 simulated tests.

**Deployment model decision preparation:** Model A (prebuilt only) is the only currently feasible restricted operating model, and its supported subset has automated verification. It avoids build-time execution, but trusted application runtime code still executes under the same identity after the separate API and Agent opt-ins; it does not satisfy M7's unchanged source-template/build-log criterion. Model B (fixed approved templates) is insufficient isolation: submitted package scripts, npm lifecycle/build behavior, dependency code, MSBuild targets/tasks and restore dependencies can execute code or access same-user files/network. It is not authorized or safe under current restrictions. Model C (company-approved isolated build service/boundary) is the viable candidate if source builds remain mandatory; none is available or approved here. It requires written IT/Security boundary and operational approvals before any implementation.

**SQLite-dependent app boundary:** Folder/ZIP intake can separate SQLite candidates/sidecars into private review staging and perform bounded read-only inspection; code package creation excludes those bytes. Current Deployment Agent uses the release directory as working directory and injects no application database path. There is no persistent app workspace, data association, managed/existing DB lifecycle or restore/backup/migration path. Thus the documented target workflow (approved candidate -> persistent workspace -> application use -> preserve on update) cannot run yet. These dependencies belong to M10-DB and related separately authorized runtime path-contract work; M10-DB remains NOT STARTED. No database was opened, copied, attached, modified or migrated for TASK 028.

**Decisions and approvals required:** Product owner must decide whether the original M7 source-build requirement remains mandatory or start a formal requirements change to prebuilt-only; this assessment does not make that change. If builds remain mandatory, the owner must request IT/Security evaluation of a managed isolated builder and its identity, filesystem, process, network/dependency, resource, secret, artifact provenance, retention and cleanup controls. Database path compatibility/mode decisions in PROJECT_REQUIREMENTS.md section 9.1 remain open for M10-DB. No product-owner decision, IT/Security approval or implementation was performed in TASK 028.

**TASK 028 verification and status:** Documentation/source inspection only; no application build, test suite, application execution, uploaded source command, database access or security/system change was run. `npm.cmd run verify` was not rerun because TASK 028 changed no application code; latest verified result remains TASK 027's 577 passed/0 failed/0 skipped. M1–M6/M11 remain COMPLETED; M7/M8 remain IN PROGRESS; M9/M10/M10-DB/M12 remain NOT STARTED. Recommended TASK 029: obtain and record the product-owner decision on source-build scope; if mandatory, request the required written IT/Security boundary evaluation. Do not implement or enable source builds before that approval.

**Status/handoff:** No milestone status changed: M1–M6/M11 COMPLETED; M7/M8 IN PROGRESS; M9/M10/M10-DB/M12 NOT STARTED. TASK 027 changed upload path validation only; no M7 completion is inferred. TASK 028 confirms source-build isolation remains unavailable and M8 actual-browser acceptance remains unverified (53 scenarios NOT RUN; production browser management remains default-disabled). Exact next recommended task: **TASK 029 — obtain the product-owner decision on whether source builds remain mandatory; if so, request written IT/Security evaluation of an approved isolation boundary. Keep source builds disabled until authorized.** No later milestone is started.

**Objective:** Accept safe application packages and perform only policy-approved preparation/build operations.

**Tasks:**

- Implemented: session/ownership-protected raw ZIP streaming; compressed/expanded/file/count/ratio/time/concurrency/quota budgets; strict structure/CRC/path/link/artifact validation; owned extraction and cleanup; SHA-256 archive/configuration/file manifests.
- Implemented: reviewed prebuilt Node ESM and framework-dependent ASP.NET Core 8 inspection shared with M5; strict required platform.json or JSON-compatible platform.yaml metadata. General YAML/source manifests, standalone React build/static-only templates and dependency installation remain unsupported; backend-served prebuilt assets are permitted within budgets.
- Implemented: automatic pending exact-package approval; distinct current Administrator fingerprint/code attestation; session/current-role/owner/hash checks at preparation/start; immutable SQL metadata/state guards and event/audit history (20 models/four migrations).
- Implemented: SQLite Agent prepare/deploy/stop queue, non-overwriting promotion, M5 lifecycle/ports/health/jobs/recovery launch guard and M6 healthy routing. API and Agent execution default disabled and independently require explicit operator opt-in. Original M5/M6 workflows remain compatible.
- Implemented: failed staging/start cleanup, prior release preservation and stopped-selection restoration; restart fails closed on interrupted preparation. No rolling update/automatic rollback is introduced.
- **Unfinished/policy-limited:** allowlisted Node.js/React/ASP.NET source-build templates and build-output evidence. No safe approved build isolation exists. Same-user processes cannot contain build scripts; no uploaded npm/install/build/shell commands run. Do not enable templates merely to pass acceptance. Future reviewed npm operations would use npm.cmd on Windows only.
- Implemented/verified: workflow, package schema/budgets, permission/trust boundary, default execution gate, failure/recovery/stale-route/storage limits and unsupported-shape documentation; full existing regression and new managed package suite. Actual results are recorded in PROGRESS.md.
- Implemented in TASK 025: API and PackageInspector failures return bounded sanitized rejected-file details and known limits; Gateway browser transport revalidates and preserves only safe detail fields; Dashboard shows fixed reasons, safe relative paths, truncation and retry guidance. Upload reservation cleanup is serialized with reservation checks so an interrupted upload cannot leave a transient stale active slot. Focused upload tests pass; the complete regression run exposed unresolved M6 routing-integration failures, so the full gate is not recorded as passing.

**Dependencies:** Completed M4–M6 and M11; documented trusted-prebuilt policy in local PACKAGES.md. Approved build isolation/policy remains unavailable. Only reviewed trusted prebuilt code may execute under the explicit opt-in; arbitrary/source code stays disabled.

**Acceptance criteria:**

- Verify rejection of malformed archives, traversal/links/reserved/unsafe entries, oversized uploads/expansions, unsupported runtime/configuration and unauthorized deployment.
- ZIP cap refinement: verify valid compressed ZIPs below and exactly **50 MiB (52,428,800 bytes)** are accepted through native API and signed Gateway, one byte above is rejected before buffering/persistence, and Dashboard displays **Maximum ZIP size: 50 MB** with client-side rejection. Preserve existing 32 MiB expanded-total/8 MiB per-file/count/ratio/CRC/path/type/fingerprint/approval budgets and ordinary proxy limits. Verify additive migration of old package rows preserves checksums, data, foreign keys, indexes and immutable metadata/state triggers; never auto-migrate configured data. Record focused/full regression evidence without claiming actual-browser acceptance or M7 source-build completion.
- Verify Node.js/ASP.NET Core prebuilt upload/inspection, exact approval/rejection/tampering, immutable preparation, trusted real M5 execution and M6 route add/remove without arbitrary script execution.
- Verify state/event/audit history, current sessions/RBAC/ownership, default-disabled execution, duplicate/collision protection, failed-start/lease cleanup and interrupted preparation recovery.
- Verify failure cleanup preserves platform source, existing applications and previous releases; validation failures return controlled codes and logs omit credential values.
- For Folder and ZIP intake, verify API errors contain only fixed messages/reasons, stable codes, up to 20 sanitized relative paths, a truncation indicator, and allowlisted applicable limits. Unsafe/sensitive paths must be withheld and unauthorized/cross-application requests must not reveal rejected-file metadata. The Dashboard must show the safe rejection details and corrective guidance. Verify malformed ZIP, unsupported extensions, path violations, byte and entry-count limits, and the signed Gateway projection without changing request or archive budgets.
- **Still unfulfilled:** approved source templates build the supported samples and retain build logs. Source builds are disabled by policy assessment, not recorded as successfully built or accepted away. Per explicit user instruction, retain IN PROGRESS/BLOCKED until a safe approved mechanism or explicit acceptance decision resolves this requirement.
- Run full M1–M6/M11 regression plus package checks, formatting/build/no-.exe scans and local smoke. No corporate deployment/OS security change or later milestone.

### SIMPLIFIED_FOLDER_UPLOAD_WORKSTREAM - M7 simplified folder intake (IN PROGRESS)

**Scope/status:** This is an M7 sub-workstream, not a new top-level milestone or waiver of M7 acceptance. Overall M7 remains **IN PROGRESS**; trusted-prebuilt ZIP stays compatible and approved source-build/isolation remains unmet. The authenticated intake, backend package-processing stages and default-off Dashboard folder-selection/review candidate are implemented and automated-test verified for supported prebuilt files. Actual-browser acceptance is NOT RUN. Source-only packages are not generated, package readiness is not deployment readiness, and this workflow does not execute or deploy application code.

**Objective:** Developer enters application name, selects a folder, sees truthful static runtime/startup and upload-size findings, then submits for exact-package Administrator review. Ordinary users do not create ZIPs/manifests, enter UUIDs or calculate fingerprints. Nothing auto-approves, builds or deploys.

**Dependencies:** M4 Registry and M11 session/RBAC/ownership/audit; M5 runtime/health and M6 route validation; existing M7 streamed ZIP intake, inspector, fingerprint, exact Administrator approval and deployment gates; M8 typed dashboard/auth flow. M8 default-off browser gate still applies. No dependency authorizes source builds. M10-DB remains NOT STARTED.

**Task tracking:**

1. **Dashboard picker and review UX — IMPLEMENTED in TASK 021; actual-browser acceptance pending:** new/existing application flow, registration through the authenticated API, supported directory picker, validated nested relative paths, selected-folder/file-count/aggregate-size preview, inclusive 50 MiB client check, duplicate-submit protection, upload/package progress, sanitized actionable errors, existing-owner selection, backend-confirmed runtime/startup/inspection/approval/build-required/deployment-blocked result, and same-intake package retry. Unsupported directory-selection capability is explained. Existing ZIP workflow remains available. This is a default-off candidate; no actual browser acceptance is claimed.
2. **Safe transport — COMPLETED in TASK 019:** Dedicated M11-authorized owner-scoped intake; streaming, actual received-byte counting at the inclusive **52,428,800-byte** payload limit, separately bounded framing, timeout, concurrency/quota controls and owned cleanup. No ordinary proxy/JSON budget was widened.
3. **Paths/content — COMPLETED in TASK 019:** Hostile relative paths normalize under private staging; traversal/rooted/ADS/device names, malformed names, links/reparse escapes, case collisions, duplicates, depth/type/entry overruns are rejected. Fixed policy excludes/rejects development and sensitive artifacts. Client manifests are not trusted and `.gitignore` is never run.
4. **Identity/canonical package/inspection/review — COMPLETED for supported prebuilt inputs in TASK 020:** Recheck Registry ownership and staged manifest/hashes; derive application UUID/slug and version metadata from authenticated state; create minimal script-free Node ESM metadata; stream a deterministic, sorted, Store-method ZIP; preserve **50 MiB compressed, 32 MiB expanded, 8 MiB/file, 256 entries and 100:1 plus 4-KiB allowance**. The exact archive passes the unchanged Package Inspector, SHA-256 and M7 fingerprint are generated server-side, and the existing distinct-Administrator review request remains pending. No auto-approval/deployment.
5. **Static runtime/startup analysis — COMPLETED for supported cases in TASK 020:** Detect supported dependency-free Node ESM prebuilt entry/dependency files and published ASP.NET Core 8 DLLs with matching `.deps.json` and `.runtimeconfig.json`. Never execute a probe, restore, install, build or uploaded file. Unknown/incomplete layouts remain blocked.
6. **run.cmd — COMPLETED as static-only analysis in TASK 020:** Exact `node <relative .js|.mjs>` and `dotnet <relative .dll>` forms are supported. `npm.cmd run start` may be interpreted as text only if the package `start` value is itself one exact direct form. No batch/shell/npm process or lifecycle hook is run. Chaining, expansion, redirection, variables, helpers and unknown forms block packaging.
7. **Source-only — COMPLETED as fail-closed classification; build remains unavailable:** Source-only intake returns **BUILD_REQUIRED** and does not generate a package. No install/build/arbitrary execution. The approved source-build/isolation criterion for M7 remains outstanding.
8. **Truthful API state — backend COMPLETED in TASK 020; Dashboard mapping IMPLEMENTED in TASK 021 candidate:** Responses distinguish staged intake, detected runtime/startup, package generation/inspection, pending/rejected approval, Build required and blocked; `deploymentReady` remains false. The default-off Dashboard maps only supplied response fields and does not invent progress. Actual-browser acceptance is still NOT RUN.
9. **Backend verification — COMPLETED for TASK 019/020:** Existing limits, hostile paths/types/secrets, interrupted stream/concurrency/cleanup, supported and unsupported runtimes/scripts, server identity, inspector/hash/fingerprint/pending approval, no-execution sentinels, ownership/session/RBAC, retry behavior and archive budgets are covered. `npm.cmd run verify` passes; actual browser acceptance remains distinct and is not claimed here.
10. **Dashboard integration verification — COMPLETED for TASK 021 automation:** focused folder-selection/onboarding tests and the full regression exercised the candidate using DOM simulation plus the real loopback Gateway/API HTTP/WebCrypto integration. These tests verify fixed endpoint mapping, backend authorization boundary, no native credentials/cookies forwarded and pending-review result. They are not real-browser acceptance evidence.

**Database/runtime limitation:** Current M5 child environment has no app-specific SQLite configuration contract. Database-dependent folders remain blocked until a separately authorized capability exists; do not start M10-DB, fall back to the Platform Registry or store app data in releases.

**Acceptance criteria:** (a) user needs no UUID/manifest/fingerprint/ZIP; (b) server enforces streamed payload and unchanged extraction budgets; (c) hostile paths/ownership fail closed with cleanup; (d) server-generated identity and ZIP pass unchanged inspector; (e) startup detection is static and scripts never execute; (f) unknown intent remains blocked; (g) source-only remains review-only with no build; (h) M11 and exact distinct-Administrator approval remain intact, no auto-deploy; (i) legacy ZIP and M1-M6/M11 regression pass; (j) evidence is recorded. This does not satisfy/waive M7's separate source-build/isolation criterion.

**Current workstream status:** IN PROGRESS. The backend path and default-off Dashboard workflow from an owned application/folder through canonical package inspection to pending Administrator review are implemented for supported prebuilt inputs. Actual-browser acceptance and the separate M7 source-build/isolation acceptance are still unmet; deployment is never automatic.

**Verified API intake component (TASK 019, 2026-10-29):** `POST /_platform/api/applications/:id/folder-intakes` requires an authenticated M11 session, `application.write`, and current application ownership. It streams multipart file bytes into `IHP_STORAGE_ROOT/Temporary/FolderIntakes/<owner-id>/<application-id>/<upload-id>` under server-generated opaque blob names. File-content bytes are counted as received and capped at 52,428,800 inclusive; multipart framing is separately bounded. The response is HTTP 202 and reports `STAGED_REVIEW_ONLY`, accepted file count/bytes, an opaque upload ID, `packageValidation: NOT_RUN`, `deploymentReady: false`, and sanitized validation codes. This route remains intake-only; TASK 020 added the separate processing route below.

**Verified staged-package component (TASK 020, 2026-10-29):** `POST /_platform/api/applications/:id/folder-intakes/:uploadId/package/:version` requires a current M11 `application.write` session and rechecks current ownership. It accepts no archive, manifest, app identity, startup command, fingerprint or client file paths; those are derived from the validated server-owned staged manifest/Registry. Expiry, manifest/hash/file integrity, link/reparse paths and single-upload processing lock are rechecked. Runtime and startup analysis are static: Node supports a constrained ESM prebuilt entry plus already-present relative runtime files and no external declared dependencies; ASP.NET Core 8 requires matching published application DLL, `.deps.json`, `.runtimeconfig.json` and .NETCoreApp 8 metadata. `run.cmd` is inert text. Accepted forms are exact `node <relative .js|.mjs>` or `dotnet <relative .dll>`; `npm.cmd run start` can be parsed only when its `package.json` start value is one exact direct form, and is translated to the approved runtime rather than invoked. Unknown commands, ambiguous runtime, unsupported dependencies and incomplete layouts are blocked; source-only code receives `BUILD_REQUIRED` without a package.

For supported prebuilt files, server-generated `platform.json` and a minimal Node `package.json` are added as needed. The canonical ZIP has sorted normalized paths, deterministic timestamps and Store entries; all source hashes/CRC/size are rechecked while streaming. Limits remain 50 MiB compressed, 32 MiB expanded, 8 MiB per entry, 256 entries and the existing compression-ratio/path/type/CRC policy. The archive is submitted through the existing package upload/inspector path. The exact package identity, archive SHA-256 and approval fingerprint derive from server-owned records; existing approval remains PENDING for a distinct authorized Administrator. Retry returns the same result, changed package identity/version conflicts, and duplicate concurrent processing is bounded. No approval, build, execution, preparation or deployment is performed; `deploymentReady` remains false.

**TASK 020 evidence:** `node --test --test-timeout=15000 test/folder-intake.test.mjs` from `src/Api` — **25 passed, 0 failed, 0 skipped**. Focused interrupted-upload/concurrency reproduction — **2 passed, 0 failed**. `npm.cmd run build` and `npm.cmd run check` passed. Required `npm.cmd run verify` exited **0** with **554 passing automated checks, 0 failures, 0 skips**, followed by successful local smoke: config/readiness 33; API 109; SQLite 14; Dashboard 43; browser-boundary/DOM simulation 51; managed Foundation 27; Gateway 113; samples 82; Deployment Agent 27; routing integration 18; package verification 37. The smoke verified Gateway 127.0.0.1:5050 plus owned loopback API/dashboard listeners, health/proxy behavior and cleanup. No real browser acceptance is inferred from the 51 simulated/protocol checks; production browser management remains default-disabled. The first full-gate attempt stopped at a Prettier check on the edited test fixture; the fixture was formatted and the full gate rerun successfully. No .exe, source/build command, Docker, Administrator access, database migration, external database engine, corporate network or deployed application was used.

**Verification commands (implemented):**

```powershell
npm.cmd run test:packages
npm.cmd run verify
# Focused gate publishes all prerequisite managed tools/samples before invoking:
dotnet .\artifacts\publish\Packages.Tests\Packages.Tests.dll
```

**Status:** IN PROGRESS

**Verified evidence (2026-10-27):** focused package gate passed 37/37; final npm.cmd run verify exited 0 with **418 passing checks**, zero failures/skips, plus real platform smoke. All twelve published roots passed no-.exe scans. Exact Node/.NET upload/review/preparation/start/proxy/stop, rejection/tampering/collision/failure cleanup, interrupted staging, default Agent gate and full M1–M6/M11 regression passed. General deployment states track package state. PROGRESS.md records commands, corrected failures, files, security decisions and remaining limits. Approved source builds/build logs remain unfulfilled, so M7 stays IN PROGRESS; never infer full completion from its verified prebuilt subset. The latest user separately authorizes M8 while preserving this M7 status.

### UNIFIED_SQLITE_INTAKE_WORKSTREAM — M7 TASK 024 (IN PROGRESS)

**Scope/status:** Extend the existing authenticated folder intake with ZIP intake that automatically separates application files from existing SQLite candidates. This is a sub-workstream within M7, not a new milestone and not the M10-DB lifecycle service. M7 remains **IN PROGRESS** because approved source-build isolation is unavailable; M8 remains **IN PROGRESS** because production browser management remains default-disabled pending actual-browser acceptance; M10-DB remains **NOT STARTED**.

**Objective:** Let an authorized owner select one application folder or ZIP containing both code and SQLite artifacts; stage each category privately, inspect candidates read-only under bounded resource limits, and show truthful review metadata without attaching or changing a database or executing the application.

**Dependencies:** Existing M7 authenticated intake, `application.write` ownership/RBAC, PackageInspector, package fingerprint and distinct Administrator approval; existing M11 authorization; current default-off signed browser boundary. M10-DB lifecycle and actual browser acceptance are separate gates.

**Implemented and verified:**

1. Authenticated folder/ZIP intake with streamed 50 MiB inclusive upload cap, 256 entries, existing expanded/per-entry/ratio constraints, one ZIP multipart field, timeout, quota, concurrency and owned cleanup.
2. `.db`, `.sqlite`, `.sqlite3`, matching `-wal`/`-shm`, and inert SQL classification; multiple candidates; normalized relative paths; opaque data blobs under per-owner/application/upload `database/` staging; manifest format 2.
3. Evidence-based potential active/backup/unknown classification, sidecar association, bounded static source scan, SQLite header check and read-only `quick_check(1)` worker with extension loading disabled, query-only/trusted-schema settings, memory limits, candidate cap and deadline. Candidates and sidecar sets remain `REVIEW_REQUIRED`.
4. Managed PackageInspector ZIP split mode applies existing ZIP structure, count, expansion, per-file, ratio, path, link, CRC and file-type checks, optionally strips one validated common wrapper directory, and emits opaque private blobs. It never evaluates application code, manifests or SQL.
5. Canonical code packages exclude database bytes/sidecars and do not include their bytes in the code file fingerprint. Inert `drizzle/*.sql` and scoped static `.webmanifest` support is narrow; exact package approval and deployment restrictions remain intact.
6. New authenticated `POST /_platform/api/applications/:id/folder-intakes/zip` and exact signed browser operation `/admin/browser/applications/:id/folder-intakes/zip`; no generic proxy or application route change. Dashboard supports folder or ZIP selection, inclusive client size checks, and backend-confirmed candidate/review results. A valid selected ZIP is actually submittable.

**Remaining acceptance gaps:** TASK 025 has now implemented and focused-tested sanitized per-file Folder/ZIP rejection details and Dashboard rendering. The private browser client remains disabled by default and actual-browser acceptance was not run; automated Node/HTTP/WebCrypto/DOM tests are not browser acceptance. Database attachment, database lifecycle, runtime database configuration, replacement, restore, migrations, and app execution remain outside scope. The latest full `npm.cmd run verify` passed the M6 routing integration but failed in Packages.Tests at the ZIP wrapper-folder type-validation case; current evidence is in PROGRESS.md.

**Verification evidence (2026-10-29):** `npm.cmd run verify` exited 0 with **569 passing automated checks, 0 failures, 0 skips**, followed by successful owned local smoke and cleanup. Groups: Node foundation/readiness 33; API/auth/routing/folder intake 116; SQLite migrations/database 14; Dashboard DOM 46; signed browser-boundary protocol simulation 53; managed Foundation 27; Gateway including HTTP/WebSocket/dynamic routes 114; samples 82; Deployment Agent 27; routing integration 18; package/ZIP approval and runtime integration 39. The 53 browser-boundary checks are Node/HTTP/WebCrypto/DOM evidence and do not represent actual-browser acceptance. All managed .NET publishes used `UseAppHost=false` and passed scans for prohibited `.exe` output. Focused `node --test src/Api/test/folder-intake.test.mjs` passed **32/32**; Dashboard production build and 46-test suite passed. No application code was executed by inspection or upload tests; approved test samples ran only in their existing managed verification scenarios.

**Verification commands:**

```powershell
npm.cmd --workspace @ihp/api run build
node --test src/Api/test/folder-intake.test.mjs
npm.cmd --workspace @ihp/dashboard run build
npm.cmd --workspace @ihp/dashboard test
npm.cmd run verify
```

**Current work-package status:** TASK 024 unified intake and TASK 025 structured validation reporting are implemented; focused upload, Dashboard and signed Gateway checks passed. The latest full regression gate is **FAILED** at Packages.Tests because ZIP wrapper-folder normalization bypasses rejection of a forbidden `node_modules/` segment; M6 routing passed 19/19. The final smoke was not reached. This does not alter any top-level milestone status: M7 remains **IN PROGRESS** for its independent approved source-build/isolation requirement, and M8 remains **IN PROGRESS** pending actual-browser acceptance.

### M8 — React Dashboard

**TASK 021 folder-upload workflow (2026-10-29):** Implemented the default-off Dashboard candidate for new/owned application selection, authenticated registration, safe directory selection, nested relative paths, file-count/aggregate-size preview, inclusive 50 MiB precheck, folder intake and package generation through the existing exact API operations, retry without re-staging, and backend-confirmed review/build/block outcomes. The final `npm.cmd run verify` exited 0 with **557 passing checks, 0 failures, 0 skips**, and local smoke passed. Detailed counts and the overlapping-run failure/correction are in PROGRESS.md. This does not establish actual-browser acceptance: 53 actual-browser scenarios remain NOT RUN, and production browser management stays default-disabled. M7/M8 remain IN PROGRESS; M10-DB remains NOT STARTED.

**Final acceptance-preparation regression (2026-10-28):** npm.cmd run verify exited **0: 520 passing checks, zero failures/skips, plus successful owned local smoke** after adding the disposable operator fixture helper. Counts remain 33 foundation/readiness, 84 API/auth/startup, 13 guarded SQLite, 42 Dashboard DOM/native-client, 44 signed protocol/WebCrypto/real-service, 27 managed Foundation, 113 Gateway, 82 Samples, 27 Agent, 18 integration and 37 packages. Twelve no-.exe publish scans/build/type/format checks passed; smoke used fixed Gateway 5050 and owned API 65503/Dashboard 65504, then removed owned processes/storage. Self-check preparation and interrupted manual cleanup are separately evidenced in PROGRESS.md. **Actual-browser matrix: 0 PASS, 0 FAIL, 53 NOT RUN; capability availability BLOCKED. M8 remains IN PROGRESS/default-off**, and unmet original wizard/settings/read-contract requirements are not waived. No other milestone status changed. The pending sentence in the preparation checkpoint below describes its earlier edit time, not the final result.

**Latest browser-acceptance assignment (2026-10-28):** Prepared a traceable **53-scenario** operator matrix in the current PROGRESS.md runbook, covering transport/profile/WebCrypto/CSP, authentication/tab/session lifecycle, Host/Origin/CSRF/replay/fixation/isolation/storage, all roles/ownership/grants, actual package/approval/deployment/route/audit data and responsive/keyboard/accessibility behavior. No permitted browser tool/session is connected; capability availability is BLOCKED and **53/53 actual-browser scenarios remain NOT RUN, zero browser PASS/FAIL**. No Node/jsdom evidence is converted to browser acceptance. Keep **M8 IN PROGRESS** and private production browser management default disabled.

**Historical TASK 032 M8 revalidation (2026-11-02, before the policy clarification):** Re-read the existing matrix and confirmed 53 unique rows (T01–T05, A01–A09, S01–S10, R01–R09, D01–D15, U01–U05), all NOT RUN. No approved browser-control tool was exposed; the disposable fixture was not started and read-only port inspection found no listener at 127.0.0.1:5050. Results were 0 PASS / 0 FAIL / 0 scenario BLOCKED / 53 NOT RUN; browser access itself was BLOCKED. No regression suite ran because no application code changed. M8 remains IN PROGRESS/default-off. Use the existing operator runbook; do not count simulations as acceptance.

**Disposable test preparation:** Added local tools/browser-acceptance.mjs: owned fresh SQLite Registry, five test-only accounts, three owned/read-granted applications, four reviewed-sample/negative ZIPs, separate explicit migration/setup, native setup-session revocation, normal loopback components and managed socket checks. It ignores inherited IHP configuration, never uses configured Registry/deployed applications, never launches a browser and defaults trusted execution off; a separate disposable reviewed-sample flag preserves all M11/M7 approval/fingerprint/operator gates. Corrected a socket-inspector argument error; self-check passed and removed owned storage/ports. Manual mode reached ready and Ctrl+C verified cleanup (outer interrupted shell exit 1). PROGRESS.md contains masked-password commands, exact localhost URLs, safe evidence protocol, conditional replay/tamper tool blockers and operator result reporting. Full regression is running at this checkpoint; do not replace the preceding completed 520-check gate until its result is captured. M7 and all other milestone statuses remain unchanged; no M10-DB/application feature/API/schema/security-policy work.

**Current supported-functionality handoff (2026-10-28):** npm.cmd run verify exited **0 with 520 passing checks, zero failures/skips, plus complete owned local smoke**. Counts: foundation/readiness 33; API/auth/startup 84; guarded SQLite 13; Dashboard DOM/real-native-client 42; signed browser-protocol/WebCrypto/real-service 44; managed Foundation 27; Gateway 113; Samples 82; Agent 27; routing integration 18; packages 37. All twelve UseAppHost=false publish roots passed no-.exe scans; build/format/type/solution checks and actual 127.0.0.1 socket/owned cleanup smoke passed (Gateway 5050, temporary API 60378, Dashboard 60379). The 497-check handoff below is the previous baseline. No actual-browser acceptance is implied.

**Verified supported scope:** validated owned Node/ASP.NET registration, confirmed name/description changes, immutable runtime/framework/slug, unsaved validated prebuilt manifest guide with actual Node/.NET ZIP inspection remaining pending approval; own safe profile/session records/revoke-all; Administrator user search/pagination/create/role/activation/session-revocation plus confirmed ownership/read-grant/revocation. Native M11 independently enforces current role/ownership/active-user/last-Administrator policy; new signed Gateway mappings are explicit and management body fields are narrowed. Viewer/read grants remain read-only. Loading/errors/confirmations/session cleanup and distinct registry/package/deployment/queued-action/unavailable-runtime states are verified. No API/schema/migration/dependency change or new source-build/execution permission was introduced.

**Remaining acceptance:** actual-browser origin/transport/profile/WebCrypto/headers/CSRF/isolation/session/workflow/viewport/accessibility acceptance remains NOT RUN because no permitted tool/session is connected. Private production browser operations remain default disabled; **M8 stays IN PROGRESS**. Current-grants read contract, environment/visibility/global policy/deployment-settings writers, general legacy runtime/release read contracts, atomic restart and original full source-template/build/online-update wizard steps remain unavailable and explicitly labeled. No completion requirement is waived. M7 stays IN PROGRESS/source builds disabled; M9/M10/M10-DB/M12 remain NOT STARTED. AGENTS.md/PROJECT_REQUIREMENTS.md remain unchanged, including Managed Application Database requirements. See PROGRESS.md for exact failures/corrections/files/evidence and the next permitted actual-browser checklist task.

**Active remaining-functionality assignment (2026-10-28, pre-code review):** Baseline 497 checks; complete supported application configuration/validated prebuilt guide, existing M11 user/ownership/grant management views and honest policy/release-state UX. The existing API has user management and owner/grant mutations but no current-grants read endpoint, no deployment-settings writer and no environment/visibility/settings editor. Do not invent those endpoints or persist unsupported fields. Keep package metadata immutable, source builds/rollback/database lifecycle disabled and browser candidate default off. Record new actual test results before claiming completion. M8/M7 remain IN PROGRESS; M10-DB and other later milestones remain NOT STARTED. PROGRESS.md records the requirement/implementation gap matrix before source changes.

**Previous verified handoff (2026-10-28, baseline for the remaining-functionality assignment):** npm.cmd run verify exited 0 with **497 passing checks**, zero failures/skips, plus complete local smoke. Counts: Node foundation/readiness 33, API/auth/startup 84, guarded SQLite 13, Dashboard DOM 28, browser protocol/WebCrypto/real-service 35, Foundation 27, Gateway 113, Samples 82, Agent 27, routing integration 18 and packages 37. Twelve publish roots passed no-.exe scans; build/format/type/solution checks, doctor and zero-vulnerability audit passed. First full attempt passed all tests but failed smoke because 5173 was occupied; disposable smoke now uses its own validated temporary API/Dashboard ports/configuration, Gateway 5050 unchanged, with owned cleanup/environment restoration. The existing listener was not modified. Final smoke passed on 127.0.0.1:5050/55084/55085.

**Acceptance limits:** Current API/SQLite readiness and controlled missing-Registry/bind/exit/failure cleanup cases are verified; the precise historical :5266 cause remains unconfirmed because unchanged reproductions passed and its old log had no API phase output. The restricted browser candidate, authorized operations and private-state protections pass automated real-service/DOM checks, but actual-browser transport/profile/headers/CSP/viewport/accessibility acceptance has no permitted connected tool and remains NOT RUN. Private production browser mode therefore remains default disabled. Original full wizard/settings/legacy read-contract gaps are not silently waived. **M8 remains IN PROGRESS**; M7 remains IN PROGRESS and M10-DB NOT STARTED. See the authoritative PROGRESS.md snapshot; following earlier M8 paragraphs retain historical edit-time evidence only.

**2026-10-28 continuation:** Focused real-service browser suite 35/35, Dashboard DOM/native-client suite 28/28 and startup/readiness suite 11/11 passed. Added safe API startup phases, actual API/SQLite readiness before dependencies, bounded body/identity/redirect/exit diagnostics and failure cleanup; session-bound queued intents, late-expiry response rejection, logout notifications, visible-session checks on focus/visibility/every 30 seconds; confirmed stop/route removal and explicit unsupported atomic restart. Real signed tests exercise role/ownership/approval restrictions, trusted start/stop/redeploy and live route changes. The historical :5266 failure did not reproduce on unchanged runs; its precise cause remains unconfirmed, not silently declared fixed. Full regression is pending. Actual-browser transport/profile/header/CSP/visual/accessibility acceptance remains unavailable, so private production browser mode stays disabled and M8 stays IN PROGRESS. M7 remains IN PROGRESS and M10-DB NOT STARTED; no new database service/schema or later milestone work.

**Objective:** Provide the responsive management interface for authorized users and deployment workflows.

**Tasks:**

- Implemented: React/TypeScript/Vite/Tailwind, local shadcn-style Radix/CVA primitives, Lucide icons, responsive sidebar/header/navigation, loading/empty/error states, semantic labels/focus/keyboard/reduced-motion styles.
- Implemented: real public health; application list/detail/registration/search/filter/pagination/current-page sort; package history/hash/events/ZIP/control views; exact approvals/rejections/history; stored routes/safe links/confirmed removal and existing Administrator audit views through typed existing contracts. No invented runtime/history/metric endpoint.
- Implemented: session/current-user/logout/expiry/RBAC/ownership view logic, safe errors and stale-response cancellation. Default BrowserApi permits only public health and disables all login/private actions; the separately opt-in signed candidate remains default off. No native-token input/header-stripping/CORS workaround.
- Implemented in the default-off candidate: validated Node/.NET registration with authenticated ownership; confirmed name/description editing; immutable runtime/framework/slug and unsupported environment/visibility display; validated unsaved trusted-prebuilt manifest guide. Existing package metadata is never rewritten, and preview/registration never approves, builds or deploys code.
- Implemented in the default-off candidate: safe own profile/session records and confirmed revoke-all; Administrator user search/pagination/create/role/activation/session-revocation and confirmed ownership/read-grant/revocation controls. Native M11 enforces roles, ownership, last-active-Administrator protection and revocation on every request. Signed mappings are explicit and account-management bodies reject duplicate/extra fields; password reset remains native-only.
- Implemented in the default-off candidate (TASK 021): create an application through the existing authenticated API or select an existing manageable application; choose a folder through the supported directory-picker capability; show validated selected files, relative paths, count and total bytes; enforce 52,428,800 bytes before upload; stage using the exact existing folder-intake endpoint and generate the package using the exact existing versioned endpoint. Backend-confirmed runtime/startup, inspection, pending approval, BUILD_REQUIRED and DEPLOYMENT_BLOCKED are displayed without accepting user-supplied IDs/fingerprints or inventing states. Package retries reuse the returned intake ID and do not repeat registration or staging. No auto-approval or deployment.
- Implemented: opt-in bounded /admin/ static Gateway hosting and exact public API-health projection; no protected API proxy. Vite retains Origin/Fetch Metadata.
- Unfinished: approved actual-browser authentication/session/CSRF/transport/origin and authenticated workflow acceptance for the default-off candidate. Native-client DOM and signed HTTP/WebCrypto tests do not establish actual-browser acceptance.
- Unfinished: actual browser viewport/pixel/screen-reader verification and production browser acceptance; original full deployment wizard/source-template/build/online-update steps; unsupported environment/visibility/global-policy editors and unavailable legacy runtime/release read contracts. Current-grants list has no API read contract; show mutation outcomes only. Monitoring is M9, rollback M10 and application database service M10-DB; none is started.

**Dependencies:** M4/M6/M11 and verified M7 prebuilt contracts are available; user explicitly assigns M8 despite M7 source-build/isolation remainder. Approved browser authentication is unavailable. Full authorized browser flow must pass before M8 completion; preserve M7 IN PROGRESS.

**Acceptance criteria:**

- Dashboard and required pages build and run locally behind the gateway.
- Lists support search/filter/sort, and detail/deployment views reflect API state.
- The M7 folder-upload candidate registers/selects an authorized application, preserves safe nested relative paths, rejects selections above the inclusive 50 MiB payload cap, uses only existing secured intake/package APIs, and shows backend-confirmed pending/blocked outcomes without auto-approval/deployment; supported-browser behavior remains pending actual-browser acceptance.
- Unauthorized users cannot see or invoke deployment controls; errors and progress reflect actual API results.
- Frontend assets, navigation, and API calls work under the configured path prefix.
- Responsive and accessibility checks are recorded.

**Verification commands (implemented):**

```powershell
npm.cmd run test:dashboard
npm.cmd run test:gateway
npm.cmd run verify
```

**Status:** IN PROGRESS

**Current M8 assessment:** latest user authorizes M8 while M7 remains IN PROGRESS. M11 rejects Origin and Sec-Fetch-Site other than none; no approved browser authentication transport exists. Do not weaken these controls or build a credential/header-stripping bridge. Implement safe dashboard/UI/API-contract work and document browser-dependent acceptance as unverified/disabled. M7 source builds remain disabled. No M9 work is authorized.

**Browser-authentication follow-up:** user now explicitly authorizes a restricted browser boundary. Implemented opt-in localhost:5050 management-origin separation on the existing listener, memory-only nonextractable P256 key/proof sessions, single-use login challenge/request nonces, CSRF/Host/Origin/metadata checks and fixed operation allowlist. M11 native guards/RBAC/ownership remain unchanged; a native-only issuer reuses M11 with a fixed shorter 30-minute expiry that the Agent already enforces. Source builds remain disabled. Candidate defaults off; actual browser/loopback transport/profile/CSP behavior has no permitted connected verification tool, so M8 remains IN PROGRESS. BROWSER_AUTHENTICATION.md records trust limits and evaluation instructions. Focused real-service protocol checks passed 28/28; full regression is pending at this edit.

**Documentation-only evidence reconciliation:** Read-only inspection of the already-ended follow-up log found the later full run failed its 28 browser checks because the owned API on 127.0.0.1:5266 was unavailable during stack readiness (ECONNREFUSED/timeout). The pending statement above describes its earlier edit; the 451-check result below is the last successful historical full gate, not a pass for the newer candidate. No application test/build/repair was performed in the documentation task. Preserve this unresolved M8 failure/default-off boundary and IN PROGRESS status for a future explicit implementation assignment; see the current PROGRESS.md record.

**Verified evidence (2026-10-27):** final npm.cmd run verify exited 0 with **451 passing checks**, zero failures/skips, plus real built /admin/ assets/CSP/public-health/Origin-rejection and owned shutdown/port/storage smoke. Includes 28 React DOM/model/owned-native-client checks and 113 Gateway checks; all twelve .NET publish roots passed no-.exe scans. Original M1–M7/M11 regression remains passing. Native adapter is test-only, not a browser bridge. Actual browser authentication/workflows/visual acceptance and original unfinished wizard/settings scope remain unfulfilled, so M8 stays IN PROGRESS; M7 also stays IN PROGRESS. See PROGRESS.md/DASHBOARD.md.

### M9 — Monitoring and Logging

**Objective:** Make platform and application status, resource use, and diagnostic logs visible and searchable.

**Tasks:**

- Add health/status collection and process-level CPU/RAM sampling for managed processes.
- Add Serilog/structured platform logs and application/release-separated stdout/stderr capture.
- Add log timestamps, levels, search/filter/download, retention controls, and secret redaction.
- Show system health, application health, deployment activity, and resource data in API/dashboard.
- Ensure collection and retention are bounded and configurable.

**Dependencies:** M4, M5, and M8; deployment logs from M7 and events from M10 can integrate as they become available.

**Acceptance criteria:**

- Health and status changes are reflected in the dashboard/API.
- Process CPU/RAM and health-check results are collected with documented sampling behavior.
- Logs are separated by application and release, searchable/filterable, downloadable, and retained according to configured policy.
- Tests verify secret redaction and that credentials do not appear in logs or downloads.
- Monitoring failure does not silently mark an unhealthy process as healthy.

**Verification commands (planned):**

```powershell
npm.cmd --prefix .\src\Api run test
npm.cmd --prefix .\src\Dashboard run test
dotnet publish .\tests\Platform.Integration.Tests\Platform.Integration.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false
dotnet .\tests\Platform.Integration.Tests\bin\Release\net8.0\publish\Platform.Integration.Tests.dll
```

**Status:** NOT STARTED

### M10 — Version Management and Rollback

**Objective:** Retain release history and switch between compatible releases with recoverable deployment behavior.

**Tasks:**

- Create immutable release records and application version history linked to package/build metadata, logs, and runtime instances.
- Implement staged update: prepare, validate, start candidate, health check, verify public route, then switch active route.
- Preserve the last known good release and route until a candidate passes checks.
- Implement rollback and cleanup/retention controls without deleting active or rollback-eligible releases.
- Record update/rollback actors and outcomes in the audit trail.

**Dependencies:** M5, M6, M7, M9 logging, and M11 authorization.

**Acceptance criteria:**

- Successful deployments retain versioned releases and deployment history.
- A failed candidate does not displace the current healthy release.
- An authorized rollback restores a previously healthy compatible release and verifies its health and public route.
- Rollback failures are visible and leave recoverable state; retained releases obey documented policy.
- Tests cover failed update, successful update, rollback, and interrupted deployment recovery.

**Verification commands (planned):**

```powershell
npm.cmd --prefix .\src\Api run test
dotnet publish .\tests\DeploymentAgent.Tests\DeploymentAgent.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false
dotnet .\tests\DeploymentAgent.Tests\bin\Release\net8.0\publish\DeploymentAgent.Tests.dll
dotnet publish .\tests\Platform.Integration.Tests\Platform.Integration.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false
dotnet .\tests\Platform.Integration.Tests\bin\Release\net8.0\publish\Platform.Integration.Tests.dll
```

**Status:** NOT STARTED

### M10-DB — Managed Application Database Service (metadata and path-validation slices in progress; lifecycle future)

**Objective:** Implement the optional **1 Platform Database + N Independent Application Databases** architecture from requirements section 9.1. TASK 036 completed a first metadata slice for review-only SQLite candidates, TASK 037 added a prospective path validator, TASK 040 added inert database-mode request metadata, and TASK 041 added Administrator review/rejection for those requests. DB-W0 remains unresolved. Active mode selection, persistent workspace materialization, runtime attachment/configuration, provisioning, backup, restore and Dashboard lifecycle remain future work and are not claimed implemented.

**Roadmap placement:** Full lifecycle work remains after M10 version management/rollback and required M8 secure Dashboard capabilities, before final M12 acceptance of this extension. The separately authorized Phase 1 inventory slice uses existing M4 Registry, M7 intake and M11 authorization foundations. Existing M1-M12 identifiers and unrelated statuses are unchanged; M11 remains a dependency.

**Remaining tasks (future, independently verifiable):**

1. Phase 1 candidate metadata contract, additive Registry migration and API are implemented by TASK 036. TASK 040 adds a distinct additive Registry migration for mode/association-intent requests, and TASK 041 adds a separate additive review-event migration and Administrator review/rejection APIs. These record intent/review only; they do not set a current application mode or activate a candidate. Remaining work includes lifecycle authorization contracts and later versioned Registry migrations for operational state. Preserve Registry data/configuration and metadata-only external-engine references; never apply a configured Registry migration implicitly.
2. Implement an application-scoped storage resolver under configurable IHP_STORAGE_ROOT and the target logical layout. Validate canonical containment, file identity, traversal/reparse/symlink/alias escapes and cross-application targets for registration, provisioning, SQLite sidecars, backups, restore and deletion. Preserve existing implemented paths unless a separately authorized compatibility migration changes them.
3. Implement idempotent/concurrency-safe non-overwriting per-application provisioning and explicit association/initialization, safe metadata/status/integrity/usage/limits, retention, authorized deletion and interrupted-operation recovery. NONE must allocate no database; EXISTING_SQLITE requires explicit registration/location authorization without automatic creation, migration or deletion authority.
4. Implement explicitly authorized SQLite-consistent backup and validated restore, with protected backup history/retention outside releases, writer coordination/downtime, source/target identity checks and recoverable failure handling. Never use an uncoordinated active-file copy as proof of backup correctness.
5. Integrate minimum runtime configuration and data preservation with M5/M7/M10: the same application database survives deployment/update/restart/rollback/release cleanup; schema initialization/migration is separate and authorized. Never run SQL scripts from uploaded packages automatically or infer database rollback from application rollback.
6. Integrate a future Application Database Management Dashboard section through secured Platform API contracts: mode selection, provisioning, safe status/metadata/usage, authorized backup/restore/history, retention and deletion requests. Apply server-side M11 RBAC/current ownership; Viewer read-only and destructive authorization/confirmation. No raw file download or public path/Registry/secret exposure.
7. Add credential-safe lifecycle/security audit events and operational documentation for backup/restore/recovery/retention, permission boundaries, limits and same-user trust. Extend existing automated/managed verification with owned SQLite-only fixtures; preserve all prior behavior and policy controls.

**Implementation work packages:** DB-W1 has started for candidate metadata and a prospective path-validation slice, and remains IN PROGRESS for its broader scope. Database mode request/review metadata is implemented as bounded TASK 040/041 sub-scopes; it does not close DB-W1 lifecycle acceptance. DB-W0 remains unresolved and must gate any compatibility-dependent mapping/runtime use. Other packages remain NOT STARTED. File paths are anticipated from the current repository layout and must be confirmed during the relevant assignment.

#### TASK 036 Phase 1A - Existing-intake candidate metadata (completed sub-scope)

- **Scope delivered:** additive migration `202611030007_database_candidate_inventory`; candidate/sidecar metadata linked to application and intake, including validated relative paths, fingerprints, sizes, owner/detector snapshot, validation/snapshot/lifecycle states and timestamps; app-scoped list/detail/validation APIs protected by M11 `application.read`; read-only staged-byte fingerprint recheck with fixed outcomes.
- **Acceptance evidence:** focused API and SQLite/migration tests passed 149/149; full `npm.cmd run verify` passed 592 checks, 0 failures and 0 skips. API responses expose no DB bytes or absolute paths. Tests cover unauthorized/cross-application denial, invalid references, fingerprint mismatch/unavailable, metadata-only persistence, immutable uploaded bytes, and absence of source execution/runtime attachment.
- **Limits:** migration was tested on disposable SQLite only and was not applied to the configured Registry. This slice creates no runtime DB association, application database file or storage resolver.
- **Status:** completed TASK 036 sub-scope. M10-DB overall is IN PROGRESS; TASK 040's intent-metadata sub-scope is also completed. DB-W1 remains IN PROGRESS for operational lifecycle metadata, resolver integration and full acceptance.

#### DB-W0 — Resolve product/security decisions and path compatibility contract

- **Scope:** Record and resolve remaining product/security gates in the 13-item matrix and path-pattern matrix in PROJECT_REQUIREMENTS.md section 9.1.18, plus the supplementary health/routing question and concise decision package in section 9.1.19: default area and workspace/data semantics; CWD/module/assembly/absolute/environment/config path behavior; MWC override and support; WAL/SHM quiescence; startup migration; import mode/provenance; backup/restore; release persistence; same-user trust; and app health/base-path/runtime/package compatibility. TASK 023 settles safe first-use of supplied valid data, upgrade KEEP default and explicit guarded replacement policy; retain those decisions and do not request them again. TASK 039 records recommendations and exact questions, but does not decide unresolved product-owner questions.
- **Dependencies:** Requirements section 9.1; current M5/M7 behavior; M11 role/ownership contract; product-owner/security decisions listed in PROGRESS.md.
- **Likely files:** `PROJECT_REQUIREMENTS.md`, `IMPLEMENTATION_PLAN.md`, `ARCHITECTURE.md`, `DEPLOYMENT_ENGINE.md`, `PACKAGES.md` (documentation/contract only).
- **Acceptance:** The documentation matrices distinguish evidence, recommendation, product-owner decision and status; all unresolved choices identify the owner decision required and remain pending; the original My Work Center remains NOT COMPATIBLE AS-IS until every recorded blocker is resolved; prior inspection of an adapted package is not treated as proof of unchanged-app/database/runtime compatibility; no unsupported path is silently remapped. Approved first-use/KEEP/explicit-replacement behavior is preserved. **Decision package preparation is complete; DB-W0 resolution is pending product-owner answers and remains a gate before compatibility-dependent runtime use.**
- **Tests:** Documentation/contract validation only; later tests derive explicit compatible/incompatible fixture cases.
- **Security:** Do not rewrite app code/config, add junctions, or imply a workspace provides OS isolation. This gate must finish before storage/runtime implementation begins.

**TASK 038 documentation result:** Static review confirms the current My Work Center path is module-relative and its existing named override is not yet an IHP binding. Startup migration, app-managed backup/pruning, raw DB download, health/base-path and package/runtime constraints remain blockers. The path matrix covers CWD-relative, module-relative, assembly/content-root-relative, absolute, environment-based and config-file-based paths. The original 13-row matrix has 1 resolved row (KEEP default) and 12 pending rows; W0-10 includes a previously approved first-use policy but its import mode/provenance remains pending. No code/tests were run.

**TASK 039 decision result (historical):** The owner package is ready; no reply/approval was supplied. W0-12 remains the only fully resolved original row; 12 original rows remain pending or partially pending, and supplementary W0-14 health/routing remains pending. The product-owner-supplied first-use policy remains accepted while import mode/provenance/location remains open. D00-D29 remain separately PENDING. My Work Center remains NOT COMPATIBLE AS-IS. No runtime/lifecycle implementation is ready.

#### TASK 040 - Metadata-only mode and association-intent requests (completed sub-scope)

- **Implemented:** additive Registry migration `202611040008_database_mode_intents`; `ApplicationDatabaseModeRequest` and append-only `ApplicationDatabaseModeRequestEvent` metadata; UUID association-intent identifiers; `NONE`, `MANAGED_SQLITE`, `EXISTING_SQLITE` modes; optional same-application review-only candidate reference; requester/time, constrained request/review/cancel fields, creation/audit-event linkage, indexes, foreign keys and database-enforced append-only/state-transition rules. No physical database path, contents, credentials, fingerprint copy or active/ready/provisioned/attached state is stored by this request schema.
- **API:** `POST /_platform/api/applications/:id/database-mode-requests` creates an intent; `GET` on the collection lists bounded/paginated app-scoped requests; `GET /:requestId` retrieves one; `POST /:requestId/cancel` cancels a pending request. Existing global M11 session, `application.read` / `application.write`, role, application ownership and grant checks are used. `EXISTING_SQLITE` verifies candidate identity/status using same-app Registry metadata only; it does not open candidate files. Idempotency keys are SHA-256 hashed at rest and omitted from responses.
- **Current state boundary:** creation starts at `REQUESTED`; cancellation is the only exposed state transition. `PENDING_REVIEW` and `REJECTED` are constrained metadata vocabulary only; no Administrator review/rejection endpoint is implemented in TASK 040. No request means permission to activate data.
- **Verification:** API build and focused API tests passed; full verification passed **608/608** with 0 failures and 0 skips, including 6 new request tests, M11/ownership/escalation denial, duplicate/concurrent idempotency, candidate cross-app denial, audit/state constraints, migration upgrade preservation, no application-file creation and the full regression. See TASK 040 in PROGRESS.md for exact commands and suite totals. Only disposable SQLite fixtures were migrated; configured Registry was not changed.

#### TASK 041 - Administrator review and rejection (completed sub-scope)

- **Implemented:** additive migration `202611040009_database_mode_request_review` preserves existing request/event rows and adds bounded review/rejection event metadata, action idempotency digests and database-enforced transitions. Review events are append-only and linked to the existing audit log. Request metadata records reviewer/time and allowlisted rejection code; it stores no physical path or application database contents.
- **API:** `POST /_platform/api/applications/:id/database-mode-requests/:requestId/review` moves `REQUESTED` to `PENDING_REVIEW`; `POST .../:requestId/reject` moves `PENDING_REVIEW` to `REJECTED`; `GET .../:requestId/history` returns bounded app-scoped event history. Review/reject require an M11 session, app-scoped `application.write`, and an explicit Administrator role check. History requires `application.read`. Review/reject bodies require a bounded idempotency key; rejection accepts only `CANDIDATE_NOT_ACCEPTED`, `POLICY_NOT_SUPPORTED`, or `UNSUPPORTED_CONFIGURATION` and defaults to the latter. Raw keys/free-form rejection text are not stored or returned.
- **State and security boundary:** Existing cancellation behavior remains. All other transitions fail closed. Transactional audit/event/state writes, idempotency replay/conflict handling and SQLite constraints protect concurrent requests. Non-Administrators and cross-application requests are denied. No physical database path, activation/approval state, application database I/O, provisioning, attachment, runtime configuration or source execution was added.
- **Verification:** Focused `test/database-mode-requests.test.mjs` passed **9/9**. API build passed. `npm.cmd run verify` exited 0: **611 passed, 0 failed, 0 skipped**, including 114 Gateway checks, 19 real API/Agent/Gateway routing integration checks, and 40 package checks; owned loopback smoke stopped its processes and removed temporary storage. All .NET projects were published with `UseAppHost=false`, checked for executable artifacts, and run by published DLL. See TASK 041 in PROGRESS.md for suite breakdown and exact commands.
- **Remaining:** M10-DB stays IN PROGRESS. DB-W0 is unresolved; no lifecycle, application database storage/runtime association, backup/restore, replacement/deletion, or Dashboard database controls are implemented. M7 remains IN PROGRESS with Source Build disabled; D00-D29 remain PENDING. The configured Registry was not migrated.

#### TASK 042 - DB-W0 decision finalization and readiness (documentation result)

- **Decision status:** W0-12 is the single fully approved product decision. TASK 023's safe first-use policy and explicit, authorized replacement with recovery safeguards are preserved. W0-10 is partially decided: import mode, provenance and authorized location/ownership remain pending. W0-01–W0-11, W0-13 and W0-14 contain unresolved decisions; see the grouped exact confirmations in PROJECT_REQUIREMENTS.md section 9.1.19 and readiness summary in 9.1.22. D00–D29 remain separately PENDING.
- **Readiness:** Per-app storage, provisioning, existing-database association, runtime configuration, backup/restore and Dashboard lifecycle management are all NOT READY. The existing path resolver is prospective only. My Work Center remains NOT COMPATIBLE AS-IS. KEEP is an approved policy, but its implementation and explicit replacement workflow are not ready.
- **Scope/evidence:** Documentation only. No source, schema, migration, database, runtime path, application or security setting changed; no application build/test or database operation was run. M7 and M10-DB remain IN PROGRESS; Source Build remains disabled.
- **Next:** TASK 043 should collect traceable product-owner answers to the grouped pending confirmations, preserve the existing approved requirements, and update only the decision statuses/readiness gate. Do not begin lifecycle implementation until the relevant answers, technical/security controls and a separately authorized implementation scope are available.
- **Remaining:** M10-DB remains **IN PROGRESS**. DB-W0 product decisions and D00-D29 are unchanged; no provisioning, attachment, runtime path/configuration, migration execution, backup/restore, replacement/deletion, application migration or Dashboard database lifecycle UI is implemented. Continue only under a separately authorized bounded task.

#### DB-W1 — Reviewed Registry metadata, migration and canonical storage resolver

- **Scope:** TASK 036 adds Phase 1 candidate inventory metadata and read APIs. TASK 037 adds a prospective validation-only resolver. Remaining DB-W1 work is explicit versioned metadata for database modes/associations, revisions and lifecycle operations, authorization and operation-level resolver integration. The resolver does not choose a default area/path or resolve DB-W0 compatibility decisions; no runtime use is permitted until those decisions are settled. Keep backups/workspace outside releases/public trees.
- **Dependencies:** DB-W0 decisions; M4 explicit migration contract; M11 ownership and authorization.
- **Likely files:** `src/Api/prisma/schema.prisma`, reviewed additive migration directory, `src/Api/src` database modules, `src/Api/test`, migration/architecture docs.
- **Acceptance:** Fresh and upgrade migrations work only through the reviewed explicit migration command; resolver rejects traversal, links/reparse points, aliases and foreign targets; NONE has no file; metadata contains no DB contents/secrets; pre-existing storage is preserved. TASK 037's resolver slice passes its focused acceptance, but DB-W1 as a whole remains in progress.
- **Tests:** Owned SQLite fixtures for migrations, uniqueness/ownership, safe relative mappings, traversal/reparse/alias/Registry denial, idempotent read state and fail-closed unknown identity. TASK 037 specifically passed 10 disposable path-security tests and the 159-check API suite.
- **Security:** Never auto-migrate on API startup or run uploaded SQL. No Registry path/config is passed to applications.

**TASK 037 completed sub-scope:** `src/Api/src/application-database-storage.ts` validates a prospective path under `<IHP_STORAGE_ROOT>/Applications/<UUID>/{workspace|data}/<relative SQLite path>`. Area selection is explicit. It checks profile/repository bounds, Registry separation, canonical UUID/path rules, supported SQLite extensions, existing directory/file types, symlinks/junctions and known hard-link aliases. It creates no directories, opens no database, exposes no API, and has no runtime caller. Ten focused tests verify these boundaries and byte preservation. DB-W0 remains open; M10-DB and DB-W1 remain IN PROGRESS.

#### DB-W2 — Separate database candidate intake and review-only validation state

- **Scope:** Existing TASK 024 Folder/ZIP intake detects and privately stages review-only SQLite candidates separately from code. TASK 036 persists their inventory metadata. Any future standalone data-artifact intake beyond those existing paths must remain authenticated/bounded, preserve normalized app-relative paths and multiple candidate identities, and create no active DB association or code-fingerprint change.
- **Dependencies:** DB-W0/DB-W1; M7 intake/session/approval contracts; M11 auth/ownership; approved candidate/WAL bundle policy.
- **Likely files:** `src/Api/src` new database-import module/controller, `src/Api/test`, `src/Dashboard/src/api.ts` only when its secured workflow is separately in scope, package/API docs.
- **Acceptance:** `.db`/sidecar artifacts enter only the dedicated reviewed path; path collisions/traversal and malformed/mismatched bundles reject; limits and cleanup hold; status is REVIEW_ONLY; retry is idempotent; no package fingerprint or code approval changes; no active DB is replaced.
- **Tests:** Node HTTP tests with temporary files: multiple DBs, duplicate names/paths, malicious relative paths, unsupported format, size/count limits, `.db` alone during WAL state, matching/mismatched WAL/SHM, upload interruption and cleanup; confirm no code/script sentinel executes.
- **Security:** Do not widen the M7 release allowlist, log file contents or expose local paths; uploads are data, never executable commands.

#### DB-W3 — Approved SQLite snapshot, read-only integrity and backup provider

- **Scope:** Review policy-compatible in-process SQLite API availability. Implement bounded read-only integrity checks and SQLite-consistent Online Backup/snapshot behavior only after provider and source-quiescence decisions. Keep WAL committed state associated with its database; treat SHM as transient coordination state. For an app that applies migrations at startup, statically hash its release migration files and compare the existing DB migration ledger read-only; launch is allowed only if every required migration is already recorded with the exact hash. Do not execute pending uploaded SQL.
- **Dependencies:** DB-W0 policy/provider decision; DB-W1 resolver; DB-W2 candidate model.
- **Likely files:** `src/Api/src` or approved managed helper project, API dependency/config, `src/Api/test` or published managed verifier, `DEPLOYMENT.md`/`SECURITY.md`.
- **Acceptance:** Valid/invalid owned fixtures return accurate integrity states; the provider never opens writable candidates, loads extensions, runs package SQL or executes app code; snapshot is consistent under the specified supported writer/quiescence model; backup is new/non-overwriting/outside release and verifiable; resource budget aborts safely. A missing database or incomplete/changed migration ledger fails closed before any app startup that could create a DB or run a migration.
- **Tests:** Corruption/truncation, integrity output, WAL committed data, sidecar mismatch, concurrent writer/quiescence, interruption, disk/size/time limits and backup restoration using owned disposable SQLite fixtures only.
- **Security:** No native executable or prohibited `.exe`; no external DB engine. If approved consistent backup cannot be delivered with permitted dependencies, retain the feature blocked rather than raw-copying an active DB.

#### DB-W4 — Persistent workspace materialization and M5 runtime integration

- **Scope:** Materialize only authorized data at the reviewed relative path below each app workspace, preserve immutable release artifacts, and add explicit app-specific path bindings. For My Work Center, evaluate only its existing `WORK_CENTER_DB_PATH` override as a fixed validated absolute workspace target while launching the exact reviewed entry point from release; never expose arbitrary environment injection. Do not assume CWD changes module-root paths and do not copy mutable executable trees into workspace.
- **Dependencies:** DB-W0 compatibility matrix; DB-W1 storage resolver; DB-W2 import; DB-W3 snapshot where required; M5 runtime and M7 release approval.
- **Likely files:** `src/DeploymentAgent/RuntimeEngine.cs`, Agent options/metadata/recovery modules, `src/DeploymentAgent/ReleaseInspector.cs`, `src/DeploymentAgent.Tests`, `src/Api/src` application mapping contract.
- **Acceptance:** Compatible fixture resolves its DB to the identical app-relative workspace suffix across v1/v2/restart; approved entry point and code-release fingerprint are unchanged; no DB/sidecar/backup resides in releases; NONE works; module-relative/absolute/unsupported cases are explicitly blocked unless the app has an approved binding; host/port remain 127.0.0.1. For My Work Center, a locked/live data file is never copied, and process health/base-path blockers are not bypassed to claim compatibility.
- **Tests:** Published managed Agent verifier, DLL-only; owned Node and ASP.NET fixtures for CWD-relative paths, explicit named env bindings, `__dirname`/assembly-relative negative cases, v1/v2 same data, release hash, restart/recovery, no data in release, loopback and shutdown. My Work Center fixture profile must also verify absent DB/migration mismatch fail-closed and must not expose `/api/backup.db` through Gateway.
- **Security:** Verify exact release approval/hash before launch; workspace content is never a new entrypoint; same-user file access is not a sandbox. Do not execute uploaded scripts/builds.

#### DB-W5 — Keep-default upgrade, explicit replacement and recoverable database operations

- **Scope:** Keep is implicit/default and never promotes incoming DB data. Implement separately authorized selection/replacement with app stop/writer coordination, backup-before-change, target revision checks, locks, journaled staging, atomic/recoverable promotion, audit and restart coordination. Treat declared related DB groups as one operation.
- **Dependencies:** DB-W1 metadata/journal, DB-W3 provider, DB-W4 workspace and M10 release lifecycle; owner decision on approval/consistency groups.
- **Likely files:** API database operation modules, Agent controlled stop/status contract, SQLite metadata migration, API/Agent managed tests and operational docs.
- **Acceptance:** Default update preserves existing DB IDs/content/path; incoming DBs do not replace unless explicit request and authority; replacement confirms exact target and verified backup; failure/interruption leaves a recoverable old or staged state; group operation does not report partial success; unrelated DBs/Registry cannot be targeted.
- **Tests:** First deploy with existing data, v1→v2 default KEEP, explicit single/multi replacement, same-name collision, stale revision, owner/viewer/anonymous denial, concurrent requests, stop failure, backup failure, interrupted promotion/recovery, audit redaction.
- **Security:** M11 authorization server-side and rechecked at commit; Viewer read-only; backup/restore/delete each explicitly authorized; destructive replacement separately confirmed; no auto SQL migration.

#### DB-W6 — Scoped API, RBAC, audit and lifecycle metadata

- **Scope:** Provide application-scoped safe reads and explicit mode/register/provision/backup/restore/retention/delete operations with ownership checks, idempotency, operation states and credential-safe audit. No arbitrary filesystem input or ordinary raw-file download.
- **Dependencies:** DB-W1 through DB-W5; M11 API and audit contract; explicit policy for destructive and external-existing file operations.
- **Likely files:** `src/Api/src` controllers/services/validation/authorization, API OpenAPI, `src/Api/test`, `API.md`.
- **Acceptance:** API enforces M11 RBAC/current owner even if UI is bypassed; Viewer reads only safe metadata; provisioning cannot overwrite; EXISTING_SQLITE registration alone grants no write/delete; failure and denials audited without secrets/paths/content; external engines never connected.
- **Tests:** Auth/session expiry, Administrator/Developer/Viewer, ownership, escalation, reference collision, audit redaction, idempotency, fail-closed provider errors, OpenAPI schema.
- **Security:** No cross-app access, Registry alias, raw DB response, secret/path leakage, auto migration or automatic destructive cleanup.

#### DB-W7 — Secured Dashboard database workflow

- **Scope:** After secure M8 browser acceptance, display modes, candidate and active state, per-file paths/actions, backup status/history, usage/retention and explicit authorized lifecycle controls.
- **Dependencies:** DB-W6 API; completed/verified M8 browser boundary; M11 permissions; M7 review and M10 release data.
- **Likely files:** `src/Dashboard/src/app.tsx`, `api.ts`, models/components/styles and Dashboard/browser tests; `DASHBOARD.md`.
- **Acceptance:** Database-free app works; KEEP is selected by default; replacement requires clear per-database selection/confirmation and server approval; role/owner UI mirrors but does not replace server authorization; loading/error/empty/session-expiry states work; no raw file/content/path/secret is exposed.
- **Tests:** DOM/API contract tests plus permitted actual-browser acceptance for auth, CSRF, origin, owner/admin/viewer, replacement confirm, session expiry and accessibility. Simulation is not actual-browser evidence.
- **Security:** Keep operations disabled if browser security acceptance or API authority is incomplete. Never use a generic API proxy or browser token storage.

#### DB-W8 — End-to-end persistence, backup/recovery and operations acceptance

- **Scope:** Verify the full SQLite-only lifecycle across approved release update, runtime restart, failure recovery and compatible code rollback; document limits/runbooks and audit outcomes.
- **Dependencies:** DB-W0 through DB-W7 and M10 release/rollback acceptance; owned test fixtures.
- **Likely files:** API/Agent/Dashboard/managed integration tests, `DEPLOYMENT.md`, `SECURITY.md`, `API.md`, `TROUBLESHOOTING.md`, four tracking records.
- **Acceptance:** All section 9.1 acceptance rows have command/evidence; data survives v1/v2/v3 update/restart/compatible rollback; incompatible release is safely blocked; backup/restore is consistent and recoverable; tests/retention/cleanup and full regression pass; no policy violation; all failures and same-user limits documented.
- **Tests:** Focused API/SQLite/Agent/Dashboard/integration suites, published `.NET` verifier DLLs, full `npm.cmd run verify`, documented actual-browser gate; use disposable SQLite only and do not touch the configured Registry or existing apps.
- **Security:** No `.exe`, Docker, admin, security/network changes, external DB engines, unapproved source builds, untrusted execution or production access.

**Dependencies:** M4 NestJS/Prisma/SQLite Platform API and explicit migration boundary; M11 sessions/RBAC/application ownership/audits; M5 trusted runtime/configuration; M7 verified trusted-prebuilt release contracts (source builds remain disabled); completed and security-verified M8 browser access/Dashboard operations for UI acceptance; and M10 release lifecycle/rollback coordination for data-persistence acceptance. No dependency grants permission to start unfinished milestones. M7 remains IN PROGRESS until its separate source-build/isolation acceptance is resolved.

**Acceptance criteria (future evidence required):**

| Area                         | Required verification before completion                                                                                                                                                                                                                                                                                                                                                                                             |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Optional modes               | NONE deploys without a database file/association/configuration; MANAGED_SQLITE provisions a dedicated file; EXISTING_SQLITE registers only explicitly authorized, validated existing SQLite files without overwriting/initializing them. Mode changes preserve existing data and enforce ownership.                                                                                                                                 |
| Provisioning and association | Concurrent/repeated provisioning creates or returns exactly one authorized association/file for one application, never truncates an existing database and rejects unrelated files, duplicate ownership and Registry aliases. Failed/interrupted provisioning is recoverable and reported accurately.                                                                                                                                |
| Storage boundaries           | Reject traversal, absolute/UNC/device/stream injection, symlink/junction/reparse escapes, aliases and cross-application substitution through management APIs. Databases/sidecars/backups are outside releases/public trees; Registry access/configuration is never handed to apps. Existing paths remain compatible unless an explicitly authorized migration is verified. Document that same-identity files are not an OS sandbox. |
| Relative-path workspace      | Preserve each supported application's declared database-relative path under its stable app workspace across releases; launch reviewed entry code from the approved release only. Verify CWD-relative support and explicitly reject module/assembly-relative, absolute or other unsupported path cases unless separately approved; do not rewrite app code/config or copy mutable executable trees.                                  |
| Database candidate boundary  | Detect/import multiple explicitly selected SQLite candidates via a separate bounded review-only path; do not treat `.db` extension as proof, widen release allowlists, execute SQL, merge databases or include data in the code-release fingerprint. Handle DB/WAL consistently and never promote SHM as database content.                                                                                                          |
| Lifecycle and limits         | Verify explicit initialization, safe metadata/status/integrity/usage, documented configurable storage limits, retention, authorized deletion and interrupted-operation recovery. Application deletion, mode changes and release cleanup never implicitly destroy data; EXISTING_SQLITE registration alone grants no file-deletion right. No silent empty-file replacement on corruption.                                            |
| Backup                       | Explicitly authorized approved SQLite-consistent mechanism produces a validated snapshot under coordinated/concurrent writes, including relevant journal/WAL behavior. Fresh backups are application-scoped, non-overwriting, outside releases, safely recorded and recoverable on failure; no active-file-copy assumption.                                                                                                         |
| Restore                      | Enforce explicit authorization/confirmation, backup provenance/integrity/compatibility and controlled downtime/writer coordination. Restore only the intended application's database, preserve a recovery path, reject unrelated/Registry/out-of-root targets and validate post-restore integrity/health before success.                                                                                                            |
| Deployment persistence       | Write owned fixture data on v1 and verify the same database/data remains through v2/v3 deployment, restart, compatible rollback and release cleanup. Incompatible schema/release combinations fail safely; app rollback never automatically reverses schema or restores data. Uploaded SQL remains inert.                                                                                                                           |
| Upgrade and replacement      | Verify KEEP is the default and preserves database identity/content/path on update; a selected replacement requires server authorization/confirmation, stopped writers, verified consistent non-overwriting backup, target-revision validation and recoverable promotion. Multiple declared related databases are handled as a consistency group; failures do not report partial success.                                            |
| Authorization                | Anonymous/expired/revoked sessions, unauthorized owners, Viewers attempting writes and privilege escalation all fail closed. Backup, restore and deletion each require explicit authorization; destructive actions also require confirmation. No credentials, internal capabilities, database contents or paths leak through Gateway/browser/logs.                                                                                  |
| Dashboard integration        | Verify real authorized mode configuration/provisioning/status/usage/backup/restore/history/retention/deletion workflows, role-aware controls and failure states under the approved M8 browser boundary. Viewer is read-only; UI never replaces server enforcement; no ordinary raw SQLite download endpoint.                                                                                                                        |
| Audit and SQLite policy      | Verify safe actors/resource IDs/actions/times/outcomes for mode/association/lifecycle/retention/recovery/denied operations, including failures. Only SQLite performs real operations; SQL Server/PostgreSQL/MySQL remain metadata-only with no connection/authentication/migration/provisioning/connectivity test. Full applicable regression passes with no forbidden executables, elevation or security/network changes.          |

**Verification commands (future extensions to existing suites; NOT RUN for this documentation task):**

```powershell
# Run only after separately authorized implementation adds the database cases.
npm.cmd --prefix .\src\Api run test
npm.cmd --prefix .\src\Dashboard run test
dotnet publish .\tests\Platform.Integration.Tests\Platform.Integration.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false -p:UseSharedCompilation=false -p:PublishDir=../../artifacts/publish/Platform.Integration.Tests/
dotnet .\artifacts\publish\Platform.Integration.Tests\Platform.Integration.Tests.dll
npm.cmd run verify
```

Before future execution, align actual scripts/fixtures with the implementation, review any explicit migrations separately and record commands/results. Real-browser acceptance requires permitted actual-browser evidence; DOM simulations alone do not satisfy it. No database file or backup should enter the progress repository.

**Status:** IN PROGRESS (Phase 1 candidate metadata only)

**Unresolved future design choices:** See PROJECT_REQUIREMENTS.md section 9.1.14 for path/CWD compatibility, default versus workspace mapping, imported mode/provenance, candidate and WAL policy, replacement approval, consistency groups, SQLite provider/limits and same-user isolation decisions. Exact safe API/schema contracts, backup layout/default retention/limits and recovery coordination require review and verification. Nothing in this record authorizes new utilities, runtime permissions, OS isolation, automatic migrations or untrusted/source execution. Existing M7/M8 gaps and no-.exe/loopback/SQLite-only restrictions remain unchanged.

### M11 — Authentication and Authorization

**Objective:** Enforce authenticated local API sessions, roles, application ownership/read grants and metadata-bound deployment approvals before M7 self-service work.

**Priority/dependencies:** Explicitly pulled forward after M1–M6 by the user. M4 identity/schema and M5/M6 contracts are present. Browser session UI remains M8; upload/build/deployment and rollback execution remain disabled. This completion does not authorize M7 or another milestone.

**Tasks — implemented and verified:**

- Node built-in full-cost salted scrypt, safe one-time capability-protected Administrator bootstrap and persistent account/global login budgets. No native executable/addon/authentication service dependency.
- Hashed opaque server-side sessions with independent client binding, User-Agent matching, idle/absolute expiry, max-session eviction, login rotation, logout and self/admin/permission-change revocation across API restarts.
- Administrator/Developer/Viewer enforcement, SQL-scoped application lists/reads, creator ownership, read-only grants, Administrator ownership transfer and privileged reference management. Prevent caller identity/role/owner escalation and protect the last active Administrator.
- Request-local serialized authorization transactions: current session/role/ownership and protected SQL/audits commit together; nested Registry/Routing joins preserve prior behavior and audit failure rolls back mutation.
- Reviewed-release approval request/review/rejection/revocation/history with requester/reviewer/time, metadata/owner fingerprint, distinct reviewer and current permission checks. Valid approval never activates an execution endpoint.
- Persistent credential-safe security audits, safe user/session views and OpenAPI descriptions. Gateway service reader and trusted M5 operator boundary remain separate.
- Real auth HTTP/SQLite tests, managed production API/Gateway/Agent integration, full M1–M6 regression and local AUTHENTICATION.md covering matrix, operation and explicit transport/recovery limits.

**Acceptance evidence (2026-10-27):**

| Criterion                                          | Actual result                                                                                                                                                                                                 |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Anonymous/invalid sessions rejected; expiry/logout | PASS: missing/forged/cookie/query/fixation/invalid-bound/expired/revoked/replayed sessions, Origin/Host/cross-site checks, idle/absolute limits and persistent restart state                                  |
| Roles and ownership for all roles                  | PASS: Administrator management, owned Developer writes, Viewer/read-only grants, scoped search/count/read and cross-app restrictions; role/owner input escalation and last-admin denial                       |
| Password/session secrecy                           | PASS: actual full-cost salted scrypt and verification, raw credentials absent from logs/audits/SQLite, safe views and once-only no-store token response                                                       |
| Approval and audit identity/status/time            | PASS: real request/review/rejection/revocation, self-review/double decision/stale metadata/role-owner invalidation; audit rollback and safe actor/resource records                                            |
| Existing architecture and no new execution         | PASS: initialized user sessions manage live owned routes through unchanged published Gateway, Viewer cannot write, real reviewed-release approval returns executionEnabled=false                              |
| Future identity provider boundary                  | Local verification/session issuance is separate from role/ownership/audit policy; no unverified external JWT/header is trusted. Entra ID is a future mapping/verifier, not an installed/configured dependency |

**Verification commands (implemented):**

```powershell
npm.cmd run test:auth
npm.cmd run test:routing
npm.cmd run verify
npm.cmd run doctor
npm.cmd audit --json
```

The dedicated auth command builds through guarded Prisma WASM generation and runs real HTTP/SQLite Node tests. Managed integration is published with UseAppHost=false and run through dotnet DLLs; no VSTest/testhost. Frontend auth tests are not claimed; M8 is unimplemented.

**Status:** COMPLETED

**Final gate:** npm.cmd run verify exited 0 with **381 passing checks**, zero failures/skips: 25 Node foundation/sample, **81 API/reference/routing/auth (including 37 auth)**, 13 guarded SQLite, 27 Foundation, 108 Gateway, 82 Samples, 27 Agent and **18 managed integration**. All ten no-.exe publish scans, build/type/format/solution checks and real platform smoke passed. Doctor passed; npm audit found zero vulnerabilities; Administrator=False. Detailed current/intermediate evidence is in PROGRESS.md, local docs/verification/M11-2026-10-27.md and artifacts/m11-verify.log.

**Schema:** Appended reviewed 202610270003_auth migration; 18 models/three migrations. Session/throttle/grant tables plus safe audit/approval fields and constraints. Earlier migrations and actual existing applications/registry/users were not changed by tests. Explicit local backup/review/migration is an operational prerequisite; contradictory legacy approval data fails closed rather than being deleted.

**Security scope/limits:** Native local API clients only, existing unencrypted loopback HTTP; no cookie/browser-login UI, TLS, MFA, SSO, device proof or same-user isolation claim. Theft of both secrets/client context and local DB/source tampering remain risks. M5 trusted local operator and hosted apps' own content auth remain separate. No recovery bypass; audit retention/tamper resistance and real backup/recovery remain future work. Legacy maintenance closes after any user exists; M6 reader remains read-only. Actual upload/deployment/process/rollback/application-deletion execution stays absent/disabled even with a valid approval.

**M11 historical handoff:** M11 is complete. M7 subsequently integrated these ownership/approval gates under its trusted-prebuilt policy. The latest request now assigns M8 while preserving M7 IN PROGRESS. M9–M10 and M12 remain NOT STARTED; no further milestone is assigned.

### M12 — Security Hardening and Final Testing

**Objective:** Verify end-to-end acceptance, failure recovery, security controls, operational procedures, and documented limitations before any separately authorized network deployment.

**Tasks:**

- Run functional, failure, security, compatibility, and regression suites from `PROJECT_REQUIREMENTS.md`.
- Test ZIP traversal, reserved routes, arbitrary destinations, unauthorized access, secret leakage, deployment interruption, port conflict, crashes, restart recovery, and insufficient storage behavior where safely reproducible.
- Verify compatibility for React + Node API, React + ASP.NET Core API, ASP.NET Core MVC/Razor, Node.js prebuilt, and ASP.NET Core published apps under URL prefixes.
- Complete backup/restore, recovery, retention, local startup/shutdown, troubleshooting, user guide, and security limitation documentation.
- Deliver the required README, architecture, installation, deployment, security, API, troubleshooting, and user guide documentation.
- Confirm no Docker, Administrator-only operation, system policy changes, existing-app modifications, or direct corporate exposure of runtime ports.
- Review all 20 acceptance criteria and record evidence/gaps. Network exposure/deployment remains a separate explicit authorization gate.

**Dependencies:** M1–M11. Any acceptance criterion that cannot be implemented safely under the available Windows permissions must remain an explicit limitation/blocker with rationale; do not weaken controls to pass a test.

**Acceptance criteria:**

- All applicable automated tests pass and required manual checks are recorded with actual results; skipped tests have a reason and owner/next step.
- All acceptance criteria AC-01 through AC-20 are evidenced or explicitly marked unresolved with rationale; do not call the product complete while required criteria remain unresolved.
- Backup/restore and recovery procedures are exercised locally.
- Security, IT constraints, process-isolation limitations, and startup-after-logout/reboot dependencies are documented accurately.
- The platform remains localhost-only unless a later explicit authorization enables the approved listener.

**Verification commands (planned):**

```powershell
dotnet publish .\tests\Gateway.Tests\Gateway.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false
dotnet .\tests\Gateway.Tests\bin\Release\net8.0\publish\Gateway.Tests.dll
dotnet publish .\tests\DeploymentAgent.Tests\DeploymentAgent.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false
dotnet .\tests\DeploymentAgent.Tests\bin\Release\net8.0\publish\DeploymentAgent.Tests.dll
dotnet publish .\tests\Platform.Integration.Tests\Platform.Integration.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false
dotnet .\tests\Platform.Integration.Tests\bin\Release\net8.0\publish\Platform.Integration.Tests.dll
npm.cmd --prefix .\src\Api run test
npm.cmd --prefix .\src\Dashboard run test
npm.cmd --prefix .\src\Dashboard run build
```

Run manual compatibility/failure/security checks from requirements sections 19 and 20. Save actual commands, exit codes, and results in `PROGRESS.md` and the appropriate test documentation.

**Status:** NOT STARTED

## Requirement coverage

| Requirement area                                                                                    | Planned milestone(s)                                                      |
| --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| Foundation, architecture, local development                                                         | M1                                                                        |
| YARP gateway and health                                                                             | M2                                                                        |
| Node.js and ASP.NET Core samples                                                                    | M3                                                                        |
| Platform API, SQLite, Prisma, schema/migrations                                                     | M4                                                                        |
| Process/runtime deployment engine and recovery                                                      | M5                                                                        |
| Registry-backed runtime routing and subpaths                                                        | M6                                                                        |
| ZIP validation, prebuilt packages, approved builds                                                  | M7                                                                        |
| React dashboard and deployment experience                                                           | M8                                                                        |
| Metrics, health, structured logs, retention                                                         | M9                                                                        |
| Releases, history, safe updates, rollback                                                           | M10                                                                       |
| Optional independent application SQLite databases, lifecycle, backup/restore and database Dashboard | M10-DB (candidate metadata/path validation in progress; lifecycle future) |
| Login, roles, ownership, approvals, audit                                                           | M11                                                                       |
| Security, resilience, compatibility, docs, final acceptance                                         | M12                                                                       |

M1–M6 and M11 are `COMPLETED`; SQLite-only policy/accepted gap remain. M7 and M8 are `IN PROGRESS` with source builds and approved browser authentication unavailable respectively. M9–M10 and M12 remain `NOT STARTED`; do not begin them. All .NET checks use published managed DLLs; uncreated later-milestone commands remain planned. Change status only with matching evidence and policy.

M10-DB is **IN PROGRESS for TASK 036 candidate metadata, TASK 037 prospective path validation, and TASK 040 inert request metadata**; its broader lifecycle acceptance remains incomplete. DB-W0 product-owner decisions are pending as recorded in sections 9.1.18-9.1.19. Provisioning, runtime attachment, application database backup/restore and Dashboard lifecycle controls are not implemented. Final M12 review must account for this extension without treating these partial slices as lifecycle acceptance.
