# Project Progress

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
