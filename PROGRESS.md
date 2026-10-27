# Project Progress

**Updated:** 2026-10-27 (Asia/Bangkok)

## Current milestone and handoff

- **Current/last completed milestone:** M11 — Authentication and Authorization, **COMPLETED**, prioritized before M7 by explicit user instruction.
- **Previously completed:** M1–M6 remain COMPLETED; their full regression and real Gateway/Agent integration passed.
- **First unfinished milestone by roadmap order:** M7 — ZIP Upload and Approved Source Builds, NOT STARTED. **Do not start M7 or any other milestone without a new instruction.**
- **Last completed task:** Finished local authentication/session/RBAC/ownership/approval/audit implementation, documented the operating boundary and passed full build/check/test/smoke.
- **Final gate:** `npm.cmd run verify` exited 0: **381 passing checks**, zero failures/skips, plus real platform smoke.
- **Current blockers:** None for approved M11 backend scope. Browser login/TLS/SSO, same-user isolation, actual upload/deployment execution and recovery procedures are not completed capabilities.
- **Git scope:** Commit/push ONLY PROJECT_REQUIREMENTS.md, AGENTS.md, IMPLEMENTATION_PLAN.md and PROGRESS.md to `https://github.com/logtranwanek-spec/IAH-Progress-Tracking-.git`, branch main. Implementation, tests, extra docs, database/credentials and artifacts remain local/untracked. A progress-only clone cannot run the implementation.
- **Continuity:** The user interrupted/resumed while formatting; the existing command completed with exit 0. Work resumed from verification, preserving all earlier work. The complete earlier log/intermediate failures is fenced below, with obsolete statuses/instructions treated as history.

## Mandatory policy and execution boundary

Only **SQLite** performs real database operations. SQL Server/PostgreSQL/MySQL labels/reference APIs remain metadata-only; installation/connections/protocol/authentication tests are prohibited. Their missing connectivity evidence remains the accepted company-policy gap. No such operation/test ran in M11.

All real listeners use literal 127.0.0.1; Gateway remains 127.0.0.1:5050. .NET artifacts are framework-dependent publishes with UseAppHost=false and run as `dotnet <published>.dll`. No apphost/.exe artifact, VSTest/testhost, native Prisma engine, database server, Docker, elevation, Windows security change, production access or corporate listening/deployment was used. Installed node/dotnet/git/npm.cmd hosts follow the existing project convention. No new package/native executable dependency was added.

No actual platform registry/user was migrated or bootstrapped by verification. Tests use owned temporary profile storage and new reviewed copies of the samples; original/existing applications are unchanged. Source/build outputs remain local by instruction.

## Features implemented and acceptance evidence

| M11 requirement             | Actual verified evidence                                                                                                                                                                                                             |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Secure local authentication | PASS: Node built-in scrypt with random salts/fixed full-cost parameters; safe one-time bootstrap; correct/wrong/unknown/inactive-user login and malformed-input checks                                                               |
| Session management          | PASS: random opaque token, independent client binding, matching User-Agent, server-side digest storage, eight-hour absolute/30-minute idle limits, max five unrevoked sessions, logout/self/admin revocation and restart persistence |
| Hijacking/reuse controls    | PASS: missing/wrong binding, changed client header, forged/cookie/query sessions, fixation input, expired/revoked/replayed tokens and cross-site/Host/Origin attempts reject; transport limits below remain explicit                 |
| RBAC                        | PASS: Administrator/Developer/Viewer allowed/denied operations; self-escalation/unknown role/unsafe fields reject; last active Administrator protected; password/role/active changes revoke sessions                                 |
| Application authorization   | PASS: authenticated ownership assignment; scoped list/count/search/read; owned Developer writes; Viewer/read grants cannot write; unauthorized cross-app reads/writes deny; transfer/grant/revocation tested                         |
| Approval controls           | PASS: owner/admin request, distinct Administrator review/rejection, status/requester/reviewer/times, unique active decision and concurrent review, stale fingerprint/owner/role revocation; valid approval never enables execution   |
| Audit/security data         | PASS: safe user/session views and one-time no-store login token response; credential values omitted from logs/audits/SQLite; safe actor/decision records persist and simulated audit failure rolls back mutation                     |
| M1–M6 preservation          | PASS: all existing regression plus actual published Gateway/Agent with initialized user sessions, live owned route changes, Viewer restrictions, replay denial and real reviewed-release approval                                    |
| Extension/scope             | Local user verification/session issuance remains separate from authorization policy; Entra ID is future integration, not a configured dependency. No browser UI or execution endpoint is claimed                                     |

The focused suite has **37 real HTTP/SQLite auth checks**. The managed integration has **18 checks**, including four added M11 scenarios against the production API and published Gateway/production M5 engine. It verifies the same Gateway PID during session-authorized live routing, then exercises metadata approval of the real reviewed Node release while authorized=false/executionEnabled=false. Public sample health stays available.

## Security architecture and decisions

- **Local native-client interface:** Existing loopback HTTP is retained; no cookie or browser login is introduced. Privileged requests reject Origin, cross-site Fetch metadata and unexpected Host/peer. Public health/static API schemas remain as M4 compatibility features. Hosted application content does not inherit Platform API authentication.
- **Passwords:** Fixed scrypt N=131072/r=8/p=1, random 16-byte salt, 64-byte key, versioned hash; no caller/stored cost settings. Two concurrent derivations maximum, 192-MiB allocation cap/job. New/reset passphrases require 15 Unicode code points and bounded bytes/code units; no plaintext password storage. Unknown/malformed stored hashes use the same-cost dummy verification path.
- **Persistent throttling:** Five attempts/account and 120 globally per 15-minute window; success resets the account bucket. SQLite global accounting bounds fresh-username work/bucket growth. Throttled and rejected authentication is audited without input values.
- **Sessions:** Login accepts username/password/clientBinding only and returns the random token once in a no-store response. Clients retain a random 32-byte base64url binding and send Authorization plus X-IHP-Session-Binding and the same User-Agent. SQLite stores only digests/IDs/server-side times/revocation. Tied issue timestamps use insertion order for oldest-session eviction.
- **Bootstrap transition:** Configured private operator capability can create the first Administrator only when no user exists; an immediate transaction rechecks this. Before bootstrap it preserves trusted M4/M6 maintenance. Once any user exists, that token cannot manage ordinary endpoints. Missing user sessions fail closed; no reset/reopen/default-password endpoint exists.
- **Atomic policy:** Request-local transactions revalidate the session/current active role, check ownership/permission, execute existing Registry/Routing operations and persist audits together. Nested operations join the transaction. Legacy maintenance also rechecks empty-user state inside it; revocation/ownership changes cannot race a stale precheck into a later unauthorized mutation.
- **Roles/grants:** Administrators manage users/security/reference metadata and all applications. Developers create/modify owned applications; Viewers only read explicitly granted applications. Read grants never give write/ownership power. Owner/role claims in ordinary bodies cannot override the session. Database references/assignment are Administrator-only.
- **Approval/execution separation:** Fingerprint binds reviewed deployment/release/manifest/base-path/build metadata plus owner context. Requester/reviewer current roles/active state, ownership and fingerprint are rechecked. Self-review is denied, decisions serialize, history persists and authority changes revoke affected approvals. No approval starts a process. HTTP upload/deploy/start/stop/restart/rollback/application deletion stays absent/disabled.
- **Separate trusted channels:** M6 reader capability still reads only its snapshot and cannot become a user session; user sessions cannot replace it. M5's existing trusted local review/process boundary is preserved. Session credentials are never injected into application environments; operator/routing capabilities are removed from credential-helper children.

## Schema, files and migration requirements

Appended reviewed `src/Api/prisma/migrations/202610270003_auth/migration.sql`; earlier migrations unchanged. New models/tables are AuthSession/auth_sessions, AuthThrottle/auth_throttles and ApplicationGrant/application_grants. Added safe audit details and approval fingerprint/owner/request-time fields with foreign keys, indexes, role/status guards and a unique active approval per deployment. There are **18 models/three migrations**. API verifies current reviewed history; migration remains an explicit Node SQLite step. Existing app ownership stays unchanged/null unless an Administrator explicitly assigns it.

Apply upgrades only with an authorized backup/review procedure. Conflicting legacy active approvals can fail the new uniqueness check; the controlled runner rolls back, never silently deletes history. This session verified fresh owned migration/reapplication/checksum/rollback/parity and persistence/restarts, not an upgrade of actual configured data.

**Created:**

- `src/Api/src/auth.ts`, `auth-controllers.ts`, `access.ts`, `approvals.ts` and `security-context.ts`.
- `src/Api/prisma/migrations/202610270003_auth/migration.sql`.
- `src/Api/test/auth.test.mjs`.
- `AUTHENTICATION.md` and `docs/verification/M11-2026-10-27.md`.
- Local verification artifacts: `artifacts/m11-api-tests.log`, `m11-auth-final.log`, `m11-routing.log` and `m11-verify.log`.

**Modified:**

- `src/Api/prisma/schema.prisma`; `src/Api/src/app.ts`, `http.ts`, `controllers.ts`, `registry.ts` and `vault.ts`.
- `src/Api/package.json`, root `package.json` and `tools/tasks.mjs` for auth checks; no package dependency/lockfile change was needed.
- `src/Api/test/database.test.mjs` and `tests/DeploymentAgent.Tests/Program.cs` for verified model/migration expectations.
- `tests/Platform.Integration.Tests/Program.cs` for session, ownership, replay and real reviewed-release approval checks.
- `README.md`, `API.md`, `DYNAMIC_ROUTING.md`, `ARCHITECTURE.md`, `SECURITY.md` and `INSTALLATION.md`.
- The four tracked records: `PROJECT_REQUIREMENTS.md`, `AGENTS.md`, `IMPLEMENTATION_PLAN.md` and `PROGRESS.md`.

Generated client/build/publish artifacts were refreshed. Production Gateway and Deployment Agent source and original sample source were unchanged by M11.

## Actual commands, failures and corrections

| Command/check                                                                     | Actual result                                                                                                                                               |
| --------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Initial `npm.cmd run build --workspace @ihp/api`                                  | Failed TS2769 (strict regexp capture nullability) and TS1206 (editing helper accidentally annotated constructors); corrected both before behavioral testing |
| API rebuild                                                                       | Exit 0: guarded WASM validation/generation of 18 models and TypeScript compilation                                                                          |
| Initial `npm.cmd run test:auth --workspace @ihp/api`                              | 31/34 pass, three failures: same-timestamp eviction ordering bug; Fetch rewrote Host test header; later fixture used a revoked session                      |
| Corrections                                                                       | Parameterized SQLite rowid tie-break; exact headers through real Node HTTP client; refreshed the intended valid fixture; no weakened authorization          |
| `npm.cmd run test --workspace @ihp/api`, intermediate gate                        | Exit 0: 78 HTTP/reference/routing/auth + 13 SQLite checks at that revision                                                                                  |
| `npm.cmd run test:auth --workspace @ihp/api`, focused final auth gate             | Exit 0: **37/37**, zero failures/skips                                                                                                                      |
| `npm.cmd run test:routing`, focused integration gate                              | Exit 0: **108 Gateway + 17 managed integration** before the final real-release approval case was added                                                      |
| `npm.cmd run format` and `dotnet format InternalHostingPlatform.sln --no-restore` | Exit 0; interrupted formatting command was checked on resume and had completed successfully                                                                 |
| `npm.cmd run verify`, final current implementation                                | **Exit 0: 381 checks**, zero failures/skips, plus full platform smoke                                                                                       |
| `npm.cmd run doctor`                                                              | Exit 0; Windows 22631 x64, Node 26.12.1, npm 11.20.0, SDK 8.0.425, runtime/ASP.NET Core 8.0.31                                                              |
| `npm.cmd audit --json`                                                            | Exit 0; zero vulnerabilities                                                                                                                                |
| Current Windows principal role check                                              | Administrator=False                                                                                                                                         |
| `git diff --check`                                                                | Passed; final documentation/index checks repeated before commit                                                                                             |

Two editing/orchestration calls were rejected before execution (duplicate patch target and an unescaped template interpolation); corrected and resubmitted. No permission/security policy bypass or partial external action occurred. Initial plan-status patch context was corrected to the M11 section. All meaningful compile/test failures are retained above and in history.

The final command used PowerShell Tee-Object to capture `artifacts/m11-verify.log` and propagated `$LASTEXITCODE`. Earlier counts are intermediate revisions, not claims that an earlier command was rerun at the final count.

### Final full regression

| Suite                                          |  Passed | Failed/skipped |
| ---------------------------------------------- | ------: | -------------: |
| Node foundation/lifecycle/sample configuration |      25 |              0 |
| API HTTP/reference/policy/DPAPI/routing/auth   |      81 |              0 |
| Guarded Prisma/SQLite/migration                |      13 |              0 |
| Managed Foundation                             |      27 |              0 |
| Managed Gateway                                |     108 |              0 |
| Managed Samples                                |      82 |              0 |
| Managed Deployment Agent                       |      27 |              0 |
| Managed routing/auth integration               |      18 |              0 |
| **Total**                                      | **381** |          **0** |

This is the previous 340 plus 37 auth checks and four managed M11 checks. Existing API 44 remain within the final 81. All **ten** .NET publish roots passed no-.exe scans; type/format/solution checks passed. Separate smoke verified fresh reviewed SQLite migration/Agent compatibility, real Gateway/API/React responses, structured Gateway logs, literal loopback listeners on 5050/5065/5173, four owned child exits 0, released ports and cleanup of owned temporary storage.

## Known limits, unresolved issues and blockers

- **Native local API only:** No browser login/session UI is implemented; M8 integration remains future. Existing HTTP loopback is not TLS. Theft of both token and binding/client context or same-user malicious code is not prevented. Binding is not a cryptographic device proof, TLS substitute, MFA or SSO.
- **Same-user trust remains:** Windows profile/source/SQLite write access can tamper with identity/audits/reviews. No OS ACL/firewall/certificate/registry/security setting was changed. Hosted application content still needs its own authorization.
- **Operational recovery:** No anonymous/default-account/password-recovery bypass. Existing legacy users close bootstrap, and conflicting legacy approval data can block upgrade safely. Actual backup/restore/identity recovery is not claimed verified; use separately reviewed procedures. No actual registry/user was changed by this session.
- **Resource/retention limits:** Login budgets and two hashing jobs bound password work, not all local denial of service. Audit/session retention/export/tamper resistance belongs to later work. Server clock must be correct. Password breach checks, MFA and automated recovery are not implemented.
- **Approval is metadata authority only:** Review fingerprints are not signatures, malware scans or immutable files. Actual uploads, builds and self-service execution remain unavailable. M7 must explicitly integrate authenticated owner/approval checks before any execution path; M11 completion does not authorize that work.
- **M5/M6 limits remain:** Trusted reviewed manual Agent boundary, launch/job-assignment window, best-effort application-log redaction, static route trust and last-good routing staleness during API failure persist. No unattended logout/reboot guarantee. Exact Node 24/manual Ctrl+C remains unverified as previously recorded.
- **Only SQLite operations:** Non-SQLite connectivity is an accepted prohibited capability gap, not a task to obtain/connect servers.
- No unresolved failing test or known blocking defect remains in the approved M11 backend scope. M7–M10 and M12 remain NOT STARTED.

## Exact next task and commands to resume

**Stop after this M11 handoff.** Await the next assigned milestone. If the user authorizes M7 later, first inspect its approved archive/build/isolation requirements and explicitly integrate M11 ownership/approval checks; do not enable arbitrary or untrusted application execution merely because M11 is complete.

Start every session with AGENTS → current PROGRESS → relevant IMPLEMENTATION_PLAN/PROJECT_REQUIREMENTS, inspect actual code and compare evidence. Keep this implementation workspace/source backup; the progress repository contains only documentation.

```powershell
Get-Content -LiteralPath AGENTS.md -Encoding UTF8
Get-Content -LiteralPath PROGRESS.md -Encoding UTF8 -TotalCount 240
Get-Content -LiteralPath IMPLEMENTATION_PLAN.md -Encoding UTF8
Get-Content -LiteralPath PROJECT_REQUIREMENTS.md -Encoding UTF8
git status --short
npm.cmd run doctor
npm.cmd run build
npm.cmd --workspace @ihp/api run db:status
npm.cmd run test:auth
npm.cmd run test:routing
npm.cmd run verify
```

Run dependency restoration only when needed (`npm.cmd ci --ignore-scripts`). For an authorized local registry upgrade, use explicit `npm.cmd --workspace @ihp/api run db:migrate` after reviewing backup/data conditions; never native Prisma Migrate or automatic/uploaded SQL. See local AUTHENTICATION.md for safe secret input/bootstrap/login, matrix, approvals and limitations. Never print session responses/headers, put passwords in arguments, or commit database/credentials.

Synchronize only the four named records:

```powershell
git add -- PROJECT_REQUIREMENTS.md AGENTS.md IMPLEMENTATION_PLAN.md PROGRESS.md
git diff --cached --name-only
git commit -m "Complete M11 local authentication and authorization"
git push origin main
git ls-remote origin refs/heads/main
```

Verify staged/committed paths and remote main SHA against local HEAD. The final handoff reports that commit; code remains deliberately excluded.

## Preserved progress history

All following entries are historical evidence. Earlier counts/statuses, milestone ordering and obsolete connectivity/runner instructions do not override the current snapshot or company policy.

````````````text
# Project Progress

## Active M11 session — 2026-10-27

M11 is IN PROGRESS under the user's explicit priority before M7, superseding the prior milestone order below. Read AGENTS/current evidence/plan and auth/role/security/approval requirements; inspected actual NestJS guards/controllers/registry, SQLite migration/adapter and M5/M6 boundaries. Baseline tracked documents are clean at 3e3cac1; implementation exists locally. Plan: Node built-in scrypt, hashed revocable server-side sessions with separate client binding, one-time operator bootstrap, fail-closed RBAC/ownership, transaction-bound sensitive authorization, metadata-only approvals and safe persistent security audits. No M7+, external database operation, corporate listener, untrusted execution or OS security change is authorized. No new functionality is yet claimed verified.

Core step: appended a reviewed auth migration (18 models/three migrations), scrypt password/session services, persistent throttling, grants/ownership, metadata-bound approvals and atomic request authorization using the existing serialized Prisma/SQLite adapter. Wired separate reader capability and one-time maintenance bootstrap; after users exist, legacy operator requests cannot bypass sessions. Upload/process/rollback execution remains absent/disabled. Initial build exposed strict TypeScript regexp-capture nullability and accidental constructor metadata from an editing helper; corrected both before behavioral verification. One patch was rejected as a duplicate-target operation and resubmitted without partial code execution. Next: real HTTP security suite and complete regression; no new behavior is yet marked completed.

Behavioral step: first auth run passed 31/34 checks; found a real same-timestamp session-eviction ordering issue, Fetch rewriting a Host test header, and a later fixture reusing an already-revoked session. Corrected eviction with parameterized SQLite insertion-order tie-breaking, sent exact headers through Node's HTTP client and refreshed that fixture. API rebuild passed. npm.cmd run test --workspace @ihp/api then passed 78 HTTP/reference/routing/auth checks plus 13 SQLite checks. Added approved-metadata/reviewer revocation, fail-closed audit rollback, global login throttling and real published Gateway/Agent session/ownership integration checks; these final additions are pending verification. No M7+ work or new execution endpoint exists.

Focused final security gate passed 37/37 auth HTTP/SQLite checks. Dedicated test:routing passed 108 Gateway plus 17 managed integration checks with actual initialized user sessions, live owner route removal/republish, Viewer restrictions, replay rejection and credential-safe logs; the last real-release approval integration check was added afterward and awaits full regression. Created AUTHENTICATION.md with permission matrix, bootstrap/login/session/approval protocol and explicit unencrypted-loopback/native-client/same-user limits; updated existing architecture/setup/security/API/routing docs. No production registry/user was bootstrapped. Next: final format and complete M1–M6/M11 build/check/test/smoke gate before completion.

**Updated:** 2026-10-27 (Asia/Bangkok)

## Current milestone and handoff

- **Current/last completed milestone:** M6 — Dynamic Routing, **COMPLETED**. Its acceptance criteria passed the final implementation gate below.
- **Previously completed:** M1–M5 remain COMPLETED; full regression passed.
- **First unfinished milestone:** M7 — ZIP Upload and Approved Source Builds, **NOT STARTED**. Do not start M7 or any later milestone without a new user instruction. Its existing M11/security dependencies remain relevant.
- **Last completed task:** Implemented validated live Gateway snapshots, local API/SQLite route intent/feed, managed end-to-end verification and operator/security documentation; passed final build/check/test/smoke.
- **Final verification:** `npm.cmd run verify` exited 0: **340 passing checks**, zero failures/skips, plus real platform smoke.
- **Blockers:** None for approved M6. Read-only feed availability/staleness and existing trusted-process isolation limits remain documented.
- **Progress Git scope:** ONLY PROJECT_REQUIREMENTS.md, AGENTS.md, IMPLEMENTATION_PLAN.md and PROGRESS.md may be committed/pushed to `https://github.com/logtranwanek-spec/IAH-Progress-Tracking-.git` on main. Source, tests, extra docs, lockfiles and artifacts remain local/untracked. A progress-only clone cannot run the implementation.
- **History:** All prior work, including intermediate M6 gates, is preserved in the fenced historical section below. Superseded statuses/wait instructions and old prohibited external-engine experiments are historical, never current instructions.

## Mandatory policy retained

**Only SQLite is supported and allowed for real database operations.** SQL Server/PostgreSQL/MySQL remain protected connection-reference metadata labels/APIs only. Their installation, operational connections and real protocol/authentication tests are prohibited. Missing real connectivity evidence remains the user-accepted company-policy gap. No such operation or test ran in M6.

.NET components/verifiers are published framework-dependent with `UseAppHost=false` and executed only as `dotnet <published>.dll`. No application apphost/.exe artifact, VSTest/testhost, native Prisma engine or database server was run. The established installed node/dotnet/git/npm.cmd CLI-host convention remains. Dependency lifecycle scripts stay disabled; Prisma validation/generation uses the guarded JS/WASM path, and migrations stay explicit reviewed Node/SQLite steps.

All real platform/sample/verification listeners bind only 127.0.0.1. Gateway remains exclusively **127.0.0.1:5050**. No Docker, elevation, Windows security change, production access, corporate-network listening/deployment or existing-application modification occurred. Original DemoNode/DemoDotnet sources remain unchanged; tests stage owned trusted copies.

## Features implemented

- **Atomic dynamic provider:** ValidatedProxyConfigProvider retains the M2 static source and optionally accepts complete registry snapshots. It validates the complete combined candidate before swapping immutable route/cluster objects and signalling the previous one-shot change token. Invalid candidates leave the exact last accepted snapshot and token intact; identical configurations avoid reloads.
- **Strict validation preserved:** Canonical path-prefix routes only; reserved segments, malformed/encoded/nested/root paths, duplicate IDs/prefixes, static collisions, unsafe hosts/schemes/URL components and component ports reject. Destinations are reconstructed as `http://127.0.0.1:<port>/` and constrained both to 6100–6999 and the configured narrower range. Widened settings cannot bypass that absolute boundary.
- **SQLite/API intent:** Operator-protected GET/PUT/DELETE `applications/:id/route` read, create/update and remove one application's route intent. PUT accepts only publicPath, requires the selected reviewed RUNNING M5 release/instance/lease/session, and enforces matching basePath. Writes/removals and audit entries commit atomically. Application/deployment deletion and process/deployment execution APIs remain disabled.
- **Separate read-only feed:** GET `routing/snapshot` projects approved eligible routes in one consistent Prisma/SQLite transaction. Its capability is separate from the local operator capability; neither grants the other's operations. Missing reader configuration fails closed, equal/malformed capabilities reject startup, literal-loopback peers are required and Origin headers are rejected.
- **Controlled polling:** The production Gateway reads IHP_ROUTING_TOKEN at its entrypoint and polls only the fixed literal-loopback API snapshot path at the validated API port. Host factory calls remain static-only unless explicit routing options are supplied. No public Gateway update endpoint or arbitrary source URL exists. The launcher supplies the reader only to API/Gateway and the operator capability only to API.
- **Runtime/release reconciliation:** Effective destinations follow the current approved M5 instance/lease instead of client input or historical routes.targetPort. Stopped/unhealthy/unowned registry state withdraws effective routes while intent persists. New approved eligible state restores them; Gateway/API restarts reconcile SQLite state without editing a route file.
- **Proxy compatibility:** Existing prefix/header/request limits/errors/logging and platform-path protection remain. New requests use accepted updates, including WebSocket upgrades. Existing WebSockets finish on their selected connection through target/prefix/removal changes. Preserved-prefix assets, redirects and cookies were verified with a trusted fixture; the two minimal samples are not claimed to implement those extra features.
- **Managed integration:** Platform.Integration.Tests is a published assertion DLL, not VSTest. It runs the production Node API and published Gateway against real production RuntimeEngine-controlled, reviewed sample copies and SQLite. It checks actual PID/start identity, owned sockets, v2 release/build identity, shutdown and port release.

## M6 acceptance evidence

| Criterion                                                 | Actual evidence                                                                                                                                                                                                    |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Add/update/remove without Gateway restart                 | PASS: managed live provider tests plus real operator API changes; published Gateway PID and start ticks remain unchanged during live operations                                                                    |
| Node and ASP.NET samples simultaneously reachable on 5050 | PASS: real approved copies healthy through /demo-node and /demo-dotnet on distinct M5 ports; both retain release/port identity                                                                                     |
| Dynamic destination/release update                        | PASS: stop owned Node fixture, stage/review v2, occupy its old loopback port, start v2 on a new approved lease; same Gateway serves release=v2/build=m6-v2; competing occupant remains alive                       |
| Unsafe/duplicate/reserved routes rejected                 | PASS: M2 validator regression, API input rejection, static ID/prefix collision rejection, absolute/narrowed port boundary and actual invalid-source scenarios                                                      |
| Last-known-good on failed update                          | PASS: rejected candidates preserve exact snapshot/token; malformed/unauthorized/redirect/oversized/timeout feeds leave the live proxy working; redirect trap receives zero requests                                |
| Restart/reconciliation                                    | PASS: deliberate published Gateway restart reloads both SQLite routes; API interruption retains last good routes; restarted API removal reaches unchanged Gateway; stopped runtime withdraws and resumes correctly |
| Static backward compatibility                             | PASS: static-only provider remains immutable, static-only sample regression passes, static route coexists with dynamic routes and survives valid registry removal                                                  |
| WebSockets/prefix compatibility                           | PASS: existing socket survives target/prefix/removal updates; new socket after update negotiates subprotocol, echoes and closes; real preserved-prefix asset/redirect/cookie checks pass                           |
| Security/execution policy                                 | PASS: Administrator=False; only literal-loopback real listeners; ten published roots scanned without .exe; managed DLL execution; no external database operation                                                   |

## Actual commands and results

| Command/check                                                                                                                                                                 | Result                                                                                      |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| `npm.cmd run build --workspace @ihp/api`                                                                                                                                      | Exit 0; guarded WASM validation/generation of 15 models and strict compilation              |
| `dotnet publish .\src\Gateway\Gateway.csproj -c Release --no-self-contained -p:UseAppHost=false -p:UseSharedCompilation=false -p:PublishDir=../../artifacts/publish/Gateway/` | Exit 0                                                                                      |
| `npm.cmd run test --workspace @ihp/api`, first routing gate                                                                                                                   | Exit 0; 43 API/reference/routing + 13 SQLite checks at that revision                        |
| `npm.cmd run test:gateway`, first live gate                                                                                                                                   | Exit 0; 92 checks                                                                           |
| `npm.cmd run test:routing`, first whole-chain gate                                                                                                                            | Exit 0; 92 Gateway + 14 managed integration checks at that revision                         |
| `npm.cmd run test:gateway`, added source-failure cases                                                                                                                        | Exit 0; 107 checks                                                                          |
| `npm.cmd run format` and `dotnet format InternalHostingPlatform.sln --no-restore`                                                                                             | Exit 0                                                                                      |
| `npm.cmd run verify`, final gate including later boundary/default-disabled/v2 checks                                                                                          | **Exit 0; 340 passing checks**, zero failures/skips, plus full smoke                        |
| `npm.cmd run doctor`                                                                                                                                                          | Exit 0: Windows 22631 x64, Node 26.12.1, npm 11.20.0, SDK 8.0.425, .NET/ASP.NET Core 8.0.31 |
| `npm.cmd audit --json`                                                                                                                                                        | Exit 0; zero vulnerabilities                                                                |
| Current Windows principal Administrator role check                                                                                                                            | **False**                                                                                   |
| `git diff --check`                                                                                                                                                            | Passed; documentation formatting/index checks repeated before commit                        |

The final verify command was captured with PowerShell `Tee-Object -FilePath artifacts/m6-verify.log` and propagated `$LASTEXITCODE`. This local artifact is not committed. The direct dedicated-gate counts above are earlier revisions; final counts below come from the final full regression, not an invented rerun of an earlier command.

No M6 application-code/build/test failure occurred. An orchestration snippet had a JavaScript syntax error before tool execution and was resubmitted correctly; an exploratory nonexistent config path was corrected to the actual package path. Neither changed application state or became a claimed test failure.

### Final regression

| Suite                                          |  Passed | Failed/skipped |
| ---------------------------------------------- | ------: | -------------: |
| Node foundation/lifecycle/sample configuration |      25 |              0 |
| API HTTP/reference/policy/DPAPI/routing        |      44 |              0 |
| Guarded Prisma/SQLite/migration                |      13 |              0 |
| Managed Foundation                             |      27 |              0 |
| Managed Gateway                                |     108 |              0 |
| Managed Samples                                |      82 |              0 |
| Managed Deployment Agent                       |      27 |              0 |
| Managed routing integration                    |      14 |              0 |
| **Total**                                      | **340** |          **0** |

API routing adds 11 checks to the prior 33. Gateway adds 29 checks to the prior 79, including setup wrappers counted consistently with earlier evidence. Integration adds 14; thus the previous 286 becomes 340.

All ten .NET publish roots passed no-.exe scans. Static/type/solution formatting checks passed. Separate full smoke verified reviewed SQLite migration/Agent compatibility, real API/React/Gateway responses, structured Gateway logs, 127.0.0.1 listeners on 5050/5065/5173, four owned child exit codes 0, released ports and removal of owned temporary storage. M6 integration separately verifies real v2 replacement, dynamic routes, API/Gateway restart and safe failure retention. No migration was applied to the actual platform registry during verification.

## Files created or modified in M6

Source and extra docs remain local/untracked by explicit progress-repository policy.

**Created:**

- `src/Gateway/Routing/RegistryRoutePoller.cs`.
- `src/Api/src/routing.ts` and `src/Api/test/routing.test.mjs`.
- `tests/Gateway.Tests/DynamicChecks.cs` and `RegistrySourceChecks.cs`.
- `tests/Platform.Integration.Tests/Platform.Integration.Tests.csproj`, `Program.cs`, `Child.cs`, `Checks.cs` and `packages.lock.json`. The verifier links the existing owned M5 Workspace helper.
- `DYNAMIC_ROUTING.md` and `docs/verification/M6-2026-10-27.md`.
- Local `artifacts/m6-verify.log` from the final gate.

**Modified:**

- `src/Gateway/Program.cs`, `GatewayHost.cs`, `Routing/GatewayRoutes.cs` and `Routing/ValidatedProxyConfigProvider.cs`.
- `src/Api/src/registry.ts`, `app.ts`, `controllers.ts`, `http.ts` and `src/Api/package.json`.
- `tests/Gateway.Tests/Program.cs` and `Fixtures.cs`.
- `tools/local.mjs`, `tools/tasks.mjs`, `tools/publish.mjs`, root `package.json` and `InternalHostingPlatform.sln`.
- `README.md`, `GATEWAY.md`, `API.md`, `INSTALLATION.md`, `ARCHITECTURE.md` and `SECURITY.md`.
- The four tracked records: `PROJECT_REQUIREMENTS.md`, `AGENTS.md`, `IMPLEMENTATION_PLAN.md` and `PROGRESS.md`.

Build/client/publish artifacts were regenerated. Original sample sources, the Prisma schema and both reviewed migrations were unchanged. M6 uses the existing 15 models/two migrations; no database server/dependency was added.

## Decisions, known issues and limits

- **Opt-in trusted local control:** M6 is a local operator capability, not login/RBAC, end-user approval or authority for unreviewed code. API application deletion/deployment/runtime execution remains disabled. Agent HTTP control is absent.
- **Polling budgets:** Defaults are implementation choices: 1-second interval, 2-second request timeout, 64-KiB streamed response, 16-KiB headers, depth 8, at most 100 combined routes. Managed source tests use shorter bounded intervals/timeouts. No arbitrary polling URL is allowed.
- **Eventual activation:** PUT acknowledges persisted route intent, not synchronous Gateway activation. Static collisions or bad combined candidates are rejected by the Gateway and retain the whole last good configuration; the operator must correct intent/configuration and verify public health. GET intent targetPort is historical metadata; effective destinations follow M5 leases.
- **Staleness during source failure:** Failed/offline/unauthorized refreshes retain the previous in-memory snapshot as required. Stops/removals cannot arrive while the API is unavailable; previous destinations may be stale and proxy errors may occur. No offline revocation guarantee is claimed. A fresh Gateway startup has only static routes until a valid API response; no stale dynamic disk cache is loaded.
- **Registry trust:** Runtime eligibility is a projection of M5-maintained SQLite state, not a new OS PID/listener/health authority inside the API. Abrupt Agent death, port reuse or same-user DB/file tampering can outdate it. M5 job/EOF/PID/socket controls and trusted operator/release discipline remain mandatory.
- **Existing connections:** Established WebSockets keep their old selected connection after updates/removal; new requests observe the new snapshot. Forced connection revocation, authenticated sessions and zero-downtime deployment/rollback orchestration are not implemented.
- **Static routes:** Operator-reviewed static JSON is still read only at startup. Dynamic snapshots cannot overwrite/remove it. Static destinations are not M5 runtime projections; preserve review discipline. Restart is required only to change that file.
- **Prefix support:** Matching reviewed basePath is enforced; no guessed prefix stripping or Location/cookie/body rewriting. Minimal samples expose identity/health; extra asset/redirect/cookie checks use a trusted test fixture, not invented sample features.
- **Existing isolation gaps remain:** Same ordinary Windows identity is not a sandbox/firewall; M5 launch-to-job assignment gap, best-effort log redaction, OS SQLite servicing and no unattended logout/reboot guarantee remain. Exact Node 24/manual Ctrl+C compatibility is still unverified; current installed runtime and supervised shutdown passed.
- **No M7+ work:** ZIP/source builds, ordinary-user execution approval/auth, dashboard management, rollback and broader hardening remain unfinished. Only SQLite real operations are allowed; the external-engine gap is accepted and must not become an instruction to install/contact those engines.
- No known blocking defect or unresolved failing test remains in the approved M6 scope.

## Exact next task and commands to resume

**End this session after the M6 handoff.** On a future explicit authorization, first read M7 and its M11/approved-build/isolation dependencies before implementing ZIP/prebuilt preparation. Do not enable unreviewed upload/build/execution merely because M6 routing is complete.

Every session: read AGENTS → current PROGRESS → relevant IMPLEMENTATION_PLAN/PROJECT_REQUIREMENTS, inspect actual local source and compare it to evidence. Implementation is not present in the progress-only remote; preserve this workspace or an explicitly approved source backup.

```powershell
Get-Content -LiteralPath AGENTS.md -Encoding UTF8
Get-Content -LiteralPath PROGRESS.md -Encoding UTF8 -TotalCount 220
Get-Content -LiteralPath IMPLEMENTATION_PLAN.md -Encoding UTF8
Get-Content -LiteralPath PROJECT_REQUIREMENTS.md -Encoding UTF8
git status --short
npm.cmd run doctor
npm.cmd run build
npm.cmd --workspace @ihp/api run db:status
npm.cmd run test:routing
npm.cmd run verify
```

Run `npm.cmd ci --ignore-scripts` only if restoration is needed. Use explicit `npm.cmd --workspace @ihp/api run db:migrate` if the configured local registry is absent/behind; never run automatic/uploaded migrations or native Prisma commands. Manual integration execution, after publishing its prerequisites:

```powershell
dotnet publish .\tests\Platform.Integration.Tests\Platform.Integration.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false -p:UseSharedCompilation=false -p:PublishDir=../../artifacts/publish/Platform.Integration.Tests/
dotnet .\artifacts\publish\Platform.Integration.Tests\Platform.Integration.Tests.dll
```

Consult local DYNAMIC_ROUTING.md for capability generation, route-intent commands, configuration parity, polling/recovery and limits; DEPLOYMENT_ENGINE.md for trusted M5 preparation. Never put capabilities/credentials in Git, chat, URLs or logs.

Progress synchronization uses exactly the four named documents:

```powershell
git add -- PROJECT_REQUIREMENTS.md AGENTS.md IMPLEMENTATION_PLAN.md PROGRESS.md
git diff --cached --name-only
git commit -m "Complete M6 validated dynamic routing"
git push origin main
git ls-remote origin refs/heads/main
```

Check staged/committed paths and verify remote main equals local HEAD. The final session handoff reports the resulting commit; source is intentionally excluded.

## Preserved progress history

All entries below are historical evidence, not active instructions. Earlier milestone counts/statuses and wait messages are superseded by this current snapshot and authoritative company policy.

```````````text
# Project Progress

## Active M6 session — 2026-10-27

M6 is IN PROGRESS under the new user authorization, superseding the M5 wait message below. Read repository instructions, current M5 evidence, M6 acceptance criteria and routing/security requirements; inspected actual Gateway provider/validator, API/SQLite schema, M5 runtime state and existing published verification. The four-document repository is clean at d38425e; implementation remains present locally. First task: preserve static routes while implementing atomic validated runtime snapshots, fixed loopback API polling with a separate read-only capability, operator-protected registry route intent derived from approved M5 runtime state, then managed real-process verification. No M7+ work, external database operation or corporate-network listening is authorized. No M6 behavior is yet marked verified.

Core step: implemented opt-in immutable Gateway snapshots/change tokens and fixed loopback API polling with a separate read-only capability, strict bounded JSON/timeout/redirect policy, combined static/dynamic validation and last-known-good retention. Added operator-protected route-intent read/publish/remove and a reader-only snapshot derived from M5 reviewed running state/leases; client-supplied destinations are refused and stopped/unowned instances are excluded. API builds with 15-model guarded WASM generation; Gateway publish is being checked. No migration/schema change was needed. Next: managed real-process checks and existing regression; route behavior is not yet claimed as passing.

First behavioral gates: API build and Gateway publish passed; API/reference/routing checks passed 43 plus 13 SQLite checks. Gateway managed verification passed 92 checks, including live add/target/prefix/remove, static compatibility, real assets/redirects/cookies, existing and new WebSockets, and immutable rejection. npm.cmd run test:routing then passed those 92 checks plus 14 new managed integration checks against real published Gateway, production Node API and production M5 engine/sample copies. Verified unchanged Gateway PID/start identity during live operations, real occupied-old-port runtime replacement, both samples on 5050, route removal, Gateway/API restart reconciliation, source interruption retention, stopped-runtime withdrawal, literal PID-owned loopback listeners, safe logs, exit 0 and port release. Added bounded malformed/unauthorized/redirect/oversized/timeout source tests and default-disabled reader coverage; those additions are pending the final regression. M6 remains IN PROGRESS until that gate passes. Next: operator documentation, formatting and full regression; no M7+ work.

Source verification step: npm.cmd run test:gateway passed 107 checks, including 12 malformed/unsafe/unavailable source cases, actual chunked size/timeout/redirect rejection, no redirect-trap request, exact last-known-good identity and valid empty recovery preserving static routes. Added the final absolute 6100–6999 validation test (even if settings attempt to widen it), exact HTTP 200 reader requirement and consistent single-load API configuration; final full regression will include these plus the new default-disabled reader test. Created DYNAMIC_ROUTING.md and updated setup/architecture/security/API/Gateway docs. The launcher now sends routing credentials only to API/Gateway and operator credentials only to API. No code/test failure has occurred in the M6 gates so far. Next: final format/build/test/smoke gate and accurate completion evidence.

**Updated:** 2026-10-27 (Asia/Bangkok)

## Current milestone and handoff

- **Current/last completed milestone:** M5 — Deployment Engine, **COMPLETED**. All M5 acceptance criteria have verified evidence below.
- **Previously completed:** M1–M4 remain COMPLETED; the full regression passed with the new engine migration/schema.
- **First unfinished milestone:** M6 — Dynamic Routing, **NOT STARTED**. Do not begin M6 or any later milestone without a new user instruction.
- **Last completed task:** Completed the controlled runtime manager, managed verification, full regression and operator/isolation documentation. Recorded the final M5 acceptance results.
- **Final verification:** `npm.cmd run verify` exited 0 with **286 passing checks**, zero failures/skips, plus the real platform smoke test.
- **Blockers:** None for approved M5. Same-user isolation, unattended startup and prohibited external database operations remain explicit limits, not implemented capabilities.
- **Git scope:** Commit/push ONLY PROJECT_REQUIREMENTS.md, AGENTS.md, IMPLEMENTATION_PLAN.md and PROGRESS.md to `https://github.com/logtranwanek-spec/IAH-Progress-Tracking-.git`, branch main. Source, tests, extra documentation, lockfiles and artifacts remain local/untracked. A fresh clone of that progress repository cannot run the implementation.
- **History:** Every earlier entry, including intermediate M5 results, is preserved verbatim apart from line-ending normalization in the fenced historical section below. Historical milestone statuses, proposed commands and old external-engine tests are superseded by current policy and this snapshot.

## Policy and scope

**Only SQLite is supported and allowed for real database operations.** SQL Server/PostgreSQL/MySQL labels and protected connection-reference metadata remain in the M4 API. Their operational connections, protocol/authentication tests and server installation are prohibited. Missing real connectivity verification remains the user-accepted company-policy gap; no such operation/test ran in M5.

.NET application/verification artifacts are published framework-dependent with `UseAppHost=false` and launched only through `dotnet <published>.dll`. No application apphost, .exe artifact, VSTest/testhost or native Prisma engine was launched. Installed node/dotnet/git/npm.cmd CLI hosts are invoked by command name under the established project execution convention. Prisma validation/generation stays on the pinned guarded JS/WASM path; migrations use reviewed SQL through Node's SQLite runner.

All actual platform/sample/fault-fixture listeners used literal 127.0.0.1. No Docker, elevation, database server, Windows security-setting change, production access, corporate-network deployment or modification to an existing application was required. Original DemoNode/DemoDotnet sources were unchanged; verification ran new owned staged copies. Agent control has no HTTP listener.

## Features implemented and verified

1. **Reviewed release boundary:** Pure file/metadata inspection detects Node ESM and published framework-dependent ASP.NET Core 8 entrypoints; installed runtime availability is checked separately. Files must be in the platform-owned `Applications/<application UUID>/releases/<version>` directory, outside source and inside the current user's profile storage. Inspection rejects traversal, links/reparse points, arbitrary runners, executable/shell/native artifacts, unmanaged DLLs, unsupported metadata/contracts and file-budget excess. Review records SHA-256 file hashes; every launch rechecks hashes and canonical path/health metadata. Inspection does not execute the application.
2. **Durable allocation:** SQLite records one unique application/port lease in 6100–6999. Immediate writer transactions serialize concurrent allocations; exclusive loopback probes skip occupied ports. Concurrency, duplicate leases, exhaustion and the real probe-to-bind race are verified. A conflicting occupant is never terminated.
3. **Lifecycle/status:** Controlled local review/start/stop/restart/status/list/logs commands; per-application operation serialization; PID/start identity; desired and actual state; release/instance/exit/end-reason persistence; working-set/CPU snapshots. New instances run only through installed Node or published managed DLL hosts with fixed reviewed entrypoints.
4. **Owned child handling:** Windows kill-on-close jobs with a single-process limit; assignment failures fail closed. Only cached owned process/job handles may stop a child. IPv4/IPv6 TCP inspection checks that the owned PID has exactly its allocated literal 127.0.0.1 listener. A clean environment omits platform tokens and runtime injection variables; loopback/port/release/build settings are supplied by the Agent.
5. **Health/logs:** Bounded HTTP checks use a reconstructed literal-loopback URL, no proxy/redirect, expected JSON status and matching release/build/port. Both stdout/stderr are captured with per-line/text budgets, deployment/instance association and best-effort labelled/JSON credential redaction. Startup, termination, known/unknown exits and failure reasons are persisted.
6. **Recovery:** An exclusive per-registry lock prevents two Agents. SQLite stores RUNNING/STOPPED intent, failure count, backoff and session identity. Graceful shutdown releases children/ports while retaining RUNNING intent; explicit app stop persists STOPPED. Agent restart reconciles old instances, restores safe intent and preserves retry exhaustion. A matching live prior PID is quarantined with its lease; a reused PID is never killed. Unknown prior exits remain null rather than invented success.
7. **Managed verification:** A published DeploymentAgent.Tests DLL uses real Node/.NET sample copies, a separate trusted managed fault fixture, owned SQLite databases and actual published Agent subprocesses. It returns nonzero on any failed assertion.

### M5 acceptance evidence

| Criterion                                     | Actual verified evidence                                                                                                                                                                                                  |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Framework/runtime detection                   | Node/.NET published metadata and installed runtime checks pass; inspection sentinel is never executed; malformed/native/unreviewed/changed/canonical-path/health-URI cases reject before launch                           |
| Persistent ports, conflicts and concurrency   | Unique/range/FK constraints and eight concurrent SQLite allocators pass; occupied port skipped, exhaustion safe, actual select-to-bind conflict preserves occupant and releases failed child lease                        |
| Start/stop/restart/status/exit                | Real concurrent Node/ASP.NET samples become healthy on distinct owned loopback ports; idempotent concurrent starts; restart creates new instance; graceful exit 0 and real exit 17/23/137 recorded                        |
| Logs and health                               | Both streams captured/redacted/bounded; expected identity health passes; startup health failure cleans up; three failing running probes trigger recovery; live metrics are present                                        |
| Loopback and safe ownership                   | Actual PID socket checks pass; pure wildcard/IPv6/foreign/extra-port rejection checks bind no unsafe listener; published Agent has zero TCP listeners; unrelated/reused PIDs remain alive                                 |
| Persistent desired state and bounded recovery | Graceful restart resumes RUNNING intent; real hard Agent kill cleans owned sample/port and restart creates a healthy new instance; persisted failure budget exhausts after three failures and remains FAILED after reopen |
| Ordinary-user/policy-compatible execution     | Administrator=False; nine published roots contain no .exe; managed DLL launch only; native-free Prisma/Node SQLite and in-process Windows SQLite interoperate                                                             |

**All M5 criteria passed.** The trust/isolation boundary below is mandatory; this does not complete later upload, routing, dashboard, rollback or user-authorization milestones.

## Storage/schema and decisions

Appended reviewed migration `src/Api/prisma/migrations/202610270002_engine/migration.sql`; the original M4 migration was not changed. Added four normalized models/tables, for **15 Prisma models and two migrations** total:

- `agent_releases`: reviewed deployment, runtime/entrypoint/relative directory, base/health paths, release/build and fingerprint.
- `agent_desired`: per-application chosen release, RUNNING/STOPPED intent, status, failure budget and next attempt.
- `agent_port_leases`: unique application/port, session ownership, constrained 6100–6999.
- `agent_instances`: existing runtime-instance linkage, session/start identity, observed exit and end reason.

The Agent verifies the reviewed engine migration checksum and refuses missing/changed history. It never auto-migrates. Apply the explicit API `db:migrate` command before starting it; builds/verifiers do not migrate the actual platform registry. Final smoke used a fresh owned Node-migrated registry that the managed Agent read successfully. API schema/model/type/FK parity and migration guards passed with all 15 models.

The provider is `Microsoft.Data.Sqlite.Core 8.0.33` plus `SQLitePCLRaw.provider.winsqlite3 2.1.11` (raw core resolved 2.1.13), loading the OS's existing `winsqlite3.dll` in-process. Managed probing observed SQLite **3.43.2**; Node registry tooling observed **3.54.0**. No native database executable/server was downloaded or launched. OS SQLite servicing remains an IT dependency.

M5 approval is an explicit trusted local operator assertion, recorded against exact reviewed hashes. It is not M11 login/RBAC/approval or proof of safe arbitrary code. Stdin controls default disabled and require `IHP_AGENT_CONTROL=1`; supervised shutdown is separate. The API still exposes no deployment/start/stop/restart/rollback execution path. The ordinary local launcher owns an Agent with controls disabled.

Default budgets are implementation choices: 12-second startup, 5-second stop, three failures, exponential 1-second backoff capped at 30 seconds, three failed running probes and a 250-ms reconciliation timer. Tests use shorter bounded settings. Failure counts persist across restarts and are not reset merely by a healthy retry; an explicit local start/restart resets the budget.

## Actual commands, failures and corrections

| Command/check                                                                     | Actual result                                                                                                                             |
| --------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Provider-only managed publish/run probe using the command below                   | Exit 0; OS SQLite 3.43.2 opened and queried in-process                                                                                    |
| Initial core publish                                                              | Passed                                                                                                                                    |
| First full-verifier publish                                                       | Failed with CS0819 (multiple declarators in an implicitly typed declaration); fixed before executing the verifier                         |
| Code review of console handling                                                   | Moved blocking console reads onto Task.Run so they cannot block the recovery timer; no runtime test failure was invented for this finding |
| `npm.cmd run test:agent`, initial behavioral gate                                 | Exit 0; 25 checks passed                                                                                                                  |
| `npm.cmd run test:agent`, after canonical metadata and actual bind-race checks    | Exit 0; **27 checks passed**, zero failures                                                                                               |
| `npm.cmd run format` and `dotnet format InternalHostingPlatform.sln --no-restore` | Exit 0                                                                                                                                    |
| `npm.cmd run verify`, final implementation gate                                   | **Exit 0; 286 checks passed**, zero failures/skips, plus full real smoke                                                                  |
| `npm.cmd run doctor`                                                              | Exit 0; installed Windows/Node/npm/.NET environment and configuration accepted                                                            |
| `npm.cmd audit --json`                                                            | Exit 0; zero npm vulnerabilities                                                                                                          |
| Windows current-principal Administrator role check                                | **False**                                                                                                                                 |
| `git diff --check`                                                                | Passed after correcting documentation whitespace; final documentation check repeated before commit                                        |

The manual provider/verifier publish/run format actually used was:

```powershell
dotnet publish .\tests\DeploymentAgent.Tests\DeploymentAgent.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false -p:UseSharedCompilation=false -p:PublishDir=../../artifacts/publish/DeploymentAgent.Tests/
dotnet .\artifacts\publish\DeploymentAgent.Tests\DeploymentAgent.Tests.dll
```

For the full verifier, its Agent, fixture and DemoDotnet publish prerequisites must exist; `npm.cmd run test:agent` publishes all four and checks their outputs automatically. The first provider-only probe was subsequently replaced by the full verifier; its early result was not treated as lifecycle evidence.

### Final regression evidence

| Suite                                          |  Passed | Failed/skipped |
| ---------------------------------------------- | ------: | -------------: |
| Node foundation/lifecycle/sample configuration |      25 |              0 |
| API HTTP/reference/policy/DPAPI                |      33 |              0 |
| Guarded Prisma/SQLite/migration                |      13 |              0 |
| Managed Foundation                             |      27 |              0 |
| Managed Gateway                                |      79 |              0 |
| Managed Samples                                |      82 |              0 |
| Managed Deployment Agent                       |      27 |              0 |
| **Total registered checks**                    | **286** |          **0** |

Build/type/format/solution checks passed. All **nine** .NET publish roots passed recursive no-.exe artifact checks. Full smoke separately verified real Gateway/API/React HTTP responses, structured Gateway logs, fresh SQLite migration/Agent compatibility, literal-loopback listeners on 5050/5065/5173, four owned child exit codes 0, released ports and cleanup of owned temporary smoke storage.

The 27 engine checks additionally cover: missing/changed migration refusal; options/path budgets; reviewed fingerprint enforcement; occupied/exhausted/racing ports; concurrent leases and same-app starts; healthy concurrent real runtimes; startup exit 17 cleanup; actual crash exit 23; declining health; uncooperative owned job termination exit 137; stdout/stderr limits/redaction and omitted inherited credentials/injection settings; singleton ownership; graceful restart; live-prior-PID quarantine; PID reuse; default-denied controls; actual Agent listener absence; actual hard Agent restart with unknown prior exit and persisted recovery. These are real published-process tests except explicitly labelled pure inspection/listener-policy checks.

## Files created or modified in M5

Implementation files remain local; they are intentionally excluded from the progress-only Git commit.

**Created:**

- `src/DeploymentAgent/EngineDatabase.cs`, `EngineOptions.cs`, `EngineStore.cs`, `ReleaseInspector.cs`, `RuntimeEngine.cs`, `WindowsOwnership.cs`.
- `src/Api/prisma/migrations/202610270002_engine/migration.sql`.
- `tests/DeploymentAgent.Tests/DeploymentAgent.Tests.csproj`, `Program.cs`, `Checks.cs`, `Workspace.cs`, `AgentProcess.cs` and `packages.lock.json`.
- `tests/DeploymentAgent.Fixture/DeploymentAgent.Fixture.csproj`, `Program.cs` and `packages.lock.json`.
- `DEPLOYMENT_ENGINE.md` and `docs/verification/M5-2026-10-27.md`.

**Modified:**

- `src/DeploymentAgent/Program.cs`, `DeploymentAgent.csproj` and `packages.lock.json`.
- `src/Api/prisma/schema.prisma` and `src/Api/test/database.test.mjs` for the appended schema/parity expectations.
- `InternalHostingPlatform.sln`, root `package.json`, `tools/publish.mjs` and `tools/tasks.mjs` for the new publish roots/verifier.
- `README.md`, `INSTALLATION.md`, `ARCHITECTURE.md`, `SECURITY.md` and `API.md`.
- The four tracked records: `PROJECT_REQUIREMENTS.md`, `AGENTS.md`, `IMPLEMENTATION_PLAN.md`, `PROGRESS.md`.

Generated API/client/build outputs and published artifacts were refreshed by approved build steps; they are not progress-repository source. Original samples/existing hosted applications were not modified.

## Known issues, limits and unfinished work

- **Trusted single-process contract only.** Releases must implement `ihp-loopback-v1` and obey fixed loopback/port/health/shutdown settings. Arbitrary uploads, package scripts, shell commands, installs/builds and unreviewed code are refused/out of scope.
- **Process separation is not a sandbox.** Children use the same ordinary Windows identity. Jobs/environment checks do not prevent malicious code accessing that identity's files, secrets or network resources. Actual listener validation can reject misbehaviour but cannot prevent untrusted code briefly opening a wrong listener. No real unsafe listener was opened to test this.
- **Small launch/assignment window.** Process.Start precedes job assignment; atomic suspended/job-list creation is not implemented. The trusted stdin-EOF contract and reconciliation reduce orphan risk, but full orphan-proof recovery is not claimed. The hard-kill test observes child/port cleanup under the job/EOF contract; it cannot distinguish which action won the race. A separate uncooperative fixture proves explicit job-based termination.
- **Probe-to-bind race is handled, not removed.** The exclusive allocation probe closes before startup. The actual competing-listener test verifies safe failure/cleanup; the Agent never claims to hold an OS reservation after probe close.
- **Same-user tampering.** Rechecked hashes/canonical metadata are not immutable ACL isolation or cryptographic IT approval; files/state can be raced by the same identity.
- **Logs are best-effort redacted.** Unknown/unlabelled secrets could leak from a badly behaved trusted app. No platform secrets are injected. Capture is limited to 2048 characters/line and 64 KiB retained text/instance; at most 1000 SQLite log rows/deployment.
- **Recovery safety beats guessing ownership.** A matching live prior PID/start identity is BLOCKED with its lease retained; it is not adopted/killed based only on persisted PID. Resolve the ownership condition, then restart the Agent or request a deliberate start. Interrupted exit status is null/unknown unless actually observed.
- **No unattended startup guarantee.** No service, scheduler, login/logout/reboot policy or security setting was installed/changed.
- **Current environment coverage.** Exact Node 24 and manual Ctrl+C compatibility remain unverified as before; current installed runtime and supervised shutdown are verified. Windows SQLite servicing belongs to the OS/IT.
- **Operator configuration.** A standalone Agent reads IHP_STORAGE_ROOT/IHP_DATABASE_PATH/IHP_RUNTIME_PORT_START/IHP_RUNTIME_PORT_END from environment. For custom JSON API settings, supply identical effective Agent values; the standard launcher exports them. Agent Intent/Instance is the live M5 status boundary; existing API application metadata status is not a new live runtime endpoint.
- **Later milestones remain unfinished.** No M6 registry-backed live routing, M7 upload/build execution, M8 deployment dashboard, M9 monitoring UI/search, M10 rollback or M11 end-user auth was implemented. M12 broad hardening remains future work.
- No unresolved failing test or known blocking defect remains within the reviewed M5 scope. The accepted non-SQLite connectivity gap is retained; it must not become a future instruction to install/contact those engines.

## Exact next task and resume commands

**Stop after the M5 handoff.** The next implementation task, only after the user authorizes M6, is to read its acceptance criteria and inspect the current Gateway route-provider boundary, M4 route registry and M5 runtime/port contracts before implementing controlled live route updates. Keep API execution disabled and all listeners on loopback.

Every new session: read AGENTS → this current snapshot → relevant IMPLEMENTATION_PLAN/PROJECT_REQUIREMENTS; inspect actual source/published inputs and verify the workspace matches evidence. A progress-only clone does not contain source; resume from the retained implementation workspace or an explicitly approved source backup.

Useful commands in the existing workspace (database commands are explicit local steps; do not change existing applications):

```powershell
Get-Content -LiteralPath AGENTS.md -Encoding UTF8
Get-Content -LiteralPath PROGRESS.md -Encoding UTF8 -TotalCount 260
Get-Content -LiteralPath IMPLEMENTATION_PLAN.md -Encoding UTF8
Get-Content -LiteralPath PROJECT_REQUIREMENTS.md -Encoding UTF8
git status --short
npm.cmd run doctor
npm.cmd run build
npm.cmd --workspace @ihp/api run db:migrate
npm.cmd --workspace @ihp/api run db:status
npm.cmd run test:agent
npm.cmd run verify
```

Run dependency restoration only if needed: `npm.cmd ci --ignore-scripts`. Never replace the pinned guarded Prisma scripts with native Prisma Migrate, bare engine commands, VSTest or dotnet test/run. See local DEPLOYMENT_ENGINE.md for staging/review/control and docs/verification/M5-2026-10-27.md for the concise evidence copy.

For four-document synchronization, inspect the index/commit paths, then:

```powershell
git add -- PROJECT_REQUIREMENTS.md AGENTS.md IMPLEMENTATION_PLAN.md PROGRESS.md
git diff --cached --name-only
git commit -m "Complete M5 trusted runtime deployment engine"
git push origin main
git ls-remote origin refs/heads/main
```

Only those four files may appear in the staged change/commit. Compare the remote main SHA with the local commit after pushing; the final session handoff reports the resulting commit. Source code is deliberately not pushed to this repository.

## Preserved progress history

The following complete earlier log is historical evidence, not active instructions or current status. Its former M4/M5 wait messages, earlier counts, blockers and superseded connectivity work must not override the current snapshot.

``````````text
# Project Progress

## Active M5 session — 2026-10-27

M5 is IN PROGRESS under the user's new explicit authorization, superseding the prior wait-for-M5 handoff below. Read AGENTS, current progress, M5 plan, process/runtime/recovery/security requirements, and actual Agent/Foundation/sample/SQLite sources. M1–M4 source/evidence is present; Agent is still a no-execution Worker shell and no M5 tests exist. Plan: reviewed fingerprinted releases only, SQLite registry migrations/leases/intent/exit state, single Agent ownership, safe child environment, Windows-owned process containment, actual listener/health verification, persisted bounded recovery and managed published verification. No M6+ work or external database operation is authorized. First task: verify a policy-compatible SQLite provider using the Windows SQLite DLL in-process (no downloaded executable) and implement the controlled core.

Provider/core step: selected Microsoft.Data.Sqlite.Core 8.0.33 plus SQLitePCLRaw.provider.winsqlite3 2.1.11; a published managed DLL probe passed against Windows' existing SQLite 3.43.2. No SQLite server/native executable was installed/launched. Added reviewed M5 SQL migration and four Prisma models (15 total), durable release/intent/port/instance state, fingerprinted file inspection with no application execution, Windows job ownership/single-process containment, PID-owned IPv4/IPv6 listener checks, bounded health/start/stop/recovery, log capture/redaction and trusted local stdin control. Core publishes with no compiler errors. Added a separate trusted managed fault fixture; behavioral verification is not yet run, so no lifecycle criterion is marked complete. Next: publish all M5 artifacts and replace the provider-only probe with real end-to-end managed checks.

First behavioral gate: the managed verifier initially had one C# multiple-var declaration error; corrected it before any execution. Review also moved console input to its own task so it cannot block the recovery timer. npm.cmd run test:agent then exited 0 with **25 real managed checks**, including concurrent real samples, exit 17/23 tracking, health failure, job-based forced termination, redacted bounded streams/clean environment, durable budgets, singleton ownership, live-PID quarantine/reuse protection, graceful recovery, and an actual published-Agent hard restart with owned-child cleanup. Added canonical persisted-release path/health URI checks before launch and real select-to-bind race verification; these final additions are pending rerun. M5 remains IN PROGRESS. Next: final focused/full regression and operator/isolation documentation; no M6+ work.

Final dedicated M5 run passed **27 checks, zero failures** after the canonical-metadata and actual select-to-bind race regressions were added. The race fixture binds only 127.0.0.1; the competing listener remains alive, the failing child is stopped, and its SQL lease is released. Actual Agent listener inspection confirms no Agent HTTP port; hard restart records an unknown prior exit honestly and starts a new healthy instance with persisted intent/budget. Created DEPLOYMENT_ENGINE.md and updated architecture/security/setup boundaries. Node/.NET samples and existing applications are unchanged; only owned test copies ran. Next: full project regression against the appended migration/15-model schema, then final evidence/status and four-document-only synchronization.

**Updated:** 2026-10-27 (Asia/Bangkok)

## Current milestone and handoff

- **Last completed milestone:** M4 — Platform API and Database, **COMPLETED under the authoritative SQLite-only policy**.
- **Previously completed:** M1–M3 remain COMPLETED; regression checks passed.
- **First unfinished milestone:** M5 — Deployment Engine, NOT STARTED. **Do not start M5 in this session.**
- **Last completed task:** Removed prohibited external-engine execution paths/dependencies/fixtures, verified SQLite/reference/policy behavior, and passed the full regression.
- **Final gate:** `npm.cmd run verify` exited 0: **259 passing automated checks**, zero failures/skips, plus real platform smoke.
- **Blockers:** None for the revised M4 scope. External-engine connectivity is an accepted policy gap, not a pending dependency or task.
- **Git scope:** Only PROJECT_REQUIREMENTS.md, AGENTS.md, IMPLEMENTATION_PLAN.md, PROGRESS.md may be committed/pushed to `https://github.com/logtranwanek-spec/IAH-Progress-Tracking-.git` on main. Implementation/additional docs/binaries remain local and untracked; a fresh progress-repository clone cannot run the implementation.

## Authoritative policy and accepted verification gap

**Only SQLite is supported and allowed for real database operations on this machine.** The latest user clarification supersedes the previous M4 blocker and instructions to obtain/test SQL Server, PostgreSQL, or MySQL instances.

SQL Server/PostgreSQL/MySQL remain supported **only as connection-reference metadata type labels** with protected registration, read, update and application assignment APIs. Their real operational drivers, connectivity, authentication, TLS/certificate checks and protocol fixtures are not supported or implemented in the current execution path. Do not install or connect to these engines, even on loopback. Do not claim real authentication or query evidence. No environment flag enables those operations.

The missing real connectivity verification is explicitly **accepted by the user as a company-policy gap** and excluded from M4 completion criteria. M4 completion records the adjusted scope, not a claim that prohibited tests passed. Earlier failed-connection/protocol evidence remains historical below and must never be replayed under current policy.

For an existing valid reference, POST /_platform/api/database-connections/:id/test returns controlled **403 database_connectivity_disabled_by_policy** after SQLite metadata validation only. It does not load credentials, import a database driver, open a database socket, emit connection-success/failure audit claims, or return a reachable value. Invalid/missing reference IDs still return 400/404.

## Features verified and acceptance evidence

| Revised M4 criterion                 | Actual evidence                                                                                                                                                                |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Native-free Prisma + real SQLite     | PASS: pinned Prisma 7.10.1 WASM validation/generation and real JS/WASM client on node:sqlite; 11 normalized models                                                             |
| Fresh/versioned registry migrations  | PASS: reviewed SQL, checksum history, FK/type/model parity, indexes/constraints, idempotent reapply, rollback and changed-history rejection                                    |
| Registry CRUD foundations            | PASS: real NestJS create/read/update, safe duplicate/reserved/path/input validation, pagination/filter/search and concurrent write/audit atomicity                             |
| Reference metadata retained          | PASS: all three external-engine labels register/read/update/assign using SQLite and protected DPAPI references; assignment idempotent; schema/migration unchanged              |
| SQLite-only enforcement              | PASS: three HTTP policy-denial checks return 403 with zero credential loads and no reachability payload; reference ID checks use only SQLite                                   |
| Credentials protected/omitted        | PASS: real published-DLL DPAPI round trips, tampering/reference-swap rejection, rotation/concurrent cleanup, post-restart decryption, raw DB/HTTP/log redaction                |
| Errors/logs/OpenAPI/guards           | PASS: safe problem responses with trace IDs, structured logs, strict body/query validation, docs describe policy denial with no success response; management defaults disabled |
| Destructive/deployment boundary      | PASS: local operator capability required; DELETE disabled; deployment/start/stop/restart/rollback endpoints absent                                                             |
| Windows/loopback/no-.exe constraints | PASS: Administrator=False; only loopback platform/sample/API listeners; all seven publish roots scanned with no .exe; .NET launched only as published DLLs                     |
| External-engine real connectivity    | ACCEPTED POLICY GAP: no real connect/protocol/authentication test was executed in this completion session; this capability is prohibited and not claimed                       |

SQLite registry health performs a real local query. Real migrations, persistence, dates/booleans/bytes/relations, FKs, unique slugs, runtime/route data constraints and concurrent transactions remain verified. There was no new data migration because the schema already uses SQLite; metadata type labels/fields and existing constraints remain intact.

## Changes made in the SQLite-only completion session

- Removed `src/Api/src/connectivity.ts` and its obsolete compiled `dist/connectivity.js` / map.
- Removed pg/mysql2/tedious/@types/pg as **direct API dependencies** and the now-unused Azure MSAL override; npm updated the lockfile. Prisma tooling may retain unused transitive libraries; none grants operational support or is imported as an external-engine execution path.
- Replaced Registry.testConnection with a metadata-only lookup and an unconditional policy rejection for an existing reference; no secret load or socket code.
- Updated controller/OpenAPI descriptions to SQLite-only operation and explicit 403; removed its successful-connectivity response.
- Removed all external database rejection/stall TCP fixtures from the executable test suite **before running tests**. Replaced them with policy-denial/no-vault-load and ID-validation tests. SQLite/reference/DPAPI tests remain real operations.
- Verified source search has no pg/mysql2/tedious/testConnectivity/protocol-fixture imports/usages in API source/test/package declarations; obsolete connectivity source and compiled JS are absent.
- Updated permanent AGENTS policy, authoritative requirements, roadmap acceptance/status, current progress, API/architecture/security docs and historical-evidence banner.
- No M5 implementation, database server installation, external-engine connection attempt, corporate-network deployment, production access, existing-application modification or Windows security-setting change.

## Actual commands and results

| Command/check                                                                                        | Result                                                                                                      |
| ---------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| `npm.cmd uninstall --ignore-scripts --workspace @ihp/api pg mysql2 tedious @types/pg`                | Exit 0; 78 packages removed; zero reported vulnerabilities                                                  |
| Exact project-owned obsolete connectivity JS/map removal                                             | Completed; subsequent Test-Path checks returned False for source and compiled JS                            |
| `npm.cmd run build --workspace @ihp/api`                                                             | Exit 0: guarded WASM validation of 11 models, native-free TypeScript client generation, strict compilation  |
| `npm.cmd run test --workspace @ihp/api`                                                              | Exit 0: **33 HTTP/reference/policy + 13 guarded SQLite** checks passed                                      |
| `npm.cmd run format`                                                                                 | Exit 0                                                                                                      |
| `npm.cmd run verify`                                                                                 | **Exit 0; 259 checks plus platform smoke**                                                                  |
| `npm.cmd audit --json`                                                                               | Exit 0; zero vulnerabilities                                                                                |
| `npm.cmd config get ignore-scripts`                                                                  | true                                                                                                        |
| Current Windows principal role check                                                                 | Administrator=False                                                                                         |
| Source/test/dependency search and artifact existence checks                                          | No external-driver/protocol-fixture usages in the API paths; deleted connectivity source/compiled JS absent |
| `node src/Api/tools/database.mjs migrate` with isolated temporary IHP_STORAGE_ROOT/IHP_DATABASE_PATH | Exit 0; fresh owned SQLite registry applied/verified                                                        |
| `node src/Api/tools/database.mjs status` against that registry                                       | Exit 0; migration history current                                                                           |

The extra CLI checks used a new user-profile directory, not the actual platform registry. An initial command including recursive fixture cleanup was rejected by automatic approval review **before execution**, with stated reason only “blocked by policy.” A safer command omitted recursive cleanup and successfully ran migrate/status. The resulting schema-only verification artifact is intentionally retained at:

```text
C:\Users\logtran\IhpM4SqliteCli-80e764c42d904a95b3d9a1873e7b5617\registry\platform.db
```

No credentials, actual application records, or persistent environment/config changes were introduced by that retained CLI fixture. Its environment overrides existed only in that command's shell. No system security control or approval mechanism was bypassed.

## Final automated evidence

| Suite                                          | Passed  | Failed/skipped |
| ---------------------------------------------- | ------- | -------------- |
| Node foundation/lifecycle/sample configuration | 25      | 0              |
| API HTTP/reference/policy/DPAPI                | 33      | 0              |
| Guarded Prisma/SQLite/migration                | 13      | 0              |
| Managed Foundation                             | 27      | 0              |
| Managed Gateway                                | 79      | 0              |
| Managed Samples                                | 82      | 0              |
| Total registered checks                        | **259** | **0**          |

**The total remains 259 because forbidden protocol cases were replaced with policy-denial/ID checks. This is new SQLite-only verification, not a replay of the former driver tests.** Prohibited external-engine authentication/TLS/query checks are outside the accepted scope and were not executed or represented as passing.

The 33 HTTP checks cover real registry operations, capabilities/default deny, reserved/duplicate/malformed input, pagination/search/filtering, atomic concurrent creates, safe errors/body limits, OpenAPI/Swagger, all three reference labels, DPAPI tamper/swap/rotation/concurrent cleanup, policy denial before vault loads, redaction, missing IDs and actual API restart persistence.

The 13 guarded SQLite checks cover native-process rejection, 11-model/table scalar/type/FK parity, fresh/idempotent migrations, real Prisma scalar/relationship mapping, P2002/P2003, reserved-route/runtime constraints, rollback, active-port uniqueness, concurrent transaction isolation, changed migration checksums, failed-DDL rollback and unsafe/uninitialized storage.

Additional successful smoke: controlled fresh SQLite migration in smoke-owned temporary storage; published Gateway JSON logs; real registry/API/React responses; only 127.0.0.1 on ports 5050/5065/5173; four owned platform processes exited 0; sockets released; smoke temporary storage removed. Managed sample checks also verified concurrent Node/.NET loopback processes on 6411/6421, prefix routing, exit 0 and port release.

All seven .NET publish roots passed no-.exe scans: Gateway, DeploymentAgent, Foundation.Tests, Gateway.Tests, DemoDotnet, CredentialProtection, Samples.Tests. The build uses UseAppHost=false and launches published DLLs through dotnet. No VSTest, apphost, testhost, dotnet run/test, Docker, elevation or policy workaround was used. Prettier, TypeScript and solution dotnet format checks passed. Testing uses the installed Windows x64 / Node 26.12.1 / npm 11.20.0 / SDK 8.0.425 / ASP.NET/.NET 8.0.31 environment.

## Files changed

**Only files authorized for commit/push:** PROJECT_REQUIREMENTS.md, AGENTS.md, IMPLEMENTATION_PLAN.md, PROGRESS.md.

**Modified local implementation/support:** src/Api/src/registry.ts, src/Api/src/controllers.ts, src/Api/src/app.ts, src/Api/test/api.test.mjs, src/Api/package.json, package.json, package-lock.json.

**Removed local implementation/build files:** src/Api/src/connectivity.ts and obsolete src/Api/dist/connectivity.js / connectivity.js.map.

**Modified local documentation:** API.md, ARCHITECTURE.md, SECURITY.md, README.md, docs/verification/M4-2026-10-27.md (historical banner).

**New local evidence:** docs/verification/M4-SQLite-only-2026-10-27.md.

Existing schema/migration, sample applications, Gateway/Agent/dashboard source, published-DLL policy and operating-system settings were not changed. Generated build output remains local/ignored. Implementation is deliberately not committed to the four-document progress repository.

## Decisions, failures and limitations

- M4 completion follows the user's explicit revised SQLite-only acceptance scope. The accepted external-engine gap is caused by company policy, not an unresolved software defect or need for IT/server approval. Do not seek servers or replay old protocol tests.
- No implementation build/test failed in this completion session. The separately rejected CLI cleanup command was handled through the safer retained-fixture approach above; no rejected action was counted as executed.
- Metadata support for SQL Server/PostgreSQL/MySQL is **not operational database support**. The current test endpoint never reports reachability.
- Prisma remains pinned and native-free using internal WASM/generator APIs and the published CommonJS generator entry. The reviewed Node migration runner is not native Prisma Migrate; automatic diff/shadow/history/drift-repair features remain unsupported. No different ORM was substituted.
- DPAPI CurrentUser is at-rest identity protection, not isolation from same-user code. Crash-time orphan reconciliation, identity/profile backup/recovery, full authentication/RBAC and production hardening remain later scope.
- Exact Node 24 and interactive manual Ctrl+C remain prior documented unverified limits. Real supervised clean shutdown and current installed-runtime checks pass.
- No current M4 blocker remains. M5–M12 are NOT STARTED.

## Exact next recommended task

Provide this M4 completion handoff and **wait for separate authorization before M5**. Do not implement the Deployment Engine now. At the next authorized session, read AGENTS/current progress/the applicable milestone, consult current SQLite-only requirements, inspect repository state, and resume only authorized unfinished work. Never infer permission to add external database operations from the retained metadata models.

Safe verification/resume commands (not authorization to start M5):

```powershell
git status --short --branch
Get-Content -LiteralPath AGENTS.md -Encoding UTF8
Get-Content -LiteralPath PROGRESS.md -Encoding UTF8
Get-Content -LiteralPath IMPLEMENTATION_PLAN.md -Encoding UTF8
npm.cmd run build
npm.cmd run test --workspace @ihp/api
npm.cmd run verify
```

Use npm.cmd ci --ignore-scripts if restore is necessary. For explicitly initializing only the platform-owned SQLite registry after build, db:migrate/db:status remain separate controlled steps documented in local API.md. Do not run bare Prisma CLI/Migrate or any SQL Server/PostgreSQL/MySQL install/connection/authentication/protocol command.

## Progress repository synchronization

Stage/commit/push only the four named Markdown paths; inspect exact staged and commit/tree paths, then compare remote main with local HEAD. The final handoff supplies the commit receipt. Source/extra docs remain local. Current M4 completion and the accepted policy gap supersede previous BLOCKED/obtain-server instructions.

## Preserved previous records

All previous progress, intermediate SQLite-only work, earlier M4 evidence/blocker decisions and M1–M3 history are retained below as historical text. Earlier external-driver tests preceded the current policy clarification and are no longer authorized. Their commands/statuses must not override the current policy or be replayed.

`````````text
# Project Progress

## Active SQLite-only M4 completion — 2026-10-27

The latest explicit user policy supersedes the prior M4 blocker: **only SQLite is allowed for real operations**. External-engine references remain metadata-only; SQL Server/PostgreSQL/MySQL installation, connectivity, and protocol-fixture checks are prohibited. Their live verification is an accepted policy gap. Read the four records and inspected actual sources; found operational drivers and earlier fixture tests that must be removed/disabled before any verification is rerun. No network or external-engine test has run in this completion session. Next: remove the execution path/dependencies, verify explicit policy rejection and unchanged SQLite/reference behavior, then mark M4 COMPLETED. M5 remains unauthorized/not started.

Implementation adjustment: removed connectivity.ts and obsolete compiled connectivity.js/map, removed pg/mysql2/tedious/@types/pg as direct API dependencies plus the unused Azure override using npm.cmd uninstall --ignore-scripts (exit 0; 78 packages removed; audit zero). The compatibility test endpoint now validates only stored SQLite metadata and throws 403 database_connectivity_disabled_by_policy without loading credentials or importing drivers. Removed all protocol rejection/stall fixtures before tests run; replaced them with policy-denial/no-credential-load and reference-ID checks. Swagger now describes SQLite-only operation and no successful-connectivity response. Reference schema/migration and registration/read/update/assignment/vault capabilities are retained. These revisions await build/tests; no new completion claim yet.

Targeted gate: npm.cmd run build --workspace @ihp/api exited 0; npm.cmd run test --workspace @ihp/api exited 0 with 33 HTTP/policy checks and 13 guarded SQLite checks. All three external-engine labels were tested only as metadata references and controlled 403 policy denials; vault load counts stayed zero for these requests, no reachability payload was returned, and no protocol fixture ran. SQLite migrations/constraints/transactions and DPAPI rotation/restart/redaction checks still pass. Updated the operational docs to remove instructions for forbidden tests. Next: format and full regression, then record M4 completion under the revised criteria.

**Updated:** 2026-10-27 (Asia/Bangkok)

## Current milestone and session handoff

- **Current/first unfinished milestone:** M4 — Platform API and Database, **BLOCKED** on one external verification dependency: successful authenticated SQL Server/PostgreSQL/MySQL connectivity.
- **Completed milestones:** M1, M2, M3 remain COMPLETED; their regressions passed.
- **Last completed implementation task:** Native-free Prisma/SQLite registry, guarded real NestJS APIs, DPAPI credential references, concurrent-rotation correction, restart persistence, and final complete regression.
- **Final gate:** `npm.cmd run verify` exited 0 with **259 passing automated checks**, zero failed/skipped registered tests, plus full real platform smoke. The three positive live database integrations are **not executed**, not implied by that automated total.
- **User clarification:** No approved local database test instances exist; the user requested documenting the verification gap. No server was installed or company database contacted.
- **Next task:** Obtain approved pre-existing loopback test instances and locally configured credentials, verify actual authenticated SELECT 1 for all three database families, then reassess M4 completion. Do not start M5.
- **Git scope:** Only PROJECT_REQUIREMENTS.md, AGENTS.md, IMPLEMENTATION_PLAN.md, and PROGRESS.md may be committed/pushed to `https://github.com/logtranwanek-spec/IAH-Progress-Tracking-.git` on main. All source/binaries/additional docs remain local/untracked. A fresh clone of that progress repository contains no runnable implementation; retain this workspace.

## M4 acceptance evidence and blocker

| Criterion                                           | Actual result                                                                                                                                                                                                                                                     |
| --------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Prisma validates schema and fresh SQLite migrations | PASS: pinned Prisma WASM validates 11 normalized models; controlled reviewed SQL creates fresh tables; real Prisma client persists/queries SQLite                                                                                                                 |
| Versioned constraints/indexes/FKs                   | PASS: model/table column names and types, FK parity, unique slugs, reserved paths, route/runtime range checks, active-port uniqueness, checksums, rollback, and idempotent reapplication                                                                          |
| Application registry CRUD foundations               | PASS: real HTTP create/read/update, duplicate/reserved/input validation, filter/search/pagination, atomic audit entries, concurrent duplicate-create conflict                                                                                                     |
| Connection registration/assignment/update           | PASS: SQL Server/PostgreSQL/MySQL metadata and reference operations, idempotent assignment, DPAPI storage/rotation, concurrent cleanup, safe list/get/assigned responses                                                                                          |
| Full connectivity behavior                          | **PARTIAL/BLOCKED:** all three actual JS drivers reached owned loopback rejection/stall fixtures and returned safe failure/timeout; overlap limit verified. Real authenticated success and server-certificate acceptance remain unverified for all three families |
| Credential protection and omission                  | PASS: real published-DLL Windows DPAPI round trip, tampering/reference swap rejection, post-restart unprotection; response/log/raw registry byte scans omit credentials/token                                                                                     |
| API validation/errors/OpenAPI                       | PASS: real NestJS HTTP checks, 400/401/403/404/409/413/429/503 behavior, trace IDs, schemas and Swagger, safe request log fields                                                                                                                                  |
| Destructive/deployment authorization boundary       | PASS: default management disabled, configured local bearer capability required, Origin denied, DELETE disabled; deployment/runtime/rollback routes absent                                                                                                         |
| Windows/no-elevation/no-native policy               | PASS for implemented workflow: Administrator=False; all listeners IPv4 loopback; all seven .NET publish roots have no .exe; helper runs only as published DLL                                                                                                     |

**M4 is not COMPLETED.** The unavailable approved database instances prevent full successful connectivity verification. The user confirmed the absence; no criterion was waived. Do not convert TCP failure/timeout evidence into a successful authentication claim. No M5–M12 work was started.

## Features implemented and architectural decisions

- Real NestJS API at `http://127.0.0.1:5065/_platform/api` (configured component port); health now reports milestone=registry and performs a real SQLite query.
- Pinned Prisma 7.10.1 schema/client/query compiler. Eleven normalized models: User, Application, Deployment, RuntimeInstance, Route, DatabaseConnection, DeploymentLog, AuditLog, ApplicationSecret, DeploymentApproval, plus ApplicationDatabaseConnection join.
- Real SQLite via a reviewed built-in `node:sqlite` driver adapter. Serialized leases prevent unrelated root requests joining interactive transactions. Date/boolean/bytes/relations and Prisma P2002/P2003 constraint mappings verified.
- Initial reviewed migration `202610270001_registry`, explicit Node application step, SHA-256 history in _ihp_migrations, immediate transactions, FK checks, rollback, changed-history rejection, and startup refusal for uninitialized/outdated registry.
- Registry defaults to `<storageRoot>/registry/platform.db` outside source under the current profile. IHP_DATABASE_PATH must stay strictly inside storageRoot without storage links; IHP_STORAGE_ROOT keeps existing validated configuration behavior.
- Health/docs are public on loopback. Management defaults to 503 management_disabled. An explicitly configured strong IHP_LOCAL_OPERATOR_TOKEN enables local capability-protected management; missing/invalid bearer yields 401, browser Origin is denied. This is a local operator boundary, not M11 login/session/RBAC.
- Application metadata create/read/update with canonical unique slugs; framework derived from node/dotnet runtime; slug/runtime immutable after creation; owner remains nullable until user/authorization work.
- Paginated/filterable/searchable read APIs; strict object/query validation and 64 KiB JSON limit; safe application/problem+json and matching X-Request-Id/traceId.
- Structured UTC request logs use route patterns, status, method, duration and trace only. No parameter values, query/body/header values, SQL, raw driver errors, credentials, or bearer tokens.
- OpenAPI JSON and Swagger UI; input credentials write-only, safe response models, bearer and error contracts. Swagger submissions and persisted authorization disabled.
- Database connection register/list/read/update/assign/test for sqlserver/postgresql/mysql with fixed literal loopback hosts. Opaque credential references are never returned; connection strings, DNS/remote hosts, SQL input, native/CLI auth, and TLS overrides are unsupported.
- Windows DPAPI CurrentUser through published CredentialProtection.dll. UUID-bound entropy prevents ciphertext swapping. Secrets use bounded private stdin/stdout, not process arguments. SQLite stores only references; ciphertext lives under storageRoot/vault. Rotation commits metadata/audit then removes the previous ciphertext; failed registration cleans new ciphertext.
- Capturing the previous credential reference inside the SQLite transaction fixes cleanup during concurrent rotations; regression confirms only referenced ciphertext remains.
- Actual pg/mysql2/tedious drivers use verified TLS and fixed SELECT 1, basic credentials, 3-second connect/query stage budgets, four active checks maximum, and no overlapping check for one reference. Rejection/stall fixtures are actual owned loopback TCP sockets, not simulated successful database servers.
- API code rejects native addon loading and every child-process path except the exact published credential-helper DLL through dotnet. The launcher passes the operator capability only to API, not Agent/Gateway/dashboard/helper.
- Destructive operations disabled and deployment/runtime/rollback endpoints absent. No database server installation, corporate-network listener/access, production access, existing application modification, or IT/security/ACL/certificate setting change.

## Prisma policy assessment and limitations

1. Registry discovery found latest Prisma `8.0.0-rc.27`; this prerelease was not adopted. Stable `7.10.1` was pinned for CLI/client/internal generator/driver contracts.
2. Normal Prisma CLI depends on @prisma/engines, whose package postinstall can fetch engines. Default SQLite adapter depends on better-sqlite3/native addon installation. No such install hook or native Prisma engine was executed.
3. Dependencies were installed using --ignore-scripts. Project .npmrc now sets ignore-scripts=true; `npm.cmd config get ignore-scripts` returned true. Future restores must preserve this.
4. A standard CLI validate probe was run with the guard preloaded. It attempted a network operation, which was rejected before any download/process. This is assessment evidence, not a successful CLI validation. Native Prisma Migrate was not run.
5. Direct pinned @prisma/internals config/schema/DMMF WASM validation succeeds under the same guard. The in-process TypeScript generator succeeds with engineType=client and binaryPaths={}; its manifest declares no required native engines. Output target containment/links and native artifacts are checked.
6. The generator package's ESM entry has unresolved extensionless imports on this Node. Its published CommonJS entry works; no node_modules package was patched. Internal Prisma APIs are version-sensitive and must remain pinned/re-audited on upgrade.
7. Runtime Prisma uses the real JS/WASM query compiler and custom node:sqlite adapter; guarded database tests demonstrate persistence and transactions while subprocess/addon/network paths are rejected.
8. The migration runner is **custom Node code applying reviewed repository SQL** in Prisma's migration directory, with its own _ihp_migrations history. It does not implement native Prisma Migrate, _prisma_migrations history, automatic SQL diff/shadow databases, arbitrary uploaded SQL, full drift repair, or interchangeability with standard migration commands. This is not a substituted ORM.

Primary references reviewed: [Prisma driver architecture](https://www.prisma.io/docs/orm/v7/core-concepts/supported-databases/database-drivers), [Prisma engine internals](https://www.prisma.io/docs/orm/v6/more/internals/engines), [Node SQLite](https://nodejs.org/api/sqlite.html), [Microsoft CurrentUser protection](https://learn.microsoft.com/en-us/dotnet/api/system.security.cryptography.dataprotectionscope), [Nest OpenAPI](https://docs.nestjs.com/openapi/introduction), [node-postgres TLS](https://node-postgres.com/features/ssl), [Tedious connection API](https://tediousjs.github.io/tedious/api-connection.html). Local package manifests/types/source were inspected before selecting/executing the supported paths.

## Actual commands and results

| Command/check                                                                                                                                                                                                      | Actual result                                                                                                                                                     |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| npm.cmd view prisma version / pinned manifests and adapter dependency queries                                                                                                                                      | Discovery: latest 8.0.0-rc.27; pinned 7.10.1 dependency/source audit. @prisma/adapter-node-sqlite lookup returned E404; no official package was assumed available |
| node built-in SQLite :memory: probe                                                                                                                                                                                | PASS; SQLite 3.54.0                                                                                                                                               |
| npm.cmd install --ignore-scripts --save-exact --workspace @ihp/api @prisma/client@7.10.1 @prisma/driver-adapter-utils@7.10.1 @nestjs/swagger@12.0.4 pg mysql2 tedious                                              | Exit 0; resolved exact saved versions pg 8.23.1, mysql2 3.24.6, tedious 20.3.8                                                                                    |
| npm.cmd install --ignore-scripts --save-dev --save-exact --workspace @ihp/api prisma@7.10.1 @prisma/internals@7.10.1 @types/pg                                                                                     | Exit 0                                                                                                                                                            |
| npm.cmd install --ignore-scripts --save-dev --save-exact --workspace @ihp/api @prisma/client-generator-ts@7.10.1                                                                                                   | Initial ECONNRESET; retry with --fetch-retries=1 --fetch-timeout=60000 exited 0                                                                                   |
| npm.cmd install --ignore-scripts --save-dev --save-exact --workspace @ihp/api @types/express --fetch-retries=1 --fetch-timeout=60000                                                                               | Exit 0 after missing declaration build failure                                                                                                                    |
| npm.cmd update @azure/msal-node --ignore-scripts --fetch-retries=1 --fetch-timeout=60000                                                                                                                           | Exit 0; pinned override 7.1.0 resolves advisory; basic SQL authentication only, Azure/CLI auth not enabled                                                        |
| npm.cmd audit --json                                                                                                                                                                                               | Exit 0; zero vulnerabilities after override/update (initial report had two high transitive findings)                                                              |
| node --import ./tools/no-native.mjs ./node_modules/prisma/build/index.js validate --schema ./src/Api/prisma/schema.prisma                                                                                          | Denied before network operation; no engine ran; not counted as a pass                                                                                             |
| node src/Api/tools/prisma.mjs generate                                                                                                                                                                             | PASS: 11 models validated in WASM and real native-free TypeScript client generated                                                                                |
| dotnet sln InternalHostingPlatform.sln add src/CredentialProtection/CredentialProtection.csproj                                                                                                                    | Exit 0                                                                                                                                                            |
| dotnet publish .\src\CredentialProtection\CredentialProtection.csproj -c Release --no-self-contained -p:UseAppHost=false -p:UseSharedCompilation=false -p:PublishDir=../../artifacts/publish/CredentialProtection/ | Exit 0; helper invoked afterward only as dotnet <published DLL>                                                                                                   |
| npm.cmd run build --workspace @ihp/api                                                                                                                                                                             | Final exit 0; guarded generation and strict TypeScript compilation                                                                                                |
| npm.cmd run migration:test --workspace @ihp/api                                                                                                                                                                    | Initial 9/12; mapping fixes then 12/12. Final expanded database suite in full verify: 13/13                                                                       |
| npm.cmd run test --workspace @ihp/api                                                                                                                                                                              | Initial HTTP 25/28; parser/slug fixes then 29/29 and later 32/32. Final expanded HTTP suite in verify: 33/33                                                      |
| npm.cmd run format; dotnet format InternalHostingPlatform.sln --no-restore                                                                                                                                         | Exit 0                                                                                                                                                            |
| npm.cmd run verify (first full)                                                                                                                                                                                    | Exit 0; 258 checks plus smoke                                                                                                                                     |
| npm.cmd run verify (final after concurrent rotation correction)                                                                                                                                                    | **Exit 0; 259 checks plus smoke**                                                                                                                                 |
| npm.cmd run doctor                                                                                                                                                                                                 | Exit 0: Windows x64 build 22631, Node 26.12.1, npm 11.20.0, SDK 8.0.425, ASP.NET/.NET 8.0.31                                                                      |
| Current Windows principal role check                                                                                                                                                                               | Administrator=False                                                                                                                                               |
| npm.cmd config get ignore-scripts                                                                                                                                                                                  | true                                                                                                                                                              |

## Final automated check breakdown

| Suite                                          | Passed  | Failed/skipped |
| ---------------------------------------------- | ------- | -------------- |
| Node foundation/lifecycle/sample configuration | 25      | 0              |
| Real API HTTP/DPAPI/connectivity               | 33      | 0              |
| Guarded Prisma/SQLite/migration                | 13      | 0              |
| Managed Foundation                             | 27      | 0              |
| Managed Gateway                                | 79      | 0              |
| Managed Samples                                | 82      | 0              |
| Total registered automated checks              | **259** | **0**          |

Additional successful smoke: separate migration into an owned temporary database; actual published Gateway JSON logs; live registry/API/React responses; only 127.0.0.1 on 5050/5065/5173; all four platform children exited 0; sockets released; temporary storage removed. M3 samples also ran concurrently at 6411/6421 and exited 0 in their managed checks. DPAPI helper is invoked via its published DLL during the HTTP checks.

All seven publish roots were scanned without .exe artifacts: Gateway, DeploymentAgent, Foundation.Tests, Gateway.Tests, DemoDotnet, CredentialProtection, Samples.Tests. Prettier, TypeScript, and dotnet format passed. The final stalled-driver check completed in about 3.33 seconds for all three concurrent checks.

**Outside this automated pass count:** real successful authentication/SELECT 1/certificate acceptance against SQL Server, PostgreSQL, and MySQL was not run. No available approved server exists. Zero automated skips must not be read as verification of those unavailable integrations.

## Files created or modified during M4

**Tracked commit/push scope:** PROJECT_REQUIREMENTS.md (authorized M4/user clarification), AGENTS.md (permanent safe commands/boundaries), IMPLEMENTATION_PLAN.md (actual implementation/evidence/BLOCKED status), PROGRESS.md (this handoff and preserved complete history).

**New local implementation/config/docs:**

- .npmrc
- API.md
- src/Api/prisma/schema.prisma
- src/Api/prisma/migrations/202610270001_registry/migration.sql
- src/Api/src/database.ts
- src/Api/src/sqlite-adapter.ts
- src/Api/src/registry.ts
- src/Api/src/validation.ts
- src/Api/src/http.ts
- src/Api/src/native-policy.ts
- src/Api/src/controllers.ts
- src/Api/src/vault.ts
- src/Api/src/connectivity.ts
- src/Api/tools/prisma.mjs
- src/Api/tools/database.mjs
- src/Api/test/database.test.mjs
- src/CredentialProtection/CredentialProtection.csproj
- src/CredentialProtection/Program.cs
- src/CredentialProtection/packages.lock.json
- tools/no-native.mjs
- docs/verification/M4-2026-10-27.md

**Modified local files:** src/Api/src/app.ts, src/Api/src/main.ts, src/Api/test/api.test.mjs, src/Api/package.json; package.json, package-lock.json; packages/contracts/index.d.ts; tools/publish.mjs, tools/local.mjs, tools/smoke.mjs; InternalHostingPlatform.sln; .gitignore, .prettierignore; README.md, ARCHITECTURE.md, INSTALLATION.md, SECURITY.md.

Generated TypeScript/compiled client, all publish/build output, registry/vault files, node_modules, and extra documentation remain local and outside the four-document commit. No actual registry is migrated automatically when a developer starts the API.

## Failures, corrections, and remaining limits

- Guarded CLI network denial and missing official adapter E404 were discovery outcomes, not successful capabilities.
- One generator install failed with ECONNRESET; retry succeeded without proxy/security changes.
- Initial attempted legacy generator-build import did not exist in this Prisma version. The pinned TS generator export was inspected and used. Its ESM extensionless-import failure was corrected by using its published CommonJS entry, not altering dependency code.
- Initial TypeScript errors: adapter column/isolation types, then missing Express declarations. Corrected; final builds pass.
- Initial database checks failed on base64 byte parameters and generic SQLite constraint error mapping. Implemented proper bytes and P2002/P2003 conversion; independent relation/bytes/FK checks now pass.
- Initial HTTP checks exposed error-code inconsistencies and a body-parser 413 incorrectly reported as 500. Corrected safe filter mapping and canonical validation; final cases pass.
- Two high transitive Azure MSAL findings were removed with an explicit pinned override/update; audit now zero. No Azure/CLI authentication is supported.
- Review found concurrent credential rotations could orphan the intervening reference; the prior reference is now captured inside the transaction, and real concurrent HTTP rotation verifies cleanup.
- Some documentation patches failed context validation and made no changes before corrected reapplication. No failed build/test has been marked passing without a successful rerun.
- No confirmed unresolved regression remains in the implemented automated scope. **Live successful external-database verification remains BLOCKED**.
- CurrentUser DPAPI protects ciphertext at rest for the designated Windows identity; same-user application code can access secrets. No separate execution identity/process isolation, identity/profile backup/recovery, or crash-time orphan-vault reconciliation is provided.
- Registry metadata itself is not encrypted; credentials are in DPAPI ciphertext. ApplicationSecret models exist, but no secrets CRUD, login/RBAC, approval execution, or deployment feature exists.
- Capability tokens are explicit local development/operator credentials, not production authorization. No secrets are seeded.
- Exact Node 24 from the specification remains unverified; installed Node 26.12.1 and SQLite 3.54.0 were used. Broader arbitrary SQLite types/adapters, native Prisma Migrate/drift workflows, and production backup/recovery were not validated.
- Interactive Ctrl+C remains the prior documented manual gap; graceful supervised shutdown and port release are verified by real smoke.

## Exact next task and safe resume commands

Read the four records and inspect actual source/artifacts first. Keep M4 BLOCKED until approved **existing** loopback SQL Server/PostgreSQL/MySQL instances are supplied. Do not install a server or contact corporate/production endpoints. Configure credentials locally through the protected API, obtain a real reachable=true result after authenticated SELECT 1 for each family, verify redaction and TLS, and only then assess M4 completion. Never bypass TLS/IT restrictions or start M5 to avoid the missing evidence.

```powershell
git status --short --branch
Get-Content -LiteralPath AGENTS.md -Encoding UTF8
Get-Content -LiteralPath PROGRESS.md -Encoding UTF8
Get-Content -LiteralPath IMPLEMENTATION_PLAN.md -Encoding UTF8
npm.cmd run doctor
npm.cmd run build
npm.cmd --workspace @ihp/api run prisma:validate
npm.cmd --workspace @ihp/api run migration:test
npm.cmd --workspace @ihp/api run test
npm.cmd run verify
```

For explicitly initializing the platform-owned registry, after build:

```powershell
npm.cmd --workspace @ihp/api run db:migrate
npm.cmd --workspace @ihp/api run db:status
npm.cmd run start:local
```

Use `npm.cmd ci --ignore-scripts` if dependencies must be restored. Consult API.md before enabling the operator capability, creating connection references, or changing storage. Do not run bare Prisma CLI/Migrate, native engines, apphosts/testhost, dotnet run/test, or unreviewed SQL. To manually publish the helper:

```powershell
dotnet publish .\src\CredentialProtection\CredentialProtection.csproj -c Release --no-self-contained -p:UseAppHost=false -p:UseSharedCompilation=false -p:PublishDir=../../artifacts/publish/CredentialProtection/
```

The API supplies the helper's bounded stdin protocol and launches `dotnet .\artifacts\publish\CredentialProtection\CredentialProtection.dll`; do not pass credentials on a command line. No credentials or tokens belong in Git/chat.

## Progress repository synchronization

Only the four authorized project records are staged/committed/pushed. Inspect exact staged/commit/tree paths, then compare remote main to local HEAD. The final handoff supplies the commit receipt. Source and extra docs remain local. M4's blocked status and missing positive connectivity evidence must persist across future sessions.

## Preserved previous records

The complete previous M3 snapshot, M4 work timeline, intermediate failures, M2/M1 evidence, and older history are preserved below as historical text. Earlier statuses and next-task statements are superseded by this current snapshot.

````````text
# Project Progress

## Active M4 session — 2026-10-27

M4 is IN PROGRESS under the user's explicit M4-only authorization. Read AGENTS.md, current progress, M4 plan, database/API/security requirements, and actual API/config/source. M1–M3 source and completed verification records are present; API is still a real health-only foundation. No Prisma packages/schema/database existed. The prior M3 snapshot below is historical.

Assessment in progress: registry shows latest Prisma 8.0.0-rc.27 (prerelease) and stable 7.10.1 adapter packages. Prisma 7.10.1 CLI depends on @prisma/engines; default SQLite adapter depends on native better-sqlite3. Do not run install hooks or native engines. Installed Node's built-in node:sqlite successfully opened :memory: and reported SQLite 3.54.0. Investigate a pinned JS/WASM Prisma client with a reviewed built-in SQLite adapter and controlled versioned SQL runner if native Prisma Migrate cannot be used. No schema/client/migration functionality has been verified yet.

Prisma/source step: installed pinned packages with --ignore-scripts. A guarded standard CLI validation attempt was denied before any engine download/process; direct @prisma/internals WASM validation passed (11 normalized models). In-process pinned TypeScript generation passed after selecting the package's published CommonJS entry (its ESM entry has unresolved extensionless imports). No dependency was patched. Added a guard blocking tool subprocesses, native addons, and downloads; a custom built-in node:sqlite adapter and separate checksummed/transactional migration runner; all model scalar columns match the actual fresh SQLite tables. CredentialProtection published as a .NET 8 DLL (UseAppHost=false). Initial npm generator installation had ECONNRESET; retry succeeded. Initial TypeScript build had two adapter type errors; corrected and build passed. First database run: 9/12 passed; found bytes parameter conversion and SQLite constraint-to-Prisma error mapping failures. Correcting both, then rerunning. No failed criterion is marked completed. A transitive Azure MSAL advisory was resolved by a pinned override and npm update --ignore-scripts; audit now reports zero vulnerabilities. Next task: finish database verification, implement guarded API/credential/connectivity paths, and test real HTTP behavior.

Database verification rerun: build exited 0; all 12 guarded database/migration/Prisma checks passed, including bytes/date/bool/relations, P2002/P2003, atomic rollback and concurrent transactions. Added local capability guard (default management disabled), registry create/read/update/filter/page APIs, structured safe errors/request logs/OpenAPI, DPAPI vault calls via published DLL, connection metadata/reference assignment/update and actual JS-driver SELECT 1 paths constrained to verified-TLS loopback. Added only the missing Express declaration package after a type-check failure; the API now compiles. HTTP/DPAPI/driver verification has not yet run, so these features are not yet recorded as passing. No database server is installed or corporate database contacted; positive authenticated connectivity for the three external database families remains unavailable for live verification.

HTTP/credential verification: first HTTP run passed 25/28; corrected invalid-slug/parser error codes and raw body-parser 413 mapping. Rerun passed 29 HTTP + 12 database checks. Added application duplicate-create concurrency, schema type/FK parity, active-port data constraints, all-three-driver stalled-handshake budgets/overlap rejection, raw registry/log redaction, and API restart persistence. Final targeted run passed **32 HTTP + 13 guarded database checks**, zero failures/skips. Real DPAPI round trips, tampering/reference-swap rejection, rotation/cleanup and post-restart unprotection passed. Real SQL Server/PostgreSQL/MySQL drivers reached owned loopback rejection/stall fixtures and returned safe failures within the budget (~3.3 seconds for the combined stalled check). No successful authenticated database query is claimed. Asked whether approved pre-existing local test databases are available; continued independent work while awaiting information. Next: operational docs, final formatting/full regression, accurate acceptance status, and four-document-only synchronization.

First full regression passed: npm.cmd run verify exited 0 with 258 checks plus smoke (including separate migration into smoke-owned storage, live registry health, loopback sockets, graceful shutdown and cleanup). The user confirmed no approved local test instances are available and requested documenting the gap. Do not call successful external-database authentication verified. Final review corrected concurrent credential-rotation cleanup by capturing the previous vault reference inside the SQLite transaction; added an actual concurrent-rotation regression. Added project .npmrc ignore-scripts=true, safe operational docs, status-command no-create guard, and cleanup containment checks. In-process generation now reads its engine version from the pinned generator manifest. These final adjustments require the final full regression. M4 is still IN PROGRESS until that gate, then must remain open/blocked on the explicitly unverified real successful SQL Server/PostgreSQL/MySQL SELECT 1 checks. No later milestone was started.

**Updated:** 2026-10-26 (Asia/Bangkok)

## Current milestone and session handoff

- **Last completed milestone:** M3 — Node.js and ASP.NET Core Sample Deployments, **COMPLETED**.
- **Previously completed:** M1 and M2 remain COMPLETED; their regression checks passed.
- **First unfinished milestone:** M4 — Platform API and Database, **NOT STARTED**. No M4 implementation was started.
- **Last completed task:** Real concurrent sample/process and prefix verification, malformed-target regression, full build/static/test/smoke pipeline, and M3 run documentation.
- **Final gate:** `npm.cmd run verify` exited 0: **215 passing checks**, zero failures/skips, plus real platform smoke.
- **Blockers:** None for completed M3. M4 must first assess the Prisma toolchain against the no-.exe policy.
- **Git scope:** Only PROJECT_REQUIREMENTS.md, AGENTS.md, IMPLEMENTATION_PLAN.md, and PROGRESS.md may be committed/pushed to `https://github.com/logtranwanek-spec/IAH-Progress-Tracking-.git`, branch main. All implementation source and additional docs remain local/untracked. Preserve this workspace: a fresh progress-repository clone contains no runnable implementation.

## M3 acceptance evidence

| Criterion                                     | Actual result                                                                                                                                                                                                               |
| --------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Two separate real sample processes            | PASS: Node source and published ASP.NET Core DLL ran concurrently with distinct PIDs                                                                                                                                        |
| Loopback-only runtime listeners               | PASS: managed TCP inspection found exactly one IPv4 loopback listener per selected port, 127.0.0.1:6411 and :6421; hostile generic HOST/PORT/ASPNETCORE_URLS did not override the binding                                   |
| Configurable approved ports                   | PASS: actual nondefault ports 6411/6421; settings accept boundaries 6100/6999 and reject malformed/out-of-range/component ports                                                                                             |
| Healthy success endpoints                     | PASS: real GET/HEAD /health; correct status=ok, sample, runtime, release, build, port, basePath, healthPath; root returns status=running                                                                                    |
| Concurrency and clean termination             | PASS: both direct and prefixed phases; supervised shutdown command and stdin EOF yielded exit code 0; listeners disappeared and ports successfully rebound                                                                  |
| Prefix foundation                             | PASS: real /demo-node/health, /demo-dotnet/health, and their roots through published M2 Gateway at 127.0.0.1:5050; prefix-mode unprefixed health returned 404                                                               |
| Safe instructions and ordinary-user operation | PASS: sample/shared README instructions; dependency-free Node and installed ASP.NET Core 8 shared framework; Administrator=False; no elevation, Docker, security changes, existing-app modifications, or network deployment |

The prefix check uses a new test-owned temporary JSON file with the existing immutable M2 route provider. It never edits config/gateway.routes.json. The verifier stops its owned Gateway, verifies port 5050 release, and deletes that exact temporary file. No dynamic route registration/removal feature was implemented.

## Features implemented

- `samples/DemoNode`: minimal real HTTP server using Node built-in modules, no external dependencies/install step.
- `samples/DemoDotnet`: real ASP.NET Core 8 Kestrel server, framework-dependent published DLL, existing managed ConsoleShutdown helper.
- Validated environment settings shared in meaning across both runtimes: fixed literal 127.0.0.1; ports 6100–6999; optional canonical lowercase base path; bounded release/build tokens.
- Defaults: Node 6101, .NET 6102, empty base path, release 0.1.0, build m3. These are demonstration implementation defaults, not inferred business requirements.
- GET/HEAD health and root identity; no-store responses; known-path method rejection 405 and unknown-path 404. Node malformed URL targets return 400 and the process remains healthy.
- Normal terminal shutdown handlers and supervised stdin command/EOF. Automated verification covers the supervised paths; interactive Ctrl+C was not manually exercised.
- Managed Samples.Tests assertion program with controlled process launch through installed node/dotnet hosts, redirected streams, bounded readiness/exit waits, owned-process cleanup, and real HTTP/socket assertions.
- Strict startup failures for unsafe host/port/base-path configuration and port conflicts; failure exit code 1, no attempt to stop an existing occupant.
- Publish allowlist and root test flow include DemoDotnet/Samples.Tests; every .NET launch uses a previously published DLL after no-.exe scanning.
- Shared/per-sample instructions and root README link. The existing platform launcher still starts only its original four platform components; samples run separately.

## Actual commands and results

| Command/check                                                                                                              | Actual final result                                                                                                                                                                 |
| -------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `dotnet sln InternalHostingPlatform.sln add samples/DemoDotnet/DemoDotnet.csproj tests/Samples.Tests/Samples.Tests.csproj` | Exit 0; projects added                                                                                                                                                              |
| `npm.cmd run test:samples` (initial dedicated run)                                                                         | Exit 0; six Node and 80 managed checks; no failures                                                                                                                                 |
| `npm.cmd run format`                                                                                                       | Exit 0; project formatting applied                                                                                                                                                  |
| `dotnet format InternalHostingPlatform.sln --no-restore`                                                                   | Exit 0                                                                                                                                                                              |
| `npm.cmd run verify` (first full regression)                                                                               | Exit 0; 213 checks plus platform smoke                                                                                                                                              |
| `npm.cmd run verify` (final after malformed-target handling/regressions)                                                   | **Exit 0; 215 checks plus platform smoke**                                                                                                                                          |
| Node foundation/lifecycle + sample configuration                                                                           | 25 passed (19 existing + six sample), zero failed/skipped                                                                                                                           |
| Real API checks                                                                                                            | Two passed, zero failed/skipped                                                                                                                                                     |
| Managed Foundation checks                                                                                                  | 27 passed, zero failed                                                                                                                                                              |
| Managed Gateway checks                                                                                                     | 79 passed, zero failed                                                                                                                                                              |
| Managed sample checks                                                                                                      | **82 passed, zero failed**                                                                                                                                                          |
| Publish output                                                                                                             | Gateway, DeploymentAgent, Foundation.Tests, Gateway.Tests, DemoDotnet, Samples.Tests all scanned; no .exe artifacts                                                                 |
| Static checks                                                                                                              | Prettier, TypeScript, and solution dotnet format all passed                                                                                                                         |
| Real platform smoke                                                                                                        | Published Gateway structured JSON logs; 127.0.0.1 listeners on 5050/5065/5173; HTTP/React/API checks; all four owned components exited 0; ports released; temporary storage removed |
| `npm.cmd run doctor`                                                                                                       | Exit 0: Windows x64 build 22631; Node 26.12.1, npm 11.20.0, SDK 8.0.425, ASP.NET/.NET runtime 8.0.31                                                                                |
| Current Windows principal role check                                                                                       | Administrator=False                                                                                                                                                                 |
| `git fetch origin main` before synchronization                                                                             | Exit 0; local and origin/main matched 1114ff50b766d941076c7d7d9686b7e7f35ecb6c                                                                                                      |

The final check count is 25 + 2 + 27 + 79 + 82 = 215. Smoke assertions are additional and are not counted as a test suite.

Actual lifecycle evidence printed by Samples.Tests:

```text
EVIDENCE direct/shutdown Node=6411, .NET=6421; exit=0 each; sockets released.
EVIDENCE prefix/EOF Node=6411, .NET=6421; exit=0 each; sockets released.
Sample checks: 82 passed, 0 failed.
```

The direct phase verifies defaults for identity labels and empty base paths; the prefix phase verifies release=2.3.4 and build=reviewed-42. The original six Node tests cover safe defaults, boundary/configurable settings, invalid ports, unsafe hosts, reserved/noncanonical paths, and invalid release/build tokens. Managed checks independently validate .NET settings, published artifacts, concurrent processes, direct/prefixed HTTP identity, HEAD, 405/Allow, 404, live sockets, real YARP forwarding, safe startup failures, occupied ports, shutdown, and port release. Two final Node socket cases send an actual malformed absolute request target, require HTTP 400, and then require a healthy response.

The publishing helper's actual argument shape (run for each allowlisted project) is:

```powershell
dotnet publish <project.csproj> --configuration Release --framework net8.0 --no-self-contained --disable-build-servers -p:UseAppHost=false -p:UseSharedCompilation=false -p:PublishDir=../../artifacts/publish/<name>/
dotnet .\artifacts\publish\<name>\<name>.dll
```

PublishDir is relative to the project (all declared projects are two directories beneath the repository root), avoiding MSBuild's comma parsing in the company OneDrive absolute path. No native apphost, testhost, VSTest, dotnet run, Docker, or security-policy workaround was used.

## Files created or modified during M3

**Tracked progress documents — the entire commit/push scope:**

- PROJECT_REQUIREMENTS.md: preserved specification plus explicit authorized M3 scope.
- AGENTS.md: persistent sample/policy/verification rules.
- IMPLEMENTATION_PLAN.md: actual sample/test layout, real commands, verified M3 criteria/status, M4 next.
- PROGRESS.md: this self-contained handoff and all preserved previous records.

**New local source, lockfiles, and documentation:**

- samples/README.md
- samples/DemoNode/package.json
- samples/DemoNode/server.mjs
- samples/DemoNode/settings.test.mjs
- samples/DemoNode/README.md
- samples/DemoDotnet/DemoDotnet.csproj
- samples/DemoDotnet/Program.cs
- samples/DemoDotnet/SampleSettings.cs
- samples/DemoDotnet/packages.lock.json
- samples/DemoDotnet/README.md
- tests/Samples.Tests/Samples.Tests.csproj
- tests/Samples.Tests/Program.cs
- tests/Samples.Tests/Checks.cs
- tests/Samples.Tests/OwnedProcess.cs
- tests/Samples.Tests/packages.lock.json
- docs/verification/M3-2026-10-26.md

**Modified local implementation support:** InternalHostingPlatform.sln, package.json, tools/publish.mjs, tools/tasks.mjs, README.md.

Generated artifacts remain ignored/local. No application registry, database, Deployment Engine, or later-milestone product code was changed.

## Decisions, corrections, limitations, and unfinished work

- Samples are trusted reviewed demonstrations. They do not execute manifests, uploaded scripts, dependency installs, or source builds.
- Sample release/build labels are explicit bounded operator values. Build=m3 is a demonstration label, not an inferred source commit or reproducible build identifier.
- Base paths are explicitly configured and prefixes are preserved. No application assets, cookies, redirects, WebSockets, database dependencies, or frontend framework compatibility is claimed.
- Test-only free-port probes and owned process supervision are not persistent port allocation or deployment-engine functionality. Bind conflicts/races fail safely.
- No observed M3 build/test failure remains. An initial documentation patch context did not match; it made no changes and was reapplied correctly.
- After the first passing full pipeline, code review found an uncaught URL parse boundary in the Node handler. Added controlled 400 handling plus real socket regressions; the final complete pipeline passed. This was a review finding, not a reproduced pre-fix test failure.
- No confirmed M3 bug or blocker remains. Interactive Ctrl+C was documented but not manually exercised; clean shutdown was verified through the production supervised command/EOF paths.
- Exact Node 24.22.1 from the requirements remains unverified; this machine has Node 26.12.1. ASP.NET Core 8 was verified using installed 8.0.31.
- No corporate-network listener/deployment, production access, service installation, firewall/registry/endpoint-policy change, or existing-application modification occurred.
- M4–M12 remain NOT STARTED. The Prisma native-engine compatibility issue is an assessment gate for M4, not a failed M3 criterion.

## Exact next recommended task and resume commands

At the next authorized M4 session, read all four project records and inspect actual source/artifacts. First evaluate whether the selected Prisma schema/migration/SQLite toolchain can operate under the strict no-.exe policy **before installing or executing any native engine**. Record an unavailable/forbidden dependency as a blocker and continue safe M4 tasks. Do not claim database functionality until verified. This M3 session does not start M4.

```powershell
git status --short --branch
Get-Content -LiteralPath AGENTS.md -Encoding UTF8
Get-Content -LiteralPath PROGRESS.md -Encoding UTF8
Get-Content -LiteralPath IMPLEMENTATION_PLAN.md -Encoding UTF8
npm.cmd run doctor
npm.cmd run test:samples
npm.cmd run verify
```

For interactive sample use, follow samples/README.md. Default direct run commands, in separate terminals from the root:

```powershell
node .\samples\DemoNode\server.mjs
dotnet publish .\samples\DemoDotnet\DemoDotnet.csproj -c Release --no-self-contained -p:UseAppHost=false -p:UseSharedCompilation=false -p:PublishDir=../../artifacts/publish/DemoDotnet/
dotnet .\artifacts\publish\DemoDotnet\DemoDotnet.dll
```

Sample verification requires port 5050 plus two free runtime ports selected from 6411–6999. It never terminates an unrelated port occupant. A fresh clone of the progress repository cannot run these commands without the locally retained implementation.

## Progress repository synchronization

The session commit includes only the four authorized Markdown paths. Inspect staged paths and the resulting commit tree before pushing; compare remote main with local HEAD afterward. The final handoff provides the commit receipt. Source and extra documentation remain local.

```powershell
git diff --cached --name-only
git diff-tree --no-commit-id --name-only -r HEAD
git ls-tree -r --name-only HEAD
git ls-remote origin refs/heads/main
```

## Preserved previous records

The complete previous snapshot, M3 work-in-progress notes, M2/M1 results, failures, and older history are preserved below as historical text. Older milestone/status and next-task statements are superseded by the verified current snapshot above.

```````text
# Project Progress

## Active M3 implementation — 2026-10-26

M3 is now IN PROGRESS under explicit user authorization. Read all four project records, inspected the actual repository, and confirmed there are no existing samples to duplicate. M1/M2 remain completed. The previous M2 handoff below is historical; its M3-not-started status is superseded by this entry.

Design: two trusted minimal HTTP processes, fixed IPv4 loopback binding, validated ports 6100–6999, configurable release/build metadata and optional explicit base paths. Reuse the existing supervised stdin shutdown convention and no-apphost publishing. Add a published managed process verifier; no VSTest, Docker, Administrator privileges, network deployment, or system-security changes.

Source step: created dependency-free DemoNode, ASP.NET Core 8 DemoDotnet, and Samples.Tests. Added safe configuration validation, GET/HEAD health and identity endpoints, supervised graceful shutdown, and test-only owned process supervision. Wired root publishing/testing to publish DLLs with no apphost and reject executable artifacts. Tests are written but have not yet run; M3 remains IN PROGRESS. Next task: publish, run dedicated checks, fix failures, and document actual run commands.

Dedicated verification step: `npm.cmd run test:samples` exited 0: six Node checks and **80 managed sample checks**, zero failures. Actual processes ran concurrently on 127.0.0.1:6411 and 127.0.0.1:6421. Verified real health/root metadata, HEAD/405/404, loopback listeners, custom release/build labels, prefix forwarding through the published Gateway on 127.0.0.1:5050, shutdown-command and stdin-EOF exits 0, and released ports. Unsafe startup settings and occupied-port startup failed safely with exit 1. All three publish roots passed no-.exe scans. Created shared and per-sample run instructions. Strengthened the generic ASP.NET URL override probe to target the sample's actual port; this final adjustment is pending the full regression run. Next task: format, run the complete verification pipeline, then update acceptance evidence and milestone status.

First full regression: `npm.cmd run verify` exited 0 with 213 checks (25 Node + 2 API + 27 Foundation + 79 Gateway + 80 Samples) and successful real platform smoke. Doctor passed on Windows build 22631, Node 26.12.1, npm 11.20.0, SDK 8.0.425/runtime 8.0.31; Administrator=False. Final code review identified an uncaught malformed-URL parse boundary in DemoNode (not an observed test failure). Added controlled HTTP 400 handling and real malformed-target/socket checks requiring subsequent healthy responses in direct and prefix modes. These additions require final regression before M3 completion.

**Updated:** 2026-10-26 (Asia/Bangkok)

## Current milestone and session handoff

- **Last completed milestone:** M2 — YARP Gateway, **COMPLETED** after published-DLL verification.
- **Previously completed:** M1 remains COMPLETED.
- **First unfinished milestone:** M3 — Node.js and ASP.NET Core Sample Deployments, **NOT STARTED**. This session implemented M2 only.
- **Last completed implementation task:** Real YARP gateway, strict startup route-provider boundary, request/error/logging controls, WebSocket forwarding, managed verification, and final full regression.
- **Final gate:** npm.cmd run verify exited 0 with **127 passing checks** plus real platform smoke.
- **Execution policy:** Framework-dependent dotnet publish with UseAppHost=false, followed by dotnet <published DLL>. No native apphost/testhost execution, elevation, Docker, or security-setting changes.
- **Git scope:** Only PROJECT_REQUIREMENTS.md, AGENTS.md, IMPLEMENTATION_PLAN.md, and PROGRESS.md are tracked/authorized for commit and push to origin/main at https://github.com/logtranwanek-spec/IAH-Progress-Tracking-.git. Source/binaries/additional docs stay local.

## M2 acceptance evidence

| Acceptance criterion                                                             | Actual verification                                                                                                                                     |
| -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Gateway publishes/starts on loopback port 5050 without Administrator privileges  | PASS: dedicated managed verification and actual published-process smoke; alternative host/port rejected before binding; Administrator token check=False |
| Gateway health endpoint succeeds locally                                         | PASS: GET JSON and HEAD on /_platform/health; unsupported health methods return structured 405                                                          |
| Gateway is not corporate-facing                                                  | PASS: live managed socket inspection found exactly one listener at 127.0.0.1:5050                                                                       |
| Reserved paths, invalid destinations, proxy errors, prefixes, WebSockets covered | PASS: managed unit and real HTTP/WebSocket cases; details below                                                                                         |
| Application test runtime ports remain loopback                                   | PASS: trusted fixture bound to a free 127.0.0.1:6100–6119 port; socket inspected and fixture stopped                                                    |

The Gateway data lives behind IApprovedGatewayRouteSource and ValidatedProxyConfigProvider (IProxyConfigProvider). Program.cs contains startup/shutdown handling, with no application route definitions. The default config file enables no application routes.

## Features implemented

- Pinned Yarp.ReverseProxy 2.3.0 with its net8.0 assets and versioned NuGet lockfiles.
- Fixed 127.0.0.1:5050 Kestrel host; alternate bind addresses/ports fail before startup. The local launcher and doctor enforce the same runtime rule.
- GET/HEAD /_platform/health response: component=gateway, status=ok, milestone=gateway.
- Reserved /admin and /_platform segments and descendants cannot become application routes; non-health management requests are rejected instead of forwarded.
- Strict startup JSON route source: unknown/missing/duplicate properties, malformed/null/oversized data, duplicate IDs/prefixes, unsafe path forms, and control characters are rejected.
- Destinations are constructed only from literal 127.0.0.1 plus a validated runtime-range port. DNS/other IPs, other schemes, userinfo, destination paths/query/fragment, component ports, and out-of-range ports are rejected.
- Immutable startup YARP route/cluster snapshot with an explicitly non-changing token. No configuration watcher, route-management HTTP API, or registry-backed live updates were added.
- Segment-based prefix matching preserves the incoming path, query, and body; trusted X-Forwarded headers and prefix replace client-supplied values. Competing Forwarded is removed.
- Proxy system-proxy use, automatic redirect following, and proxy-side cookie storage are disabled. Application redirects/cookie paths and application-generated errors pass through.
- Known-length and streaming/chunked body budgets, header and line limits, timeouts, and connection caps.
- Sanitized application/problem+json gateway errors with a matching X-Request-Id/traceId. Failures after committed response headers terminate the stream.
- Timestamped structured JSON request/error logs with method, route ID, status, elapsed time, trace, and controlled error codes. Gateway logs omit request body/query/credential values and raw transport exception messages.
- Actual HTTP/1.1 WebSocket upgrade, subprotocol negotiation, echo, and normal-close forwarding verified through YARP.
- Dedicated managed Gateway verification program; no external/native test runner.

## Final build and test results

| Command/check                    | Actual final result                                                                                                       |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| npm.cmd run verify               | PASS, exit 0; complete publish, static checks, managed/Node checks, platform smoke                                        |
| Node foundation/lifecycle checks | 19 passed, 0 failed                                                                                                       |
| Real API HTTP checks             | 2 passed, 0 failed                                                                                                        |
| Managed Foundation checks        | 27 passed, 0 failed                                                                                                       |
| Managed Gateway checks           | 79 passed, 0 failed                                                                                                       |
| Combined check count             | 127 passed, 0 failed, no skipped checks                                                                                   |
| DLL publishing                   | Gateway, DeploymentAgent, Foundation.Tests, Gateway.Tests all published framework-dependent                               |
| Publish output scan              | All four active publish roots contained no .exe artifacts                                                                 |
| Formatting/type checks           | Prettier, TypeScript, and dotnet format passed                                                                            |
| Doctor                           | PASS on installed Node 26.12.1/npm 11.20.0/.NET SDK 8.0.425, runtime 8.0.31                                               |
| Production log smoke             | PASS: actual published Gateway output parsed as timestamped structured JSON with status and trace                         |
| Live smoke sockets               | PASS: 5050/5065/5173 each bound only to 127.0.0.1                                                                         |
| Smoke lifecycle                  | PASS: real health/page/React module/API responses; all four children exited 0; ports released; owned test storage removed |
| Non-Administrator check          | False for Administrator role                                                                                              |

The Gateway check set exercises default deny, fixed listener guards, reserved/conflicting prefixes, arbitrary destination rejection, strict JSON, immutable source snapshots, explicit request limits, GET/HEAD/405 health behavior, 404/400 errors, path/query/body preservation, forwarding-header spoof resistance, cookie/redirect handling, upstream application errors, controlled 502 and 504 failures, interrupted response streams, known-length/chunked 413, parser 431/414, WebSocket negotiation/echo/close, loopback sockets, structured logs/credential omission, and shutdown.

The known-length oversize check uses Expect: 100-continue and still requires a 413 problem plus zero upstream requests. The chunked check transmits actual oversized data. The partial-response fixture releases its abort only after the client receives 200 headers, then requires a truncated-body transport error.

## Current M2 configuration and defaults

The local launcher uses config/gateway.routes.json. It defaults to:

```json
{ "routes": [] }
```

An approved route document has id, pathPrefix, and destination fields. Prefixes are one canonical lowercase ASCII segment (maximum 63 characters). Destination syntax is http://127.0.0.1:<approved runtime port>/; the effective URI is rebuilt after validation. Files are limited to 64 KiB and 100 routes.

IHP_GATEWAY_ROUTES_PATH may select a trusted operator-reviewed file; standalone published startup otherwise uses its packaged empty file. A changed source requires restart. Route approval/process ownership is an operator responsibility in M2; registry/deployment authorization integration belongs to later milestones.

Implementation defaults, not newly inferred business requirements:

| Budget                                | Current default                                                     |
| ------------------------------------- | ------------------------------------------------------------------- |
| Body                                  | 1 MiB; IHP_GATEWAY_MAX_REQUEST_BYTES permits 1 byte–64 MiB          |
| Total headers                         | 16 KiB; IHP_GATEWAY_MAX_HEADER_BYTES permits 1–64 KiB               |
| Request line                          | 8 KiB                                                               |
| Request headers / keep-alive timeouts | 15 / 60 seconds                                                     |
| Proxy activity timeout                | 30 seconds; IHP_GATEWAY_PROXY_TIMEOUT_SECONDS permits 1–120 seconds |
| HTTP / upgraded connections           | 128 / 64                                                            |

Tests use a 2-second proxy activity timeout for deterministic stalled-upstream verification. Production retains the 30-second default.

## Files created or modified during M2

**Tracked progress documents (the only commit/push scope):** PROJECT_REQUIREMENTS.md, AGENTS.md, IMPLEMENTATION_PLAN.md, PROGRESS.md.

**New local implementation/evidence files:**

- config/gateway.routes.json
- src/Gateway/GatewayHost.cs
- src/Gateway/GatewayLimits.cs
- src/Gateway/GatewayProblem.cs
- src/Gateway/Routing/GatewayRoutes.cs
- src/Gateway/Routing/ValidatedProxyConfigProvider.cs
- src/Gateway/packages.lock.json
- tests/Gateway.Tests/Gateway.Tests.csproj
- tests/Gateway.Tests/Program.cs
- tests/Gateway.Tests/Verification.cs
- tests/Gateway.Tests/UnitChecks.cs
- tests/Gateway.Tests/Fixtures.cs
- tests/Gateway.Tests/IntegrationChecks.cs
- tests/Gateway.Tests/packages.lock.json
- GATEWAY.md
- docs/verification/M2-2026-10-26.md

**Modified local files:** src/Gateway/Program.cs and Gateway.csproj; InternalHostingPlatform.sln; tools/publish.mjs, tasks.mjs, local.mjs, doctor.mjs, smoke.mjs; package.json; tests/foundation/lifecycle.test.mjs; packages/contracts/index.d.ts; README.md, ARCHITECTURE.md, INSTALLATION.md, SECURITY.md, TROUBLESHOOTING.md.

The Gateway health milestone changed from foundation to gateway; shared TypeScript contracts and smoke expectations were updated accordingly. M1 Foundation checks continue to pass.

Source and extra evidence documents remain local/untracked in the four-document progress repository. A fresh clone contains project records, not runnable implementation; preserve this workspace for continuity.

## Failures encountered and corrections

1. Initial publish failed on the .NET 8 WriteAsJsonAsync overload; supplied explicit serializer options and republished successfully.
2. First Gateway run: 74 passes/2 failures. Replaced a callback-advertising cancellation wrapper with NullChangeToken, and replaced a Windows timing-dependent closed-port expectation with a controlled upstream disconnection for 502 while keeping the separate stalled-upstream 504 test.
3. Added a post-header failure case. Immediate upstream abort could occur before Gateway headers were committed; synchronized the fixture with client receipt of headers so the intended incomplete-stream boundary is now verified.
4. An intermediate full pipeline passed 126 checks. Final validation review then tightened full-string regex anchors and added a control-character regression.
5. Next full run passed 78/79 Gateway checks; early known-length rejection raced the client's upload writer before the response/upstream-count assertions. Added Expect: 100-continue without removing those assertions. Streaming enforcement still uses actual chunked data.
6. Final full pipeline passed all 127 checks and platform smoke. No failure was skipped or represented as complete before its correction.

## Known bugs, blockers, and practical limits

**No confirmed M2 bug or blocker remains.**

- Startup routes are trusted local configuration, default empty. No live registry routes or route update API exist; that is M6.
- Prefixes are preserved. Applications still need explicit base-path compatibility; the Gateway does not guess at asset, redirect, cookie, or response-body rewrites.
- Kestrel parser errors such as oversized headers/lines can use native response bodies before the application pipeline. Gateway-generated pre-header proxy problems are structured JSON; post-header/upgrade failures close the stream instead of replacing committed headers.
- An oversized upload already in flight can be observed by a client as a reset; Expect: 100-continue permits clean early rejection for a known Content-Length.
- HTTP/2/SPDY, per-message WebSocket resource budgets, broad load/security hardening, log retention/search, and interactive browser review were not verified in M2.
- Exact Node 24 and older runtime patches from the specification remain unverified; testing used the installed versions above.
- Authentication, credential storage, upload/build approval, process isolation, registry/database, and unattended startup remain later milestone/IT design work.

No existing application was modified or deployed. No corporate-network access/listener was enabled and no security setting was changed.

## Exact next recommended task

M3 is the first unfinished milestone: build the trusted Node.js and ASP.NET Core sample applications under the existing loopback/published-DLL rules, then verify their documented health and prefix behavior. **Do not start it as part of this M2-only session.**

## Commands to resume

Read AGENTS.md, this current snapshot, M3 in IMPLEMENTATION_PLAN.md, relevant requirements, and inspect actual source first.

```powershell
git status --short
npm.cmd run doctor
npm.cmd run verify
npm.cmd run test:gateway
npm.cmd run start:local
```

The dedicated Gateway command publishes and runs the managed DLL. Equivalent manual verification:

```powershell
dotnet publish .\tests\Gateway.Tests\Gateway.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false -p:UseSharedCompilation=false -p:PublishDir=../../artifacts/publish/Gateway.Tests/
dotnet .\artifacts\publish\Gateway.Tests\Gateway.Tests.dll
```

Do not run native apphosts, testhost, dotnet run, or VSTest. The Gateway verification fixtures need port 5050 and one free port in 6100–6119; existing occupants are never terminated.

## Progress repository synchronization

This snapshot is limited to the four authorized Markdown paths. Before committing, inspect the staged list and commit tree; after pushing origin/main, compare its remote hash to local HEAD. Never include source, binaries, local config/evidence documents, or credentials. The final handoff provides the commit receipt.

## Preserved previous records

The previous M1-complete snapshot, M2 implementation timeline, intermediate failures, and older history are retained below. Historical statuses are superseded by the current verified snapshot above.

<details>
<summary>Previous snapshots and M2 implementation timeline</summary>

``````markdown
# Project Progress

**Updated:** 2026-10-26 (Asia/Bangkok)

## Active M2 session

- User authorized M2 only. Read the repository instructions, current progress, M2 plan, and the relevant gateway/security requirements; inspected the actual source and Git state before coding.
- M1 remains verified. M2 is now IN PROGRESS; M3 and later milestones will not be started.
- Design: actual YARP with a validated route-source/config-provider boundary; fixed 127.0.0.1:5050; no arbitrary proxy destinations; trusted startup-only route configuration (runtime registry updates remain M6).
- Verification will be a separately published managed Gateway program exercising real HTTP/WebSocket traffic against trusted loopback-only test fixtures. No native apphost, testhost, deployment package, or production service is involved.
- Git tracks only the four progress documents; all source stays local and only those four documents will be committed/pushed after verification.
- Added the first YARP implementation: strict startup-only route source, reserved-prefix/destination validation, immutable IProxyConfigProvider snapshot, fixed listener, request budgets, problem responses, forwarded headers, and structured request/error logging. Default routes remain empty.
- First publish found a .NET 8 WriteAsJsonAsync overload mismatch; corrected the explicit serializer-options argument. Added per-route body limits as well as Kestrel/global Content-Length checks so streaming requests retain a budget. Publish and behavior verification are still pending.
- Gateway DLL publishing now succeeds with YARP 2.3.0 targeting net8.0. Added a dedicated published Gateway verification project and integrated its publish/run task into local tooling.
- First managed Gateway run: 74 checks passed, 2 failed. Changed the startup snapshot to an explicitly non-changing token. The closed-port test reached the 2-second timeout before Windows reported refusal; replaced that timing-dependent test with a controlled upstream disconnection for 502, retaining the independent stalled-upstream 504 test. No failed check was marked complete or skipped.
- Existing successful checks already covered reserved routes, destination validation, prefix preservation, forwarded headers, cookie/redirect behavior, fixed/chunked body limits, header/line limits, WebSocket echo/close, loopback sockets, and credential/query omission from structured logs. Fresh corrected verification is pending.
- Corrected Gateway verification passed: 77 checks, 0 failures. Added one final case for failure after response headers are committed: such a stream must terminate rather than masquerade as a complete response; a problem body cannot safely replace already-sent headers.
- The additional partial-response probe initially aborted upstream before the gateway necessarily committed headers, so a safe complete 502 could race the intended truncation assertion. Added fixture/client synchronization: only abort after the client has received 200 response headers, then require an incomplete-body transport error. This verifies the actual post-header failure boundary without weakening proxy error checks.
- Corrected final Gateway run passed: 78 checks, 0 failed. Added UTC timestamps to the production JSON console logger and an actual published-process log check to the platform smoke procedure. Full publish/check/regression/smoke verification remains the final gate.
- Full npm.cmd run verify passed: 21 Node/API checks, 27 managed Foundation checks, 78 managed Gateway checks (126 total), four DLL publish artifact scans, static/format checks, and real platform smoke including timestamped JSON logs. All fixture and platform processes stopped cleanly; test storage was removed.
- Final tooling review made the log probe parse only complete newline-delimited records (stdout chunks may split JSON) and aligned doctor with the fixed Gateway port. Gateway production behavior is unchanged; final targeted tooling/smoke checks are pending.
- Final route validation review tightened regular-expression end anchors to reject trailing control characters in identifiers, prefixes, and destination input. Added a regression check covering all three fields. A fresh final verification is required for this validation change before M2 is marked complete.
- The next full run passed builds/static checks, Node/Foundation checks, and 78 of 79 Gateway checks; the known-length oversize probe hit a client-side in-flight write/reset race before it could consume the expected rejection or reach its upstream-count assertion. Changed that probe to the HTTP Expect: 100-continue handshake while retaining strict 413/problem-body and untouched-upstream assertions. The chunked-body test still transmits actual oversized data and must receive 413.
- Final npm.cmd run verify passed with the hardened validator and corrected upload probe: 79 Gateway checks, 27 Foundation checks, 21 Node/API checks (127 total), all four managed publishes with no .exe artifacts, static/format checks, production JSON log inspection, live loopback sockets, and clean four-process smoke shutdown. M2 acceptance criteria can now be marked COMPLETED; M3 and later remain NOT STARTED.

## Current milestone and last completed task

- **Last completed milestone:** M1 — Project Foundation, **COMPLETED** after verification using published managed DLLs.
- **First unfinished / next milestone:** M2 — YARP Gateway, **NOT STARTED**. No M2 feature code was added in this session.
- **Last completed task:** Policy reassessment, replacement managed Foundation verification, published-DLL startup, full verification, and a negative control for failure propagation.
- **Authorization:** The user expressly requires no .exe artifacts to run and permits .NET through dotnet publish followed by dotnet <published DLL>. The conditional instruction to start M2 if M1 remained unverified was unnecessary because M1 passed.
- **Progress repository:** https://github.com/logtranwanek-spec/IAH-Progress-Tracking-.git, branch main. Its tracked tree contains only the four project Markdown files. Source, publish output, and extra evidence documents stay local.

## M1 acceptance reassessment

The original functional criteria were retained. Requirements mandate automated verification, not VSTest specifically. Company policy changes the permitted execution format; it does not waive behavior checks. The dedicated managed program references the actual production Foundation library, reports every assertion, and returns nonzero on failure. It does not load or rehost VSTest or testhost.

| Acceptance criterion                                                     | Evidence and result                                                                                                                                            |
| ------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Agreed layout and component boundaries are documented and present        | PASS: four component shells, shared Foundation/config/contracts, lifecycle tools, and architecture/setup/security documentation inspected                      |
| Local restore/compile/publish without Docker or Administrator privileges | PASS: API/dashboard compilation and Gateway, Agent, verifier DLL publishes; Administrator-role check returned False                                            |
| Configurable storage/ports with loopback defaults                        | PASS: 27 managed Foundation checks, 17 Node foundation checks, and three managed live socket assertions                                                        |
| Documented local start/stop and verification work on Windows             | PASS: full verify exited 0; actual HTTP responses, React module, API proxy, disabled deployment, graceful exit code 0, port release, temporary storage cleanup |
| Baseline checks/results recorded accurately                              | PASS: current results here; prior failures and unexecuted VSTest history retained below                                                                        |

**Conclusion:** M1 is COMPLETED. The prior error 1260 is historical and was not overridden or disabled. The old VSTest/xUnit suite never executed; the passing results belong to the new managed verification program.

## Implemented foundation

- NestJS health-only API; no management or deployment endpoints.
- ASP.NET Core gateway shell health endpoint; YARP routing remains M2 work.
- React/TypeScript/Vite/Tailwind foundation page with a real API health check through its fixed local proxy.
- Foreground .NET Worker with supervised shutdown and no upload/application execution.
- Shared configurable loopback ports, profile storage, runtime range, source/storage separation, port collision and restricted-port validation.
- Foreground launcher with occupied-port checks, bounded readiness, graceful child shutdown, and owned-process cleanup.
- Framework-dependent .NET publishing with UseAppHost=false; active publish outputs are scanned for .exe artifacts before execution.
- Managed Foundation assertions and TCP listener inspection without VSTest or a direct PowerShell executable launch.

M1 does not implement application registration, database/Prisma integration, YARP proxying, upload/build execution, authentication, monitoring, or rollback.

## Files changed in this policy reassessment

**Tracked documents, and the only files authorized for commit/push:**

- PROJECT_REQUIREMENTS.md — added the authoritative no-.exe/publish-DLL policy while retaining the complete original specification.
- AGENTS.md — preserved the user's new progress-repo instructions, fixed its code fence, and documented managed DLL verification and the four-file commit restriction.
- IMPLEMENTATION_PLAN.md — completed M1 with an evidence matrix and updated future .NET command examples to publish/DLL execution.
- PROGRESS.md — this current state, detailed results, limitations, source inventory, and preserved history.

**Local implementation changes (excluded from the progress-repository commit):**

- Added tests/Foundation.Tests/Program.cs — managed assertion entrypoint, failure exit code, and managed listener inspection.
- Added tools/publish.mjs — fixed project allowlist, framework-dependent publishes, published DLL/runtime metadata checks, and recursive rejection of .exe artifacts.
- Modified Directory.Build.props — disables native apphosts, self-contained/single-file output, and shared compilation.
- Modified tests/Foundation.Tests/Foundation.Tests.csproj, SettingsTests.cs, and packages.lock.json — removed all VSTest/xUnit runner packages; verification now directly tests production settings without an external test host.
- Modified tools/tasks.mjs — build publishes DLLs; test:dotnet publishes/runs the dedicated verifier.
- Modified tools/local.mjs — launches the published Gateway and Agent DLLs, validates allowed component hosts, and retains owned-child cleanup.
- Modified tools/process.mjs — rejects direct .exe command paths; uses the required existing command hosts for project tooling.
- Modified tools/smoke.mjs — uses the published managed listener check instead of directly starting a PowerShell executable.
- Modified .gitignore and .prettierignore — exclude artifacts.
- Updated local README.md, ARCHITECTURE.md, INSTALLATION.md, SECURITY.md, TROUBLESHOOTING.md, and docs/verification/M1-2026-10-26.md for the policy and new evidence.

Source changes are still local/untracked in this documentation-only Git repository. A fresh clone of the progress repository provides project records, not the runnable source. Preserve this workspace for development continuity.

## Commands executed and actual results

| Command/check                                                      | Actual result                                                                                                                                                  |
| ------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Initial managed publish using an absolute output property          | FAILED: MSB1006 due to comma in the OneDrive organization path; corrected using a project-relative PublishDir property                                         |
| npm.cmd run test:dotnet                                            | PASS: published Foundation.Tests.dll ran 27 checks, 0 failures, exit 0                                                                                         |
| npm.cmd run verify                                                 | PASS, exit 0: compilation, DLL publishes, formatting/types, Node/API checks, managed assertions, live smoke                                                    |
| Node foundation/lifecycle checks in full verify                    | PASS: 17                                                                                                                                                       |
| API real HTTP checks in full verify                                | PASS: 2                                                                                                                                                        |
| Published managed Foundation checks in full verify                 | PASS: 27                                                                                                                                                       |
| Publish artifact validation                                        | PASS: Gateway, DeploymentAgent, Foundation.Tests outputs contained no .exe files                                                                               |
| Managed live listener check                                        | PASS: exactly one IPv4 loopback listener on each configured port 5050, 5065, 5173                                                                              |
| Full smoke lifecycle                                               | PASS: real health bodies, HTML/React module, local API proxy, deployment endpoint 404; all four child processes exited 0; ports released; test storage removed |
| Negative control: verifier requested a listener after API shutdown | PASS as a negative control: expected assertion message `Expected 1; got 0.` and exit code 1 were both observed                                                 |
| Current Windows token check                                        | Administrator role = False                                                                                                                                     |

The passing automated behavior count is **46 checks: 19 Node/API + 27 managed Foundation**, plus the live socket and smoke checks. None are historical VSTest results. No interactive browser visual/accessibility review or exact Node 24 compatibility test was performed. The NPM dependency tree is unchanged in this session; the prior baseline audit had zero vulnerabilities.

No apphost/testhost executable was launched, no blocked runner was relocated/rehosted, and no company security setting was changed. Stale historical build output is not an approved launch target; startup only accepts validated publish artifacts.

## Architectural decisions and policy

- .NET target remains net8.0 on the installed SDK 8.0.425/runtime 8.0.31; Node 26.12.1/npm 11.20.0 remain the tested installed versions.
- All .NET applications/checks use framework-dependent publish and dotnet <published DLL>. A managed console entrypoint may have OutputType=Exe in MSBuild while UseAppHost=false ensures that the published runnable artifact is a DLL.
- Active output directories: artifacts/publish/Gateway, artifacts/publish/DeploymentAgent, artifacts/publish/Foundation.Tests.
- Use project-relative PublishDir properties because an absolute --output value encounters MSBuild comma parsing in this repository path.
- Gateway/API/dashboard bind only to 127.0.0.1 on 5050/5065/5173. The future managed runtime range remains 6100–6999. The Agent exposes no management HTTP surface.
- Default storage remains under the current user's profile and separate from source; no username is hardcoded.
- Separate Windows processes are not strong isolation. Upload/build execution remains disabled until the approved identity, security boundary, authorization, and deployment policy are implemented.
- The permitted DLL publishing approach follows [Microsoft's framework-dependent publishing guidance](https://learn.microsoft.com/en-us/dotnet/core/deploying/).

## Current blockers and unresolved issues

**No blocker remains for M1.**

- Exact Node 24.22.1 and older .NET patches from the original infrastructure snapshot remain unavailable/unverified.
- Prisma/SQLite and YARP package integration are future milestones. Verify Prisma's compliance with the no-.exe policy before selecting/running its tooling in M4.
- Execution identities, credential protection, trusted package approvals, and unattended startup still need their planned design/IT review; they are not silently treated as implemented.
- Source remains local because the progress repository is explicitly limited to four Markdown files.

## Exact next recommended task

M2 is the first unfinished milestone. Begin its architecture/dependency review using the no-.exe constraints, then implement the managed YARP gateway and its permitted DLL-based verification. This session completed M1 and did not start M2.

## Commands to resume

Read AGENTS.md, this current snapshot, M2 in IMPLEMENTATION_PLAN.md, the relevant requirements, and inspect actual local source first.

```powershell
git status --short
npm.cmd run doctor
npm.cmd run verify
npm.cmd run start:local
```

The existing test:dotnet task always publishes the managed verifier before invoking its DLL. The equivalent permitted form is:

```powershell
dotnet publish .\tests\Foundation.Tests\Foundation.Tests.csproj -c Release --no-self-contained -p:UseAppHost=false -p:UseSharedCompilation=false -p:PublishDir=../../artifacts/publish/Foundation.Tests/
dotnet .\artifacts\publish\Foundation.Tests\Foundation.Tests.dll
```

Do not run dotnet test, dotnet run, native apphosts, or a testhost executable. No IT policy exception is required for the now-verified M1 format.

## Progress repository synchronization

Commit and push only PROJECT_REQUIREMENTS.md, AGENTS.md, IMPLEMENTATION_PLAN.md, and PROGRESS.md to origin/main at the specified progress repository. Verify the staged path list and the committed tree contain exactly those four paths. Confirm the pushed remote commit matches local HEAD. Do not add source, binaries, credentials, or local evidence documents. The final handoff provides the commit receipt.

## Session handoff

M1 acceptance is verified and marked COMPLETED. All verification component processes stopped, their ports were released, and temporary test storage was removed. No corporate-network deployment, production access, existing-application modification, or system security-setting change occurred. M2–M12 remain NOT STARTED.

## Preserved previous records

The preceding blocked snapshot, incremental reassessment timeline, and earlier session history are retained verbatim below. They describe historical states superseded by this verified current snapshot.

<details>
<summary>Previous blocked snapshot and policy reassessment history</summary>

`````markdown
# Project Progress

**Updated:** 2026-10-26 (Asia/Bangkok)

## Active policy reassessment session

- The user confirmed the no-`.exe` policy and expressly required `dotnet publish` followed by `dotnet <published DLL>`. M1 is being reassessed; the prior blocked snapshot below is historical until fresh evidence is recorded.
- Read AGENTS.md, PROGRESS.md, IMPLEMENTATION_PLAN.md, and the relevant infrastructure, security, testing, and acceptance requirements before changes.
- The remote now exists on `origin/main` with initial commit `026588e`; it tracks the four documentation files. The user-added AGENTS.md instructions were retained, and its missing code-fence terminator was corrected.
- Plan: replace the VSTest dependency with a dedicated managed Foundation verification program, publish without apphosts, use published DLLs for Gateway/Agent startup, and replace the smoke check's direct PowerShell executable invocation with a managed socket check.
- No blocked runner was executed. M1 is not yet claimed complete in this reassessment.
- Converted Foundation.Tests into a standalone managed verification program with production settings assertions and a managed TCP listener check; removed VSTest/xUnit runner package dependencies. Apphosts and shared compilation are disabled globally, and application startup now targets publish artifacts.
- First publish failed with MSB1006 because the absolute output path contains a comma in the OneDrive organization name. Updated publishing to use a project-relative PublishDir property; verification is pending. No executable artifact was launched by the failed publish.
- Corrected publish succeeded. The published Foundation.Tests.dll executed 27 production settings checks: 27 passed, 0 failed, exit code 0. The output scan found no .exe artifacts. This is fresh managed verification evidence, not a claim that the historical VSTest suite ran.
- Updated local startup, smoke inspection, and setup/security documentation for published DLLs. Full publish/check/Node-test/smoke verification remains pending before changing M1 status.
- Full `npm.cmd run verify` passed (exit 0): API/dashboard compilation, three framework-dependent DLL publishes, no-.exe output scans, formatting/type checks, 19 Node/API checks, 27 managed Foundation checks, and the four-component smoke check including managed loopback socket inspection and clean shutdown/port release.
- The current Windows token is not an Administrator (`False` role check). The historical testhost restriction was not changed; the replacement program verified the required Foundation behavior through the newly confirmed permitted DLL execution format.
- Negative control: requesting a listener on the now-released API port produced the expected managed assertion failure and exit code 1. A separate recursive scan of artifacts/publish found no .exe files. M1 acceptance criteria are now verified; IMPLEMENTATION_PLAN.md marks M1 COMPLETED. M2 remains NOT STARTED because the conditional fallback to M2 was unnecessary.

## Current milestone and last completed task

- **Current milestone:** M1 — Project Foundation, `BLOCKED` on company policy preventing .NET test execution.
- **Last completed task:** Fresh npm ci, static checks, 19 Node/API tests, and dependency audit passed. The independent four-component smoke check also passed.
- **Next milestone:** M2 — YARP Gateway, `NOT STARTED`. Do not start it while M1 acceptance remains incomplete.
- **Authorization:** The user approved local M1 implementation on 2026-10-26. No repeated approval is needed for M1. Corporate-network deployment and production access remain unauthorized.
- **Evidence:** [M1 verification report](docs/verification/M1-2026-10-26.md).

## Completed foundation functionality

- NestJS API shell with a real health endpoint and no management/deployment endpoints.
- ASP.NET Core 8 gateway shell with a real health endpoint. YARP routing remains M2 work.
- React/TypeScript/Vite/Tailwind foundation page checking the API through the local development proxy.
- Foreground .NET Worker shell with supervised graceful shutdown and no HTTP listener or application execution.
- Shared configuration/contracts; configurable user-profile storage, ports, and future runtime range. Loopback-only binds, source/storage separation, distinct port ranges, and restricted HTTP ports are validated.
- Foreground local component launcher with occupied-port checks, bounded readiness/shutdown, owned-child cleanup, and no automatic restart.
- Architecture, setup, security limitations, troubleshooting, build/check/test commands, and reproducible dependency lockfiles.

These are M1 foundations. Application registration, registry/database, YARP proxying, uploads/builds, application process management, authentication, monitoring, and rollback are not implemented.

## Files created or modified

Modified existing session documents: `AGENTS.md`, `IMPLEMENTATION_PLAN.md`, and `PROGRESS.md`.

`PROJECT_REQUIREMENTS.md` was read and retained unchanged (40,210 bytes). A local Git repository was initialized on main; no remote, commit, or staging was performed. Build outputs and node_modules are ignored.

<details>
<summary>Files created during M1</summary>

- `.editorconfig`
- `.gitattributes`
- `.gitignore`
- `.prettierignore`
- `.prettierrc.json`
- `ARCHITECTURE.md`
- `Directory.Build.props`
- `INSTALLATION.md`
- `InternalHostingPlatform.sln`
- `README.md`
- `SECURITY.md`
- `TROUBLESHOOTING.md`
- `config/platform.local.json`
- `docs/verification/M1-2026-10-26.md`
- `global.json`
- `package-lock.json`
- `package.json`
- `packages/config/index.d.ts`
- `packages/config/index.mjs`
- `packages/config/package.json`
- `packages/contracts/index.d.ts`
- `packages/contracts/package.json`
- `src/Api/package.json`
- `src/Api/src/app.ts`
- `src/Api/src/main.ts`
- `src/Api/test/api.test.mjs`
- `src/Api/tsconfig.json`
- `src/Dashboard/index.html`
- `src/Dashboard/package.json`
- `src/Dashboard/src/main.tsx`
- `src/Dashboard/src/style.css`
- `src/Dashboard/tsconfig.json`
- `src/Dashboard/vite.config.ts`
- `src/DeploymentAgent/DeploymentAgent.csproj`
- `src/DeploymentAgent/Program.cs`
- `src/Foundation/ConsoleShutdown.cs`
- `src/Foundation/Foundation.csproj`
- `src/Foundation/PlatformSettings.cs`
- `src/Gateway/Gateway.csproj`
- `src/Gateway/Program.cs`
- `tests/Foundation.Tests/Foundation.Tests.csproj`
- `tests/Foundation.Tests/SettingsTests.cs`
- `tests/Foundation.Tests/packages.lock.json`
- `tests/foundation/config.test.mjs`
- `tests/foundation/lifecycle.test.mjs`
- `tools/dashboard.mjs`
- `tools/doctor.mjs`
- `tools/local.mjs`
- `tools/process.mjs`
- `tools/smoke.mjs`
- `tools/tasks.mjs`

</details>

## Tests executed and actual results

| Check                                      | Result                                                                                                                                                                     |
| ------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| npm ci                                     | PASS; pinned dependencies restored, zero audit vulnerabilities                                                                                                             |
| npm run doctor (using npm.cmd)             | PASS; installed versions and local settings reported                                                                                                                       |
| API, dashboard, and .NET builds            | PASS; .NET: 0 warnings, 0 errors                                                                                                                                           |
| Prettier, TypeScript, dotnet format checks | PASS                                                                                                                                                                       |
| Node configuration/lifecycle tests         | PASS: 17; includes restricted-port and occupied-listener regression checks                                                                                                 |
| API real HTTP tests                        | PASS: 2; health and unavailable deployment APIs                                                                                                                            |
| .NET tests                                 | NOT EXECUTED: VSTest aborted when Windows group policy blocked testhost.exe (Win32 error 1260)                                                                             |
| Full npm run verify                        | FAILED at the .NET test gate; cannot be described as passing                                                                                                               |
| Independent local smoke                    | PASS: four processes, health bodies, React module, fixed API proxy, disabled deployment, actual loopback sockets, graceful exit code 0, port release, test storage cleanup |
| dotnet restore --locked-mode               | PASS                                                                                                                                                                       |
| npm audit --audit-level=low                | PASS: 0 vulnerabilities                                                                                                                                                    |

Commands are detailed in the linked verification report. No interactive browser visual/accessibility test was run; exact Node 24/older runtime patch compatibility is unverified because those versions are unavailable here.

## Known bugs and resolved failures

No additional confirmed functional bug remains after the safe checks. The .NET configuration behavior is not fully unit-verified because that suite did not run.

Resolved during M1: npm tool shims failing on the ampersand path; vulnerable Multer transitive pin; generated-lockfile formatting noise; API port 5060 rejected by Fetch. The active API default is **5065**, and the corrected smoke passed. Earlier failures and their actual results are preserved in the session history below.

## Current blockers

**IT dependency:** Windows group policy blocks VSTest's `tests/Foundation.Tests/bin/Debug/net8.0/testhost.exe` with Win32 error 1260. The .NET suite aborted before any test assertions executed. IT must provide an approved test execution arrangement. No policy changes, elevation, runner relocation, rehosting, or other workaround was attempted.

This prevents M1 completion. Builds, Node checks, and the independently configured local application hosts ran under an ordinary non-Administrator token.

## Unresolved technical issues

- Node 24.22.1 and the older .NET patches in the original infrastructure snapshot are not installed; only the observed toolchain was verified.
- The execution identity/isolation/approval boundary for uploaded applications and builds remains undecided. No packages are accepted or executed in M1.
- Credential protection and database connectivity require design and verification in later milestones; no credentials or database were initialized.
- Unattended startup after logout/reboot requires an IT-approved mechanism if not possible within ordinary account permissions.
- Prisma/SQLite and YARP dependency/runtime integration are future milestone work and have not been claimed as verified.

## Architectural decisions

- Source layout: src/Gateway, src/Api, src/DeploymentAgent, src/Dashboard; shared .NET host support in src/Foundation; Node configuration and contracts in packages; local tooling in tools.
- Keep the specified architecture and .NET 8 target. Use the installed runtimes as ordinary foreground processes; no Docker or Windows Service.
- Keep all HTTP components on **127.0.0.1**: gateway **5050**, API **5065**, dashboard **5173**. The Agent has no HTTP listener. The configured **6100–6999** range is reserved for future managed applications.
- Storage defaults beneath the current user's profile, separate from source; the launcher creates platform-owned directories and the smoke uses a validated unique temporary directory.
- The Worker uses the installed ASP.NET Core shared framework for Generic Host APIs. Normal hosts run through the installed dotnet runtime; the blocked VSTest executable is a separate unresolved test dependency.
- Pin npm and NuGet dependencies. NestJS's Multer dependency has a scoped 2.5.0 override with a zero-vulnerability audit.
- Separate Windows processes are not strong security isolation; no untrusted code execution is enabled.
- Do not modify existing applications, expose runtime ports, access production systems, or change IT/security settings.

## Exact next recommended task

Obtain an IT-approved arrangement for running the standard VSTest testhost under the company policy. Then run the existing .NET suite and full verification without changing security settings. Fix any actual failures, record their results, and mark M1 complete only when every acceptance criterion is verified. M2 remains queued until that gate passes.

## Commands to resume development

Read AGENTS.md, this file, M1 in IMPLEMENTATION_PLAN.md, relevant PROJECT_REQUIREMENTS.md sections, and inspect the actual repository first.

Safe existing checks:

```powershell
git status --short
npm.cmd ci
npm.cmd run doctor
npm.cmd run build
npm.cmd run check
npm.cmd run test:node
npm.cmd run smoke
```

After IT resolves the test execution dependency:

```powershell
npm.cmd run test:dotnet
npm.cmd run verify
```

Do not repeatedly run the blocked test command while the external policy state is unchanged. `npm.cmd run start:local` starts the existing foundation; Ctrl+C stops its owned processes. See README.md.

## Session handoff

All local platform processes started for verification have stopped. The platform ports were released. No corporate-network deployment, production access, existing-application modification, uploaded-code execution, or system security-setting change occurred. M1 remains `BLOCKED`; no later milestone was started.

## Preserved session history

The original documentation baseline and incremental implementation log are retained below. Their historical state statements are superseded by the current snapshot above.

<details>
<summary>Documentation baseline and M1 implementation history</summary>

````markdown
# Project Progress

**Session date:** 2026-10-26 (Asia/Bangkok)

## Current state

- **Current milestone:** M1 — Project Foundation (`BLOCKED` on .NET test execution under company policy)
- **Last completed task:** Read the complete project specification and inspected the repository; established the documentation baseline in this session. No application implementation task has been completed.
- **Completed application features:** None.
- **Documentation status:** `PROJECT_REQUIREMENTS.md` already contained the full 24-section specification and was preserved. `IMPLEMENTATION_PLAN.md`, `PROGRESS.md`, and `AGENTS.md` were created for review.

## Files created or modified this session

- Created `IMPLEMENTATION_PLAN.md`.
- Created `PROGRESS.md`.
- Created `AGENTS.md`.
- Read, but did not modify, `PROJECT_REQUIREMENTS.md`.

## Verification and tests

- Read all 1,842 lines of `PROJECT_REQUIREMENTS.md`, including sections 1–24.
- Enumerated repository files. The only project file found was `PROJECT_REQUIREMENTS.md`; there is no application scaffold or test suite yet.
- `git status --short --branch` returned `fatal: not a git repository`; this folder is not currently a Git worktree, so tracked-change state/history could not be checked.
- A structural review found 12 milestone sections, each with an objective, tasks, dependencies, acceptance criteria, verification commands, and status; all 12 statuses are `NOT STARTED`.
- **Application tests:** Not run; no application code or test projects exist. No test has been represented as passing.
- **Documentation consistency review:** Planned statuses are all `NOT STARTED`; the roadmap's technology, ports, permissions, upload restrictions, and security gates follow `PROJECT_REQUIREMENTS.md`.

## Known bugs

- None observed. There is no application code to inspect yet.

## Unresolved technical issues and decisions

- Confirm the proposed source folder/solution layout and verification scripts during M1; no layout or project files exist yet.
- Define the approved security boundary and deployment approval process for uploaded builds and application processes. The specification prohibits unrestricted execution and states that separate Windows processes are not strong isolation.
- Confirm the allowed local credential protection method and designated execution identity before storing database credentials or application secrets.
- Confirm whether any unattended startup mechanism is available through IT; do not assume startup after logout/reboot.
- Future network listening on `10.141.79.182:5050` requires local verification and explicit authorization. No network listener was enabled in this session.

## Current blockers

- **Documentation approval gate resolved:** On 2026-10-26 the user authorized implementation of M1. Corporate-network deployment remains unauthorized.
- No other blocker to documentation work was encountered. Runtime/toolchain availability has not yet been checked.

## Architectural decisions recorded

- `PROJECT_REQUIREMENTS.md` is the canonical source for project scope and constraints; its existing complete specification remains unchanged.
- The preferred stack remains the specified React + TypeScript + Vite frontend, NestJS API, SQLite + Prisma registry, ASP.NET Core 8 + YARP gateway, and .NET 8 Deployment Agent.
- Initial development and verification are localhost-only at `127.0.0.1:5050`; managed application processes bind only to loopback, with the planned internal range `6100–6999`.
- Docker, Administrator privileges for normal operation, changes to IT/security controls, and modifications to existing applications are out of scope.
- Proposed source layout is documented in `IMPLEMENTATION_PLAN.md` and remains subject to confirmation during M1 because the repository currently has no scaffold.
- Initial baseline: all implementation milestones were `NOT STARTED`; no acceptance criterion was claimed as verified. M1 is now `IN PROGRESS` following user authorization.

## Exact next recommended task

Continue M1: create the component shells, shared configuration, local lifecycle tooling, and foundation tests; build and verify them before completing M1.

## Commands to resume

Run these from the project root at the beginning of the next session:

```powershell
Get-Content -LiteralPath .\AGENTS.md -Raw
Get-Content -LiteralPath .\PROGRESS.md -Raw
Get-Content -LiteralPath .\IMPLEMENTATION_PLAN.md -Raw
Get-Content -LiteralPath .\PROJECT_REQUIREMENTS.md -Raw
rg --files -g '!node_modules' -g '!bin' -g '!obj'
```

After the documentation approval gate is satisfied, the initial M1 environment checks are:

```powershell
dotnet --info
npm.cmd --version
Get-ChildItem -Force
```

Record actual outputs, failures, and test results before updating milestone status. Refer to the milestone verification commands in `IMPLEMENTATION_PLAN.md` after the relevant projects exist.

## Implementation session — 2026-10-26

- User authorized M1 implementation; documentation approval is no longer a blocker.
- Re-read the four project documents and confirmed the repository still contains only those documents. No existing application code is present to duplicate.
- Tool inspection: Windows build 22631 x64; PowerShell 5.1; Node v26.12.1; npm 11.20.0; .NET SDK 8.0.425; ASP.NET Core/.NET runtimes 8.0.31. These differ from the original infrastructure snapshot; requirements were preserved.
- Architecture review: ordinary user processes, Kestrel loopback listeners, npm workspace tooling, and a foreground .NET Worker can use installed runtimes without Docker, Windows Services, or Administrator privileges.
- Node 24 and the older runtime patches listed in the requirements are not installed; compatibility with those exact versions is not yet verified.
- No corporate-network listener or system security setting has been changed.
- Created the M1 source scaffold: NestJS health-only API, React/Vite/Tailwind foundation page, ASP.NET Core gateway shell, .NET Worker shell, shared contracts/configuration, and local foreground lifecycle tooling. No upload or application execution endpoints were added.
- Added foundation configuration and occupied-port tests plus a four-component startup/shutdown smoke procedure. Builds and tests are not yet verified.
- Initial npm install succeeded but reported three high-severity dependency advisories. Investigating the dependency paths before marking the foundation verified.
- First build failed because npm-generated executable shims mishandled the ampersand in the repository folder name. Project scripts now invoke local TypeScript/Vite/Prettier entrypoints through Node directly; validation is pending.
- Audit identified vulnerable Multer via NestJS platform-express. Added a scoped override to patched Multer 2.5.0; a fresh install and audit will verify the result.
- First corrected build passed: API and dashboard compiled; the .NET solution built with 0 warnings and 0 errors. Full automated tests are still pending.
- npm initially retained the previous Multer lock entry after the override. `npm.cmd update multer` resolved 2.5.0; the update audit reported 0 vulnerabilities. No forced downgrade or system setting change was used.
- Added architecture, local installation, security, troubleshooting, and root usage documentation. M1 local configuration and lifecycle contracts are documented; future data/deployment controls are explicitly distinguished from implemented foundation behavior.
- Initialized a local Git repository on `main` (no remote, staging, or commit). The previous non-Git state is historical.
- Ordinary token check returned `False` for Administrator role. `npm.cmd run doctor` passed on the installed toolchain.
- The first full verification stopped at formatting of the generated NuGet lockfile; generated lockfiles are now excluded from Prettier.
- The next verification passed builds (0 warnings/errors), formatting, TypeScript checks, 16 Node configuration/lifecycle tests, and 2 API tests. .NET test execution was aborted before tests ran: Windows group policy blocked `tests/Foundation.Tests/bin/Debug/net8.0/testhost.exe` (Win32 error 1260). No policy workaround or security-setting change was attempted. M1 cannot be completed while required .NET tests remain unverified.
- `dotnet restore InternalHostingPlatform.sln --locked-mode` passed, and `npm.cmd audit --audit-level=low` reported 0 vulnerabilities. Local startup/shutdown smoke verification is the next safe check.
- The initial smoke attempts timed out at readiness despite direct HTTP checks returning 200 for all three endpoints. Cleanup stopped owned processes, released the ports, and removed test storage. Added bounded startup diagnostics and reduced polling/log noise; investigating the readiness failure before accepting local lifecycle behavior.
- Diagnosed the smoke failure: the initial API port 5060 is restricted by the Fetch Standard; Node reported `fetch failed` with cause `bad port`. Changed the local API default to 5065 and added restricted-HTTP-port validation/regression coverage in Node and .NET. No firewall or security control was changed.
- Corrected smoke passed: all four components started; health responses, React entry module, local API proxy, and disabled deployment endpoints were verified. Windows socket inspection found exactly three listeners (5050, 5065, 5173), all on 127.0.0.1, and no Agent listener. All four processes exited with code 0, their ports were released, and the temporary test storage was removed.
- M1 is now `BLOCKED` solely because the required .NET suite has not run under company group policy. M2 has not been started. Final reproducible install and Node/static checks are being recorded before handoff.
````

</details>
`````

</details>
``````

</details>
```````
````````
`````````
``````````
```````````
````````````
