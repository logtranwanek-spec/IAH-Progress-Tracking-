# MASTER PROMPT
# INTERNAL APPLICATION HOSTING & DEPLOYMENT PLATFORM

## Current TASK 024 — Unified application and SQLite upload intake

TASK 024 authorizes authenticated folder and ZIP upload/intake that separates supported SQLite artifacts from application code, stages them privately per application, validates them with bounded read-only checks, and displays safe review metadata. Preserve M7 package validation/fingerprinting and Administrator approval. Database bytes must never enter the code-release package or its fingerprint. Existing limits remain in force. SQL remains inert. This scope does not implement DB lifecycle management (M10-DB), replace/restore/provision databases, run migrations or applications, modify My Work Center, alter runtime working directories/environment, or change Gateway routes. M7 remains IN PROGRESS and M10-DB remains NOT STARTED.

## Current TASK 023 — My Work Center SQLite compatibility investigation — 2026-10-29

This assignment is a read-only compatibility investigation and documentation update. Do not implement Managed Application Databases, modify IHP or My Work Center code/configuration, alter runtime directories/allowlists, execute the application, or inspect/modify database contents. The product owner has approved first deployment from supported existing SQLite data and **Keep Existing Database** as the upgrade default, with database replacement only as an explicit user choice subject to authorization, confirmation, consistent backup, safe shutdown, validation and recoverable failure handling. These behaviors remain unimplemented. M7/M8 remain IN PROGRESS and M10-DB remains NOT STARTED. See section 9.1.15 for verified findings and proposed gates.

## Current TASK 022 — Persistent application workspace and SQLite design — 2026-10-29

This assignment is **investigation and documentation only**. It records approved requirements to preserve existing SQLite data and application-relative database paths, and proposes an implementation design for a per-application persistent workspace distinct from approved releases and backups. No API, Dashboard, Agent, schema, migration, package-validation or database behavior is implemented by this document. M7/M8 remain IN PROGRESS; M10-DB remains NOT STARTED. Do not treat design proposals or historical tests as implementation evidence. Wait for a separate explicit implementation assignment before starting M10-DB.

**Project Code:** Internal Hosting Platform (IHP)  
**Project Type:** Internal Developer Platform / Self-Service Application Hosting  
**Target Environment:** Windows 11 Enterprise Environment  
**Development Approach:** Production-oriented, modular architecture  
**Primary Objective:** Self-service deployment of Node.js and ASP.NET Core applications through a centralized hosting platform.

## Current TASK 021 implementation record — 2026-10-29

The default-off Dashboard candidate now provides the M7 folder upload flow through the existing authorized APIs. Keep M7 and M8 IN PROGRESS; actual-browser acceptance remains NOT RUN, and private production browser management stays disabled by default. Source builds remain disabled and M10-DB remains NOT STARTED. Section 5.4 distinguishes this implemented, automated-test-verified candidate from remaining acceptance; the normative security and workflow requirements below remain in force.

## Current M8 continuation authorization — 2026-10-28

The latest user resumes M8 implementation after the documentation task: resolve and verify the recorded API startup/readiness failure, assess and secure the opt-in browser authentication candidate, integrate only authorized real Dashboard operations and run focused/full regression. Preserve M11 protections, approved trusted-prebuilt execution gates and all company restrictions. Actual-browser acceptance requires permitted real-browser evidence; keep private production operations disabled and M8 IN PROGRESS if that gate or another required criterion remains unmet. M7 remains IN PROGRESS; M9/M10/M10-DB/M12 are not assigned. The Managed Application Database requirements in section 9.1 are unchanged future work. This supersedes the prior documentation-only scope for the current task, not any security restriction.

## Current documentation-only directive — 2026-10-27

Define the future Managed Application Database architecture in section 9.1: **1 Platform Database + N Independent Application Databases**, with application databases optional. This assignment authorizes changes only to PROJECT_REQUIREMENTS.md, AGENTS.md, IMPLEMENTATION_PLAN.md and PROGRESS.md; it does not authorize implementation, schema changes, migrations, application execution, builds or application tests. It supersedes earlier active implementation assignments for this task only. Preserve all existing milestone statuses: M1–M6/M11 COMPLETED, M7/M8 IN PROGRESS, and M9/M10/M12 NOT STARTED. The new M10-DB sub-milestone is NOT STARTED and requires a future explicit implementation assignment.

## Authoritative company policy update — 2026-10-26

- Company policy prohibits running `.exe` artifacts. Do not execute application apphosts, VSTest/testhost executables, or dependency executables, and do not change security policy to enable them.
- For .NET, publish framework-dependent output with `dotnet publish -c Release --no-self-contained -p:UseAppHost=false`, then run the published application or verification DLL with `dotnet <filename>.dll`.
- Automated .NET checks may use a dedicated published managed verification program that exercises production code, reports each check, and returns failure when any assertion fails. Running or rehosting the blocked VSTest runner is not required or authorized.
- Re-evaluate M1 against the original functional acceptance criteria using this permitted execution format. If M1 still cannot be fully verified safely, document its exact limitation and proceed with M2 under the user's explicit exception.
- Commit and push only `PROJECT_REQUIREMENTS.md`, `AGENTS.md`, `IMPLEMENTATION_PLAN.md`, and `PROGRESS.md` to `https://github.com/logtranwanek-spec/IAH-Progress-Tracking-.git` on `main`. Source code and other files remain local.

This update takes precedence over earlier command examples or historical verification notes that use native executables, `dotnet run`, or VSTest. All other requirements remain in force.

## Authorized M2 scope — 2026-10-26

For M2, the Gateway must listen exclusively on `127.0.0.1:5050`. Implement the health endpoint, reserved platform path protection, path-prefix forwarding foundation, a route provider boundary with no application routes hardcoded in Program.cs, request limits, consistent proxy errors, structured logs, and WebSocket support foundation. Verify through a dedicated published managed Gateway program. Do not start later milestones or enable corporate-network access. Only the four named project Markdown files may be committed and pushed to the progress repository.

## Authorized M3 scope — 2026-10-26

After completed M1/M2, implement trusted minimal `samples/DemoNode` (Node.js) and `samples/DemoDotnet` (ASP.NET Core 8). Both must expose a healthy success endpoint, identify the sample and release/build in responses, bind only to `127.0.0.1` on configurable ports in `6100–6999`, run concurrently on distinct ports, and stop cleanly. Provide clear run instructions. Do not require Docker or Administrator privileges, modify existing applications/security settings, or deploy to the company network. For .NET always publish with `UseAppHost=false` and run the published DLL; prefer managed verification over VSTest/testhost. Finish and verify M3 only. Commit/push only the four named progress documents to the progress repository.

## Authorized M4 scope — 2026-10-27

After completed M1–M3, implement M4 only: NestJS Platform API, normalized SQLite registry with Prisma, versioned migrations/constraints, validated registry read/create/update foundations, pagination/filtering/errors/logging/OpenAPI, and protected database connection reference registration/assignment/update/connectivity checks for SQL Server/PostgreSQL/MySQL. Assess Prisma against the no-.exe policy first and document limitations; prefer Node JS/WASM or published managed DLLs. Never execute forbidden native engines or install database servers. Keep all listeners and any connectivity verification on loopback, credentials protected, destructive/deployment operations protected or disabled, and existing applications/security settings unchanged. No later milestone or corporate-network deployment is authorized. Commit/push only the four project records to the progress repository.

The user confirmed on 2026-10-27 that no approved local SQL Server/PostgreSQL/MySQL test instances are available. Document successful authenticated connectivity as unverified; do not install servers or contact corporate databases to fill that gap.

## Authoritative SQLite-only policy clarification — 2026-10-27

Only SQLite is allowed and available for real database operations on this machine. Do not install, connect to, or perform real connectivity tests against SQL Server, PostgreSQL, or MySQL, including local test fixtures that exercise their protocols. Keep their connection-reference data models and registration/read/update/assignment APIs as metadata-only capabilities; do not implement or claim real authenticated connectivity for those engines. Their connectivity verification is an **accepted gap caused by company policy**, not a blocker or an outstanding M4 acceptance requirement. Finish the remaining SQLite-only M4 work, verify the revised behavior, and mark M4 COMPLETED. Do not start M5. This clarification supersedes earlier requirements/instructions to enable or verify SQL Server/PostgreSQL/MySQL connections. All no-.exe, published-DLL, loopback, no-Administrator/Docker, credential protection, and four-document-only commit/push restrictions remain in force.

## Authorized M5 scope — 2026-10-27

After completed M1–M4, implement only M5: a controlled .NET 8 process/runtime manager for reviewed approved releases, Node.js/ASP.NET Core detection, persistent SQLite port leases in 6100–6999 with conflict/concurrency/recovery handling, lifecycle/status/exit/log/health tracking, loopback-only children, persistent desired state and bounded restart/backoff/reconciliation. Verify using published managed DLLs and document process-isolation limitations. Existing applications/security settings must remain unchanged; no Docker, Administrator requirement, .exe artifacts, non-SQLite operations, corporate-network deployment, or later milestone is authorized. Commit/push only the four project records.

This new M5 authorization supersedes the preceding M4 session's instruction to wait before M5. The SQLite-only policy and all security constraints remain unchanged.

## Authorized M6 scope — 2026-10-27

After completed M1–M5, implement M6 only: validated dynamic YARP path-prefix snapshots; live add/update/remove without Gateway restart; SQLite registry/API integration; backward-compatible static startup routes; published managed verification of changes, rejections, real proxying, WebSockets and restart reconciliation. Preserve exact loopback destinations on approved runtime ports, reserved paths, last-known-good configuration and all existing limits. Only SQLite operations, published DLL execution, ordinary-user operation and 127.0.0.1 listeners are authorized. No existing application/security changes, Docker, corporate listening or later milestone. Commit/push only the four project records. This supersedes the M5 session's wait for M6.

---

## Authorized M11 priority — 2026-10-27

After completed M1–M6 (340 passing checks), implement M11 before M7: secure local authentication, login/logout/expiry/revocation and session misuse protection; Administrator/Developer/Viewer RBAC; application ownership/access; deployment approval request/review/status controls; credential-safe security audits; NestJS/SQLite integration; fail-closed protected operations and automated security/full regression tests. Preserve M1–M6 architecture/functionality and all SQLite-only, published-DLL/no-.exe, 127.0.0.1, no-Administrator/Docker/security-change restrictions. Never expose credentials in logs or enable untrusted code. Actual uploads/deployment execution stay disabled until their milestone. Document architecture, permissions, limitations and operation; mark complete only after verified criteria. M7 and all other milestones are not authorized. Commit/push only the four named records to progress origin/main. This supersedes the preceding M6 session's milestone order.

# 1. PROJECT OVERVIEW

## Authorized M7 scope — 2026-10-27

The user attachment authorizes M7 after M1–M6/M11 and 381 passing checks: authenticated owned ZIP upload; bounded safe archive extraction/inspection/integrity and strict metadata; exact-package Administrator approval; controlled non-overwriting release preparation; approved-build assessment; trusted prebuilt M5 execution and M6 routing; persistent validated deployment states/audits and real security/full regression tests. No arbitrary uploaded code, scripts/npm commands/installers/executables, corporate listening, existing application/security changes or non-SQLite operation is allowed. Source builds must stay disabled if no safe approved mechanism exists; document that limitation and keep M7 IN PROGRESS/BLOCKED if builds/execution cannot be implemented safely. Do not start M8 or another milestone. Preserve history/requirements and commit/push only the four records to progress origin/main, verifying the push. This supersedes the M11 session's wait; it does not authorize untrusted execution.


## Authorized M8 assignment — 2026-10-27

The latest user request authorizes M8 React Dashboard after M1–M6/M11 and the verified M7 prebuilt subset (418 checks). M7 remains IN PROGRESS until source-build/isolation acceptance is satisfied. Implement responsive sidebar/header/navigation/states; safe authentication UI; role-aware application/detail/creation/search/filter, package/release/upload/preparation/deployment/approval/history, route/health and available audit views through existing secured API contracts. Do not weaken M11 Origin/Fetch Metadata/session checks: if approved browser authentication is unavailable, document the exact limitation, disable affected features and keep M8 IN PROGRESS/BLOCKED. Do not invent M9 monitoring or unavailable API data. Preserve all prior functionality and SQLite-only, published-DLL/no-.exe, literal loopback, no Docker/Administrator/security/existing-app changes/unapproved builds/corporate listening constraints. Verify UI/auth/roles/data/workflows/errors/expiration/responsiveness and full regression. Preserve history and push only the four records to progress origin/main, verifying the push. Do not start M9 or another milestone. This supersedes earlier M8 wait instructions only.

## 1.1. Background

### Authorized M8 browser-authentication follow-up — 2026-10-27

Resolve remaining M8 browser authentication: assess M11 browser rejection and loopback HTTP transport; design same-origin sessions, CSRF/Origin/Host validation, expiry/revocation/fixation/replay protection and no authentication secrets to application routes. Implement login/logout/current user/private-state cleanup/RBAC and existing application/package/upload/approval/routing/audit operations only after security verification, preserving server ownership/approval enforcement and M7 restrictions. No unrestricted API proxy, cross-origin management, reserved-path bypass, token storage in localStorage/sessionStorage, unapproved certificates/listeners/OS changes, browser exe/driver, source builds or arbitrary execution is allowed. Verify actual browser behavior only with permitted tools; if unavailable, record it and never count DOM simulation as browser acceptance. Run full regression, mark M8 complete only on verified criteria, keep M7 IN PROGRESS/source isolation unresolved, preserve history and push only the four records to progress origin/main with verified delivery. No M9/other milestone is assigned.

Our company currently develops multiple internal web applications using two primary backend technologies:

- Node.js
- ASP.NET Core (.NET 8)

Frontend technologies include:

- React
- ASP.NET Core MVC / Razor

Each application typically contains three main components:

1. Frontend
2. Backend
3. Database

Currently, after an application is developed, the developer must request the IT department to open a network port on an internal Windows laptop functioning as a server.

Example:

```text
Server IP: 10.141.79.182

Application A:
http://10.141.79.182:3000

Application B:
http://10.141.79.182:3001

Application C:
http://10.141.79.182:3002
```

Each new application requires a separate port-opening request.

As the number of internal applications increases, this process creates several problems:

- Developers depend heavily on IT for deployment.
- Application deployment takes longer than necessary.
- Network port management becomes increasingly complicated.
- Applications are managed independently.
- There is no centralized deployment dashboard.
- Monitoring and troubleshooting are difficult.
- Application versions and deployment history are not centrally managed.

Existing applications must remain unchanged.

The new platform will primarily manage newly developed applications.

## 1.2. Project Objective

Build a centralized **Internal Application Hosting Platform** that allows authorized developers to upload application folders or ZIP packages and deploy them without requesting a new externally accessible port for every application.

The platform must provide a self-service experience similar to Vercel.

Example:

**Platform URL**

```text
http://10.141.79.182:5050
```

A developer uploads an application named:

```text
quality-management
```

The platform automatically:

1. Validates the uploaded package.
2. Identifies the application framework.
3. Reads deployment configuration.
4. Builds the application if necessary.
5. Allocates internal runtime ports.
6. Starts the application.
7. Performs health checks.
8. Registers a dynamic reverse-proxy route.
9. Generates an accessible URL.
10. Displays the deployment result.

Expected output:

```text
Application: Quality Management
Framework: Node.js
Status: Running

URL:
http://10.141.79.182:5050/quality-management/
```

Another application:

```text
Application: IoT Dashboard
Framework: ASP.NET Core
Status: Running

URL:
http://10.141.79.182:5050/iot-dashboard/
```

Both applications must share the same externally accessible port, 5050.

The platform must not require opening additional network ports for each new application.

---

# 2. CURRENT INFRASTRUCTURE

## 2.1. Server Information

| Component | Specification |
|---|---|
| Operating System | Windows 11 |
| Windows Build | 22631 |
| Server IP | 10.141.79.182 |
| Platform Port | 5050 |
| Node.js | v24.22.1 |
| npm | v11.19.1 |
| .NET SDK | 8.0.424 |
| ASP.NET Core Runtime | 8.0.30 |
| .NET Runtime | 8.0.30 |
| Docker | Not permitted |
| Administrator Privileges | Not available |
| Existing Applications | Must remain unchanged |

The machine also has .NET 7 runtimes installed, but new development should target .NET 8 unless a compatible version is explicitly approved.

## 2.2. IT Restrictions

The following restrictions are mandatory:

**Docker is prohibited.**

Do not use:

- Docker Engine
- Docker Desktop
- Docker Compose
- Docker-based deployment
- Kubernetes
- Container orchestration requiring Docker

**Administrator privileges are unavailable.**

The platform must not depend on:

- Administrator access.
- Installing Windows Services without IT assistance.
- Modifying Windows Firewall rules.
- Changing system-wide registry settings.
- Changing Windows security policies.
- Disabling antivirus or endpoint protection.
- Bypassing PowerShell Execution Policy.
- Installing privileged system components.

Port 5050 has already been opened by IT.

However, the platform must initially be tested using localhost before enabling network access.

Any deployment operation must remain within the permissions explicitly approved by IT.

## 2.3. npm Execution Policy

PowerShell currently blocks execution of `npm.ps1`.

However, npm works correctly through:

```powershell
npm.cmd --version
```

Result:

```text
11.19.1
```

Therefore, when invoking npm from PowerShell or the Deployment Engine, use `npm.cmd`.

Do not change the PowerShell Execution Policy.

---

# 3. SYSTEM ARCHITECTURE

## 3.1. High-Level Architecture

The system must follow a modular architecture.

```text
                 COMPANY NETWORK
                        |
                        |
                        ▼
            10.141.79.182:5050
                        |
                        ▼
           ┌────────────────────────┐
           │                        │
           │      YARP GATEWAY      │
           │                        │
           │  Dynamic HTTP Routing  │
           │                        │
           └────────────┬───────────┘
                        |
         ┌──────────────┼──────────────┐
         |              |              |
         ▼              ▼              ▼
      /admin          /idea          /quality
         |              |              |
         ▼              ▼              ▼
    Platform UI       Node.js       ASP.NET Core
         |              |              |
         |              ▼              ▼
         |         localhost:6101  localhost:6102
         |
         ▼
    PLATFORM API
         |
         ├── Authentication
         ├── Application Registry
         ├── Upload Manager
         ├── Deployment Manager
         ├── Version Manager
         ├── Monitoring
         ├── Audit Logging
         └── Configuration Manager
                    |
                    ▼
              DEPLOYMENT AGENT
                    |
         ┌──────────┼──────────┐
         |          |          |
         ▼          ▼          ▼
      Node.js     ASP.NET     Process
      Builder     Builder     Manager
         |          |          |
         └──────────┼──────────┘
                    |
                    ▼
             APPLICATION STORAGE
```

## 3.2. Architecture Principles

The system must follow these principles:

1. Single externally accessible HTTP entry point.
2. Dynamic path-based routing.
3. Separate application processes.
4. No Docker dependency.
5. No Administrator dependency for normal operation.
6. Persistent application registry.
7. Controlled deployment execution.
8. Centralized logging.
9. Versioned application releases.
10. Modular components.
11. Recoverable deployments.
12. Secure handling of credentials.
13. Compatibility with Node.js and ASP.NET Core.
14. Support for future infrastructure migration.

The platform must not modify or interfere with existing applications.

---

# 4. TECHNOLOGY STACK

Use the following preferred stack.

| Layer | Technology |
|---|---|
| Frontend | React + TypeScript |
| Frontend Build | Vite |
| UI Framework | Tailwind CSS |
| UI Components | shadcn/ui |
| Backend API | NestJS |
| Platform Database | SQLite |
| ORM | Prisma |
| Reverse Proxy | YARP |
| Gateway Runtime | ASP.NET Core 8 |
| Deployment Agent | .NET 8 Worker |
| Node.js Hosting | Node.js |
| .NET Hosting | ASP.NET Core / Kestrel |
| Logging | Serilog / structured application logs |
| API Documentation | OpenAPI / Swagger |
| Authentication | Configurable local authentication, extensible to Microsoft Entra ID |
| Monitoring | Process-level CPU/RAM and health checks |

The Agent may recommend alternatives only when there is a clear technical justification.

Avoid introducing unnecessary infrastructure dependencies.

The platform must run on the existing Windows laptop.

---

# 5. CORE FUNCTIONAL REQUIREMENTS

## 5.1. Dashboard

Develop a modern dashboard inspired by Vercel.

The dashboard must display:

- Total applications.
- Running applications.
- Stopped applications.
- Failed deployments.
- Recent deployments.
- CPU usage.
- RAM usage.
- Application health.
- Deployment activity.

Example:

```text
INTERNAL HOSTING PLATFORM

Dashboard | Applications | Deployments | Monitoring

Total Applications: 25
Running: 22
Stopped: 2
Failed: 1

APPLICATIONS

Name             Framework       Status
------------------------------------------------
Idea Network     Node.js         Running
IoT Dashboard    ASP.NET Core    Running
Quality System   Node.js         Running
Production App   ASP.NET Core    Stopped

[+ NEW APPLICATION]
```

The dashboard must support searching, filtering and sorting.

## 5.2. Application Registration

Developers must be able to register new applications.

Required fields:

```text
Application Name
Application Slug
Description
Framework
Deployment Type
Application Owner
Environment
Application Visibility
```

Example:

```text
Application Name: Quality Management
Slug: quality
Framework: Node.js
Environment: Production
```

The system must automatically generate:

```text
http://10.141.79.182:5050/quality/
```

Application slugs must be unique.

Reserve internal paths such as:

```text
/admin
/_platform
/_platform/api
/_platform/assets
/_platform/health
```

Prevent applications from registering conflicting paths.

## 5.3. Upload Manager

Support two deployment methods.

### Method A: Upload Source Code

Developer uploads a ZIP containing the source code.

The platform must:

1. Validate the archive.
2. Extract it to a controlled temporary directory.
3. Detect the application framework.
4. Read the deployment manifest.
5. Validate the declared build configuration.
6. Build the application in an approved execution environment.
7. Store build logs.
8. Prepare the release.
9. Start the application.
10. Perform health checks.

### Method B: Upload Prebuilt Application

Developer uploads a previously built application.

Examples:

React:

```text
dist/
```

ASP.NET Core:

```text
publish/
```

Node.js:

```text
package.json
server.js
```

Node.js packages must include all required runtime dependencies or an approved dependency installation procedure.

The platform must validate the package and launch it using the appropriate runtime.

### Upload Security

The maximum compressed ZIP upload is **50 MiB (52,428,800 bytes), inclusive**, consistently enforced by the Dashboard, authenticated Platform API, Gateway browser-upload boundary and package inspector/storage constraint. The Dashboard displays **"Maximum ZIP size: 50 MB"**; that label uses the same binary 50 MiB limit, not 50,000,000 bytes. Validate size before reading/uploading the file; reject even one byte above the cap with a clear size error/HTTP 413 at server ingress. Archives below/at the cap still require all normal integrity, structure, ownership, authorization, fingerprint and Administrator approval checks.

This compressed-size change does **not** raise extraction limits: retain **32 MiB expanded total, 8 MiB per expanded file, 256 entries and maximum 100:1 expansion ratio with the existing 4-KiB allowance**. Keep ZIP Slip/path/link checks, strict CRC/structure/type validation, compression-bomb protection, bounded upload concurrency/deadlines and owned temporary cleanup. Only the explicitly allowlisted browser ZIP operation receives the larger request budget; ordinary application proxy and JSON request limits remain unchanged. The native API streams to owned storage; avoid redundant archive-sized memory copies at the signed browser boundary. Source builds/unreviewed execution remain disabled.

Implement:

- Maximum upload size.
- File type validation.
- ZIP extraction security.
- Path traversal prevention.
- Archive size limits.
- Package integrity verification.
- Upload ownership.
- Deployment authorization.
- Temporary file cleanup.

Do not execute arbitrary uploaded scripts merely because they are present in an archive.

Source builds and runtime execution must require an approved deployment policy.

## 5.4. Simplified folder intake for M7 (target workflow; default-off Dashboard candidate implemented)

The implementation provides authenticated, owner-scoped folder intake and package-generation APIs plus a default-off React Dashboard candidate that registers or selects an authorized application, previews selected files and size, stages the folder, invokes the server-side package inspection path, and displays backend-confirmed outcomes such as pending review, BUILD_REQUIRED or DEPLOYMENT_BLOCKED. The Dashboard uses only explicit existing API operations and does not auto-approve, build, execute, prepare or deploy. Automated DOM and loopback HTTP/WebCrypto integration checks passed, but actual-browser acceptance has not been run; private production browser management remains disabled by default. The requirements below remain the target for full acceptance, including actual-browser verification and any unsupported capability.

The target ordinary-user workflow is: enter an application name, select its folder, review detected runtime/startup and upload size, and submit for review. The browser must not ask the user to create deployment ZIPs/manifests, enter an application UUID, release/version token or calculate fingerprints. The server owns application identity, slug, unique package/release identifier, build/version tokens, manifest generation, archive validation and fingerprinting from validated Registry state. M11 identity, role, ownership and authorization checks still apply.

A folder picker can send the selected files with relative-path metadata (including a browser-supplied selected-root component), but browser paths and all client-supplied size/type/runtime/metadata claims are untrusted. Define one protocol for recognizing and stripping only the validated common selected-folder prefix; reject inconsistent roots and never trim arbitrary path components. The Platform API must canonicalize every path and enforce limits from bytes actually received. A later implementation must test the approved browser's directory-selection support; no actual-browser acceptance is implied by this requirement. Keep the existing authenticated ZIP operation available for backward compatibility. Folder intake is a separate explicitly allowlisted upload operation; it must not create a general file or proxy endpoint.

The folder payload budget is **52,428,800 bytes (50 MiB) inclusive**, measured as the sum of file-content bytes, with a separately bounded transport/framing budget. Enforce the limit while streaming, not only from browser file sizes or Content-Length. Reconcile the transport bound with Gateway request limits without widening ordinary proxy/JSON budgets. Retain existing ZIP/package ceilings: **32 MiB extracted total, 8 MiB per file, 256 file-and-directory entries, 100:1 expansion with the 4-KiB allowance**, existing timeout/concurrency/storage quotas, and the inclusive 50 MiB compressed ZIP limit. Do not raise them without a separate threat analysis and explicit approval. A folder upload can be accepted for review while remaining unpackageable or deployment-blocked by stricter release limits; show that distinction clearly.

The server must stream files into an owned, non-public temporary intake; bind the intake to the authenticated user and selected/registered application; reject ambiguous/duplicate names and never attach to an existing app by name alone; require existing-app selection/ownership to be resolved server-side; normalize and validate paths, detect duplicate/case-colliding names, reject absolute paths, traversal, device/alternate-stream names, malformed encoding, links/reparse points and unsafe artifacts, and limit depth and entry count. Never use a client path as a filesystem path before validation. Reject conflicting client-supplied platform manifests or treat them as untrusted review material; generate deployment metadata from validated server-side Registry state. Keep uploads out of releases and static/public trees until separately approved preparation. Clean owned incomplete intake on all failures without changing the existing application or its data.

Ignore or reject development-only and sensitive inputs by an explicit reviewed policy (including .git, dependency trees, environment/credential files and private keys); do not run .gitignore, package hooks, dependency installation, builds or user scripts to decide what to upload. Tell the user which files were excluded/rejected without displaying secret contents or logging sensitive filenames/values. Keep the file count and byte budgets explicit. Source-only material may be retained in a protected review state, but does not become a validated package or runnable release.

The folder workflow must distinguish these user-facing conditions: **Upload accepted** (transport/storage only), **Package validated** (static format/security checks passed), **Ready for deployment** (supported reviewed prebuilt runtime and required approvals/configuration exist), **Build required** (source material has no accepted prebuilt output), and **Deployment blocked** (unsupported startup, limit/security issue or unavailable approved build boundary). These are proposed UI meanings, not new persisted API enum values; map them only to evidence supplied by the backend. Do not claim build/deployment readiness from a successful upload.

Runtime detection and startup discovery are static only. Candidate Node.js evidence includes a reviewed ESM package descriptor and an existing supported server entry; candidate ASP.NET Core evidence includes a compatible framework-dependent published DLL and matching dependency/runtime metadata. Ambiguous or incomplete layouts remain review-only. Supported startup intent may be translated into fixed Node or dotnet arguments only from a strict documented grammar and allowlisted relative entrypoint. Never call cmd.exe /c, execute run.cmd/.bat/PowerShell, or launch arbitrary commands. See section 13.1A. No source build, npm install, restore or build template is authorized by folder upload.

For supported prebuilt folders, the Platform generates the canonical package metadata from authenticated Registry identity and detected runtime; it assigns a unique nonoverwriting package/release identifier server-side. Create a minimal canonical Node ESM package descriptor with no scripts and only already-present, reviewed runtime dependencies. Do not install dependencies; reject unresolved imports. Create a bounded internal archive using a reviewed server-side process, then run the existing ZIP inspector over the result and compute file/archive fingerprints server-side. The exact inspected package must still pass current-user authorization, package ownership and a distinct current Administrator's exact-fingerprint review before any preparation/start. Submission never auto-approves or deploys. If no safe archive can be produced, keep the upload review-only and report a specific reason. Existing trusted-prebuilt ZIP uploads remain compatible.

Source-only uploads may be stored solely for authorized review under independent bounded retention/quota and cleanup controls. Until an approved isolated build service exists, label them **Build required / deployment blocked** and never build, run, prepare as a release or pass them to the Agent. Administrator review alone does not authorize executing arbitrary source. M7's source-build/isolation acceptance remains unfulfilled. Folder intake does not implement M10-DB: application database selection, initialization, migration, backup or restore remains separately governed by section 9.1 and its future milestone.

The existing M5 RuntimeEngine clears inherited child-process environment and supplies only its fixed allowlist plus supervised host/port/release values; it does not currently project an application SQLite path/configuration. Until a separately authorized and implemented app-specific configuration capability exists, a database-dependent upload must remain deployment-blocked. Never point it at Platform Registry storage, put data inside a release, inherit host secrets or silently create/migrate a database. The folder workflow does not start M10-DB.

Dashboard review should show the entered name, server-resolved application identity to authorized roles, owner, detected runtime/startup, aggregate/entry count, excluded/flagged-file summary, package-validation result, exact package identity/fingerprint to authorized reviewers, and the next safe action. Keep UUIDs and fingerprint calculation internal for ordinary users; keep detailed review evidence administrator-only. Show only backend-confirmed progress; while source builds are disabled do not show Build SUCCESS, synthetic runtime metrics or unverified readiness. Do not fabricate status or hide unsupported features.

## 5.5. Unified folder/ZIP intake with SQLite artifact separation (TASK 024)

An authenticated developer may select an existing application folder or submit one ZIP containing both application files and existing SQLite database artifacts. The user is not required to manually separate those files. The server must classify files internally, preserve normalized relative-path mapping, and keep application-code review separate from database-candidate review. This is an upload and review workflow only; it does not authorize application execution, deployment, database attachment, replacement, restore, provisioning, initialization, migration, runtime configuration, or Gateway application-route changes.

The current API operation is `POST /_platform/api/applications/:id/folder-intakes/zip`, with exactly one multipart file field named `archive`. It requires an authenticated M11 session, `application.write`, and current application ownership. The signed browser boundary may expose only the exact `POST /admin/browser/applications/:id/folder-intakes/zip` operation; it is not a generic API proxy and retains the existing Host, Origin, Fetch Metadata, CSRF, proof, replay, session, role, ownership, and bounded-body checks. The production browser management client remains disabled by default until its separate M8 browser acceptance gate is met.

Recognize `.db`, `.sqlite`, and `.sqlite3` as database candidates and the matching `-wal`/`-shm` suffixes as sidecars. Preserve their original normalized relative paths in the private owner/application/upload-scoped intake manifest. Place their bytes under a separate opaque-identifier `database/` staging subdirectory, never among code blobs. Candidate role is an evidence-based suggestion only: backup-directory/name patterns can identify a potential backup, a bounded static source scan can identify a possible active reference, and ambiguous candidates remain `UNKNOWN`. Multiple candidates are supported. Sidecar sets and all candidate snapshots remain `REVIEW_REQUIRED`; SHM is never treated as authoritative content and a passing SQLite integrity check does not prove a live database was captured consistently.

SQLite inspection is read-only and bounded. Validate the SQLite file header, then use the approved Node SQLite reader with read-only mode, extension loading disabled, `query_only`, `trusted_schema=OFF`, worker memory limits, a deadline, candidate-count limits, and `quick_check(1)`. Do not run SQL files, application code, migrations, or uploaded commands. Never create or replace a database, mutate an uploaded database, copy a live database as a consistent backup, or delete WAL files automatically. SQLite sidecars are staged separately and require further snapshot review.

The canonical code-package path must exclude database files and SQLite sidecars from the release archive and its file fingerprint. Existing package validation, exact SHA-256 review, distinct Administrator approval, and deployment gates remain unchanged. `.sql` is inert and accepted only under the reviewed `drizzle/` path; `.webmanifest` is accepted only below the reviewed static roots. ZIP limits remain 50 MiB compressed inclusive, 256 entries, 32 MiB expanded total, 8 MiB per entry, and the existing 100:1 ratio plus 4-KiB allowance. Preserve ZIP integrity, CRC, path, link, traversal, reserved-path, ownership, quota, timeout, and cleanup controls. Do not broaden source-build or uploaded-code execution permissions.

The Dashboard may display the submitted format, counts, bytes, candidate relative paths, candidate sizes, evidence-based roles, sidecars, and read-only validation status. It must clearly state that staging is review-only and that no database has been attached, replaced, restored, migrated, or deployed. The current API returns a sanitized reason code for an unsupported file and rejects the upload, but does not yet return a per-file unsupported-path list; Dashboard reporting of individual rejected files and reasons remains unfinished. No raw database content or server filesystem path may be returned.

This intake does not implement the Managed Application Database Service in section 9.1 or start M10-DB. Private staging under one Windows identity is not a sandbox or OS-level isolation boundary. No schema or migration is required or introduced by this work package.

---

# 6. DEPLOYMENT CONFIGURATION

Each application should support an optional deployment manifest.

Recommended filename:

```text
platform.yaml
```

Example:

```yaml
name: quality-management
slug: quality

runtime: nodejs

deployment:
  type: source

frontend:
  framework: react
  path: ./frontend
  build: npm run build
  output: dist
  basePath: /quality/

backend:
  path: ./backend
  start: npm start
  port: auto

database:
  type: sqlserver
  connectionSecret: QUALITY_DB_CONNECTION

routing:
  path: /quality

healthcheck:
  path: /health
```

The manifest is a platform-specific configuration format.

The Agent must define a strict schema and validation rules.

Manifest commands must not be executed blindly.

Use approved runtime templates and allowlisted operations.

Custom build commands should require elevated deployment approval.

For ASP.NET Core, support a separate deployment template containing:

```text
Project Path
Target Framework
Build Configuration
Publish Directory
Runtime Configuration
Application Entry Point
Health Check Path
```

---

# 7. DEPLOYMENT ENGINE

The Deployment Engine is a core component.

It must support:

- Framework detection.
- Runtime validation.
- Source build.
- Prebuilt deployment.
- Internal port allocation.
- Process startup.
- Process termination.
- Process restart.
- Health checks.
- Deployment logs.
- Version management.
- Rollback.
- Deployment status reporting.

## 7.1. Deployment Workflow

```text
UPLOAD
   |
   ▼
VALIDATE PACKAGE
   |
   ▼
DETECT FRAMEWORK
   |
   ▼
VALIDATE CONFIGURATION
   |
   ▼
CHECK DEPLOYMENT PERMISSION
   |
   ▼
BUILD / PREPARE APPLICATION
   |
   ▼
ALLOCATE INTERNAL PORT
   |
   ▼
START APPLICATION PROCESS
   |
   ▼
HEALTH CHECK
   |
   ├── FAILED → SAVE LOGS / CLEANUP
   |
   ▼
REGISTER GATEWAY ROUTE
   |
   ▼
VERIFY PUBLIC ROUTE
   |
   ▼
DEPLOYMENT SUCCESS
```

## 7.2. Internal Port Management

Use an internal port range such as:

```text
6100–6999
```

Example:

```text
Application A → 127.0.0.1:6101
Application B → 127.0.0.1:6102
Application C → 127.0.0.1:6103
```

All application processes must bind to localhost.

Port allocation must:

- Detect occupied ports.
- Prevent duplicate allocations.
- Handle concurrent deployments.
- Release ports after termination.
- Persist allocation information.
- Recover allocation state after platform restart.

The Agent must account for the race condition between selecting a free port and the application binding to it.

The platform must never expose internal application ports directly to the company network.

## 7.3. Process Management

Develop a process manager capable of:

```text
START
STOP
RESTART
STATUS
HEALTH CHECK
LOGS
```

Each application must run in a separate process.

The system must track:

```text
Application ID
Process ID
Runtime
Internal Port
Start Time
Current Status
Exit Code
CPU Usage
RAM Usage
```

Process startup and termination must be controlled.

Avoid orphaned child processes.

The Deployment Agent must not run with Administrator privileges.

Because Docker is unavailable, the Agent must explicitly document the limitations of process isolation on Windows.

Do not claim that separate processes provide strong security isolation.

## 7.4. Runtime Recovery

The system must handle:

- Application crashes.
- Failed startups.
- Unexpected process exits.
- Gateway restarts.
- Deployment Agent restarts.
- Laptop restarts.
- Interrupted deployments.

Persist the desired state of each application.

After the platform restarts, reconcile the desired state with actual running processes.

Do not automatically restart an application indefinitely.

Use bounded restart attempts and backoff.

If the Windows user session ends, background processes may stop.

Do not assume automatic startup after logout or reboot is available without an IT-approved startup mechanism.

---

# 8. DYNAMIC REVERSE PROXY

Use YARP with ASP.NET Core 8.

The gateway must listen on port 5050.

Initial development must use:

```text
http://127.0.0.1:5050
```

Network access through:

```text
http://10.141.79.182:5050
```

must only be enabled after local testing and confirmation of the approved listening configuration.

## 8.1. Routing Requirements

Example:

```text
/idea/       → 127.0.0.1:6101
/quality/    → 127.0.0.1:6102
/iot/        → 127.0.0.1:6103
```

The gateway must:

- Load routes from the Application Registry.
- Support runtime route updates.
- Add routes without restarting the gateway.
- Remove routes when applications are deleted.
- Update routes during deployment.
- Route traffic only to approved destinations.
- Support WebSocket connections.
- Handle reverse-proxy errors.
- Provide gateway health checks.

## 8.2. Dynamic Configuration

Do not hardcode application routes in `Program.cs`.

Implement a YARP dynamic configuration provider.

The gateway must receive routing updates through a controlled internal interface or configuration-change mechanism.

Routing changes must be validated before activation.

## 8.3. Base Path Compatibility

This requirement is critical.

Applications must support deployment under a URL prefix.

Example:

```text
/quality/
```

React applications may require:

- Vite base configuration.
- React Router basename.
- Correct static asset URLs.
- Correct API base URLs.

ASP.NET Core applications may require:

- PathBase configuration.
- Forwarded headers.
- Correct redirect URLs.
- Correct cookie paths.
- Correct static asset paths.

The platform must support framework-specific deployment templates.

Do not assume that simply removing the URL prefix in the proxy will make every application compatible.

If an application cannot operate under a subpath, deployment must report the incompatibility clearly.

The platform should be designed to support hostname-based routing in the future, although it is not required for the initial release.

---

# 9. DATABASE MANAGEMENT

**Current policy and scope:** Only SQLite is allowed for real database operations. SQL Server, PostgreSQL and MySQL are **metadata-only connection references**; no real connection, authentication, migration, provisioning or connectivity testing against those engines is permitted. This restriction supersedes the original existing-company-server/connectivity wording retained below. Their missing real connectivity verification remains the accepted company-policy gap, not a claim of operational support. Section 9.1 defines future application database requirements; it does not describe implemented provisioning, backup or restore features.

The Platform Registry will use SQLite initially.

Application databases may use existing company database servers.

Supported database connection types should include:

- SQL Server
- PostgreSQL
- MySQL

The platform must not automatically install database servers.

Develop a Database Connection Manager that allows authorized users to:

- Register database connections.
- Assign connections to applications.
- Test connectivity.
- Update connection settings.
- Manage connection references.

Credentials must not be stored in plaintext.

Use Windows-supported credential protection where appropriate, with access restricted to the designated execution identity.

Never expose database passwords through frontend APIs or application logs.

Database schema migrations must be separate, controlled deployment steps.

Never automatically execute arbitrary SQL files from uploaded packages.

## 9.1. Managed Application Databases

**Target architecture:** **1 Platform Database + N Independent Application Databases**. The dedicated SQLite Platform Registry contains platform management records. Each deployed application may optionally use its own independent SQLite database; applications without a database remain fully valid. N may be zero. This service is future work in M10-DB, NOT STARTED. Existing connection-reference APIs are not evidence that application database lifecycle operations have been implemented.

### 9.1.1. Optional application database modes

| Mode | Required behavior |
| --- | --- |
| `NONE` | The application does not require a database. Do not provision a file or require a database association/configuration. |
| `MANAGED_SQLITE` | The Platform provisions and manages a dedicated SQLite database for exactly one application, with an explicit application-to-database association and the lifecycle controls below. |
| `EXISTING_SQLITE` | The application uses an existing SQLite file explicitly registered, validated and authorized through the Platform. Registration must not create, replace, import, migrate or take deletion authority over the file implicitly. |

Record the selected mode and authorized association in Platform metadata. Configuration and mode changes require M11 authorization and application ownership checks. Changing a mode or association must not silently move, delete, replace or migrate data. No application is required to select a database mode other than NONE. SQL Server/PostgreSQL/MySQL references are separate metadata records and cannot enable operational database modes under current policy.

### 9.1.2. Database separation and storage boundaries

- Every MANAGED_SQLITE application must have its own independent SQLite file; a managed database belongs to exactly one application. Prevent duplicate associations or aliases that would attach another application's file or the Platform Registry.
- Application databases must never share the Platform Registry database. Deployed applications must never receive direct access to the Registry, its connection configuration, protected credential material or internal management capabilities. Do not attach the Registry as an application database.
- Resolve database identities to Platform-controlled paths within approved storage locations. Validate both new managed paths and registered EXISTING_SQLITE paths; arbitrary browser-supplied filesystem paths are not authority to access a file. An existing database outside approved storage requires an explicitly approved registration/location decision before any operation.
- Enforce canonical containment and file identity checks for database, journal/WAL sidecar, backup, staging and restore paths. Reject path traversal, absolute/UNC/device-path injection, alternate streams, symlink/junction/reparse-point escapes, file aliases and cross-application substitutions. Revalidate at the operation boundary and fail closed if containment or identity cannot be established; string-prefix checks alone are insufficient.
- Prevent unauthorized database file access through Platform APIs, including cross-application registration, status, backup, restore and deletion. Apply M11 RBAC/ownership even to metadata reads. Never expose raw paths as a substitute for an authorized resource ID.
- Document the actual execution identity and effective filesystem permissions. **Filesystem separation under the same Windows identity is not a security sandbox. Separate SQLite files do not provide OS-level isolation against malicious application code.** Path validation protects Platform operations; it cannot stop same-user malicious code from directly accessing files permitted to that identity. Preserve trusted-reviewed-code restrictions; do not change Windows security settings or claim new identity isolation.

### 9.1.3. Target logical application storage

Use the existing configurable `IHP_STORAGE_ROOT`. The required target logical structure is:

```text
<STORAGE_ROOT>/
|
+-- registry/
|   +-- platform.db
|
+-- Applications/
|   |
|   +-- <application-uuid>/
|       |
|       +-- releases/
|       |   +-- v1/
|       |   +-- v2/
|       |   +-- v3/
|       |
|       +-- workspace/
|       |   +-- database/                 (example app-relative directory)
|       |       +-- workcenter.db         (example app-relative file)
|       |       +-- settings.db           (optional second database)
|       |
|       +-- data/
|       |   +-- application.db
|       |
|       +-- backups/
|       |   +-- <database-id>/
|       |       +-- <backup-id>/
|       |
|       +-- logs/
|       |
|       +-- config/
|
+-- Uploads/
|
+-- Temporary/
|
+-- Logs/
```

`workspace/` is a **proposed target** for stable application-relative data paths and is distinct from every release. A managed database that must retain a path such as `database/workcenter.db` is stored at that same relative path below the application's workspace root. `data/application.db` remains the default target for a future MANAGED_SQLITE configuration where the application has no existing required relative path; it is not a second copy of a database already represented in the workspace. NONE does not require either location. EXISTING_SQLITE uses its explicitly approved registered location and is not implicitly copied into workspace. The exact relationship between the default `data/application.db` convention and app-relative workspace mappings requires product-owner approval before implementation.

Application UUIDs, rather than mutable slugs or release labels, identify target application storage. Database files and their SQLite sidecars must remain outside every release directory. Backups must remain outside releases and public trees in a separately approved application-scoped location. This structure is a future logical target only; preserve implemented `IHP_STORAGE_ROOT`, Registry/application paths and current runtime behavior until an explicitly authorized implementation and compatibility/recovery plan are approved.

This is a **target logical structure**, not an assertion about current physical paths. Preserve implemented `IHP_STORAGE_ROOT`, Registry location, slug/UUID directories and all existing configurations. The older examples in sections 10 and 17 are retained as examples; they do not authorize relocating data. Any future physical layout migration requires an explicitly authorized implementation milestone, compatibility/recovery planning and verified data preservation. This documentation task does not create or move directories/databases.

### 9.1.4. Managed SQLite lifecycle requirements

| Capability | Required behavior before implementation can be accepted |
| --- | --- |
| Provisioning | Idempotent, concurrency-safe provisioning for one application. A retry must return the existing authorized association; never overwrite an existing database, adopt an unrelated file or truncate data. Detect collisions and incomplete provisioning, fail safely and record the outcome. |
| Association | Persist mode, stable application/database identifiers and authorized file identity separately from releases. Enforce one-application ownership of each managed database, with authorization rechecked for association changes. |
| Initialization | Initialize a newly provisioned database only under an explicitly reviewed, separately authorized initialization/schema operation. Do not execute uploaded SQL, package hooks or application-provided migration commands automatically. Preserve existing files and data on retries. |
| Metadata and status | Record safe identifiers, mode, association, lifecycle/validation status, schema/version information when known, timestamps, storage usage, configured limits and backup/recovery outcomes. Unknown or failed states must not be presented as healthy or successfully provisioned. Do not return internal paths or secrets in ordinary metadata. |
| Integrity validation | Validate the SQLite file type and integrity through approved SQLite APIs, using bounded, coordinated checks appropriate to the database state. Do not treat extension/header checks alone as proof of integrity. Fail closed on invalid files, preserve evidence/data and expose only safe diagnostic results. |
| Backup and restore | Require explicit authorization and the SQLite-consistent procedures in section 9.1.8. Track application ownership, operation state and validated backup identity; never imply that release archives are database backups. |
| Retention | Store explicit retention policy for application data and backups, including retained data after application removal. Do not choose an undocumented retention duration or perform implicit destructive cleanup. Policy-driven deletion must remain explicitly authorized and auditable. |
| Deletion | Require explicit authorized retention/deletion decision, destructive-action confirmation and verified target identity. Removing an application, changing mode or deleting a release must not automatically destroy application data/backups. EXISTING_SQLITE registration does not grant authority to delete externally owned files. |
| Recovery | Detect interrupted provisioning/backup/restore/deletion, retain recoverable state and report partial/failed outcomes accurately. Validate recovery targets and backup provenance; require authorization for data-changing recovery and never silently replace a corrupt database with an empty file. |
| Storage limits | Define configurable per-application database/sidecar/backup limits and available-space checks. Report usage and enforce documented Platform-operation budgets; never truncate data to meet a limit. Direct same-user application writes are not a hard filesystem quota or sandbox. Exact defaults require future implementation review. |
| Access permissions | Enforce M11 RBAC, current application ownership and approved storage identity for each operation. Supply only the application's required runtime access; no Registry access or implicit cross-application grant. State same-identity limits explicitly. |
| Audit logging | Audit requests, authorization denials, association/mode/retention changes, lifecycle actions, actor, application/database/backup identifiers, timestamps and outcomes. Include failures and recovery decisions; omit database contents, secrets, sensitive configuration and raw filesystem paths. |

Application deletion must not automatically destroy application data without an explicit authorized retention/deletion decision. Retained data must keep an authorized management identity and audit association even if the application record is later removed. Concrete metadata/schema/API changes belong to future reviewed implementation and versioned migrations, not this documentation update.

### 9.1.5. Deployment and versioning behavior

Application data persists independently of application releases:

```text
Application v1 -> application.db
Application v2 -> same application.db
Application v3 -> same application.db
```

Deploying, updating, restarting or rolling back an application must not automatically delete, replace, copy over or reset its database. Release cleanup must exclude application data and backups. Mode NONE must continue to work without database setup.

Database schema migrations must be explicit, controlled and separately authorized operations, with reviewed compatibility, backup/recovery and application coordination requirements. Never automatically execute SQL scripts from uploaded packages. Application rollback does not automatically reverse database schema changes; verify the target release's compatibility with the retained schema and refuse unsafe rollback rather than silently downgrading or discarding data.

### 9.1.6. Application database access configuration

Provide only the minimum application-specific database configuration required at runtime, through approved configuration delivery outside immutable release artifacts. Never provide Platform Registry configuration/credentials or internal management capabilities. Where configuration contains secrets, exclude it from application source code, release artifacts, ordinary API responses and logs.

Database files, backups, contents and filesystem paths must not be served through public Gateway routes or application static assets. Platform APIs must not expose database contents without authorization, and ordinary browser endpoints must not download raw SQLite files. Dashboard metadata should use safe resource IDs and status/usage summaries. These are future enforcement requirements, not a guarantee against malicious trusted-identity code directly reading permitted files.

### 9.1.7. Future Dashboard database management

Provide an **Application Database Management** section as part of M10-DB, after secure Dashboard access is verified:

- View and select the optional database mode during application configuration; register an authorized EXISTING_SQLITE reference without arbitrary filesystem browsing.
- Provision a managed SQLite database and view real provisioning/validation status and safe metadata.
- View database storage usage and documented limits.
- Initiate explicitly authorized backups and restores; review backup history and operation results.
- Manage authorized retention settings and request authorized database deletion with explicit destructive-action confirmation.

Every management operation must enforce M11 RBAC and application ownership on the server; UI action visibility is supplementary. Administrators act within platform policy, Developers only within authorized owned-application permissions, and Viewers have read-only access to authorized safe metadata/history. Restore, deletion and other destructive actions require explicit authorization and confirmation; read access must never confer those rights. Backup also requires explicit authorization. No raw database file is exposed through ordinary browser endpoints. Until implemented and verified, label these capabilities as planned/disabled rather than claiming they work; existing M8 status is unchanged.

### 9.1.8. SQLite-consistent backup and controlled restore

- Use an approved SQLite-consistent backup mechanism, such as the SQLite Online Backup API through a policy-compatible in-process provider. Do not assume copying an active `.db` file is a safe backup procedure; active transactions and journal/WAL state must be handled correctly. No prohibited executable, external database engine or unapproved utility is authorized. SQLite documents a consistent snapshot mechanism in its [Online Backup API](https://www.sqlite.org/backup.html) and explains the [risks of copying active databases](https://www.sqlite.org/howtocorrupt.html#_backup_or_restore_while_a_transaction_is_active).
- Require explicit backup authorization, validate source ownership and destination containment, create a new non-overwriting backup identity and verify completion/integrity before declaring success. Persist application/database association, safe schema/version metadata where available, time, size, integrity/hash result and operation outcome. Do not expose contents or sensitive paths in history.
- Keep backups outside application release directories and public trees. Apply approved access/retention limits; an incomplete backup must not replace a valid retained backup or appear restorable.
- Restore requires explicit authorization, destructive-action confirmation, verified application/database/backup identity, integrity and compatibility checks, adequate space, and controlled application downtime or coordinated access that excludes conflicting writers/connections. Coordinate SQLite sidecar handling through the approved provider; do not overwrite a live file blindly.
- Stage and validate the restore with a documented failure/recovery plan that preserves the current recoverable database. Prevent restore from overwriting the Registry, another application's database, an unrelated registered file or any path outside approved storage. Revalidate target containment/identity before the destructive step.
- Record restore status, audit actor/outcome and post-restore integrity/application health evidence before reporting success. A database restore and an application release rollback are distinct authorized operations; neither implies automatic permission for the other.

### 9.1.9. Existing application SQLite files and current IHP compatibility

**Approved requirements:** Preserve application-relative database paths; do not rewrite uploaded source, binaries, configuration or connection strings; do not merge databases; support multiple separately identified databases; do not create an empty replacement when a valid existing database is supplied; keep existing database content by default on upgrade. These requirements do not make the workflow available today.

**Current implementation observed during TASK 022 (read-only source inspection; not a new test run):**

- Folder intake in `src/Api/src/folder-intakes.ts` applies an explicit allowlist and denylist in `validateContentPath`. It returns HTTP 415 `folder_file_type_unsupported` when an entry is denied or absent from the allowlist. `.db`, `.sqlite`, `.sqlite3` and `.sql` are explicitly denied. `.db-wal` and `.db-shm` are not in the allowlist, so they hit the unsupported-type rule. `.webmanifest` is also absent from the allowlist. A `.cmd` file is allowed only when its basename is exactly `run.cmd`; that file remains inert and is never executed. `.git` and other excluded development directories are rejected/excluded by directory policy; `.gitignore` is a filename exception. The Dashboard maps `folder_file_type_unsupported` to the generic text “The folder contains an unsupported file type.” The response does not identify the first rejected relative path, so when a folder has several disallowed entries the exact first entry cannot be determined from that message alone.
- The canonical folder-package generator in `src/Api/src/folder-packages.ts` has its own narrower package extension allowlist and reports `package_file_type_forbidden`; changing folder intake alone would not make database files part of a valid release package.
- ZIP inspection in `src/DeploymentAgent/PackageArchive.cs` independently rejects entries outside its code/static-file allowlist with `package_file_type_forbidden`. `.db`, `.sqlite`, `.sqlite3`, `.db-wal`, `.db-shm` and `.sql` are not valid release-package entries. Current ZIP checks also bound compressed archive size to 50 MiB, total expanded bytes to 32 MiB, each entry to 8 MiB, entry count to 256, and expansion ratio to 100:1 with a 4096-byte floor; paths/features/CRC are validated and links are rejected. Keep this allowlist narrow: database artifacts require a separate reviewed intake path, not a global package type exception. ZIP structural/security failures use other codes such as `zip_invalid`, `zip_features_unsupported`, `zip_path_unsafe` and `zip_link_forbidden`; do not conflate those with the folder intake's generic file-type message.
- M7 stores uploaded ZIPs under server-generated `Uploads/<deployment-id>` paths. It records a release fingerprint over deployment identity and archive/metadata/file-manifest evidence; distinct Administrator review binds to that exact fingerprint. The Agent rechecks approval and hashes, then extracts a non-overwriting release to `Applications/<application-uuid>/releases/<version>`. Existing releases are preserved.
- M5 `RuntimeEngine` currently sets `ProcessStartInfo.WorkingDirectory` to the selected release directory and launches the reviewed entry point from that release. `ReleaseInspector` and the package workflow inspect/hash release files. There is no persistent app workspace, application-database association, SQLite configuration contract, database backup/restore service or database lifecycle API. Database-dependent M7 deployments remain blocked by the file policies and missing runtime database support.
- M5 supervises owned process stop/restart (restart is stop then start). M10 version selection/rollback is future work. No application database state is coordinated by the current process lifecycle.

**Exact error assessment:** For the supplied file classes, `.db` and `.sql` hit explicit deny rules; `.db-wal`, `.db-shm` and `.webmanifest` hit the allowlist-miss rule; a `.cmd` other than exact `run.cmd` is rejected. Thus multiple entries can independently cause the same generic folder error. The API response omits the offending path, so this inspection cannot identify which entry failed first in a particular upload. The verified cause is the unsupported-file rule, not an unreadable-ZIP conclusion. ZIP releases containing those data entries are rejected separately by the PackageArchive allowlist.

### 9.1.10. Proposed persistent workspace and compatibility boundary

**Proposed, not implemented:** Keep exact code/release artifacts in their existing versioned release directories and retain their code-only approval fingerprint. Keep application database files in stable application-owned workspace/data paths outside every release. Stage candidate database artifacts in private `Temporary` storage until independently validated and explicitly selected. Store SQLite-consistent backups under the application's non-public backup area. Registry metadata stores opaque IDs, mappings, state and provenance references, never database contents or deployed-application connection secrets.

```text
<IHP_STORAGE_ROOT>/
|-- registry/platform.db                  # Platform Registry only
|-- Applications/<application-uuid>/
|   |-- releases/v1/                       # approved release artifact
|   |-- releases/v2/
|   |-- workspace/                         # persistent app-relative data paths
|   |   `-- database/workcenter.db         # example; preserve declared relative path
|   |-- data/application.db                # default managed path only when applicable
|   |-- backups/<database-id>/<backup-id>/ # SQLite-consistent backup
|   |-- logs/
|   `-- config/                            # access-controlled, not release content
|-- Uploads/                               # existing package intake
|-- Temporary/DatabaseImports/<opaque-id>/ # proposed private DB-artifact staging
`-- Logs/
```

The release tree remains the source of executable code and package-integrity evidence. The workspace contains data only for the narrow supported path contract; it must not become a second mutable source of executable files. The conceptual alternative of mirroring `dist/`, `package.json` and the full application tree into a writable workspace could preserve more module-relative paths, but is rejected as the default: launching from that mutable copy would disconnect executed code from the approved release fingerprint, while same-identity code can mutate either tree and no approved filesystem overlay/sandbox exists. A proposed runtime mode may set a reviewed fixed workspace-root working directory while launching the entry point by its absolute path under the approved release. This is compatible only with database paths demonstrably resolved from that working directory and with the application's other working-directory expectations. Paths derived from Node `__dirname`/module location, .NET assembly/base directory, hardcoded absolute locations, arbitrary environment-variable names or external paths cannot be transparently redirected without explicit app configuration/code changes or another approved mechanism. Do not rewrite binaries/configuration, add junctions/symlinks, copy an executable tree into the writable workspace, or claim universal compatibility. Review and verify each application's actual path behavior. Current M5 instead uses the release as working directory; the proposed change requires separate authorized M5/M10-DB implementation.

Application processes and Agent currently use the permitted Windows identity. Hash checks and application-scoped paths do not stop malicious code under that identity from writing files it can access, potentially including release or other user-writable locations. A writable workspace, process separation, SQLite file separation, managed-code validation and loopback binding are **not** OS isolation or a sandbox. Keep trusted-reviewed-code and exact-approval gates; do not claim protection against a malicious approved process without a separately approved identity/ACL or stronger IT-approved isolation boundary.

### 9.1.11. Proposed candidate detection and SQLite sidecar handling

Database candidate discovery is a separate, review-only data-artifact lane. It must not widen global folder/package extension allowlists or treat every `.db` as active SQLite data. Require an explicit manifest/owner selection mapping an opaque candidate ID to a normalized relative path and database identity. Treat extension, SQLite header/profile, file identity, size and bounded read-only validation as evidence, not proof of application use or business compatibility. Support multiple distinct candidates without merging; never select or replace by basename alone. Keep `.sql` inert and excluded. No uploaded script, application code or extension may execute for validation.

An import source must be stopped/quiescent or first captured by an approved SQLite-consistent mechanism. A byte copy of an active `.db` is insufficient. In WAL mode the matching `.db-wal` may contain committed state and must remain associated with the exact database snapshot; do not separate, rename or discard it before a consistent snapshot is produced. `.db-shm` is a transient WAL index/coordination file, not authoritative database content; do not promote it as application data or copy it as a substitute for a consistent snapshot. Require a quiescent source/provider workflow that permits SQLite to rebuild transient shared-memory state. Reject ambiguous/mismatched sidecars or report a clear review-only result. See SQLite's [WAL documentation](https://www.sqlite.org/wal.html), [WAL format](https://www.sqlite.org/walformat.html) and [Online Backup API](https://www.sqlite.org/backup.html).

The future validator should use an approved in-process SQLite provider opened read-only, with extension loading disabled, fixed validation operations (for example `PRAGMA integrity_check`, and `foreign_key_check` only if separately approved), bounded database/page/output/time/resource budgets and no application SQL. `integrity_check` is structural evidence, not a schema/business compatibility or safety proof. `query_only` alone is not a read-only boundary. SQLite's [PRAGMA reference](https://www.sqlite.org/pragma.html) documents integrity checks. Same-identity code may still access files available to that identity; validation is not isolation.

### 9.1.12. Proposed first-deploy, update, replacement and recovery workflow

1. **Review and separate intake:** Preserve the current M7 code-package path and its archive/metadata/file-manifest fingerprint. A separate authenticated data-artifact intake stages explicitly submitted candidates under private temporary storage; it cannot set arbitrary destinations, execute scripts, approve packages or deploy. Candidate validation and data provenance are recorded separately from the release code fingerprint. Exact approval/ownership requirements for data artifacts and whether distinct Administrator review is needed remain owner decisions.
2. **First deployment:** Select NONE, MANAGED_SQLITE or EXISTING_SQLITE under M11. NONE provisions nothing. If a valid existing candidate is supplied, preserve it and its relative path; do not silently create an empty file or overwrite it. An imported file copied into platform-managed workspace likely maps to MANAGED_SQLITE with `imported-existing` provenance, but this interpretation needs owner approval. EXISTING_SQLITE remains a separately registered/authorized file; registration alone grants no create, modify, backup or deletion authority.
3. **Upgrade default — Keep Existing Database:** Default every candidate action to KEEP. Validate/review incoming candidate metadata, but promote no incoming data artifact. Install/select the approved v2 release while retaining the same app workspace/database IDs, files and relative paths. Never delete or recreate data as a deployment side effect.
4. **Explicit replacement:** Require an authenticated non-Viewer actor with server-checked current ownership/authority, a separate authorized replacement operation, per-database selection and fresh destructive confirmation. Stop the owned application; verify it is stopped and has no known writers. Recheck candidate/target IDs, app/path containment, current-file identity/revision and idempotency key. Create and verify a non-overwriting SQLite-consistent backup; validate candidate read-only; promote through a journaled staging/rename protocol with recovery that preserves the prior database. Never merge or overwrite the Registry/another app's file. Restart only through the separately authorized approved runtime lifecycle where it is enabled; this data operation does not authorize package approval or code deployment.
5. **Related database groups:** Multiple databases are independent unless the application owner declares a consistency group. If related, backup/replacement must be one coordinated stop/backup/validate/promote/recovery transaction for the group. No success is reported until every member is consistent. Group declaration and atomicity semantics require approval. No cross-file merge is supported.
6. **Failure/restart/rollback:** On validation or pre-promotion failure, retain active data and keep the package in review. On interruption, recover from a durable operation journal and preserve old and staged data until a verified outcome. Application rollback selects older approved code and leaves workspace/database unchanged. If schema compatibility is unknown or unsafe, block or require an explicit operator decision; never reverse schema changes or automatically restore a backup.

### 9.1.13. Proposed API and Dashboard contracts (unimplemented)

No endpoint or Dashboard behavior below is implemented. Names and payloads are design proposals requiring API/M11 review. Keep operations application-scoped and explicit; do not add a generic filesystem browser, arbitrary path input or generic proxy.

| Proposed operation | Contract sketch | Required server checks |
| --- | --- | --- |
| Submit candidates | `POST /_platform/api/applications/{id}/database-imports`; bounded multipart streams, normalized relative paths and idempotency key; return `202`, opaque `importId`, `REVIEW_ONLY`, candidate IDs and sanitized validation states. | Active session, `application.write`, current owner/Admin policy, quotas, safe paths/sidecars; no destination path disclosure, code execution or active DB changes. |
| Inspect import | `GET /_platform/api/applications/{id}/database-imports/{importId}`; candidate IDs, app-relative paths, sizes, type/integrity/snapshot result, status and safe diagnostics. | Recheck ownership and import-to-app association; no contents, host paths or secrets. |
| Read database state | `GET /_platform/api/applications/{id}/databases`; mode, opaque IDs, approved relative mapping, status, safe size/limit and backup state. | M11 RBAC and current ownership; Viewer read-only; no raw file/path access. |
| Replace selected data | `POST /_platform/api/applications/{id}/database-replacements` with `{ importId, selections: [{ databaseId, candidateId }], expectedTargetRevision, confirm: true, idempotencyKey }`; return operation ID/status. | Non-Viewer authorization/ownership, confirmation, candidate/target identity, verified prior backup, stopped runtime, lock/journal, group semantics and fail-closed recovery. Whether distinct Admin approval is additionally required is open. |
| Create backup | `POST /_platform/api/applications/{id}/databases/{databaseId}/backups` with idempotency key and optional approved consistency-group ID; return `202` operation ID/status. | Explicit M11 authorization/ownership, fixed SQLite-consistent provider, verified app/database identity, contained non-overwriting target, writer/snapshot coordination and bounded resources. |
| Restore backup | `POST /_platform/api/applications/{id}/databases/{databaseId}/restore` with `{ backupId, expectedTargetRevision, confirm: true, idempotencyKey }`; return operation ID/status. | Explicit authorization and destructive confirmation, backup provenance/integrity, current target identity, compatibility/downtime plan, writer exclusion and recoverable staging; never arbitrary path. |
| Retention/deletion | `PATCH /_platform/api/applications/{id}/databases/{databaseId}/retention` to set a reviewed policy; `POST .../deletion-requests` for explicit deletion review. | Authorized M11 policy, explicit retention choice, verified identity, required confirmation/approval and audit; registration or application deletion never implies file deletion. |
| Read operation history | `GET /_platform/api/applications/{id}/database-operations`; audit-safe IDs, times and outcomes. | Application scope and RBAC; no content/raw path. Backup/restore remain separate authorized operations, not ordinary reads. |

Dashboard should show detected candidates, exact relative paths and sizes, validation state, current versus incoming data, per-file action and group warnings. On first deployment and upgrade, preselect **Keep Existing Database (Recommended)**. Replacement requires separate selection/confirmation, explains data-loss and backup status, and shows compatibility as unknown unless verified. Show NONE clearly and continue supporting database-free apps. Never provide raw SQLite downloads, contents, secret configuration, fabricated status, silent deployment or automatic SQL migration. Viewer is read-only; UI checks never replace backend enforcement. These controls remain planned/disabled until M10-DB and secure M8 acceptance are separately completed.

### 9.1.14. Compatibility limits and product-owner decisions

These are implementation gates, not silently chosen defaults:

1. Confirm the supported path contract. Proposed MVP: reviewed working-directory-relative SQLite paths under workspace. Decide handling for module/assembly-relative paths, CWD-relative non-database assets, application config files and apps that assume release CWD. Unsupported paths stay blocked; never rewrite files automatically.
2. Approve the relationship among `data/application.db`, app-relative workspace paths and `MANAGED_SQLITE`/`EXISTING_SQLITE`, including whether imported-and-relocated existing data is managed with provenance.
3. Define candidate manifest rules, supported SQLite format, maximum database count/bytes/pages, WAL bundle rules, treatment of missing/mismatched sidecars and the quiescence/snapshot procedure. `.db` extension alone is insufficient.
4. Decide whether replacement requires independent Administrator review in addition to owner authority and destructive confirmation; define self-approval and separation-of-duties rules under M11.
5. Define multi-database consistency-group declaration, all-or-nothing replacement/backup behavior and recovery semantics.
6. Review approved SQLite provider/API availability, read-only snapshot/backup support, resource budgets, retention, quotas, default limits and restore downtime/recovery requirements.
7. Decide whether future runtime identity/ACL/isolation is required. Current same-user execution cannot promise isolation from a malicious app; absent an approved boundary, support remains limited to trusted reviewed code and narrowly approved data paths.

No decision above authorizes Docker, elevation, Windows security changes, an external database engine, uploaded-code execution, unapproved source builds, package-allowlist expansion, Registry/application data sharing or corporate-network listening.

### 9.1.15. My Work Center compatibility case study — TASK 023

This is a read-only inspection of the local `22.My work Center Platform/sites/my-work-center` application folder and IHP source. It did not execute the application, build it, query a database, change files, or verify database integrity. The product owner has already approved first-use of a valid supplied SQLite database where safe, upgrade default KEEP, and explicit guarded replacement. The technical compatibility decisions below remain open.

A separate TASK 022 review artifact is documented in `PROGRESS.md`: it was an adapted export in a temporary directory, with an IHP manifest and authored Node adapter, and it passed the package inspector. It omitted the application database and was never runtime-tested. That result verifies only that adapted artifact's archive/package checks; it does not establish that the original application, original folder structure, or its existing database works with IHP unchanged.

#### SQLite inventory and path resolution

- Static application source in `server/index.mjs` names one active server database: `data/my-work-center.db` relative to the application root. `root` is computed from `import.meta.url` (the module directory), not from `process.cwd()`. `WORK_CENTER_DB_PATH`, if set, overrides that default and is resolved to an absolute path by Node's `resolve`. `server/database.mjs` also resolves the supplied path before opening it.
- The folder contains this one referenced database (`data/my-work-center.db`, 495,616 bytes at inspection) and four files under `data/backups/`: `my-work-center-2026-10-25.db` (45,056 bytes), `my-work-center-2026-10-27.db` (249,856 bytes), `my-work-center-2026-10-28.db` (385,024 bytes), and `my-work-center-2026-10-29.db` (446,464 bytes). Source code identifies the latter as app-created snapshots, not active databases: one date-stamped snapshot is made through SQLite's backup API at startup and the app prunes its own history to 30 files. These files were inventoried by path/size only; their contents/integrity were not checked.
- `data/my-work-center.db-wal` (412,032 bytes) and `data/my-work-center.db-shm` (32,768 bytes) existed at the later metadata listing. Application source sets `PRAGMA journal_mode = WAL`, and an earlier read-only open attempt reported that the main DB was in use by another process. The process owner was not identified or stopped. File sizes may change. No active-file copy, database query, header/integrity check or snapshot was attempted; the sidecars and source code make a live/WAL-aware snapshot requirement material, not a verified database-health result.
- `server/database.mjs` uses Node's built-in `node:sqlite` `DatabaseSync` and `backup` APIs. It creates the parent directory and opens with the default create-if-missing behavior, sets WAL/FULL/busy_timeout, creates `local_migrations`, then reads sorted `drizzle/*.sql` files. It checks each migration's SHA-256 against `local_migrations`; if a migration is absent, it executes the file with `db.exec(sql)` during startup and records it. Therefore a missing DB can become a new DB and pending uploaded migrations run automatically unless IHP blocks that path before launch. Both behaviors conflict with the approved IHP data-preservation and no-automatic-uploaded-SQL rules.
- Static server imports reference only the Node built-in SQLite and local server modules. `package.json` requires Node `>=24.0.0`; the project machine's recorded Node 24.22.1 meets that requirement. `sql.js` 1.15.1 is declared as a package dependency but is vendored under `dist/vendor/sql-asm.js` for browser-side SQLite import/export. It is not required by the Node server import graph inspected. The current M7 folder-package analyzer nevertheless rejects any non-empty `dependencies` object, so this declaration is an independent package-validation blocker without a narrowly reviewed runtime-versus-browser dependency rule.
- `dist/storage.js` keeps an application cache in browser `localStorage` and uses vendored SQL.js in memory for SQLite import/export. Pending local browser changes are not necessarily present in the server DB file or the uploaded package and will not move automatically to a different Gateway origin. This is a separate data-preservation boundary, not another server-side `.db` file.

#### Current IHP and package compatibility

- **Current result: the original My Work Center application is not compatible with IHP as currently implemented under the no-modification requirement.** A database path mapping alone is insufficient. The current folder includes `.git`, `node_modules`, `.webmanifest`, `.sql`, `.cmd`, and database/backup files that the folder/package intake rejects or excludes. The IHP ZIP inspector also excludes SQLite/SQL files and requires a root-level IHP `platform.json` or `platform.yaml`; the original application folder has no such root IHP manifest. The earlier operator-supplied ZIP report described 16,927,697 bytes and 466 entries: 16,927,697 is below the 50 MiB compressed limit, but 466 exceeds IHP's 256-entry limit. That archive report is prior operator evidence, not a new archive inspection for this task.
- `package.json`'s `start` runs `node scripts/prepare-client.mjs && node server/index.mjs`; IHP must not run this chained script or an npm lifecycle/build script. `scripts/build-hosted.mjs` generates a Worker/D1-oriented `dist/server/index.js`, while `server/index.mjs` is a distinct local Node HTTP adapter. A direct reviewed `node server/index.mjs` entry could avoid `npm start`, but it still needs `dist`, `drizzle` and local server files. `server/index.mjs` reads migration SQL at runtime, so simply omitting `.sql` files does not work; current PackageArchive rejects those files. The package declares the `sql.js` dependency, which the current M7 analyzer also rejects. No package was prepared during TASK 023. The separate TASK 022 adapted export passed PackageInspector but included authored adapter/metadata changes, omitted DB files, and was not run; it does not demonstrate unchanged-app or existing-database compatibility.
- The Node server binds `127.0.0.1` and reads `PORT`, which aligns with loopback/port injection. However, it does not implement the M5 `/health` contract: M5 requires `{status:"ok", release, build, port}` at the canonical app `/health` path, while static scan found no such route in the app server/API. The app's static asset resolution and API calls use root/module paths (`/api/state`, `./vendor/...`) and do not account for the IHP `/mwc/` prefix. M6 forwards path prefixes; therefore the app cannot be assumed to work under that route.
- The server exposes unauthenticated `GET /api/backup.db`, which creates a SQLite backup and returns raw database bytes. It validates only the Host header, not an IHP M11 session/role. This conflicts with section 9.1's restriction on raw SQLite downloads through public Gateway routes. The server also logs the resolved database path. No path-specific Gateway denial exists today. Shared local application use does not authorize exposing this backup route if Gateway access is later broadened.
- The server/API has no application-user authentication; the previously approved shared-local access model is not per-user isolation. Do not expose it beyond the approved loopback boundary or imply that Dashboard/M11 protects the app route.

#### Runtime strategy assessment

| Approach | Path compatibility | Fingerprint, security and release lifecycle | Finding for My Work Center |
| --- | --- | --- | --- |
| **A. Keep the original tree under each immutable release** | The module-root default resolves `data/my-work-center.db` under that release. CWD is irrelevant. Module-relative app files work. Absolute paths work only if they match that exact tree; paths tied to old host locations do not. Similar ASP.NET applications may use content-root/assembly-relative paths. | The DB and backups become release contents, are fingerprinted as code, can be replaced only by creating a new release, and are not stable across versions. This violates release/data separation and cannot satisfy KEEP by default. Package validator currently rejects these files. | **Reject.** It preserves default path resolution but fails persistence, policy and package acceptance. |
| **B. Launch approved code from release with workspace as CWD** | Works only for applications whose database and relevant assets are genuinely CWD-relative. It does not redirect this app's default path because the app derives `root` from `import.meta.url`; it also does not change Node `__dirname` or .NET assembly/content-root references. | Launching code from the immutable release retains code fingerprint evidence, but a mutable working tree is not an overlay and may break other relative assets. Same-user code can access permitted files. KEEP could work only for paths actually resolved beneath workspace. | **Insufficient by itself.** Does not map the My Work Center DB default. |
| **C. Use an explicit app-specific persistent path mapping** | My Work Center already supports `WORK_CENTER_DB_PATH`. A future Agent could pass a canonical absolute workspace path ending in the same relative suffix, e.g. `<app>/workspace/data/my-work-center.db`; source and checked-in config stay unchanged. This is not literal preservation of the old absolute resolved path—the storage root changes. Relative values must not be accepted because `resolve` would depend on CWD. Apps with no documented override, or module/assembly-relative paths, still need another approved mechanism or remain unsupported. ASP.NET Core is conditional on its actual provider/content-root behavior. | Code package/fingerprint can remain separate from DB bytes; bind the exact environment variable to the authorized database resource in Registry metadata and recheck canonical app/path identity. Existing `TrustedEnvironment` clears inherited variables and does not currently inject `WORK_CENTER_DB_PATH`; it must not become arbitrary environment injection. Stable mapping supports KEEP and code rollback but not schema compatibility. | **Best technical candidate for this app, conditional on approval.** It changes the physical resolved path, so the owner must confirm that preserving the source path expression and `data/my-work-center.db` suffix under workspace satisfies “database paths unchanged.” It is not implemented. |

For My Work Center, even approach C does not resolve auto-created DBs/migrations, SQL and dependency allowlists, missing health contract, `/mwc/` base-path incompatibility, unauthenticated raw-backup route, or browser-local pending data. General published Node/ASP.NET support cannot be inferred from this one application; path-base and database-provider behavior must be statically reviewed per application.

#### Required safeguards and implementation changes before any compatibility claim

1. Require an existing, app-selected database candidate for first deployment. Do not launch My Work Center against a missing mapped file: `DatabaseSync` creates a new DB, and app startup creates schema/migration state. Candidate intake must be separate from the code ZIP, preserve the approved relative path mapping, distinguish the one active DB from dated backups, and keep every DB/WAL/SHM artifact outside releases/public content. `.db` extension alone does not prove active SQLite data.
2. The source DB must be stopped/quiescent or supplied as an approved SQLite-consistent snapshot. Do not copy the live file alone. The application's own backup API uses SQLite's backup helper, but it is a public raw-DB endpoint and was not invoked; using it as an import source requires a separate policy/authorization decision. Do not inspect/copy the in-use DB in this investigation.
3. Add an explicit validated per-application `WORK_CENTER_DB_PATH` mapping only after owner approval. Supply a canonical absolute target below that app's workspace, keep `data/my-work-center.db` as its relative suffix, clear all other environment variables as today, and never take the target path from the browser/package. This is a targeted M5/M10-DB capability, not a general arbitrary-env feature.
4. Before app launch, use an approved read-only SQLite provider to require the existing database and compare the release's exact `drizzle` migration filenames/hashes with `local_migrations`. Permit startup only when all included migrations are already recorded with matching hashes; block missing/changed/pending migration state. Never execute uploaded SQL or let app startup perform a pending migration. If this gate cannot reliably prevent the app's own startup migration, My Work Center remains unsupported without app modification.
5. Review a narrowly scoped static package policy for the SQL migration files that the app reads and the browser-vendored `sql.js` asset. Current global `.sql` and non-empty dependency rejection remain unchanged unless a separate security-approved rule is authorized. Any SQL file retained for runtime must be hash-reviewed and proven already applied before launch; no npm install, build, prep script or migration execution is authorized. Handle `.webmanifest` only as an explicitly reviewed static asset type; do not loosen global allowlists in this task.
6. Resolve M5 health and M6 route compatibility without weakening them. Either the application must provide the approved exact health contract and base-path behavior, or a narrowly scoped IHP-owned adapter/route policy requires separate design/authorization and verification. Do not claim healthy/routable based only on process start.
7. Deny or otherwise secure the app's `GET /api/backup.db` at the Gateway before any route exposes it; preserve no-raw-DB-download requirements. Review automatic app-side backup pruning (keeps 30 dated files), DB-path logging and same-user file access. Keep these app-owned behaviors distinct from IHP-managed backups/retention. A policy decision is required before allowing a raw export.
8. Treat browser `localStorage` pending data as a separate user-controlled source. It is not in the SQLite file and cannot be claimed preserved by importing the server DB. Define user export/consent/recovery handling before origin changes or migration.

Approved first-deploy/upgrade behavior is already stated above: use valid supplied existing data when safely supported; update defaults to KEEP; replacement is explicit and protected by authorization, confirmation, a consistent backup, safe stop, validation and recovery. Further product/security decisions are: whether a changed absolute path via the app's existing `WORK_CENTER_DB_PATH` override is acceptable while retaining the relative suffix; whether an IHP-only compatibility adapter may supply health/base-path behavior without changing the app; whether the raw app DB-export route can ever be allowed or must be denied; and whether a new/pending app migration must always block deployment until a separately authorized process applies it. Existing policy against automatic SQL execution is not an open question and must not be weakened.

---

# 10. APPLICATION VERSION MANAGEMENT

Section 9.1 governs future application database persistence independently of these releases. The original layout below is illustrative; it does not authorize moving existing paths or placing databases/backups inside releases.

Every deployment must create a new release.

Example:

```text
Applications/
└── quality/
    ├── releases/
    │   ├── v1/
    │   ├── v2/
    │   └── v3/
    ├── logs/
    └── config/
```

The platform must support:

- Deployment history.
- Version identification.
- Active release tracking.
- Release status.
- Deployment timestamps.
- Deployment owner.
- Rollback to previous versions.

## 10.1. Safe Update Workflow

When deploying a new version:

1. Preserve the currently active release.
2. Prepare the new release.
3. Start the new version on a separate internal port.
4. Perform health checks.
5. Update the gateway route.
6. Confirm the new version is accessible.
7. Stop the previous version after successful cutover.

If the new deployment fails, retain the previous working version.

Rollback must also account for database compatibility. Application rollback does not automatically reverse database migrations.

---

# 11. MONITORING & LOGGING

Develop a centralized monitoring interface.

## 11.1. Application Monitoring

Display:

```text
Application Status
CPU Usage
RAM Usage
Process ID
Uptime
Internal Port
Last Deployment
Health Check Status
```

Example:

```text
QUALITY MANAGEMENT

Status: RUNNING
CPU: 4.5%
RAM: 256 MB
Uptime: 2 days
Port: 6102
```

Use process-level metrics available to the Windows account.

If certain system metrics are unavailable without elevated privileges, show them as unavailable rather than requesting privilege escalation.

## 11.2. Logging

Collect:

- Build logs.
- Deployment logs.
- Application stdout.
- Application stderr.
- Gateway errors.
- Health-check failures.
- Process exit events.
- User actions.

Support:

- Log search.
- Log filtering.
- Log timestamps.
- Log download.
- Log retention.
- Secret redaction.

Application logs must be separated by application and release.

---

# 12. AUTHENTICATION & AUTHORIZATION

The platform must not allow anonymous deployment.

Implement role-based access control.

Recommended roles:

| Role | Permissions |
|---|---|
| Administrator | Manage platform settings and users |
| Developer | Create and deploy authorized applications |
| Viewer | View authorized applications and status |

The platform must support:

- User authentication.
- Session management.
- Role-based permissions.
- Application ownership.
- Deployment authorization.
- Audit logging.

Design authentication so that Microsoft Entra ID integration can be added later.

For the initial implementation, use a secure local authentication mechanism if company SSO is not yet available.

Do not store plaintext passwords.

Application management endpoints must be protected from unauthorized access.

---

# 13. SECURITY REQUIREMENTS

Security is a mandatory design requirement.

The system operates on a corporate Windows laptop without Docker or Administrator privileges.

Therefore, the Agent must explicitly address the risks of running uploaded code.

## 13.1. Execution Security

The platform must not execute arbitrary untrusted applications under the same unrestricted Windows identity as the management platform.

Implement a controlled deployment policy.

At minimum:

- Only authorized developers can submit packages.
- Deployment approval must be supported.
- Packages must be validated.
- Build commands must be restricted.
- Application execution identities must be explicitly defined.
- File access permissions must be documented.
- Database credentials must be protected.
- Network access restrictions must be documented.

Where adequate isolation cannot be achieved with the available Windows permissions, the platform must restrict deployment to trusted, reviewed applications.

The Agent must not attempt to bypass enterprise endpoint security.

## 13.1A. Static startup-intent inspection for M7 (proposed)

Any run.cmd/.bat file is untrusted data. Read/parse it as text only; never invoke it through cmd.exe, ShellExecute, PowerShell, WSH, a manifest command field or any other interpreter. Do not run a command to ask the script what it would do. Never use cmd.exe /c for an uploaded command.

A strict, versioned static grammar may recognize only exact, root-relative application launch forms such as node dist/server/index.js and dotnet App.dll. npm.cmd run start may be considered only as text: parse the start value from package.json, and translate it to a fixed Node/dotnet host and validated relative entrypoint only if the complete script is exactly one supported form and contains no hooks, chaining, arguments with unsafe semantics, expansion or shell metacharacters. Never execute npm.cmd run start; npm lifecycle hooks are not part of the approved runtime. Validate target files and framework/dependency metadata by static inspection. Do not accept CMD syntax such as call, start, pipes, redirects, variable expansion, delayed expansion, substitutions, control flow, PowerShell, nested scripts or opaque helpers as a launch configuration.

Unknown, ambiguous, multi-command or unsupported intent must be surfaced as **Administrator review required** and deployment remains blocked. Administrator review is a policy decision on evidence; it does not by itself make an arbitrary command safe or authorize execution. Preserve the run.cmd content only in the access-controlled review source when allowed; never place an executable run.cmd into the deployment package or output raw command contents/secrets to ordinary users or logs. Static script parsing does not build, restore or execute source code. Only a separately approved execution boundary could change the source-build prohibition.

---

## 13.2. Gateway Security

Implement:

- Route validation.
- Reserved path protection.
- Internal destination allowlisting.
- Request size limits.
- Upload limits.
- Authentication for management APIs.
- Protection against arbitrary proxy destinations.
- Secure error handling.

Do not allow users to register arbitrary internal IP addresses or URLs as proxy destinations.

## 13.3. Operational Security

Implement:

- Audit trails.
- Deployment approval states.
- Secure configuration management.
- Backup procedures.
- Recovery procedures.
- Log retention.
- Least-privilege execution.

The Agent must document all limitations introduced by the absence of Docker and Administrator privileges.

---

# 14. PLATFORM DATABASE DESIGN

This is the dedicated Platform Registry schema. Future managed application databases are separate SQLite files governed by section 9.1; store only their management metadata/associations here, never their application tables or contents. No schema/migration change is authorized by the documentation-only assignment.

Design a normalized database schema.

Minimum entities:

### Users

```text
id
username
password_hash
role
is_active
created_at
```

### Applications

```text
id
name
slug
description
framework
runtime
owner_id
status
created_at
updated_at
```

### Deployments

```text
id
application_id
version
deployment_type
package_path
build_status
deployment_status
created_by
started_at
finished_at
```

### RuntimeInstances

```text
id
deployment_id
process_id
internal_port
runtime_status
started_at
stopped_at
```

### Routes

```text
id
application_id
public_path
target_host
target_port
is_active
created_at
```

### DatabaseConnections

```text
id
name
database_type
host
port
database_name
credential_reference
created_at
```

### DeploymentLogs

```text
id
deployment_id
level
message
timestamp
```

### AuditLogs

```text
id
user_id
action
resource_type
resource_id
timestamp
```

### ApplicationSecrets

```text
id
application_id
secret_name
protected_value
created_at
updated_at
```

### DeploymentApprovals

```text
id
deployment_id
requested_by
approved_by
status
reviewed_at
```

Use appropriate indexes, constraints and foreign keys.

Application slugs must be unique.

Database changes must use versioned migrations.

---

# 15. BACKEND API DESIGN

Build REST APIs using NestJS.

Suggested endpoints:

### Authentication

```http
POST /_platform/api/auth/login
POST /_platform/api/auth/logout
GET  /_platform/api/auth/me
```

### Applications

```http
GET    /_platform/api/applications
POST   /_platform/api/applications
GET    /_platform/api/applications/:id
PATCH  /_platform/api/applications/:id
DELETE /_platform/api/applications/:id
```

### Deployments

```http
POST /_platform/api/applications/:id/deploy
GET  /_platform/api/applications/:id/deployments
GET  /_platform/api/deployments/:id
POST /_platform/api/deployments/:id/rollback
```

### Runtime Control

```http
POST /_platform/api/applications/:id/start
POST /_platform/api/applications/:id/stop
POST /_platform/api/applications/:id/restart
GET  /_platform/api/applications/:id/status
```

### Monitoring

```http
GET /_platform/api/applications/:id/metrics
GET /_platform/api/applications/:id/logs
GET /_platform/api/system/health
```

### Configuration

```http
GET   /_platform/api/settings
PATCH /_platform/api/settings
```

Implement:

- Input validation.
- Authorization guards.
- Consistent error responses.
- Pagination.
- Filtering.
- API documentation.
- Request logging.

Long-running deployments must execute asynchronously.

The deployment endpoint should return a deployment ID so the frontend can track progress.

---

# 16. FRONTEND DESIGN

Build a professional interface inspired by modern cloud deployment platforms.

## 16.1. Design System

Preferred appearance:

```text
Theme: Dark Mode
Primary Colors: Black / White / Blue
Style: Minimal / Modern / Professional
Layout: Sidebar + Main Content
```

Use:

- React
- TypeScript
- Tailwind CSS
- shadcn/ui
- Lucide Icons

The interface must be responsive.

## 16.2. Required Pages

### Dashboard

Display application statistics and recent activity.

### Applications

Display all applications with search, filters and status indicators.

### New Application

The default M7 intake is a folder-based workflow:

1. Enter the application name.
2. Select its local folder.
3. Review server-detected runtime/startup intent and the aggregate upload size.
4. Submit for package validation and Administrator review.

The Dashboard must not require ordinary users to manually create a ZIP or manifest, enter an application UUID or calculate a fingerprint. The server resolves application identity and creates deployment metadata only from authorized Registry state. Advanced configuration/review information remains available to authorized roles. Folder selection does not authorize source builds or execution; the UI must show review-only/build-required/blocked outcomes honestly. The existing authenticated trusted-prebuilt ZIP workflow remains as a backward-compatible advanced option.

### Deployment Progress

Show real-time deployment progress.

Example:

```text
Validating package          SUCCESS
Detecting framework         SUCCESS
Building application        SUCCESS
Allocating internal port    SUCCESS
Starting application        SUCCESS
Running health check        SUCCESS
Registering route           SUCCESS

DEPLOYMENT SUCCESSFUL
```

### Application Detail

Future M10-DB adds the Application Database Management section defined in section 9.1.7. It is planned, not an implemented M8 capability or a new M8 completion claim.

Display:

```text
Application Name
Framework
Version
Status
Public URL
Internal Port
CPU Usage
RAM Usage
Deployment History
Logs
```

Include:

```text
OPEN
COPY URL
START
STOP
RESTART
UPDATE
ROLLBACK
```

### Monitoring

Show application performance and health information.

### Settings

Manage:

- Users.
- Roles.
- Runtime configuration.
- Deployment policies.
- Application limits.
- Platform configuration.

---

# 17. FILE STORAGE STRUCTURE

For Managed Application Databases, section 9.1.3 defines the target logical `IHP_STORAGE_ROOT` structure, dedicated `registry/platform.db` and per-application `data/application.db`. Preserve current paths/configurations; the historical example below does not override those database separation requirements or authorize an automatic storage migration.

Use a configurable storage root under the authorized Windows user profile.

Example:

```text
C:\Users\<username>\InternalHosting\
│
├── Platform\
│   ├── Gateway\
│   ├── API\
│   ├── Frontend\
│   └── DeploymentAgent\
│
├── Applications\
│   ├── quality\
│   │   ├── releases\
│   │   ├── config\
│   │   └── logs\
│   │
│   ├── idea\
│   └── iot\
│
├── Uploads\
│
├── Temporary\
│
├── Database\
│   └── platform.db
│
├── Logs\
│
└── Config\
```

Do not hardcode the Windows username.

All filesystem paths must be configurable.

Uploaded packages must be extracted safely.

Application storage must be separated from Platform source code.

---

# 18. DEVELOPMENT PHASES

Implement the project incrementally.

## Phase 1 – Foundation

Build:

- Monorepo structure.
- Platform API.
- Database schema.
- YARP Gateway.
- Basic authentication.
- Basic dashboard.

Deliverable:

A working management portal accessible through localhost.

## Phase 2 – Routing Proof of Concept

Create two sample applications:

1. Node.js demo.
2. ASP.NET Core demo.

Expected routes:

```text
http://127.0.0.1:5050/demo-node/
http://127.0.0.1:5050/demo-dotnet/
```

Both must operate simultaneously.

Demonstrate that the gateway can register and remove routes dynamically.

## Phase 3 – Application Deployment

Implement:

- ZIP upload.
- Package validation.
- Runtime detection.
- Prebuilt deployment.
- Internal port allocation.
- Process management.
- Health checks.

## Phase 4 – Source Deployment

Implement:

- Node.js source builds.
- React builds.
- ASP.NET Core builds.
- Approved build templates.
- Build logs.
- Deployment configuration.

Do not enable unrestricted arbitrary build commands.

## Phase 5 – Professional Features

Implement:

- Deployment history.
- Application versioning.
- Rollback.
- Monitoring.
- Logs.
- User management.
- RBAC.
- Audit logging.

## Phase 6 – Hardening

Implement:

- Security testing.
- Failure recovery.
- Backup and restore.
- Resource controls.
- Concurrent deployment handling.
- Error handling.
- Operational documentation.

---

# 19. TESTING REQUIREMENTS

The Agent must provide automated tests and manual verification procedures.

## 19.1. Functional Tests

Verify:

- Application registration.
- Duplicate slug prevention.
- ZIP upload.
- Framework detection.
- Node.js deployment.
- ASP.NET Core deployment.
- Dynamic route registration.
- Application startup.
- Application shutdown.
- Application restart.
- Health checks.
- Version updates.
- Rollback.
- Log retrieval.

## 19.2. Failure Tests

Verify:

- Invalid ZIP.
- Missing runtime.
- Build failure.
- Application startup failure.
- Port conflict.
- Application crash.
- Gateway restart.
- Deployment interruption.
- Invalid configuration.
- Unauthorized deployment.
- Insufficient disk space.

## 19.3. Security Tests

Verify:

- Unauthorized API access is rejected.
- ZIP path traversal is blocked.
- Reserved routes cannot be overwritten.
- Arbitrary proxy destinations are rejected.
- Credentials do not appear in logs.
- Deployment permissions are enforced.
- Uploaded packages cannot modify Platform files through extraction.

## 19.4. Compatibility Tests

Test at least:

1. React + Node.js API.
2. React + ASP.NET Core API.
3. ASP.NET Core MVC/Razor.
4. Node.js prebuilt application.
5. ASP.NET Core published application.

Verify static assets, redirects, API requests, cookies and routing under application URL prefixes.

---

# 20. ACCEPTANCE CRITERIA

The platform is considered functionally complete when:

**AC-01:** The management portal is accessible through the configured Gateway.

**AC-02:** An authorized developer can create an application.

**AC-03:** An authorized developer can upload a ZIP package.

**AC-04:** The platform supports approved Node.js and ASP.NET Core deployments.

**AC-05:** Applications run on separate internal processes.

**AC-06:** Internal runtime ports are not directly exposed to the company network.

**AC-07:** Each deployed application receives a unique URL.

**AC-08:** New routes can be registered without restarting the Gateway.

**AC-09:** Application health and deployment status are visible.

**AC-10:** Developers can start, stop and restart authorized applications.

**AC-11:** Deployment logs are available.

**AC-12:** Application version history is maintained.

**AC-13:** Rollback is supported for compatible releases.

**AC-14:** Authentication and role-based permissions are enforced.

**AC-15:** The platform runs without Docker.

**AC-16:** Normal operation does not require Administrator privileges.

**AC-17:** Existing applications remain unaffected.

**AC-18:** No new externally accessible port is required for each new application.

**AC-19:** Unsupported application packages produce clear validation errors.

**AC-20:** The platform provides complete installation, configuration and operating documentation.

---

# 21. REQUIRED DELIVERABLES

The Agent must produce:

### 1. Complete Source Code

Include:

```text
Frontend
Backend API
YARP Gateway
Deployment Agent
Database Schema
Shared Contracts
Configuration
```

### 2. Documentation

Include:

```text
README.md
ARCHITECTURE.md
INSTALLATION.md
DEPLOYMENT.md
SECURITY.md
API.md
TROUBLESHOOTING.md
```

### 3. Sample Applications

Provide:

- Node.js sample application.
- ASP.NET Core sample application.
- React frontend sample.
- Example deployment manifests.

### 4. Automated Tests

Provide unit tests, integration tests and deployment smoke tests.

### 5. Windows Startup Instructions

Document how to start all components without Administrator privileges.

If unattended operation requires IT configuration, document that dependency rather than attempting to bypass it.

### 6. User Guide

Explain:

- How to upload applications.
- How to deploy applications.
- How to access application URLs.
- How to update applications.
- How to view logs.
- How to restart applications.
- How to rollback versions.

---

# 22. IMPLEMENTATION RULES FOR THE CODING AGENT

Follow these instructions strictly.

**Rule 1:** Do not use Docker.

**Rule 2:** Do not require Administrator privileges for development or ordinary operation.

**Rule 3:** Do not modify Windows Firewall or security policies.

**Rule 4:** Do not disable PowerShell Execution Policy.

**Rule 5:** Use `npm.cmd` when invoking npm from PowerShell.

**Rule 6:** Do not modify existing applications or their ports.

**Rule 7:** Use port 5050 as the only intended network-facing entry point.

**Rule 8:** Bind internal application processes to localhost.

**Rule 9:** Do not expose the Deployment Agent directly to the company network.

**Rule 10:** Implement dynamic routing rather than hardcoded application routes.

**Rule 11:** Prioritize reliability, security and maintainability.

**Rule 12:** Do not execute untrusted uploaded code without an approved security boundary or deployment approval process.

**Rule 13:** Do not assume all applications are compatible with path-based routing.

**Rule 14:** Implement error handling and logging throughout the system.

**Rule 15:** Keep all paths, ports and runtime settings configurable.

**Rule 16:** Provide clear setup and operating instructions.

**Rule 17:** Do not claim that a feature works unless it has been implemented and tested.

**Rule 18:** Do not deploy to the company network before local testing and authorization.

**Rule 19:** Identify any feature that cannot be implemented safely under the current Windows permissions.

**Rule 20:** Never bypass company IT restrictions to satisfy a functional requirement.

---

# 23. AGENT EXECUTION INSTRUCTIONS

You are acting as a Senior Software Architect, Full-Stack Engineer and DevOps Engineer.

Your responsibility is to design and implement the Internal Application Hosting Platform described above.

Do not stop after generating a high-level architecture or a development plan.

Produce actual working source code.

Use an incremental implementation approach.

### Step 1 – Architecture Review

Before coding:

1. Review all requirements.
2. Identify technical risks.
3. Validate the proposed technology stack.
4. Define component boundaries.
5. Design the database.
6. Design the deployment lifecycle.
7. Define security boundaries.
8. Identify any required IT approvals.

Document assumptions and unresolved dependencies.

### Step 2 – Project Initialization

Create the project structure.

Initialize the frontend, backend, Gateway and Deployment Agent.

Configure shared development settings.

### Step 3 – Gateway Implementation

Implement the YARP Gateway.

Demonstrate routing to Node.js and ASP.NET Core sample applications.

Use localhost for initial testing.

### Step 4 – Platform API

Implement the Application Registry, authentication and deployment APIs.

### Step 5 – Deployment Engine

Implement controlled package handling, application startup, process management and health checks.

### Step 6 – Frontend

Build the complete dashboard and application management interface.

### Step 7 – Professional Features

Implement version management, rollback, monitoring, logs and authorization.

### Step 8 – Testing

Execute the relevant automated tests.

Fix discovered issues.

Document test results.

### Step 9 – Delivery

Provide:

- Complete source code.
- Setup instructions.
- Development commands.
- Test commands.
- Configuration examples.
- Sample deployment packages.
- Security limitations.
- IT approval dependencies.
- Operational documentation.

### Important Working Method

Implement the system in independently testable milestones.

After each milestone:

1. Summarize completed functionality.
2. Identify created or modified files.
3. Run relevant tests.
4. Report test results.
5. Identify remaining issues.
6. Proceed to the next milestone when dependencies are satisfied.

Do not silently skip requirements.

If a feature is blocked by the absence of Administrator privileges, Docker or an approved isolation mechanism, explain the limitation and propose a compliant alternative.

Do not weaken security controls merely to make a demonstration succeed.

---

# 24. FINAL PRODUCT VISION

The finished product should provide a developer experience similar to:

```text
Developer
    |
    ▼
Open Internal Hosting Platform
    |
    ▼
Create Application
    |
    ▼
Upload ZIP / Source Code
    |
    ▼
Configure Application
    |
    ▼
Deploy
    |
    ▼
Platform Builds & Starts Application
    |
    ▼
Gateway Registers Application URL
    |
    ▼
Deployment Successful
    |
    ▼
http://10.141.79.182:5050/application-name/
```

The ultimate objective is to transform the company's existing Windows laptop into a controlled internal application hosting environment.

Developers should be able to deploy approved Node.js and ASP.NET Core applications through a centralized interface without repeatedly requesting new firewall ports.

The platform must remain compliant with company IT restrictions.

**START WITH ARCHITECTURE VALIDATION AND PHASE 1 IMPLEMENTATION. BUILD REAL, TESTABLE SOFTWARE — NOT JUST MOCKUPS OR DESIGN DOCUMENTS.**
