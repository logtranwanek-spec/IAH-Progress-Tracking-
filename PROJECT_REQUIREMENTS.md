# MASTER PROMPT
# INTERNAL APPLICATION HOSTING & DEPLOYMENT PLATFORM
**Project Code:** Internal Hosting Platform (IHP)  
**Project Type:** Internal Developer Platform / Self-Service Application Hosting  
**Target Environment:** Windows 11 Enterprise Environment  
**Development Approach:** Production-oriented, modular architecture  
**Primary Objective:** Self-service deployment of Node.js and ASP.NET Core applications through a centralized hosting platform.

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

---

# 1. PROJECT OVERVIEW

## 1.1. Background

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

---

# 10. APPLICATION VERSION MANAGEMENT

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

Provide a deployment wizard:

```text
Step 1: Application Information
Step 2: Upload Package
Step 3: Framework Configuration
Step 4: Database Configuration
Step 5: Review & Deploy
```

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
