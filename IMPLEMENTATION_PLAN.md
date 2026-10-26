# Implementation Plan

## Project

Internal Application Hosting & Deployment Platform (project code: Internal Hosting Platform / IHP). The complete, canonical requirements are in [PROJECT_REQUIREMENTS.md](PROJECT_REQUIREMENTS.md).

This roadmap translates those requirements into independently verifiable milestones. It does not claim that planned functionality already exists.

## Status rules

- `NOT STARTED`: work has not begun.
- `IN PROGRESS`: work is underway and acceptance criteria remain.
- `COMPLETED`: every acceptance criterion for the milestone has been verified, with actual evidence recorded in `PROGRESS.md`.
- `BLOCKED`: work cannot proceed because a documented external decision, permission, or dependency is unavailable. Record the reason and the unblock condition in `PROGRESS.md`.

Do not mark a milestone complete based on code being written alone. Verification must match the acceptance criteria and run in the approved local environment. A test that was not run is not a pass.

## Repository layout

M1 created the four component shells and shared foundations shown below. Samples and the later milestone test projects remain planned. Keep future verification commands aligned with actual paths as those projects are added.

```text
src/Gateway/                 ASP.NET Core 8 + YARP
src/Api/                     NestJS + Prisma + SQLite
src/DeploymentAgent/         .NET 8 Worker
src/Dashboard/               React + TypeScript + Vite
src/Foundation/              shared .NET host configuration
packages/config/             shared Node configuration
packages/contracts/          TypeScript contracts
tools/                       local development lifecycle and checks
tests/Foundation.Tests/      published managed Foundation verification program
tests/foundation/            Node foundation tests
samples/DemoNode/            planned M3
samples/DemoDotnet/          planned M3
tests/Gateway.Tests/         managed YARP HTTP/WebSocket verification
tests/DeploymentAgent.Tests/ planned M5
tests/Platform.Integration.Tests/ planned integration suite
```

M1 commands below refer to existing artifacts. Commands for later milestones remain planned and require their future projects/tests. Use `npm.cmd` from PowerShell.

## Cross-milestone constraints and gates

- The Gateway is fixed to `http://127.0.0.1:5050` for the current authorized scope; alternate ports/hosts are rejected at startup. Do not enable the corporate-network listener or deploy to the corporate network without local verification and explicit authorization.
- Port 5050 is the only intended network-facing entry point. Application processes must bind to loopback, using an internal port in the planned `6100–6999` range. Do not expose runtime ports to the corporate network.
- Do not use Docker or require Administrator privileges for normal operation. Do not alter firewall rules, execution policy, endpoint protection, registry settings, or other IT controls.
- Do not run .exe artifacts. All .NET applications and verification programs must use framework-dependent dotnet publish with UseAppHost=false, followed by dotnet <published DLL>. Do not use VSTest/testhost, native apphosts, or dotnet run. Publish outputs must be checked for forbidden executable artifacts.
- Treat uploads as untrusted. Do not execute arbitrary uploaded code or manifest commands without the approved deployment policy and security boundary. Where available permissions cannot provide adequate isolation, restrict deployment to reviewed, trusted applications and record the limitation.
- Do not modify existing applications. Use only platform-owned storage and sample applications.
- Keep secrets out of browser responses and logs. Validate routes and destinations; proxy only to approved loopback destinations.
- Use bounded restart attempts and backoff. Do not assume unattended startup after logout or reboot without IT approval.
- Authentication and deployment authorization must fail closed. Before their milestone is complete, any development-only management access must remain local and must not enable untrusted or network-accessible deployment.

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

**Verification commands (planned):**

```powershell
npm.cmd --prefix .\samples\DemoNode install
npm.cmd --prefix .\samples\DemoNode test
dotnet publish .\samples\DemoDotnet\DemoDotnet.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false
dotnet .\samples\DemoDotnet\bin\Release\net8.0\publish\DemoDotnet.Tests.dll
dotnet publish .\samples\DemoDotnet\DemoDotnet.csproj -c Release --no-self-contained -p:UseAppHost=false
dotnet .\samples\DemoDotnet\bin\Release\net8.0\publish\DemoDotnet.dll
Invoke-WebRequest http://127.0.0.1:6101/health
Invoke-WebRequest http://127.0.0.1:6102/health
```

Start the Node.js and ASP.NET Core samples in separate terminals with their configured ports before running the two HTTP checks. Record observed output and exit codes.

**Status:** NOT STARTED

### M4 — Platform API and Database

**Objective:** Implement the NestJS management API and persistent SQLite registry using Prisma migrations.

**Tasks:**

- Verify the selected Prisma toolchain is compatible with the no-.exe policy before use; do not execute native engine artifacts. Define policy-compatible package scripts for schema validation/migrations.
- Define normalized Prisma models for users, applications, deployments, runtime instances, routes, database connections, deployment logs, audit logs, application secrets, and approvals.
- Add versioned migrations, constraints, indexes, foreign keys, and unique application slugs.
- Implement validated application registry and read APIs, pagination/filtering foundations, consistent errors, request logging, and OpenAPI documentation.
- Implement authorized database-connection reference management, assignment to applications, connection tests, and updates for SQL Server, PostgreSQL, and MySQL configurations; do not install database servers.
- Make database and storage paths configurable and keep database credentials out of API responses and logs.
- Keep destructive operations and deployment endpoints protected or disabled until authorization is available.

**Dependencies:** M1. M2 may proceed in parallel; M4 does not depend on gateway implementation.

**Acceptance criteria:**

- Prisma validates the schema and migrations create a fresh local SQLite database.
- Application CRUD validation rejects duplicate slugs and reserved paths.
- Database connection references can be registered, assigned, updated, and connectivity-tested without returning credentials in API responses or logs.
- API responses and logs do not contain plaintext secrets.
- API validation, error format, and OpenAPI documentation are covered by tests.
- No deployment operation is anonymously accessible.

**Verification commands (planned):**

```powershell
npm.cmd --prefix .\src\Api run test
npm.cmd --prefix .\src\Api run prisma:validate
npm.cmd --prefix .\src\Api run migration:test
npm.cmd --prefix .\src\Api run build
```

**Status:** NOT STARTED

### M5 — Deployment Engine

**Objective:** Build a controlled .NET 8 process and runtime manager for approved application releases.

**Tasks:**

- Implement framework/runtime detection and runtime availability checks for Node.js and ASP.NET Core.
- Implement persistent internal port allocation in the configured range, including occupied-port detection, concurrency control, restart recovery, and the select-to-bind race.
- Implement process start, stop, restart, status, exit tracking, log capture, and health checks for separate application processes.
- Bind child processes to loopback and prevent orphaned child processes as far as supported by the approved Windows execution model.
- Persist desired state and reconcile actual processes on Agent restart; use bounded restart attempts and backoff.
- Document process-isolation limits and the approved trust/approval boundary; do not imply that separate processes provide strong isolation.

**Dependencies:** M1, M3, and M4's registry contracts. M11 authorization is required before deployment is enabled for ordinary users.

**Acceptance criteria:**

- Unit/integration tests cover process lifecycle, health checks, occupied ports, concurrent allocation, failed startup cleanup, exit status, and bounded recovery.
- Only loopback runtime binds are allowed and allocated ports are persisted/reconciled.
- Failure logs are retained and associated with application/release identifiers.
- The Agent runs without Administrator privileges and is not directly exposed to the company network.
- Uploaded/unreviewed code is not executed as a side effect of detection or validation.

**Verification commands (planned):**

```powershell
dotnet publish .\tests\DeploymentAgent.Tests\DeploymentAgent.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false
dotnet .\tests\DeploymentAgent.Tests\bin\Release\net8.0\publish\DeploymentAgent.Tests.dll
dotnet publish .\tests\Platform.Integration.Tests\Platform.Integration.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false
dotnet .\tests\Platform.Integration.Tests\bin\Release\net8.0\publish\Platform.Integration.Tests.dll
```

**Status:** NOT STARTED

### M6 — Dynamic Routing

**Objective:** Connect approved application routes to YARP at runtime without restarting the gateway.

**Tasks:**

- Implement a YARP dynamic configuration provider fed by a controlled internal API/configuration-change mechanism.
- Load and validate route changes from the application registry; reject reserved/conflicting paths and non-loopback or unapproved destinations.
- Support route add, update, removal, and deployment replacement while retaining the last known good configuration on invalid updates.
- Verify prefix behavior, redirects/static assets where supported, WebSockets, gateway restart, and route reconciliation.
- Keep the route-provider boundary extensible for future hostname-based routing without making it part of the initial release.
- Provide a route smoke test for both sample applications.

**Dependencies:** M2, M3, M4, and the route/port contract from M5.

**Acceptance criteria:**

- A route can be added, updated, and removed without restarting the gateway.
- Node.js and ASP.NET Core samples are reachable simultaneously through distinct paths on port 5050.
- Invalid paths, reserved paths, conflicts, and arbitrary proxy destinations are rejected.
- A failed update does not replace the last known good route configuration.
- Route state can be reconciled after gateway restart.

**Verification commands (planned):**

```powershell
dotnet publish .\tests\Gateway.Tests\Gateway.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false
dotnet .\tests\Gateway.Tests\bin\Release\net8.0\publish\Gateway.Tests.dll
dotnet publish .\tests\Platform.Integration.Tests\Platform.Integration.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false
dotnet .\tests\Platform.Integration.Tests\bin\Release\net8.0\publish\Platform.Integration.Tests.dll
Invoke-WebRequest http://127.0.0.1:5050/demo-node/
Invoke-WebRequest http://127.0.0.1:5050/demo-dotnet/
```

**Status:** NOT STARTED

### M7 — ZIP Upload and Approved Source Builds

**Objective:** Accept safe application packages and perform only policy-approved preparation/build operations.

**Tasks:**

- Implement upload size/file checks, package/archive integrity checks, archive entry/expanded-size limits, safe extraction, path traversal and link handling protections, ownership, authorization hooks, and temporary cleanup.
- Detect supported Node.js, React, and ASP.NET Core package shapes; validate strict `platform.yaml` and ASP.NET deployment templates.
- Support prebuilt packages and allowlisted build templates for Node.js, React, and ASP.NET Core.
- Invoke npm through `npm.cmd` on Windows. Do not execute manifest commands blindly; require the configured approval policy for custom commands.
- Capture build output and failure state, clean up failed staging areas, and prepare immutable release directories.
- Do not run untrusted builds or application code unless the approved security boundary/policy is in place; restrict to reviewed, trusted packages if adequate isolation is unavailable.

**Dependencies:** M4, M5, M6, M11 authorization/approval gates, and a documented deployment policy. M11 may be implemented before this milestone; until then, deployment remains disabled.

**Acceptance criteria:**

- Tests reject malformed archives, path traversal, reserved/unsafe entries, oversized uploads/expansions, unsupported packages, and unauthorized deployments.
- Prebuilt Node.js and ASP.NET Core packages can be staged and validated without arbitrary script execution.
- Approved source templates build the supported sample applications and retain build logs.
- Failure cleanup does not touch platform source or existing applications.
- Clear validation errors are returned for incompatible packages.

**Verification commands (planned):**

```powershell
npm.cmd --prefix .\src\Api run test
dotnet publish .\tests\DeploymentAgent.Tests\DeploymentAgent.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false
dotnet .\tests\DeploymentAgent.Tests\bin\Release\net8.0\publish\DeploymentAgent.Tests.dll
dotnet publish .\tests\Platform.Integration.Tests\Platform.Integration.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false
dotnet .\tests\Platform.Integration.Tests\bin\Release\net8.0\publish\Platform.Integration.Tests.dll
```

**Status:** NOT STARTED

### M8 — React Dashboard

**Objective:** Provide the responsive management interface for authorized users and deployment workflows.

**Tasks:**

- Build the React + TypeScript + Vite frontend using Tailwind CSS, shadcn/ui, and Lucide Icons as specified.
- Add dashboard statistics/recent activity; searchable, filterable, sortable application list; application detail; new application wizard; deployment progress; monitoring; and settings pages for users, roles, runtime configuration, deployment policies, application limits, and platform configuration.
- Connect to documented API contracts and show asynchronous deployment status without claiming success before health/routing verification.
- Support clear validation/error states, accessible keyboard flows, responsive layouts, and session-aware controls.
- Ensure API URLs and application base paths work under the gateway path prefix.

**Dependencies:** M4 API contracts, M6 routing/static hosting, M7 for real upload/deploy flows, and M11 authentication/session behavior. Page shells may be built earlier against tested API contracts, but M8 is complete only when the full authorized flow is verified.

**Acceptance criteria:**

- Dashboard and required pages build and run locally behind the gateway.
- Lists support search/filter/sort, and detail/deployment views reflect API state.
- Unauthorized users cannot see or invoke deployment controls; errors and progress reflect actual API results.
- Frontend assets, navigation, and API calls work under the configured path prefix.
- Responsive and accessibility checks are recorded.

**Verification commands (planned):**

```powershell
npm.cmd --prefix .\src\Dashboard run test
npm.cmd --prefix .\src\Dashboard run build
dotnet publish .\tests\Platform.Integration.Tests\Platform.Integration.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false
dotnet .\tests\Platform.Integration.Tests\bin\Release\net8.0\publish\Platform.Integration.Tests.dll
```

**Status:** NOT STARTED

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

### M11 — Authentication and Authorization

**Objective:** Enforce authenticated sessions, roles, application ownership, and deployment approvals across management operations.

**Tasks:**

- Implement secure local authentication without plaintext passwords and establish session protection/expiry/logout behavior.
- Add Administrator, Developer, and Viewer role enforcement, application ownership checks, and endpoint authorization guards.
- Enforce deployment and rollback authorization and approval states; audit security-sensitive actions.
- Protect management APIs and ensure the platform does not allow anonymous deployment.
- Keep the design extensible for future Microsoft Entra ID integration without assuming SSO is available.

**Dependencies:** M4 identity schema/API and the approval/security design. M11 may be pulled forward and must be complete before enabling deployments in M7 or rollback in M10; frontend session integration is verified in M8.

**Acceptance criteria:**

- Anonymous management/deployment requests are rejected; session expiry and logout are verified.
- Role and application ownership tests cover allowed and denied actions for all roles.
- Passwords are hashed with a suitable password hashing mechanism; secrets and session tokens are not logged or returned improperly.
- Deployment approval and audit records show requester, approver, status, and review time.
- Entra ID remains an extension point, not a dependency for local operation.

**Verification commands (planned):**

```powershell
npm.cmd --prefix .\src\Api run test
npm.cmd --prefix .\src\Dashboard run test
dotnet publish .\tests\Platform.Integration.Tests\Platform.Integration.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false
dotnet .\tests\Platform.Integration.Tests\bin\Release\net8.0\publish\Platform.Integration.Tests.dll
```

**Status:** NOT STARTED

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

| Requirement area                                            | Planned milestone(s) |
| ----------------------------------------------------------- | -------------------- |
| Foundation, architecture, local development                 | M1                   |
| YARP gateway and health                                     | M2                   |
| Node.js and ASP.NET Core samples                            | M3                   |
| Platform API, SQLite, Prisma, schema/migrations             | M4                   |
| Process/runtime deployment engine and recovery              | M5                   |
| Registry-backed runtime routing and subpaths                | M6                   |
| ZIP validation, prebuilt packages, approved builds          | M7                   |
| React dashboard and deployment experience                   | M8                   |
| Metrics, health, structured logs, retention                 | M9                   |
| Releases, history, safe updates, rollback                   | M10                  |
| Login, roles, ownership, approvals, audit                   | M11                  |
| Security, resilience, compatibility, docs, final acceptance | M12                  |

M1 and M2 are `COMPLETED` with published-DLL verification. M3–M12 remain `NOT STARTED`. Future .NET verification projects must be managed DLL programs; the publish/run commands above remain planned until those projects exist. Change a status only with a matching progress entry and verified evidence.
