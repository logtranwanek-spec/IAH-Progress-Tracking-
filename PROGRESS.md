# Project Progress

## 50 MiB ZIP upload limit — verified handoff, 2026-10-28

**Last completed task:** increased the compressed ZIP cap consistently to **50 MiB = 52,428,800 bytes, inclusive**. Dashboard label: **Maximum ZIP size: 50 MB**. Final `npm.cmd run verify` exited **0 with 529 passing checks, zero failures/skips, plus complete local smoke**. Earlier checkpoints/failures below are retained as history. This completes the assigned size-limit refinement only: **M7/M8 remain IN PROGRESS**, M1–M6/M11 COMPLETED and M9/M10/M10-DB/M12 NOT STARTED. Actual-browser acceptance remains **53 NOT RUN**, and production browser management remains default disabled.

### Implemented and verified behavior

| ZIP byte size / case           | Actual evidence                                                                                                                                                                                                                                                                |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 52,428,799 (one byte below)    | Dashboard validation accepts; real native API and signed Gateway uploads accepted/inspected with PENDING_APPROVAL and correct stored archiveBytes                                                                                                                              |
| 52,428,800 (exactly 50 MiB)    | Dashboard validation accepts; real native API and signed Gateway uploads accepted/inspected with PENDING_APPROVAL and correct stored archiveBytes                                                                                                                              |
| 52,428,801 (one byte above)    | Dashboard/client rejects before reading/hashing/network/persistence with explicit 50 MB/52,428,800-byte error; Gateway returns 413 before buffering via valid Expect: 100-continue negotiation; native API rejects a real over-limit body with 413 before deployment recording |
| Malformed ZIP / unsafe content | Signed invalid-ZIP test rejects; existing managed ZIP/CRC/path/link/type/count/ratio/extraction/approval/tampering regression passes                                                                                                                                           |

The React form keeps existing submitting/loading/error behavior. BrowserSessionApi also validates before hashing, so bypassing the form cannot trigger an oversized file read. API Content-Length and streamed-byte enforcement use the same cap; raw application/zip ingress is streamed to owned files, with no multipart/in-memory middleware substitution. Gateway Kestrel/body overrides raise only the explicit browser ZIP route budget; ordinary application proxy and JSON budgets remain unchanged. Inspector/Agent archive-length and SQLite storage CHECK now match the cap; OpenAPI operation text documents the exact maximum.

**Security/extraction limits unchanged:** 32 MiB expanded total, 8 MiB per expanded file, 256 entries and 100:1 ratio with the existing 4-KiB allowance. Existing strict ZIP directory/local-header/CRC/overlap/trailing-data rules, ZIP Slip/path/link checks, file/runtime restrictions, hashes/fingerprints, session/RBAC/ownership revalidation, distinct Administrator review, default-disabled execution and trusted-prebuilt opt-ins are preserved. Larger input size does not make invalid, over-expanded, unapproved or source-build packages acceptable. Exact-size tests use valid empty non-final DEFLATE stored blocks inside a declared compressed stream; they do not add ZIP comments/trailing data or change extraction budgets. No unreviewed uploaded code runs.

**Memory/streaming review:** API continues backpressure-controlled disk streaming. Gateway preallocates one exact bounded body rather than growing MemoryStream plus ToArray duplication; existing two-upload admission bounds those body arrays to 100 MiB total. Signed browser proof generation still needs a bounded WebCrypto input buffer; forwarding reuses the original immutable Blob instead of a second archive-sized Blob. Review download copies from the bounded buffered response stream rather than another full byte-array clone. Inspector holds one bounded archive array plus extraction buffers. These are implementation buffer bounds, not total process/browser RSS measurements or a sandbox claim. Existing 15-second upload, six-second inspector and other request deadlines/concurrency/50-record-per-uploader quotas remain; slow requests can still time out. No ordinary proxy budget or unrelated feature was expanded.

### Additive Registry migration and operating requirement

Added **202610280005_zip_upload_50mib** as the fifth reviewed migration; 20 Prisma models remain unchanged. The original four migration files/checksums were not edited. The new migration transactionally rebuilds only deployment_packages with the larger compressed-byte CHECK, copies all rows/columns, and restores indexes, foreign keys and immutable-metadata/state triggers. Upgrade/idempotence tests verify preserved rows/history/guards, exact-cap acceptance, cap-plus-one and expanded-limit rejection, foreign-key rejection and rollback/integrity behavior. Agent publication includes the new SQL checksum input; preflight verifies it in addition to the old package migration.

**Configured user data was not migrated or modified.** API/Agent fail closed until a configured Registry has the current migration chain. Before operating updated components on an existing Registry, an explicitly authorized operator must stop those platform components and use the existing controlled migration procedure:

```powershell
# Existing configured Registry: explicit authorized migration step, NOT run by this task.
npm.cmd --workspace @ihp/api run db:migrate
npm.cmd --workspace @ihp/api run db:status
```

Only fresh/upgrade owned test databases were migrated here. The disposable browser acceptance helper will create fresh five-migration test data when the operator next runs it; it does not authorize migration of configured storage. No new application database lifecycle capability, external database connection, schema server or OS/security configuration was introduced.

### Files created or modified (source remains local)

| Scope                                   | Files                                                                                                                                                             |
| --------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| API                                     | src/Api/src/packages.ts; src/Api/src/package-controllers.ts                                                                                                       |
| Migration / upgrade tests               | src/Api/prisma/migrations/202610280005_zip_upload_50mib/migration.sql (created); src/Api/test/database.test.mjs                                                   |
| Dashboard                               | src/Dashboard/src/api.ts; src/Dashboard/src/model.ts; src/Dashboard/src/app.tsx; src/Dashboard/src/browser-session.ts                                             |
| Dashboard / protocol fixtures and tests | src/Dashboard/test/dashboard.test.mjs; src/Dashboard/test/browser.test.mjs; src/Dashboard/test/package-fixture.mjs                                                |
| Gateway                                 | src/Gateway/BrowserBoundary.cs; src/Gateway/GatewayHost.cs                                                                                                        |
| Inspector / Agent / managed test        | src/DeploymentAgent/PackageArchive.cs; src/DeploymentAgent/PackageWorkflow.cs; src/DeploymentAgent/DeploymentAgent.csproj; tests/DeploymentAgent.Tests/Program.cs |
| Local operating docs                    | PACKAGES.md; BROWSER_AUTHENTICATION.md; API.md; ARCHITECTURE.md                                                                                                   |
| Official tracking records               | PROJECT_REQUIREMENTS.md; AGENTS.md; IMPLEMENTATION_PLAN.md; PROGRESS.md                                                                                           |

Root implemented/integrated/verified all changes and owns documentation/Git. One bounded read-only Explorer used gpt-6-luna/high to review migration requirements and valid exact-size ZIP fixture design; it performed no edits, tests, application execution, migrations or network calls. No other agent was spawned. No existing deployed company application, security/firewall setting, configured Registry, credential store or production listener was modified. All verification listeners stayed literal 127.0.0.1; .NET used UseAppHost=false publication and published DLL execution, never apphost/testhost/VSTest/browser executables. Only SQLite performed real database operations; other engine references remain metadata-only.

### Actual verification and failure history

| Command / attempt                                                       | Actual result and evidence                                                                                                                                                                                                                               |
| ----------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| npm.cmd run test:packages (first attempt)                               | Exit 1 before tests: published Agent DLL locked by the operator's running manual browser fixture (PID 35508, confirmed parent browser-acceptance.mjs). Operator explicitly confirmed Ctrl+C/cleanup; no process was killed. artifacts/zip50-packages.log |
| npm.cmd run test:packages (retry)                                       | Exit 0, 37/37 managed package/security/lifecycle checks; artifacts/zip50-packages-retry.log                                                                                                                                                              |
| npm.cmd run test --workspace @ihp/api                                   | Exit 0, 84 API/auth/startup + 14 guarded SQLite checks; artifacts/zip50-api.log                                                                                                                                                                          |
| npm.cmd run test --workspace @ihp/dashboard                             | Exit 0, 43/43 including UI exact-cap/over-cap error/no-persistence; artifacts/zip50-dashboard.log                                                                                                                                                        |
| npm.cmd run test:browser (first expanded attempt)                       | Exit 1, 50/51: header-only probe ended a nonempty declared request and got socket reset. All four below/exact real uploads passed. Corrected the test to negotiate Expect: 100-continue; artifacts/zip50-browser.log                                     |
| npm.cmd run test:browser --workspace @ihp/dashboard (corrected fixture) | Exit 0, 51/51; artifacts/zip50-browser-fixed.log                                                                                                                                                                                                         |
| npm.cmd run verify (first full attempt)                                 | Exit 1 at Agent 26/27: a pre-existing assertion expected four migrations, observed five. Updated that assertion only; four Agent tables remain four. Remaining integration/package/smoke phases were not reached. artifacts/zip50-verify.log             |
| npm.cmd run test:agent (after assertion correction)                     | Exit 0, 27/27; artifacts/zip50-agent-fixed.log                                                                                                                                                                                                           |
| npm.cmd run verify (final complete rerun)                               | **Exit 0, 529 PASS / 0 FAIL / 0 SKIP plus full smoke**; artifacts/zip50-verify-final.log                                                                                                                                                                 |

Final breakdown: Node foundation/readiness **33**, API/auth/startup **84**, guarded SQLite **14**, Dashboard DOM/native-client **43**, signed HTTP/WebCrypto/real-service **51**, managed Foundation **27**, Gateway **113**, Samples **82**, Agent **27**, routing integration **18**, packages **37**. This adds nine checks to the preceding 520-check gate. All twelve publish roots passed no-.exe scans; build/guarded Prisma generation/type/format/solution checks passed. Smoke used Gateway=5050, owned temporary API=64384/Dashboard=64385 and verified actual assets/CSP/public health/browser rejection, structured logs, loopback sockets, readiness/default-disabled deployment and owned shutdown/port/storage cleanup. PowerShell Tee-Object logs are UTF-16LE. SDK formatting and targeted Markdown/source formatting passed; no dependency installation was required.

Read-only consistency checks passed: aligned byte caps, preserved extraction budgets, old package migration matching published checksum input, original instructions/history and Managed Application Database requirements retained, all milestone statuses unchanged and all 53 browser cases still NOT RUN. These protocol/DOM/fixture results are not actual-browser acceptance. No unresolved size-limit regression remains in the final suite; configured migration and existing M7/M8 acceptance gaps remain operational/unfulfilled scope, not silently completed features.

**Exact next recommended task:** resume M8 operator-assisted T01–T05 only when the operator starts the existing disposable manual fixture and confirms browser readiness; use the preserved runbook below and collect sanitized actual-browser evidence. Do not enable production private browser management, execute source builds or start M9/M10/M10-DB/M12. Existing configured Registry migration remains a separate explicit authorized operator action. If upload code changes again, resume with npm.cmd run test:packages, npm.cmd run test:browser and npm.cmd run verify as appropriate; record actual failures/results. Deliver only the four changed official Markdown files to progress origin/main and verify the remote commit; source/SQL/tests/local docs/config/data/ZIPs/logs/artifacts remain local.

## Active 50 MiB ZIP upload refinement — 2026-10-28

**Agent correction verified:** focused npm.cmd run test:agent exited 0 with 27/27 after updating only its migration-count expectation to five (Agent table count remains four). Evidence: artifacts/zip50-agent-fixed.log. Complete npm.cmd run verify rerun is now active, captured separately in artifacts/zip50-verify-final.log. No full success is claimed until that rerun exits 0 through smoke.

**First full gate failed accurately recorded:** npm.cmd run verify exited 1 after the Agent suite reported 26/27. Its existing migration-history assertion still expected four migrations and observed the correct new total five; Node/API/SQLite/Dashboard/signed boundary checks, Foundation/Gateway/Samples had passed. Updated that stale test assertion only (the four Agent tables remain four). Focused Agent rerun and a complete full rerun are required; routing/package/smoke phases were not reached by this first full attempt. Evidence: artifacts/zip50-verify.log. No upload/runtime security check was weakened.

**Focused gate passed:** corrected signed/real-service suite exited 0 with **51/51**, including valid one-byte-below/exact-limit uploads through Gateway and native API, pre-hash client rejection, header-only 413 via Expect: 100-continue, unchanged ordinary proxy budget and invalid ZIP rejection. Focused totals are API 84/84, guarded SQLite 14/14, Dashboard DOM 43/43 and managed packages 37/37, all exit 0. Full npm.cmd run verify is running; only its eventual exit/result may establish a new complete gate. Actual-browser tests are still 53 NOT RUN. Local PACKAGES/BROWSER_AUTHENTICATION docs now state 50 MiB and unchanged extraction/memory/authorization budgets; API/ARCHITECTURE current schema summaries now state five reviewed migrations.

**Boundary checkpoint:** first expanded signed suite passed 50/51. Valid 52,428,799-byte and exactly 52,428,800-byte ZIPs passed through both signed Gateway and native API, retained unchanged expanded limits and stayed pending approval. Client one-byte-over rejection and malformed ZIP rejection passed. The remaining header-only oversized HTTP probe ended a declared nonempty request without supplying its body and received ECONNRESET; corrected the fixture to negotiate Expect: 100-continue so rejection can be observed before body transmission. No server security budget was loosened. Focused rerun is in progress; artifacts/zip50-browser.log preserves the initial result. Full gate remains pending.

**Focused checkpoint:** native API/auth/startup suite passed 84/84 and guarded SQLite suite 14/14, including the additive 4-to-5 migration, preserved rows/checksums/indexes/triggers/foreign keys and new byte CHECK boundaries. Dashboard DOM/real-native-client suite passed 43/43, including exact size validation/50 MB label and no persisted upload above the limit. First test:packages attempt failed before tests because the operator's existing manual acceptance fixture locked the published Agent DLL (PID 35508, parent tools/browser-acceptance.mjs). Requested safe Ctrl+C cleanup; the operator confirmed the fixture stopped. No process was killed or configured data modified. Retry test:packages exited 0, 37/37 managed package/security/lifecycle checks. Signed native/Gateway exact-size tests are now running; full regression remains pending. Evidence: artifacts/zip50-api.log, zip50-dashboard.log, zip50-packages.log (failure), zip50-packages-retry.log (pass).

Latest user authorizes compressed ZIP uploads up to 52,428,800 bytes, inclusive, plus focused/full regression. Read all four official records and traced the existing 8 MiB cap through Dashboard, API streamed upload, signed Gateway/body override, managed inspector/Agent and SQLite CHECK. Root owns implementation/docs/integration. One read-only Explorer (gpt-6-luna/high) investigated the additive migration and exact-size ZIP fixtures; no edits/tests/DB operations were delegated. No other agent was spawned.

Implementation checkpoint: updated compressed limits and clear Dashboard label/error, preserved expanded/file/count/ratio/CRC/path/type/approval/fingerprint controls and ordinary proxy/JSON budgets. API remains streaming; Gateway now preallocates one bounded body instead of MemoryStream growth/ToArray copies, and browser transport reuses the immutable Blob after hashing. Existing two-upload concurrency and deadlines remain. Prepared additive 202610280005_zip_upload_50mib migration preserving old migration bytes/checksums, rows, indexes/triggers/foreign keys; Agent preflight checks the new checksum. No configured Registry migration was run. Added boundary/UI/signed/native API tests, a data-preserving upgrade test and valid exact-size DEFLATE empty-block fixtures that do not expand beyond existing limits. Focused/full verification is pending; no new functionality is marked verified yet. M7/M8 stay IN PROGRESS, browser cases remain 53 NOT RUN/default-off; no later milestone or application database service is started.

## M8 operator handoff readiness revalidation — 2026-10-28

**Current authorized task:** advance operator-assisted M8 acceptance by inspecting/reusing the existing disposable environment, validating safe startup and providing exact operator instructions. The latest user assignment supersedes the preceding workflow-configuration-only assignment for this task; it does not authorize another milestone or weaken any security restriction. No application/tool/test infrastructure or configuration was changed. Only this progress record requires updating; AGENTS.md, IMPLEMENTATION_PLAN.md and PROJECT_REQUIREMENTS.md remain unchanged. Existing model preferences/local configuration were not changed or claimed active.

**Lean execution actually used:** Root-alone mode under Lean Multi-Agent Orchestration. No agents spawned; the bounded infrastructure inspection, one existing fixture check and handoff did not benefit from delegation. Native capabilities remain available, but no unnecessary model/delegation probe or parallel investigation was performed. Root retains verification/documentation/Git responsibility.

**Actual verification:** read the four records, existing BROWSER_AUTHENTICATION.md, tools/browser-acceptance.mjs, native browser/client guards and LocalStack dependency/loopback environment boundary. Confirmed the existing matrix has **53 unique scenarios, all NOT RUN**. Browser-tool discovery returned no permitted browser/computer/driver capability; no operator evidence was supplied. No browser/driver executable was launched, profile/security setting changed or new acceptance tool recreated.

Ran **node tools/browser-acceptance.mjs --self-check**, exit **0**. Evidence: **artifacts/m8-operator-readiness.log** (PowerShell Tee-Object UTF-16LE). Existing reviewed migrations were explicitly applied only to a new owned test SQLite Registry. Five test-only accounts, three applications with expected ownership/read grants, three trusted-prebuilt sample ZIPs and the source-denied negative ZIP were prepared. Native setup sessions were revoked; Viewer administration was denied. Managed socket inspection verified literal **127.0.0.1:5050 / 63253 / 63254** for Gateway/API/Dashboard. Trusted package execution stayed disabled; no package was uploaded/approved/prepared/executed during this check. Published DLLs were checked through existing no-.exe validation; no new build/publish, testhost/VSTest, Docker, elevation, firewall/Windows-policy change, external database, corporate listener or existing deployed-app/configured-data modification occurred.

The fixture reported ready, then stopped owned components, removed its validated IhpM8Acceptance-ZibNxO storage and verified all owned ports released. **Validated startable, not left running.** The operator must run the existing manual command below to create a fresh session with privately chosen credentials; printed ports/IDs/hashes will be new. Current production/configured Registry browser management remains default disabled. No actual browser acceptance is inferred from fixture readiness or native/managed assertions.

**Browser results this assignment:** PASS **0**, FAIL **0**, BLOCKED **0**, NOT RUN **53**. Browser capability availability is an external **BLOCKED prerequisite**, not a tested browser scenario or a reason to relabel all unperformed cases. **M8 remains IN PROGRESS**; M7 remains IN PROGRESS/source builds disabled. M1–M6/M11 COMPLETED; M9/M10/M10-DB/M12 NOT STARTED. Original unsupported wizard/settings/read-contract requirements and Managed Application Database future scope are preserved. No acceptance criterion was waived.

**Verification scope:** no source/infrastructure changes justify a full-suite rerun, so **npm.cmd run verify was NOT RUN this assignment**. The last successful full gate remains the historical **520 PASS / 0 failures/skips plus smoke**, recorded in artifacts/m8-acceptance-verify.log; it is not new evidence here. Read-only fingerprints before/after the self-check matched across 184 local source/tool/test/sample/package/config files (generated bin/obj/dist outputs excluded). Documentation consistency checks exited 0: all 53 unique browser rows remain NOT RUN, AGENTS/plan/requirements and all milestone statuses are unchanged, complete historical progress is preserved, readiness/cleanup receipts are present and the owned fixture root no longer exists. Targeted Markdown formatting and git diff --check passed. Source/tooling, local Codex config, secrets, databases/packages/descriptors/logs and artifacts must not enter the progress commit.

### Concise operator handoff

1. In a normal approved PowerShell terminal at this project root, use the **exact masked-password setup block in “Disposable setup and exact operator commands” below**, ending with `node tools/browser-acceptance.mjs --manual`. Enter a new unique test-only 15–128 character password privately; it is never printed or passed as a command argument. The helper clears it from child environments; the parent terminal removes its environment entry in finally cleanup. No real company password or terminal transcript.
2. Wait for `Fixture ready; browser acceptance NOT RUN.` Keep the terminal running. In an **already-open company-approved browser**, open **http://localhost:5050/admin/** and confirm the address-bar origin is exactly **http://localhost:5050**. Gateway still listens only on IPv4 127.0.0.1:5050. The IP URL or Vite development port is not an authorized private-management substitute.
3. Use the privately chosen test password for accept-admin/accept-reviewer (Administrators), accept-dev/accept-other (Developers) and accept-viewer. Printed safe acceptance-setup.json identifies the fresh application UUIDs, ports and package hashes. Do not reuse descriptor IDs or ZIP assumptions from a previous deleted fixture.
4. Follow existing cases **T01–T05 first**, then A01–A09, S01–S10, R01–R09, D01–D15 and U01–U05 in the order/coordination notes below. Use only the helper's sample ZIPs. For approved deployment cases, stop and start a separate fresh reviewed-sample fixture using the documented `--manual --enable-reviewed-samples` command; separate exact review/Administrator approval/current M11 authority remain mandatory. It never enables source builds or auto-deployment.
5. Record `case ID | PASS/FAIL/BLOCKED/NOT RUN | time | approved browser/version | fixture/run ID | role | actual steps | safe status/code/UI outcome | redacted evidence reference`. No passwords, tokens, proof/nonce/key material, raw request dumps, HAR or filled credential screenshots. Missing permitted replay/tamper/accessibility features remain explicitly BLOCKED/NOT RUN, never replaced by Node/jsdom evidence.
6. Press **Ctrl+C** in the owning terminal to stop/remove its fixture. Verify both `Owned acceptance storage removed.` and `Owned acceptance ports released.`; the masked-password wrapper clears the parent environment entry. Preserve only sanitized evidence outside the temporary root first. Never kill an unrelated port occupant or recursively delete an unverified directory.

**Exact next safe task:** the authorized operator starts the fresh manual fixture and supplies sanitized T01–T05 results from an already-open approved browser. Root then correlates those case receipts with the current descriptor/role/time/evidence before updating individual outcomes and proceeding with the remaining matrix. Until that evidence exists, keep all 53 cases NOT RUN, M8 IN PROGRESS and production browser management default off. Do not repeat automated suites merely to stand in for missing browser evidence or start M9/M10/M10-DB/M12.

## Lean multi-agent workflow configuration — 2026-10-28

**Current assignment: workflow configuration and documentation only.** Added a permanent **Lean Multi-Agent Orchestration** section to AGENTS.md and prepared minimum repository-local .codex/config.toml. No application feature/source/test/schema/migration, application execution/build/test, dependency installation, extension, global Codex setting, production access, Windows/firewall/security setting or milestone work was changed/performed. Historical progress is preserved below. Last successful application verification remains the preceding **520 PASS / 0 failures / 0 skips plus smoke**, not a new run in this task. M8 actual-browser acceptance remains **53 NOT RUN**, production browser management default off; M7/M8 IN PROGRESS, M1–M6/M11 COMPLETED, M9/M10/M10-DB/M12 NOT STARTED.

### Capability evidence and configuration applied

| Capability              | Observed evidence / limitation                                                                                                                                                                                                                                                                                                                                                                              |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Native delegation       | collaboration.spawn_agent and list/message/follow-up/wait/interrupt tools are exposed. Read-only list_agents returned only /root running. No child was spawned: this bounded documentation/configuration task does not benefit from delegation. Availability is based on tool contracts, not an invented successful child run                                                                               |
| Per-agent models        | spawn_agent accepts model; advertised IDs include gpt-6.1-sol, gpt-6-luna and gpt-6-astra, exactly matching requested preferences. No account/model launch or entitlement probe was performed                                                                                                                                                                                                               |
| Per-agent reasoning     | spawn_agent accepts reasoning_effort; requested low/medium/high levels are supported in its metadata. On this host overrides require fork_turns=none and a concise self-contained task; a full-history fork does not accept overrides                                                                                                                                                                       |
| Repository control      | Applicable AGENTS.md instructions may authorize optional delegation within the current task when the host permits it. They cannot override higher-priority/user restrictions, force an active Root model switch, grant permissions, create an OS sandbox or guarantee model/configuration loading                                                                                                           |
| Existing configuration  | No repository .codex or .agents directory existed at inspection. Read only the nonsecret model/effort settings from the existing user configuration: global default gpt-6-luna/xhigh. Left global settings and unrelated configuration untouched; do not infer that file's defaults identify the live Root model                                                                                            |
| Prepared local override | Created .codex/config.toml with documented model="gpt-6.1-sol", model_reasoning_effort="medium" and [agents] max_concurrent_threads_per_session=2. This is a supported project configuration format, not a claim that the running host reloaded it. Loading depends on a compatible client and existing project trust. No trust, sandbox, approval, provider, authentication or network setting was changed |

Preferred routing is recorded permanently: Root/Integrator Sol medium (one accountable agent); Explorer Luna high/read-only bounded investigation; Worker Sol high/assigned files and focused tests; Researcher Luna high/read-only narrow documentation question; Reviewer Astra low/optional independent evidence-based review. Role preferences are supplied through actual explicit native spawn arguments when delegation is justified; no unnecessary custom-role files/default subagent configuration were added. Use current Root or another verified supported choice if unavailable, report the fallback and do not repeatedly probe models. Current live Root selection/reasoning was not switched or attested by this task. Local configuration is prepared for compatible future trusted-project sessions, not delivered as a global change.

Policy remains Root-alone by default, normally one child at a time and at most two for genuinely independent scopes, subject to any lower runtime limit (this session advertises four total slots). No automatic all-role fan-out, duplicate investigation or unsupervised recursive delegation. Exact owner/file scope/deliverable/verification/stopping condition is required; no concurrent edits to one file. Root controls official tracking records/configuration/integration/final gate/Git delivery. Read-only roles do not modify files or run applications/builds/tests without explicit assignment. High effort is justified by real complexity; trivial tasks stay on Root with no extra agents. Workers use focused checks; Root runs the required final gate at integration rather than repeating it in each child. Documentation-only work uses consistency/configuration/whitespace checks only.

All agents inherit identical IHP restrictions, including SQLite-only operations/metadata-only other engines, no executable artifacts or Docker/elevation/security changes, managed DLL publication/execution, literal loopback/Gateway 5050, no existing deployed-app changes, no secrets in logs/control, M11 authority, M7 approval/fingerprint/trusted-prebuilt/no-source-build rules and the future-only Managed Application Database service. Delegation/file ownership/read-only directions are behavioral controls, not isolation or additional authority. Workflow invocation never starts another milestone or provides missing browser acceptance.

### Verification, delivery and exact next action

Used the OpenAI Docs skill and fetched [official subagent documentation](https://learn.chatgpt.com/docs/agent-configuration/subagents) and [configuration reference](https://learn.chatgpt.com/docs/config-file/config-reference) to confirm supported settings and project-trust/loading limits. Runtime tool metadata is the authority for the available spawn arguments and models; official docs do not prove that a local file was applied by this live host. No Codex/application executable, CLI model probe, extension install, application build/test or browser was launched. Requested role/model/effort values are advertised and recorded, but live Root switching, configuration reload and delegated model execution were not verified or claimed.

Actual verification: read-only Node documentation/configuration assertions exited 0, validating the new file's fixed TOML subset against the documented three-key allowlist, unchanged global configuration checksum, preserved existing AGENTS instructions/security rules, complete progress history and unchanged requirements/plan/all 13 statuses. This is a structural/documentation check, not a Codex parser/load or live inference test. Targeted Prettier --check AGENTS.md PROGRESS.md and git diff --check each exited 0. No application tests/builds were run, and no historical test count was relabeled as current-task evidence.

Only AGENTS.md and PROGRESS.md change within the authorized progress allowlist. IMPLEMENTATION_PLAN.md/PROJECT_REQUIREMENTS.md need no clarification and remain unchanged. .codex/config.toml remains local/untracked and must not be pushed to the progress repository. Do not stage any application source, tests, credentials, database, artifacts/logs, local configuration or additional documentation. Validate documentation/allowed configuration keys, unchanged statuses/requirements/history and git diff --check before commit; verify origin/main matches the delivered commit.

**Future invocation:** "Use the lean workflow for [authorized task]; delegate only if it helps." For a specific bounded investigation: "Use one read-only Explorer on [module/question], return references, then have Root decide the next step." Root must still assess whether delegation helps and use only verified supported controls. **Next safe project task remains M8 operator-assisted acceptance using the disposable runbook below**, not a new milestone. This workflow task does not authorize executing that runbook or converting its 53 NOT RUN cases into PASS. No pending application defect was repaired by this configuration change.

## M8 operator-assisted browser acceptance runbook — 2026-10-28

**Current acceptance decision:** M8 remains **IN PROGRESS**, private production browser management remains disabled by default, M7 remains IN PROGRESS/source builds disabled and M9/M10/M10-DB/M12 remain NOT STARTED. Browser capability discovery found no connected browser/computer/driver tool. Availability is **BLOCKED**; every actual-browser scenario below is **NOT RUN**, with no operator results supplied. The existing 520 automated passes are not browser acceptance. This current runbook and the checkpoint below supersede earlier setup suggestions for this assignment; preserve all prior evidence as history.

**Final local verification for this assignment:** `npm.cmd run verify` exited **0 with 520 passing checks, zero failures/skips, plus complete owned local smoke** after adding fixture tooling. Counts: Node foundation/readiness 33, API/auth/startup 84, guarded SQLite 13, Dashboard DOM/native-client 42, signed protocol/WebCrypto/real-service 44, managed Foundation 27, Gateway 113, Samples 82, Agent 27, routing integration 18 and packages 37. All twelve published roots passed no-.exe scans; workspace build/Prettier/type/.NET format checks passed. Smoke used Gateway 5050, temporary API 65503 and Dashboard 65504, verified real assets/health/reserved-path browser rejection/structured logs/loopback sockets, then stopped owned components, released ports and removed temporary storage. Evidence: artifacts/m8-acceptance-verify.log (UTF-16LE). This is regression evidence, not an actual-browser PASS.

**Separate preparation evidence:** `node tools/browser-acceptance.mjs --self-check` initially exited 1 on the socket-inspector argument format; all owned children/storage were cleaned. Corrected run exited 0, creating five test users/three apps/four ZIPs and verifying setup revocation, literal loopback listeners 5050/59507/59508, owned storage removal and port release (artifacts/m8-acceptance-fixture-fixed.log). The first failure is preserved in artifacts/m8-acceptance-fixture.log. `--manual --enable-reviewed-samples` reached ready on 5050/64124/64125; Ctrl+C produced both cleanup receipts and outer interrupted PowerShell/PTY exit 1. No browser opened and no package uploaded/approved/executed in preparation; the full existing regression separately executes only its approved trusted fixtures. Read-only postchecks confirmed both prepared fixture roots no longer exist. Nothing is left listening for operator use; rerun the documented manual command to create a new disposable session.

**Files changed in this assignment:** created local tools/browser-acceptance.mjs; updated local BROWSER_AUTHENTICATION.md evaluation instructions; updated official PROGRESS.md (full runbook/matrix/evidence/next task) and IMPLEMENTATION_PLAN.md (M8 acceptance gate). AGENTS.md/PROJECT_REQUIREMENTS.md are unchanged and still authoritative, including Managed Application Database requirements. No Dashboard/Gateway/API/Agent/sample application source, database schema/migration, dependency, configured application/Registry, production browser setting, Windows/firewall/security policy or corporate-network exposure changed. The helper runs only existing reviewed migrations against its new owned test SQLite Registry. Source/tooling/local docs/logs/descriptors/packages/databases/build outputs remain excluded from the progress commit; only the two changed official records need delivery within the four-path allowlist.

Final read-only documentation checks passed: all 53 browser IDs are unique and NOT RUN, all 13 milestone statuses match the preceding commit, complete previous progress is retained and requirements/AGENTS are unchanged. Targeted Prettier --check for the four official records plus local BROWSER_AUTHENTICATION.md/helper exited 0; git diff --check exited 0. Delivery must validate the staged/committed path allowlist and verify remote main matches the delivered commit; no fixture data or source/tooling may be staged.

### Traceability and evidence rules

Acceptance sources: IMPLEMENTATION_PLAN.md M8 build/pages/data/authorization/prefix/responsive/accessibility criteria; PROJECT_REQUIREMENTS.md 5.1–5.3 Dashboard/registration/upload, 7 runtime/deployment, 8 routing, 12 authentication/RBAC, 13 security, and authorized M8 browser follow-up; local BROWSER_AUTHENTICATION.md session/transport/allowlist/checklist; actual BrowserBoundary.cs, browser-session.ts, app.tsx/management.tsx, native M11 and M7 contracts. M9 metrics/M10 rollback/M10-DB database lifecycle are not assigned and cannot be invented to close M8.

Allowed result states: **PASS** only with actual matching evidence; **FAIL** for an observed failed expectation; **BLOCKED** when an external restriction prevents execution; **NOT RUN** when no test was performed. All browser rows start NOT RUN and must be updated individually from real evidence. Do not infer PASS from Node HTTP/WebCrypto, jsdom, fixture readiness, static code review or a screenshot of an unrelated session. If only part of a row is tested, split its results and leave the untested part NOT RUN/BLOCKED; never call the whole row PASS.

Record one safe receipt per case: `ID | PASS/FAIL/BLOCKED/NOT RUN | UTC timestamp | approved browser product/version | fixture descriptor/run ID | role | steps actually performed | HTTP status/safe code and observed UI/state | redacted evidence reference | limitation/next action`. Safe evidence may be cropped UI, status/code-only notes, header-name/attribute presence, capability booleans, application/deployment IDs and package SHA-256 fingerprints. Do not export HAR, Copy as cURL/fetch, raw request/response bodies, console object dumps, full headers, authentication screens with filled passwords, cookies, signatures, challenges/nonces, browser session handles, native tokens/bindings, keys, capabilities, passwords or environment dumps. No terminal transcript/recording while entering a test password. Inspect sensitive request material locally only as needed; share only a sanitized outcome. Manual screenshots/records must follow company rules and stay outside the progress commit unless reduced to safe text in these official records.

Browser DevTools must already be permitted within the existing approved browser. Do not launch a browser/driver, create a remote-debugging listener, install extensions/tools/certificates, modify a hosts/DNS/proxy/firewall/profile security setting, disable CSP/CORS/CSRF, or bypass a self-XSS/pasting warning. If a safe built-in inspection/replay/accessibility feature is unavailable, mark that subtest BLOCKED. Browsers control Host/Origin/Fetch Metadata; do not pretend JavaScript can set forbidden headers or count a native forged-header test as browser evidence.

### Disposable setup and exact operator commands

Local **tools/browser-acceptance.mjs** now provides repeatable preparation. It never opens a browser, never reads configured IHP storage/settings, clears inherited IHP environment choices before selecting its own validated configuration, and creates a fresh `IhpM8Acceptance-*` directory under the current user's profile outside the repository. It explicitly applies only the existing reviewed Registry migrations to that fresh SQLite file, never actual configured data. The API, normal Agent, Gateway and Dashboard run through existing Node/published-DLL hosts. Gateway stays **127.0.0.1:5050**; API/Dashboard use printed, validated temporary loopback ports outside 6100–6999. Static routes start empty. A 5050 conflict fails safely without killing or reconfiguring its owner.

Five disposable accounts use a single unique **test-only** password chosen privately by the operator: `accept-admin` and `accept-reviewer` are Administrators; `accept-dev` and `accept-other` are Developers; `accept-viewer` is Viewer. Never use a company password. `accept-dev` owns accept-node and accept-dotnet; accept-other owns accept-foreign. Viewer and accept-other initially have read grants to accept-node only. Native setup sessions are explicitly logged out and verified unusable before the fixture is handed to the operator. No package is automatically uploaded/reviewed/promoted/executed.

Run these commands from this project root in an ordinary approved PowerShell terminal. Existing reviewed build outputs are required; if missing/stale, `npm.cmd run build` publishes with UseAppHost=false through the existing helper and does not migrate configured data. No dependency installation/browser/driver is required.

```powershell
# Optional preparation validation; starts/stops owned fixtures, NOT browser acceptance:
node tools/browser-acceptance.mjs --self-check

$ihpSecurePassword = Read-Host 'New test-only password (15-128 characters)' -AsSecureString
$ihpSecretPointer = [Runtime.InteropServices.Marshal]::SecureStringToBSTR($ihpSecurePassword)
try {
    $env:IHP_ACCEPTANCE_PASSWORD = [Runtime.InteropServices.Marshal]::PtrToStringBSTR($ihpSecretPointer)
} finally {
    [Runtime.InteropServices.Marshal]::ZeroFreeBSTR($ihpSecretPointer)
    $ihpSecurePassword.Dispose()
}
try {
    node tools/browser-acceptance.mjs --manual
    # For a separately reviewed disposable deployment run, replace the preceding command with:
    # node tools/browser-acceptance.mjs --manual --enable-reviewed-samples
} finally {
    Remove-Item Env:IHP_ACCEPTANCE_PASSWORD -ErrorAction SilentlyContinue
}
```

Keep that terminal running while testing. The helper removes the password before creating child environments, never prints/persists it, and writes only safe IDs/roles/ports/ZIP hashes to `<printed test root>/acceptance-setup.json`. The parent terminal still holds its test password environment entry until finally cleanup; this is not protection against the same Windows identity. Do not put a password in command arguments, source, shell profiles, URLs or evidence. Mode `--manual` enables the browser candidate only inside its fresh disposable stack and leaves trusted execution disabled. The separate `--enable-reviewed-samples` flag enables only the existing API/Agent execution policy in another fresh fixture; exact fingerprint review, distinct Administrator approval and current server authorization are still mandatory. It does not approve or start anything itself. Source builds remain disabled in both modes.

In an **already-open company-approved browser**, type **http://localhost:5050/admin/**. Do not start the ordinary configured stack or enable its browser flag. Use the printed descriptor's IDs for detail URLs: `http://localhost:5050/admin/#/applications/<application-uuid>`. Applications are at **http://127.0.0.1:5050/accept-node/** and **http://127.0.0.1:5050/accept-dotnet/** only after reviewed deployment and route publication; health URLs are the same prefixes plus `health`. Before deployment/publication expect no live application route. Public Gateway health is **http://127.0.0.1:5050/_platform/health**. The IP management shell **http://127.0.0.1:5050/admin/** must remain private-operation disabled. Existing browser tabs may be opened within this already-running approved session; no new executable/driver/profile/security setting is authorized. Use separate tabs for Administrator and Developer because page-memory sessions are intentionally independent.

Use only `<printed test root>/AcceptancePackages/accept-node-v1.zip`, accept-node-v2.zip, accept-dotnet-v1.zip and accept-node-source-denied.zip. Their exact application IDs/versions/SHA-256 values are in the descriptor. They contain only the existing trusted DemoNode or published managed DemoDotnet sample; the source-denied variant changes deploymentType only and must be rejected without executing/building it. Do not substitute company applications, source commands or arbitrary uploads. Review actual archive contents/hash through approved local inspection/browser download before Administrator attestation; a hash alone does not establish trust. Use accept-node v1 for approval/start, v2 for rejection and accept-dotnet v1 for managed-DLL workflow. Do not rename these apps before inspection, because package name/slug/framework must match immutable metadata.

Recommended order: transport/profile prerequisites; authentication/security; read-only/ownership scopes; registration and package workflows; route/audit/UX; grant/ownership/user-role mutations last because they revoke authority/approvals; expiry in a fresh uninterrupted sign-in. Restart the disposable helper for independent destructive/revocation cases as needed; every run has new IDs and loses prior state. If transport/profile/CSP/isolation prerequisites fail, stop private browser testing and leave the candidate disabled pending a reproducible M8 defect review. Do not turn protections off.

Approval sequence: upload accept-node v1 as accept-dev, review and approve as accept-reviewer; upload v2 as accept-admin, verify that same Administrator cannot approve their own request, then reject v2 as accept-reviewer. Upload accept-dotnet v1 as accept-dev and review separately as accept-reviewer. For a real backend ownership-denial check, leave the old Developer's supported settings confirmation open, transfer ownership in the Administrator tab, then confirm in the old tab using its normal freshly signed client request. A replayed stale nonce would prove Gateway rejection only, not backend RBAC; do not conflate those results. If the stale UI disappears first, record that protection separately and leave the unperformed negative request NOT RUN. Use a fresh fixture for last-active-Administrator tests if permission changes would disrupt other scenarios; never use bootstrap as a recovery bypass.

Ctrl+C stops only helper-owned children, verifies the resolved `IhpM8Acceptance-*` path under the user profile, removes that fixture and checks port release. An interrupted parent shell may report exit 1 even after both cleanup messages; record the actual exit and cleanup receipts. Copy only sanitized evidence outside the root before stopping, because the descriptor/test DB/packages are deleted. If an approved browser downloaded a review ZIP, manually remove only that exact test download using the permitted browser/OS workflow. On crash/failed cleanup, record the owned root/PIDs privately and inspect identity before recovery; never kill a port occupant or recursively delete a computed/unverified directory. No fixture was left running by this agent.

### Browser acceptance matrix — all scenarios NOT RUN

| ID / requirement                          | Procedure in the permitted existing browser                                                                                                                                                                    | Expected result / safe evidence to record                                                                                                                                                                                                                      | Actual status |
| ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| T01 — transport / M8 security follow-up   | Open exact localhost:5050/admin/; inspect actual connection/origin and profile/proxy context without changing settings                                                                                         | Reaches fixed IPv4 loopback; origin exact; trusted-local-host/unencrypted HTTP limitation acknowledged; no unexpected substitution/proxy. If not established, stop and BLOCK dependent cases                                                                   | NOT RUN       |
| T02 — secure context / WebCrypto          | Inspect isSecureContext and crypto.subtle; use permitted built-in console to generate a nonextractable P-256 signing pair and attempt private-key export; keep key objects local                               | Secure context/subtle available, generated private key extractable=false, export rejected; report booleans only. This probes browser support, not an inspected production private key                                                                          | NOT RUN       |
| T03 — security headers / prefix assets    | Inspect /admin/ and built /admin/assets responses in Network without exporting bodies                                                                                                                          | Correct asset prefix/MIME, CSP self-only/no unsafe inline or eval, no framing/base/form action, no-store/nosniff/referrer/CORP/COOP protections present; record attribute names/status only                                                                    | NOT RUN       |
| T04 — actual CSP enforcement              | With permitted DevTools, append a script element whose src is http://127.0.0.1:5050/ihp-csp-probe.js, then remove it; do not relax CSP                                                                         | Browser blocks foreign-origin script before loading/execution and reports CSP violation; no native/tool launch or corporate request. Record redacted violation, not console objects                                                                            | NOT RUN       |
| T05 — profile/service worker prerequisite | Inspect existing controller/service-worker state on management origin without silently clearing or unregistering anything                                                                                      | No controlling worker; if present candidate fails closed. Stop and record profile blocker rather than changing security/profile policy                                                                                                                         | NOT RUN       |
| A01 — successful login / requirements 12  | Sign in accept-dev using private fixture password; view identity and owned apps                                                                                                                                | Legitimate signed flow works on localhost; correct role/ownership; no native token/binding returned to browser. Record username/role and response statuses only                                                                                                | NOT RUN       |
| A02 — invalid login                       | One wrong-password attempt and one unused fixture username, then correct login; avoid rate-limit stress                                                                                                        | Same controlled login failure for invalid credentials; no private records or credential echo; successful recovery within M11 throttle policy                                                                                                                   | NOT RUN       |
| A03 — logout                              | Sign out, use Back/navigation/refresh, then try former private view                                                                                                                                            | Explicit server revocation, private UI clears, later unauthorized requests fail, no reused former authority. Observe safe codes/UI only                                                                                                                        | NOT RUN       |
| A04 — expiration                          | Fresh sign-in; retain real clock and wait beyond issued 30-minute absolute expiry, then use protected view                                                                                                     | Client clears private state and server rejects expired session; no clock/SQLite/lifetime edits. Record timestamps and safe unauthorized result                                                                                                                 | NOT RUN       |
| A05 — revocation                          | Developer in one existing tab; Administrator revokes that user's sessions in another tab                                                                                                                       | Next protected operation fails immediately server-side; visible view clears on focus/visibility or bounded 30-second revalidation. No claim of instantaneous push                                                                                              | NOT RUN       |
| A06 — browser refresh                     | Refresh authenticated tab without persisting/copying credentials                                                                                                                                               | Page-memory proof/session lost, sign-in required, no silent token restoration. Backing native record may remain until expiry/revocation; refresh alone is not server logout                                                                                    | NOT RUN       |
| A07 — tab lifecycle                       | New tab to same management URL; close/reopen authenticated tab; navigate away/back, including after revocation                                                                                                 | New/reopened page gets no inherited authority; restored pages cannot use revoked/expired authority and clear on revalidation. Document actual bfcache behavior, never assume close instantly revokes server record                                             | NOT RUN       |
| A08 — account switching                   | Logout Developer and login Viewer in same tab; keep a prior detail/request pending where supported                                                                                                             | New account sees only allowed data; old queued intent cannot execute as new user and late responses cannot repopulate private records                                                                                                                          | NOT RUN       |
| A09 — private-state cleanup               | While a list/detail/profile response is delayed using permitted per-tab browser throttling, logout/revoke; restore normal per-tab throttling                                                                   | Old data/actions do not reappear; form/private user state removed; safe retry/login state. Do not change OS/network/security settings                                                                                                                          | NOT RUN       |
| S01 — same-origin requests                | Inspect legitimate login and protected GET/mutation from management origin                                                                                                                                     | Browser-generated exact Origin where applicable, same-origin Fetch Metadata, no credentials forwarding; signed legitimate operations succeed. Record header presence/metadata values only, not proof/session values                                            | NOT RUN       |
| S02 — cross-origin access / CSRF          | From already-open 127.0.0.1 application/IP-shell tab, attempt fetch to localhost:5050/admin/browser/bootstrap with X-IHP-Browser:1 and credentials omitted                                                     | Preflight/request denied and no CORS access; no database mutation. Record browser block plus Network status such as 403 when available, not just a generic TypeError                                                                                           | NOT RUN       |
| S03 — Host validation                     | At 127.0.0.1:5050/admin/ try same-origin bootstrap GET with X-IHP-Browser:1; compare localhost flow                                                                                                            | IP shell private login disabled; IP-Host browser boundary rejected even with browser-generated same-origin metadata. Do not forge forbidden Host or add DNS/hosts aliases; arbitrary-Host browser subtests needing unavailable tooling stay BLOCKED            | NOT RUN       |
| S04 — missing CSRF/proof                  | At management origin submit a harmless protected application POST with empty JSON and X-IHP-Browser only, omitting proof/session/nonce; also inspect absent-cookie behavior                                    | Rejected (401/403), no application created; automatic browser cookies cannot grant authority. No token copied into console or storage                                                                                                                          | NOT RUN       |
| S05 — nonce replay                        | If approved built-in DevTools can resend an exact completed read-only /admin/browser/me request in place, replay it once without copying/exporting/editing its headers                                         | Consumed nonce rejected, no action/reuse; record status/code only. If browser lacks permitted safe replay, record BLOCKED and retain Node replay evidence as supporting only                                                                                   | NOT RUN       |
| S06 — fixation resistance                 | Observe fresh sign-in after logout in same/new tab; if permitted built-in edit/resend exists, alter an existing login to include a caller-chosen nonsecret sessionId without exporting/copying its credentials | New proof/session authority; caller-selected or stale/tampered login rejected, no usable fixed session. If safe tamper feature unavailable, split rotation evidence from BLOCKED negative subtest                                                              | NOT RUN       |
| S07 — unauthorized native/internal API    | Browser fetch/direct visit to Gateway /_platform/api/users and, using printed port, native API /_platform/api/users without native credentials                                                                 | Gateway reserved path never proxies it (404); direct browser management rejected by native guard/CORS (403 when visible), never bypassed by a former operator capability                                                                                       | NOT RUN       |
| S08 — Gateway origin isolation            | After approved route exists, open localhost:5050/accept-node/ and 127.0.0.1:5050/accept-node/ separately                                                                                                       | Management Host never serves application content; IP application Host serves only reviewed sample; application cannot read management pages/private responses or frame management                                                                              | NOT RUN       |
| S09 — application credential leakage      | Inspect only header-name presence/query shape on actual IP sample navigation/health/asset requests                                                                                                             | No browser session/proof/nonce/Authorization/binding/cookie/native capability sent to app routes or runtime ports; no secrets in URL/referrer                                                                                                                  | NOT RUN       |
| S10 — browser storage                     | Inspect management cookies/localStorage/sessionStorage/IndexedDB/Cache Storage and browser history before/after sign-in/logout without exporting data                                                          | No sensitive auth storage/cookies/URL credentials; page-memory design holds. Do not erase unrelated origin data; record names/absence and any unexpected state safely                                                                                          | NOT RUN       |
| R01 — Administrator                       | Login accept-admin; use Settings/account administration and application views                                                                                                                                  | Authorized users/apps/safe audits shown; mutations still backend-authorized and confirmed; own package approval prohibited                                                                                                                                     | NOT RUN       |
| R02 — owning Developer                    | Login accept-dev; inspect/edit accept-node/accept-dotnet                                                                                                                                                       | Owned app registration/metadata/package workflow available; no user/role/grant/owner administration; server rechecks ownership                                                                                                                                 | NOT RUN       |
| R03 — Viewer                              | Login accept-viewer; view granted accept-node and attempt navigation to registration/mutation controls                                                                                                         | Authorized read only; no application writes/upload/review/deploy/route management. Own logout/revoke-all remains available                                                                                                                                     | NOT RUN       |
| R04 — read-granted Developer              | Login accept-other; inspect accept-node versus owned accept-foreign                                                                                                                                            | Read grant does not become write/deploy permission; own foreign app is manageable                                                                                                                                                                              | NOT RUN       |
| R05 — foreign ownership denial            | Login accept-dev or Viewer; visit accept-foreign detail using descriptor UUID                                                                                                                                  | Safe not-found/denied response and prior private detail clears; unrelated application is absent from scoped lists/search                                                                                                                                       | NOT RUN       |
| R06 — ownership transfer                  | After stopping any running test app, Administrator confirms accept-node transfer to active accept-other; retry former owner                                                                                    | Stored owner changes; former owner's write authority removed immediately server-side; affected approvals revoked, no automatic deployment                                                                                                                      | NOT RUN       |
| R07 — grant/revoke                        | Administrator revoke then grant Viewer's read access; Viewer retries                                                                                                                                           | Read access follows actual API result; ownership unaffected; UI states current-grants list unavailable rather than inventing a list                                                                                                                            | NOT RUN       |
| R08 — permission escalation / last admin  | Developer/Viewer try direct management navigation; Administrator changes disposable target role/active state and revokes sessions; safely test last-active-Administrator rejection using remaining account     | Unauthorized actions rejected; target sessions lose authority; final active Administrator cannot be removed/demoted. Do not lock out the fixture then bootstrap around M11                                                                                     | NOT RUN       |
| R09 - real forbidden mutation             | Keep a Developer settings confirmation open; transfer ownership as Administrator in a different existing tab; confirm the old Developer form through its normal client                                         | Freshly signed request is rejected by current backend ownership (403), no mutation. A 401 caused only by stale nonce is NOT backend RBAC evidence. If UI clears before any request, record UI protection separately and keep negative-request evidence NOT RUN | NOT RUN       |
| D01 — registration                        | Developer registers a separate disposable app; test duplicate/reserved slug and invalid input                                                                                                                  | Correct owner/runtime/framework persisted; unique/reserved slug validation; no code starts or unsupported config persists                                                                                                                                      | NOT RUN       |
| D02 — details/updates                     | Edit supported description/name on a newly registered app; Cancel and then confirm                                                                                                                             | Cancel no write; confirmed name/description persists; runtime/framework/slug immutable; package compatibility warning accurate                                                                                                                                 | NOT RUN       |
| D03 — deployment guide/policy             | Generate Node/.NET previews, reject unsafe entry path, inspect environment/visibility/source/rollback/database labels                                                                                          | Preview unsaved, not package approval/build; fixed loopback/auto-port contract; unsupported features explicit; no M9/M10/M10-DB side effects                                                                                                                   | NOT RUN       |
| D04 — user management                     | Administrator searches/paginates, creates disposable account and confirms activation/role/session actions; repeat navigation as Developer/Viewer                                                               | Correct real records and safe validation; password form clears; unauthorized administration hidden/denied; no password reset/credential recovery workaround                                                                                                    | NOT RUN       |
| D05 — ZIP inspection                      | Developer uploads exact accept-node-v1 and accept-dotnet-v1 ZIPs using descriptor versions; try source-denied archive                                                                                          | Real hashes/file metadata/state displayed; valid packages pending approval, no auto-start; source package rejected/failed safely without build/execution                                                                                                       | NOT RUN       |
| D06 — separate approval                   | accept-reviewer reviews archive contents/hash then exact fingerprint and attestation; test Developer decision and self-approval denial                                                                         | Only distinct Administrator can approve exact current package; safe audit/history recorded; approval does not itself deploy                                                                                                                                    | NOT RUN       |
| D07 — rejection/history                   | Upload accept-node v2, Administrator rejects, inspect history                                                                                                                                                  | Terminal rejection recorded without promotion/start; history distinguishes decision from package/deployment states                                                                                                                                             | NOT RUN       |
| D08 — unapproved operation                | Before valid approval, try preparation/deploy using allowed UI or safe built-in resend                                                                                                                         | No execution; current authorization/approval required; no bypass through Dashboard                                                                                                                                                                             | NOT RUN       |
| D09 — default execution gate              | In ordinary --manual fixture, inspect separately approved package controls/authorization                                                                                                                       | Existing execution-disabled gate remains; approval cannot enable Agent/operator policy; do not call unavailable start a browser failure                                                                                                                        | NOT RUN       |
| D10 — approved deployment                 | Only in fresh --enable-reviewed-samples fixture, repeat exact review and explicitly prepare/deploy Node and .NET via authorized user                                                                           | Real queued -> prepared/running evidence, distinct loopback runtime ports in 6100–6999, exact sample identity/health at published routes; no fake healthy state from 202                                                                                       | NOT RUN       |
| D11 — stop/redeploy                       | Confirm Stop, observe route withdrawal, then explicitly deploy same approved release again while authorization valid                                                                                           | Owned sample stops and restarts through existing gates; Cancel does nothing; no atomic restart/rollback claim or unrelated process stop                                                                                                                        | NOT RUN       |
| D12 — routes                              | Inspect current stored intent, publish supported prefix for healthy approved sample, follow safe public link; confirm removal                                                                                  | API-derived loopback destination, preserved path, no host/port field; removed route stops proxying after bounded poll, management reserved paths intact                                                                                                        | NOT RUN       |
| D13 — audit                               | Administrator views actual login/failure/permission/approval/denial events; Viewer attempts same page                                                                                                          | Real safe M11 audit information; unauthorized read denied; no secrets/raw inputs, M9 metrics or invented deployment history                                                                                                                                    | NOT RUN       |
| D14 — loading/error/empty states          | Search nonexistent record; use permitted per-tab DevTools throttling/offline temporarily, navigate owned detail and retry after restoring                                                                      | Honest loading/empty/error/permission/session messages, accessible feedback, no raw errors/secrets or stale successful-state claim                                                                                                                             | NOT RUN       |
| D15 — data semantics                      | Compare actual registry status, package workflow state, requested action, stored route and unavailable runtime information                                                                                     | Distinct truthful labels; no fabricated live metrics, unavailable legacy histories, current-grant lists or successful unsatisfied workflow steps                                                                                                               | NOT RUN       |
| U01 — desktop                             | Inspect permitted browser at 1280x800 and 1920x1080, normal/200% zoom                                                                                                                                          | Sidebar/header/forms/tables/previews readable, no lost actions or unexpected page overflow; record viewport/zoom and cropped empty or safe test-data view                                                                                                      | NOT RUN       |
| U02 — mobile/responsive                   | Use existing browser's permitted responsive mode at 390x844 and 320x568                                                                                                                                        | Navigation toggle/Escape, form stacking, internal table/manifest scrolling, readable dialogs/actions without trapping content. Emulation is real-browser layout evidence, not a physical-device claim                                                          | NOT RUN       |
| U03 — keyboard                            | Tab/Shift+Tab from address/page, skip to main, operate sidebar/forms/selects/actions without pointer                                                                                                           | Visible focus, correct order/labels, working skip link and no keyboard trap; keyboard activation does not bypass permissions                                                                                                                                   | NOT RUN       |
| U04 — confirmation focus                  | Open permission/ownership/grant/stop/route dialogs; Tab/Shift+Tab/Escape/Cancel/confirm                                                                                                                        | Cancel-first focus, background inert, focus contained and restored, Cancel never mutates, confirmed request once; record actual focus behavior                                                                                                                 | NOT RUN       |
| U05 — accessibility                       | Use an already-permitted screen reader or built-in accessibility tree/contrast inspection, errors/live regions, zoom/reduced-motion behavior                                                                   | Names/roles/status/error announcements and focus readable; contrast/reflow assessed with actual tool. Missing assistive tool stays BLOCKED; DOM semantics do not certify screen-reader acceptance                                                              | NOT RUN       |

### Current results and next action

**Actual-browser totals for this assignment: PASS 0, FAIL 0; every matrix row NOT RUN.** Capability availability is BLOCKED. There is no browser defect finding or browser security certification from preparation alone. Fixture preparation/managed socket checks are separate local evidence, not a browser PASS. Preserve existing default-off transport/local-account/same-identity trust limitations and all M11/M7 controls. Original full-wizard/settings/legacy-read-contract gaps remain unmet requirements and are not waived by this checklist; even future successful supported browser flows cannot silently close those scope gaps.

**Exact next safe step:** Authorized operator runs the disposable --manual command above, opens the exact management URL in an already-open approved browser, completes T01–T05 first and supplies sanitized receipts keyed to these IDs. Then correlate each receipt with fixture descriptor, role, timestamp, native safe audit/HTTP evidence and actual source behavior; split partial cases, investigate reproducible M8 defects without weakening controls and rerun npm.cmd run verify after any source fix. Use the separate reviewed-sample fixture only for explicit approved deployment cases. Keep production private mode off, M8 IN PROGRESS and all later milestones untouched until every mandatory browser/security and remaining M8 criterion is genuinely verified.

## Active M8 real-browser acceptance preparation — 2026-10-28

**Current checkpoint:** Read the attached request at attachments/36c355cd-4697-4ebb-ae3d-1474b9748eb0/Pasted text.txt, the four official records, local BROWSER_AUTHENTICATION.md and actual Gateway/client/M11/test fixture code. No permitted browser-control tool/session is exposed. Browser execution is therefore BLOCKED by capability availability, and every browser acceptance scenario is NOT RUN. No browser/driver executable was launched, no browser protection was disabled and no browser evidence was supplied by an operator.

Added local tools/browser-acceptance.mjs to prepare an owned disposable Registry/accounts/apps/packages without reading configured storage or inheriting IHP settings. Initial self-check failed because the existing managed socket inspector requires separate port arguments, not a comma-separated string. Corrected the invocation; the next self-check exited 0, verified literal loopback sockets on Gateway 5050/API 59507/Dashboard 59508, seeded test data, revoked native setup sessions and removed storage/released ports. Manual mode with the separate reviewed-sample execution opt-in also reached ready on 5050/64124/64125; Ctrl+C printed successful storage/port cleanup, with outer interrupted PowerShell/PTY exit 1. No package was uploaded/approved/executed in these preparation runs. Test passwords/capabilities were random memory-only values and were not logged or persisted. A fresh operator run will require a masked, test-only password prompt and new fixture data.

The complete npm.cmd run verify command is running because fixture tooling changed. The last completed full gate remains 520 PASS/0 fail/0 skip plus smoke from commit 7ce9a20 until this run finishes. A detailed operator acceptance matrix/setup/evidence protocol is being prepared. M8/M7 remain IN PROGRESS; M10-DB/M9/M10/M12 remain NOT STARTED. No application feature/API/schema/migration/security/default browser policy changed; only existing reviewed migrations ran against fresh owned test databases. Source/tooling and local runbooks will not be pushed; only official progress records may be committed.

## M8 supported-functionality handoff — 2026-10-28 (Asia/Bangkok)

**Authoritative current snapshot:** M8 remains **IN PROGRESS**. This assignment completed the remaining Dashboard workflows supported by existing authorized APIs and verified them with real local services plus explicit DOM simulation. `npm.cmd run verify` exited **0: 520 passing checks, zero failures/skips, plus complete local stack smoke**. This supersedes the 497-check baseline as current full-gate evidence. Actual-browser security/transport/visual/accessibility acceptance is still **NOT RUN**; the candidate remains opt-in/default disabled for private production browser operations. No later milestone was started. Earlier snapshots/checkpoints below are preserved historical evidence, not current completion claims.

**Unchanged milestone statuses:** M1–M6/M11 COMPLETED; M7/M8 IN PROGRESS; M9/M10/M10-DB/M12 NOT STARTED. M7 source builds remain disabled because approved execution isolation is unavailable. Only exact-fingerprint, separately Administrator-reviewed trusted prebuilt packages may use the existing independent operator opt-ins. Upload/preview never automatically prepares, approves or deploys code. Managed Application Database requirements remain intact; no database provisioning/backup/restore/schema/service work was performed.

### Completed local functionality and decisions

- Reviewed all four official records and compared M8 acceptance against Dashboard/Gateway/native M11 implementation before editing source. The pre-code gap matrix below records the original missing screens and unsupported backend contracts.
- Application registration now validates supported name/description/slug/runtime values, displays authenticated ownership and derived Node/ASP.NET framework, and rejects reserved/unsafe slugs before review. The API assigns ownership. Runtime/framework/slug remain immutable after registration; changing ownership is a separate Administrator operation.
- Owners/Administrators can review and confirm name/description changes through the existing application PATCH. Viewer/read-granted Developer access is read-only. A rename warns about immutable package metadata compatibility; existing releases are never rewritten.
- A prebuilt deployment guide validates version/build/relative entry point and generates an **unsaved** platform.json preview. It fixes the loopback/automatic-port/prefix/health contract and explains reviewed Node ESM or framework-dependent ASP.NET Core 8 DLL output. Actual Node and ASP.NET preview ZIPs passed the existing inspector and remained PENDING_APPROVAL; unapproved preparation was rejected. Preview is neither a trust decision nor a persisted deployment configuration/build.
- Account view displays real safe user/role/active-state/session metadata and confirms revoke-all/sign-out. Administrator views support real user search/pagination/create/role changes/activation/session revocation. New-account passwords are cleared from the form on submit, never returned/logged/stored. Password reset/bootstrap/recovery remain native-only. M11 last-active-Administrator protection is preserved and verified.
- Administrator application access view confirms ownership transfer, read grants and explicit grant revocation. Developers still manage only owned apps; read grants do not authorize writes or deployment. No current-grants read endpoint exists: the UI explicitly labels this gap and reports mutation outcomes without inventing a current-grant list. Owner transfer uses current M11 eligibility and revokes affected approvals.
- Added explicit signed Gateway mappings for existing user/owner/grant/own-session APIs. Account-management JSON accepts only the operation's allowlisted fields and rejects duplicate/extra fields; password changes cannot pass through the role/activation mapping. All current roles/ownership/active-user/last-Administrator checks remain native M11 decisions. There is no generic management proxy, service-account bypass or native-token input in the browser.
- Added safe validation/permission messages, loading/empty/error handling, identity-scoped results, semantic form labels, responsive configuration preview and explicit confirmation dialogs. Pure form validation does not refresh records. User actions wait for current list data; pending results cannot restore a forgotten private view. Own revocation removes Gateway/client authority and clears authenticated state.
- Package details now distinguish actual registry status, stored package/deployment workflow state, queued action and unavailable live runtime probes. Existing package history/release metadata/hash/events/routes/audits remain actual API data. Environment/visibility/global settings, general legacy runtime/release read contracts, atomic restart, source builds, M9 metrics, M10 rollback and M10-DB lifecycle controls are explicitly unsupported/planned rather than fabricated.

### Security and operating boundary

The original M11 native Origin/Fetch Metadata/Host/session/RBAC/ownership checks and API/schema/migrations are unchanged. The existing signed candidate still separates management at http://localhost:5050/admin/ from applications at http://127.0.0.1:5050/<prefix> on the fixed 127.0.0.1:5050 Gateway listener. Keys/session proof stay in page memory; native sessions remain server-held. No cookies, localStorage/sessionStorage credentials, CORS workaround, unrestricted proxy or app-route credential exposure was introduced. New private operations use the same Host/Origin/metadata/CSRF/signature/nonce/expiry enforcement as existing operations and fail closed when unavailable or unauthorized.

Localhost HTTP is not TLS or an OS sandbox. Trusted local OS/account/origin assumptions, proxy/IPv4 resolution, browser headers/WebCrypto/service-worker/CSP behavior and visual/accessibility acceptance require permitted actual-browser evidence. No permitted browser tool/session is connected; no browser executable/driver was launched or installed. Automated Node HTTP/WebCrypto and jsdom results are supporting evidence only. The strict browser factory/default-off feature policy was not changed or enabled against configured user data.

Only SQLite performs real operations; SQL Server/PostgreSQL/MySQL remain metadata-only under the accepted policy gap. Tests used fresh owned registries and existing reviewed explicit migrations; no configured user/application database or deployed application was altered. No migration/model/dependency/security-policy/firewall/ACL/network-listening change occurred. All test listeners were literal 127.0.0.1; Gateway stayed at 5050. .NET publication used UseAppHost=false and published DLL execution via dotnet, never apphost/testhost/VSTest/browser executables. Twelve publish roots passed no-.exe scans. Existing trusted M7 sample execution policy, fingerprint/distinct-review checks and source-build prohibition remain unchanged.

The historical :5266 ECONNREFUSED root cause remains unconfirmed: its old log lacked API phase/output evidence and unchanged reproductions previously passed. This assignment's full real API/startup/readiness regression passed; that does not establish a previously unrecorded cause. Existing bounded identity/SQLite readiness, actionable safe diagnostics and owned cleanup remain intact; no arbitrary delay or security change was added.

### Local files created or modified in this assignment

| Scope                           | Files                                                                                                                                                                                                                                 |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Created Dashboard modules       | src/Dashboard/src/configuration.ts; src/Dashboard/src/management.tsx; src/Dashboard/src/components/use-resource.ts                                                                                                                    |
| Dashboard/client/style modified | src/Dashboard/src/api.ts; src/Dashboard/src/browser-session.ts; src/Dashboard/src/app.tsx; src/Dashboard/src/style.css                                                                                                                |
| Gateway boundary modified       | src/Gateway/BrowserBoundary.cs                                                                                                                                                                                                        |
| Verification/tooling modified   | src/Dashboard/test/dashboard.test.mjs; src/Dashboard/test/browser.test.mjs; src/Dashboard/test/native-harness.mjs; src/Dashboard/test/package-fixture.mjs; tools/tasks.mjs (DemoDotnet prerequisite for focused browser verification) |
| Local operating documentation   | DASHBOARD.md; BROWSER_AUTHENTICATION.md                                                                                                                                                                                               |
| Official progress records       | IMPLEMENTATION_PLAN.md; PROGRESS.md                                                                                                                                                                                                   |

AGENTS.md and PROJECT_REQUIREMENTS.md require no clarification and remain unchanged in this assignment. No API/Agent/sample/deployed-application source, Prisma schema, migration, dependency lock or configured data was modified. Source/tests/extra documentation/logs/build artifacts/databases remain local and must not be pushed to the progress repository.

### Commands, actual results and failure corrections

| Command or iteration                                                                            | Actual result                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ----------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| npm.cmd run build --workspace @ihp/dashboard                                                    | Exit 0, TypeScript/Vite production build                                                                                                                                                                                                                                                                                                                                                                                                           |
| npm.cmd run test --workspace @ihp/dashboard                                                     | Initial expanded run 37/40: inherited fieldset-disabled assertion and account-list refresh timing caused three failures (including the dependent ownership case). Corrected assertions, awaited real refreshed data and prevented duplicate React roots. Intermediate 40/40 passed with act warnings; wrapped real API state changes in React act and removed key-spread warnings. Final expanded run **42/42**, exit 0, no React act/key warnings |
| npm.cmd run test:browser                                                                        | Exit 0, **44/44** real Gateway/API/SQLite/Agent signed-protocol checks, including nine new management/configuration checks; DemoDotnet published as a prerequisite                                                                                                                                                                                                                                                                                 |
| npm.cmd run test:gateway                                                                        | Exit 0, **113/113** published managed checks                                                                                                                                                                                                                                                                                                                                                                                                       |
| dotnet format InternalHostingPlatform.sln --no-restore --include src/Gateway/BrowserBoundary.cs | Exit 0                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| npm.cmd run verify                                                                              | Exit **0**, **520 passing / 0 failing / 0 skipped** plus successful complete local smoke; all workspace build/format/type/solution checks and twelve no-.exe publish scans passed                                                                                                                                                                                                                                                                  |

Read-only four-document consistency checks exited 0: all 13 milestone statuses unchanged, complete previous progress retained, requirements/AGENTS unchanged (including Managed Application Database requirements), M8 default-off/actual-browser gap and 520-check evidence consistent. The initial helper's multiline section parsing was corrected before this pass; it did not change project files or run applications. Targeted Prettier --check for the four official records and both updated local operating docs passed; git diff --check exited 0. Final formatting/whitespace and staged-path checks remain mandatory before commit.

The 520-check breakdown is: Node foundation/readiness **33**, API/auth/startup **84**, guarded SQLite **13**, Dashboard DOM/real-native-client **42**, signed browser-protocol/WebCrypto/real-service **44**, managed Foundation **27**, Gateway **113**, Samples **82**, Deployment Agent **27**, routing integration **18**, packages **37**. This adds 14 DOM and nine signed-service checks to the previous 497-check full gate. Exact evidence: artifacts/m8-functionality-verify.log; focused logs artifacts/m8-functionality-dashboard-ready.log, artifacts/m8-functionality-browser-first.log and artifacts/m8-functionality-gateway.log. PowerShell Tee-Object logs use UTF-16LE; evidence extraction was corrected to decode that format rather than treating an empty match as test failure.

Full smoke used owned temporary API=60378 and Dashboard=60379 with Gateway=5050. It verified built /admin/ assets/CSP/public health/browser-management rejection, structured Gateway logs, actual loopback sockets, four real components ready, disabled deployments, owned process shutdown/port release and removal of temporary test storage. No existing listener/application was stopped or reconfigured.

### Remaining gaps, blockers and exact next task

1. **Release gate:** permitted actual-browser security/transport/profile/headers/isSecureContext/WebCrypto/CSP/isolation/session/role/approval/private-state/upload/download and viewport/keyboard/screen-reader acceptance is unavailable and NOT RUN. Keep private production browser operations disabled and M8 IN PROGRESS. Node/DOM success does not close this gate.
2. **Unsupported API contracts:** current grants cannot be listed; environment/visibility/global policy/deployment settings have no writer; general legacy runtime-health/release read contracts and atomic restart are absent. Supported views/actions and honest disabled states are complete for this assignment; fuller workflow requirements are not silently waived. Original source-template/build/online-update wizard acceptance is still unavailable, and M10 rollback is not implemented.
3. **M7 dependency:** source builds remain disabled until a separately approved execution isolation boundary exists. Keep M7 IN PROGRESS. M10-DB remains NOT STARTED; separate SQLite file/lifecycle work is not authorized by this Dashboard task.

**Exact next recommended task:** Continue M8 only by obtaining a permitted actual-browser session/tool and executing the NOT RUN checklist in local BROWSER_AUTHENTICATION.md against a disposable authorized loopback Registry. Confirm management-origin/transport assumptions first, then supported account/configuration/role/ownership/package/route/state-cleanup flows and real responsive/accessibility behavior. If no permitted browser is available, retain the release block/default-off behavior; do not launch a prohibited executable, invent evidence, add unsupported endpoints or start a later milestone. Reconcile any remaining full-wizard/backend contract requirements explicitly before M8 completion.

**Resume commands after reading the four records and inspecting the actual local state:**

```powershell
git status --short
npm.cmd run test:dashboard
npm.cmd run test:browser
npm.cmd run test:gateway
npm.cmd run verify
git diff --check
```

These automated commands do not provide actual-browser acceptance or authorize production opt-in. Push only the four approved Markdown paths to the existing progress origin/main; this assignment changes only IMPLEMENTATION_PLAN.md/PROGRESS.md within that allowlist. Verify committed paths and remote main before final handoff. No source, tests, database, credentials, artifacts, extra docs or logs may enter the commit.

## Active M8 functionality completion — 2026-10-28

**Expanded focused gate captured:** Dashboard DOM/real-native API checks now pass **42/42**, zero failures/skips and no React act/key warnings in artifacts/m8-functionality-dashboard-ready.log. This adds confirmed Administrator activation/reactivation and target-session revocation coverage to the prior 40 checks, plus invalid prebuilt-entry feedback. Signed real-service checks remain **44/44** and managed Gateway checks **113/113**, all exit 0. The full npm.cmd run verify command is running; no full-gate success is claimed at this checkpoint. Local DASHBOARD.md/BROWSER_AUTHENTICATION.md now describe the supported account/configuration operations and remaining unsupported contracts. AGENTS.md/PROJECT_REQUIREMENTS.md are unchanged because no rule/requirement clarification was needed.

**Focused evidence checkpoint:** Expanded Dashboard suite passed 40/40 after correcting inherited-disabled assertions, waiting for real user-list refresh and avoiding duplicate React roots. New signed Gateway/API/SQLite suite passed **44/44**, including user search/create/change/revocation, owner eligibility/transfer, read-grant/revocation, forbidden password fields, immutable application metadata, native/Origin/Host/CSRF rejection and actual Node/.NET preview ZIP inspection without approval or auto-deployment. User/grant-list state remains honestly unavailable where there is no read endpoint. React test synchronization was tightened; final focused UI run and full regression still require final result capture. No actual-browser verification or production browser enablement occurred.

**Implementation checkpoint:** Added typed disabled/signed client methods, explicit Gateway mappings for existing M11 user/owner/grant/self-session operations with narrowed management bodies, profile/session views, Administrator account/role/activation/revocation and ownership/read-grant controls, application metadata editing and a validated unsaved prebuilt manifest guide. Backend APIs/schema/migrations/auth guards and default-off browser factory are unchanged. Initial Dashboard build passed; expanded DOM/real-native API suite passed 37/40, retaining all previous 28 checks. Correcting inherited fieldset-disabled assertions and waiting for the refreshed user row; removed a redundant pure-validation refresh and React key-spread warnings. Gateway published successfully with UseAppHost=false/no executable artifacts. These new features are not marked verified yet; signed-wire and full gates remain pending.

**Pre-code requirement/implementation review:** Latest user assigns remaining M8 UI only; baseline c939e99542456abe2613e31c0a569ce21be2983c and 497 verified checks. Read the four official records and inspected the Dashboard, signed Gateway boundary and existing M11 controllers/authorization. No application code has been modified at this checkpoint. M7/M8 remain IN PROGRESS; M9/M10/M10-DB/M12 remain NOT STARTED. No permitted actual-browser tool is connected; production private browser mode must stay default disabled.

| M8 gap identified before code changes | Existing capability and safe implementation scope                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Creation/configuration workflow       | Two-step registry creation already works, with server-assigned owner and Node/.NET selection. Add supported configuration guidance and validated prebuilt manifest preview, owner/permission display and name/description editing through existing application PATCH. Runtime/framework/slug are immutable after registration; do not invent persistence for unsupported environment/visibility/settings fields.        |
| User/access screens                   | Current policy page only describes roles. Existing M11 supports Administrator user list/create/change/revoke-sessions and owner/grant/revoke mutations. Add authorized views/actions with confirmations and server enforcement. There is **no GET grants endpoint**; do not fabricate a current-grant list or infer current authority from audit history. Grant/revoke outcomes must be labeled as action results only. |
| Deployment configuration/policy       | Package metadata is immutable and validated by M7. Provide guidance/preview only; changes must enter a new reviewed package, never rewrite approved metadata or run a build. Platform settings/commands/environment/visibility have no supported editor API and must be visibly unavailable.                                                                                                                            |
| Release/state clarity                 | Existing package states/history/hash/events/routes are real API data. Separate registry status, package/deployment workflow state, queued action and unavailable live runtime probes. Preserve unsupported atomic restart/M10 rollback/M9 metrics labels.                                                                                                                                                               |
| UX/security                           | Extend accessible forms, real validation/errors, loading/empty states, explicit confirmations and role-aware UI; preserve signed-session/Host/Origin/CSRF/RBAC/ownership enforcement, state clearing and default-off browser factory. Test real native/signed API behavior and explicit DOM simulation, never claim actual-browser acceptance.                                                                          |

Implementation and new verification are pending. No backend/schema/migration or M10-DB implementation is authorized or needed for this scope. Only the official four Markdown paths may be pushed; AGENTS.md/PROJECT_REQUIREMENTS.md need no changes unless a permanent rule/requirement actually needs clarification.

## M8 continuation handoff — 2026-10-28 (Asia/Bangkok)

**Current milestone: M8 — React Dashboard, IN PROGRESS.** The latest user explicitly resumed M8 implementation after the documentation-only task. The complete project verification now exits 0 with **497 passing checks, zero failures/skips, plus real local stack smoke**. Actual-browser acceptance is unavailable; private production browser operations remain default disabled. No later milestone was started.

**Unchanged statuses:** M1–M6/M11 COMPLETED; M7 IN PROGRESS; M9/M10/M10-DB/M12 NOT STARTED. M7 trusted-prebuilt workflows remain verified, but approved source-build isolation/templates/build evidence remain unavailable and source builds stay disabled. Managed Application Database architecture/requirements are preserved; provisioning/backup/restore/service implementation remains NOT STARTED.

### Last completed work and implemented features

- API startup now records safe configuration/module_load/initialization/listen/ready phases and elapsed times; failures expose fixed phase/codes, not raw errors, secrets or paths.
- LocalStack verifies actual API JSON identity and a real SQLite health query before launching dependent Agent/Gateway/Dashboard components. Probes reject redirects, wrong-service HTTP 200 and oversized/malformed/stalled bodies, have bounded deadlines and preserve the last concrete failure. Each dependency phase is bounded to 45 seconds with actual health polling, not a fixed startup sleep. Unexpected owned exits are reported immediately with safe PID/phase/exit state; startup failure cleans owned processes only.
- BrowserSessionApi binds queued operations, including archive downloads, to the identity that scheduled them. An old intent cannot execute under a later login. Identity/expiry is checked before dispatch and after response; expired/forgotten authority cannot be restored by late data. Successful logout notifies private-state subscribers; uncertain response/nonce state fails closed without retrying mutations.
- Visible authenticated views revalidate on focus/visibility changes and every 30 seconds. Failed identity/session checks clear private state; this is bounded authentication freshness, not instant revocation push or M9 monitoring.
- Confirmed deployment stop and route removal now have explicit dialogs with inert background, Cancel-first focus, keyboard containment/Escape and focus restoration. Backend authority remains mandatory. Atomic restart is unavailable and labeled honestly; existing stop -> READY -> separately authorized deployment can restart the same exact approved release.
- Real signed tests verify application creation/list/detail/ownership, Administrator/Developer/Viewer enforcement, ZIP inspection/pending state, exact approval/rejection/history, trusted preparation/start/stop/redeploy, live authorized route remove/publish, audit access, request tampering/replay/Host/Origin/CSRF rejection, session revocation/expiry and private-state cleanup. DOM coverage remains simulation.
- The smoke fixture uses its own validated temporary API/Dashboard ports/configuration and restores environment/storage. Gateway stays fixed at 127.0.0.1:5050; existing development listeners are never stopped, moved or reconfigured.

### Investigation of historical API ECONNREFUSED

**Observed cause of the refused connection:** no API listener was available on 127.0.0.1:5266 at the historical readiness probe. The old log has no API output or phase evidence, so the precise underlying reason for not reaching listen is **unconfirmed**. Do not claim the old failure is conclusively diagnosed or permanently resolved.

Unchanged reproduction attempts passed: focused browser 28/28 and full Node sequence 175 checks. First-test/setup durations were about 31 s and 7.6 s, including migration/bootstrap, not isolated API startup measurements. Import latency, startup ordering, scheduling and other environmental explanations remain possibilities, not established root causes.

Current startup/readiness behavior and controlled failures are verified: real initialized API/SQLite readiness, missing Registry failure at initialization, bind collision at listen, refused/timed-out probes, hanging bodies, child exit and cleanup. The final full run recorded API phases configuration=0, module_load=8, initialization=2565, listen=3017, ready=3053 ms after its owned restart. New diagnostics allow a future recurrence to identify the failed phase without exposing sensitive data. No firewall/security change or arbitrary startup delay was used.

### Security design and operating boundary

The existing opt-in candidate isolates http://localhost:5050/admin/ management from hosted application content at http://127.0.0.1:5050/<prefix> on the same fixed IPv4 listener. It uses page-memory nonextractable P-256 keys, single-use login challenges/request nonces and signatures over exact method/target/body. Native M11 tokens/bindings remain in Gateway memory. There are no authentication cookies or localStorage/sessionStorage tokens, no unrestricted API proxy and no browser-supplied native credentials. Only explicit operation mappings construct native M11 requests. Original native Origin/Fetch Metadata/session/RBAC/ownership guards remain unchanged; the fixed shorter native browser issuer and Agent authorization continue to enforce expiry/revocation.

Localhost HTTP is not TLS or OS identity isolation. The design assumes a trusted local OS/account/platform origin and cannot attest browser proxy/resolution/profile/service-worker/CSP behavior from Node tests. No permitted actual-browser tool/session is connected, and no browser executable/driver was launched or installed. Keep the candidate default off for private production use pending permitted actual-browser and security acceptance. The local BROWSER_AUTHENTICATION.md checklist is explicitly NOT RUN, not evidence of approval.

Only SQLite performs real database operations. SQL Server/PostgreSQL/MySQL remain metadata-only with the accepted policy gap; no connection/authentication/migration/provisioning/connectivity test was attempted. No schema/migration was added: guarded generation remains 20 models and four reviewed migrations. Tests used only fresh owned registries and the existing explicit migration runner; actual configured Registry/users/applications were not migrated or bootstrapped.

All real listeners/fixtures are literal 127.0.0.1, Gateway 5050. .NET inputs are framework-dependent UseAppHost=false publishes run via dotnet <published DLL>; twelve publish roots contain zero .exe artifacts. No apphost/testhost/VSTest, browser executable/driver, Docker, elevation, Windows policy/firewall/ACL change, corporate listener/deployment or existing application modification occurred. Installed node/dotnet/npm.cmd/git hosts follow the established project convention. Only reviewed trusted sample packages executed under existing independent API/Agent opt-ins; uploads never auto-deploy, approvals/fingerprints are mandatory and source/custom commands remain disabled.

### Files created or modified in this continuation

| Scope                           | Local files                                                                                                                                                                                                                                      |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Created                         | tools/readiness.mjs; tests/foundation/readiness.test.mjs; tests/foundation/fixtures/startup.mjs; src/Api/test/startup.test.mjs; src/Dashboard/src/components/confirm-action.tsx                                                                  |
| API/startup tooling modified    | src/Api/src/main.ts; src/Api/package.json; tools/local.mjs; tools/tasks.mjs; tools/smoke.mjs                                                                                                                                                     |
| Dashboard/client/tests modified | src/Dashboard/src/browser-session.ts; src/Dashboard/src/app.tsx; src/Dashboard/src/style.css; src/Dashboard/test/browser.test.mjs; src/Dashboard/test/dashboard.test.mjs                                                                         |
| Documentation modified          | BROWSER_AUTHENTICATION.md locally; PROJECT_REQUIREMENTS.md current-assignment clarification only (section 9.1 unchanged); AGENTS.md continuation/permanent rules; IMPLEMENTATION_PLAN.md M8 evidence/status; PROGRESS.md current handoff/history |

No Gateway/Agent/sample source, Prisma schema, migration, dependency lock or deployed existing application was changed in this continuation. Earlier local browser-candidate source remains present; this session hardened/integrated/verified it rather than claiming it was newly created. Source/tests/extra docs/logs/artifacts/data stay local and must not enter the progress repository.

### Actual commands and failure history

| Command/iteration                                                             | Actual result                                                                                                                                                                     |
| ----------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Unchanged npm.cmd run test:browser                                            | Exit 0, 28/28; historical refusal not reproduced                                                                                                                                  |
| Unchanged npm.cmd run test:node                                               | Exit 0, 175 checks: 25 foundation/sample + 81 API + 13 SQLite + 28 DOM + 28 browser protocol                                                                                      |
| npm.cmd run build --workspace @ihp/api                                        | Exit 0; pinned guarded WASM generation, 20 models, no native Prisma engine                                                                                                        |
| First node --test tests/foundation/readiness.test.mjs                         | 7/8; Windows unavailable-listener probe timed out before refusal; assertion corrected to report either observed bounded transport failure                                         |
| node --test tests/foundation/readiness.test.mjs src/Api/test/startup.test.mjs | Initially 9/11 because the final deadline hid a prior specific error; helper now retains it. Final focused run exit 0, 11/11                                                      |
| Expanded root npm.cmd run test:browser                                        | 34/35: an expiry fixture's direct SQLite write hit SQLITE_BUSY; added production-equivalent bounded busy timeout to that owned fixture only                                       |
| Next expanded browser run                                                     | 34/35: focus-revocation fixture referenced an undeclared dom variable; corrected its destructuring. No production authorization was relaxed                                       |
| npm.cmd run test:browser --workspace @ihp/dashboard                           | Final focused exit 0, 35/35, no skips                                                                                                                                             |
| npm.cmd run test --workspace @ihp/dashboard                                   | Exit 0, 28/28; real native-client/SQLite integration and simulated React DOM, not browser acceptance                                                                              |
| First complete npm.cmd run verify                                             | All 497 automated checks passed, but command EXIT 1 at smoke: existing loopback port 5173 occupied. This attempt is a failure, not a successful full gate                         |
| npm.cmd run smoke after disposable-port fix                                   | Exit 0 on API 49191/Dashboard 49192, Gateway 5050; real assets/CSP/health/Origin rejection, socket inspection, four clean exits, released owned ports and removed fixture storage |
| Final npm.cmd run verify                                                      | **EXIT 0: 497 passing checks, zero failures/skips, plus complete real stack smoke on API 55084/Dashboard 55085/Gateway 5050**                                                     |
| npm.cmd run doctor                                                            | Exit 0: Windows 22631 x64, Node 26.12.1, npm 11.20.0, .NET SDK 8.0.425, .NET/ASP.NET runtime 8.0.31                                                                               |
| npm.cmd audit --json                                                          | Exit 0, zero vulnerabilities; no dependency installation or policy hook changes                                                                                                   |
| Principal/publish inspection                                                  | Administrator=False; 12 publish roots and zero .exe artifacts                                                                                                                     |
| Read-only port-owner inspection                                               | Existing 127.0.0.1:5173/node PID 14848 remained running after focused smoke; never killed or reconfigured                                                                         |
| Git baseline check                                                            | Branch main, exact approved origin; HEAD/origin/main both ad5cb3b309d5ca5507572c275c8a620cb8002cc7 before delivery                                                                |

Local evidence logs: artifacts/m8-resume-browser-baseline.log, m8-resume-node-baseline.log, m8-readiness-first.log, m8-startup.log, m8-startup-fixed.log, m8-resume-api-build.log, m8-resume-browser.log, m8-resume-browser-fixed.log, m8-resume-browser-final.log, m8-resume-dashboard.log, m8-resume-verify.log, m8-resume-smoke.log and m8-resume-verify-final.log. Corrected patch-context failures occurred before file mutation; previous attempts/results are retained accurately. The final command propagated LASTEXITCODE through Tee-Object.

### Final full-suite results

| Suite                                                            |  Passed | Failed/skipped |
| ---------------------------------------------------------------- | ------: | -------------: |
| Node foundation/lifecycle/sample configuration/readiness         |      33 |              0 |
| API reference/policy/DPAPI/routing/auth/real startup             |      84 |              0 |
| Guarded Prisma/SQLite/migration                                  |      13 |              0 |
| React DOM/model/owned-native-client Dashboard                    |      28 |              0 |
| Browser protocol/WebCrypto/real services plus explicit DOM cases |      35 |              0 |
| Managed Foundation                                               |      27 |              0 |
| Managed Gateway                                                  |     113 |              0 |
| Managed Samples                                                  |      82 |              0 |
| Managed Deployment Agent                                         |      27 |              0 |
| Managed routing/auth integration                                 |      18 |              0 |
| Managed packages                                                 |      37 |              0 |
| **Total**                                                        | **497** |          **0** |

The total is historical 451 plus 35 browser-protocol checks and 11 new startup/readiness checks. Smoke and documentation checks are separate from that assertion total. Build/Prettier/type/solution checks and all twelve publish scans passed in the final gate. Existing M1–M7/M11 behavior, WebSockets, live routing and trusted Node/.NET package execution regressions passed.

### Remaining blockers, limitations and exact next task

1. **M8 actual-browser/security acceptance remains unmet.** No permitted browser tool/session is available under the no-executable/driver policy. Actual secure context/P-256, browser headers, origin/proxy/profile/service-worker/CSP behavior, user workflows and real viewport/assistive-technology checks were NOT RUN. Keep private production operations default disabled and M8 IN PROGRESS.
2. **Historical :5266 cause remains an evidence gap.** Current real startup/readiness and negative cases pass; the exact old pre-listen failure was not reproduced. If it recurs, retain the new safe phase/PID/exit/probe diagnostics; do not label speculation a confirmed cause or alter IT controls.
3. **Original M8 page/API scope remains.** Existing two-step registration plus package/review/action views cover current supported workflows. The original full unified wizard, user/grant/configuration/policy editors and unavailable legacy release/runtime read contracts remain incomplete/disabled; no endpoint or monitoring data is fabricated. Atomic restart is unsupported; no M9 metrics or M10 rollback was added.
4. **M7 stays IN PROGRESS.** Approved source-build isolation and build evidence remain unavailable; no uploaded scripts, installs or custom builds run. Prebuilt regression success does not waive that acceptance requirement.
5. **Future database work stays NOT STARTED.** M10-DB requirements/target storage are unchanged; no managed application database provisioning, backup, restore or Dashboard service is enabled. Same-user files/processes are not a sandbox.
6. **Trust/operational limits persist.** Plain loopback HTTP, same-user process/storage access, Gateway polling/last-known-good staleness, bounded recovery and logout/reboot startup dependencies remain as previously documented. Response loss clears local authority without claiming confirmed server revocation; fixed server expiry/revocation still applies.

**Smallest safe next step:** Obtain permitted actual-browser evidence in a disposable authorized local setup using the NOT RUN checklist in BROWSER_AUTHENTICATION.md and review the localhost transport/profile assumptions. Never launch a prohibited executable/driver or change security/network settings to do so. Then finish any remaining M8 page/API scope under explicit M8 authorization, preserving source-build/default-off gates. If startup refuses again, capture the new phase diagnostics before changing code. Do not start M9/M10/M10-DB/M12 or change M7 status.

### Commands to resume and progress-only delivery

Read the latest snapshot and inspect actual local source before relying on historical notes. These verification tasks use owned fixtures and do not authorize migrating actual configured user data.

```powershell
Get-Content -LiteralPath AGENTS.md -Encoding UTF8
Get-Content -LiteralPath PROGRESS.md -Encoding UTF8 -TotalCount 175
Get-Content -LiteralPath IMPLEMENTATION_PLAN.md -Encoding UTF8
Get-Content -LiteralPath PROJECT_REQUIREMENTS.md -Encoding UTF8
Get-Content -LiteralPath BROWSER_AUTHENTICATION.md -Encoding UTF8
git status --short
npm.cmd run test:browser
npm.cmd run verify
```

Only PROJECT_REQUIREMENTS.md, AGENTS.md, IMPLEMENTATION_PLAN.md and PROGRESS.md may be committed/pushed to https://github.com/logtranwanek-spec/IAH-Progress-Tracking-.git, branch main. Source code/tests/data/credentials/configuration/build artifacts/logs/additional docs remain local; a progress-only clone cannot reconstruct the application. Final documentation consistency/format/whitespace and staged-path checks run before commit; compare remote main with HEAD after push. The final handoff reports that verified commit identity.

## Preserved earlier progress and session checkpoints

Final documentation checks passed: **29/29** consistency assertions verified all 13 milestone statuses, unchanged Managed Application Database requirements, preserved prior content, balanced Markdown fences and accurate results/limitations/delivery scope. The checker was adjusted to normalize Markdown table separator widths introduced by Prettier; no historical requirement/progress content was removed. Targeted Prettier and git diff --check passed. Staged/committed path checks and remote-main identity verification accompany the progress-only delivery.

All earlier records and intermediate M8 checkpoint/failure notes follow as history. Their earlier active assignments/pending states do not override the current handoff. Prior table alignment was normalized by the project formatter; historical content was not removed.

````````````````text

# Project Progress

## Active M8 continuation — 2026-10-28

**First complete verify attempt:** Build/format/type/solution checks and **497 automated checks** passed, but `npm.cmd run verify` exited 1 at final smoke because an existing loopback listener occupied 5173. Its owner was inspected read-only and was not stopped or modified. Changed only the disposable smoke fixture to select validated temporary API/Dashboard ports via an owned configuration file; Gateway remains fixed 127.0.0.1:5050. Fixture environment/configuration/storage are restored/cleaned and existing application ports remain untouched. Smoke and complete verify must be rerun; the failed command is not reported as success. Earlier 31 s/7.6 s figures are first-test/setup durations including migration/bootstrap, not isolated API timings or proof of the historical root cause.

**Focused verification checkpoint:** Corrected an undeclared DOM-fixture variable in the focus-revocation case; no production security restriction changed. Final focused browser protocol/real-service suite passed **35/35**, and React DOM/native-client suite passed **28/28**, both exit 0/no skips. API startup phases after restart were configuration=0, module_load=4, initialization=1414, listen=1615, ready=1637 ms in that run. Startup/negative/cleanup checks passed 11/11. All testing is real owned loopback services plus Node WebCrypto and explicitly simulated DOM, not actual-browser acceptance. Full project verification is next; do not infer its result from focused checks.

**Expanded browser checkpoint:** First expanded real-service run passed 34/35 checks, including signed role/ownership/approval/route controls, confirmed real stop, approved stop/start, queue identity isolation, response-loss fail-closed behavior and late-response expiry. One existing expiry fixture's direct SQLite write failed with SQLITE_BUSY while the Agent used its connection. Added the same bounded busy timeout used by production SQLite connections to that fixture only; no sleep, policy bypass or migration. Added active-session revalidation on window focus/visibility and every 30 seconds while visible so revocation does not require a manual data refresh; private state clears on a failed identity check. Final reruns remain pending; no real-browser claim.

**Implementation checkpoint:** New readiness/real API entry-point suite passed 11/11 after correcting diagnostics to retain the last concrete failure when a final deadline probe times out. Missing Registry fails in initialization, actual listener collision fails at listen, owned processes are cleaned, and an unrelated occupant survives. Added session-bound queued-intent checks and rejection of private responses arriving after client expiry; added inert-background/focus-contained cancellation/confirmation for stop and route removal, with atomic restart explicitly unavailable. Expanded real Gateway/API/Agent protocol and DOM-simulation tests are running. Only fresh owned test registries use the existing four reviewed migrations; no schema/migration or configured user data change. Private browser mode remains default off, and no actual-browser acceptance is claimed.

**Readiness investigation checkpoint:** Unchanged focused browser gate passed 28/28; unchanged full Node/API/Dashboard sequence passed 175 checks (25 foundation/sample + 81 API + 13 SQLite + 28 DOM + 28 protocol). Original :5266 refusal was not reproduced; observed first-stack readiness about 31 seconds versus 7.6 seconds on the subsequent run. Historical logs do not establish why the old API had not listened, so no exact historical root cause/resolution is claimed. Added API phase-only startup diagnostics, actual JSON identity/registry readiness, bounded streaming probes, API-before-dependent-components startup, immediate owned-exit diagnostics and automatic failure cleanup. First new readiness run passed 7/8: a 500-ms unavailable-listener fixture produced a legitimate probe timeout before Windows reported refusal; the assertion now accepts either observed bounded transport failure. New real-entry-point startup checks and final focused/full verification remain pending. No browser or configured data operation occurred.

The latest user explicitly resumes M8 implementation/testing, superseding the preceding documentation-only task. Read the four official records and inspected the existing readiness launcher, API entry point, real-service browser suite and restricted browser-session candidate. Baseline commit ad5cb3b; last successful historical full gate 451 checks, later full browser run failed all 28 browser checks after API :5266 readiness refused connections while API output was empty. No root cause/fix is yet claimed. Reproduce first, then add actionable bounded readiness diagnostics without arbitrary delays; retain loopback/no-.exe/SQLite-only/M11/M7 policies. No permitted browser tool is connected, so actual-browser acceptance is unavailable and must not be inferred from DOM/WebCrypto tests. M7/M8 remain IN PROGRESS and M10-DB NOT STARTED; no other milestone is assigned. Full verification and final evidence remain pending.

## Current task — Managed Application Database documentation only — 2026-10-27

**Authoritative handoff:** This documentation-only record supersedes the earlier active M8 implementation/resume instructions preserved below. No application code, schema, migration, configuration, database or existing application was changed by this task. No application build, test or execution was initiated or resumed for this task. Earlier local M8 implementation changes and artifacts were left untouched. Managed Application Database provisioning, initialization, backup, restore, lifecycle APIs and Dashboard controls are **not implemented or verified** by this update.

### Milestone status — unchanged

| Milestone                                                                | Status      |
| ------------------------------------------------------------------------ | ----------- |
| M1–M6                                                                    | COMPLETED   |
| M7 — ZIP Upload and Approved Source Builds                               | IN PROGRESS |
| M8 — React Dashboard                                                     | IN PROGRESS |
| M9 — Monitoring and Logging                                              | NOT STARTED |
| M10 — Version Management and Rollback                                    | NOT STARTED |
| M11 — Authentication and Authorization                                   | COMPLETED   |
| M12 — Security Hardening and Final Testing                               | NOT STARTED |
| M10-DB — Managed Application Database Service (new future sub-milestone) | NOT STARTED |

M7's trusted-prebuilt subset remains verified; approved source-build isolation/templates/build evidence remain unavailable and source builds stay disabled. M8's secure browser workflow/actual-browser acceptance and other recorded scope gaps remain unresolved; the browser-authentication candidate stays default disabled. No completion status or acceptance waiver was changed.

### Last completed task and requirements added

Defined **1 Platform Database + N Independent Application Databases**, with application databases optional and NONE, MANAGED_SQLITE and EXISTING_SQLITE modes. The Platform Registry remains dedicated management storage. Each managed database belongs to exactly one application and is never the Registry; registered existing SQLite files require explicit location/association authorization.

Only the four official Markdown records were edited for this task:

| File                    | Sections added or updated                                                                                                                                                                                                                                                                                    |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| PROJECT_REQUIREMENTS.md | Current documentation-only directive; section 9 policy precedence and new 9.1 Managed Application Databases (modes, isolation, target storage, lifecycle, release persistence, minimum configuration, future Dashboard, SQLite-consistent backup/restore); cross-references in sections 10, 14, 16.2 and 17. |
| IMPLEMENTATION_PLAN.md  | Current task override; cross-milestone database boundary; future M10-DB objective/tasks/dependencies/acceptance/verification/status after M10; requirement coverage and final extension-status note; reconciliation of the prior M8 failure from existing logs. Existing milestone IDs/statuses retained.    |
| AGENTS.md               | Current documentation-only scope and permanent database separation/storage/data-preservation/SQLite-only/authorization/backup/restore/migration/audit rules. Existing security instructions retained.                                                                                                        |
| PROGRESS.md             | This documentation-only architecture/change/verification/handoff record; complete earlier progress preserved below as history.                                                                                                                                                                               |

The target logical IHP_STORAGE_ROOT layout is registry/platform.db plus Applications/<application-uuid>/releases/, data/application.db, logs/ and config/, with shared Uploads/, Temporary/ and Logs/. It is a future target, not a statement that existing paths were moved. Implemented storage/configuration is preserved until a future explicitly authorized, compatible migration.

Added future lifecycle requirements for idempotent non-overwriting provisioning, association, separately authorized initialization, metadata/status/integrity, storage usage/limits, permissions, backup/restore/history, retention, explicit deletion, interrupted-operation recovery and safe audits. Application deletion must not automatically destroy data; deployment/update/restart/rollback/release cleanup must preserve the independent database. Uploaded SQL never runs automatically and application rollback does not reverse schema changes.

Future Dashboard database management must enforce M11 roles/current ownership, read-only Viewer access, explicit backup/restore/deletion authorization and destructive-action confirmation. Raw SQLite files, contents, internal paths, Registry configuration and secrets must not be exposed through public routes or ordinary browser endpoints.

### Architecture and policy decisions

- **Real database support:** SQLite only. SQL Server/PostgreSQL/MySQL remain metadata-only references. No real connection, authentication, migration, provisioning or connectivity testing is permitted; their verification gap remains accepted company policy.
- **Isolation:** Validate approved path containment/file identity, traversal, symlink/junction/reparse escapes, aliases and cross-application access at Platform operation boundaries. Same-Windows-identity filesystem separation is not a sandbox or OS isolation against malicious code; trusted-reviewed-code restrictions remain.
- **Backups/restores:** Require an approved SQLite-consistent in-process mechanism, not uncoordinated copying of an active .db file. Restore requires authorization/confirmation, backup integrity/provenance/compatibility, intended target identity, writer coordination/downtime, recovery planning and post-restore checks. Backups remain outside releases/public trees. These are requirements, not working features.
- **Future scope:** M10-DB is NOT STARTED, placed after M10 and before final M12 acceptance of this extension. It depends on existing M4 API/Registry and M11 authorization, M5/trusted M7 release contracts, verified secure M8 Dashboard capabilities and M10 data-preserving release coordination. It does not start M9/M10, resolve M7/M8 or change their status.
- **Deferred choices:** Concrete API/schema/migration contracts, approved backup-provider capability, backup layout, retention/limit defaults and recovery coordination require future implementation review. No new executable, database server, OS permission/security setting or network listener is authorized.

### Actual verification and remaining issues

**Documentation verification: PASS, 44/44 consistency checks.** An inline read-only review used System.IO.File.ReadAllText through PowerShell/ConvertTo-Json and JavaScript assertions over the four Markdown texts. It verified that every pre-task line remains in order, all Markdown fences are balanced, all 12 existing milestone statuses are unchanged, exactly one NOT STARTED M10-DB was added, and the modes/storage/lifecycle/backup/restore/authorization/SQLite-only/future-scope requirements agree across the records. These are documentation checks, not new application tests or additions to the historical 451-check total. The initial checker expression had a syntax error before any checks executed; it was corrected and the complete 44-check run passed.

`git diff --check` exited 0. Read-only git status/diff inspection lists only the four authorized tracked records; git ls-files confirms no source/data/artifacts are tracked. Application builds/tests are deliberately not run for this documentation-only task. Review also used existing repository/log evidence and SQLite's official Online Backup API/active-copy safety documentation linked in section 9.1.8. Staged-path/whitespace and final remote identity checks are part of the delivery commands below.

**Prior M8 verification correction (existing evidence only):** The last fully successful historical full gate remains 451 checks. Read-only inspection of the already-ended earlier browser-authentication full-run log, artifacts/m8-auth-verify.log, found exit/failure evidence: its 28 browser checks failed after owned-stack setup could not reach API 127.0.0.1:5266 (ECONNREFUSED/readiness timeout). The earlier focused browser-protocol run is recorded as 28/28 in the pre-task plan; neither it nor DOM simulation proves actual-browser acceptance or a successful new full regression. No failing application code/test was repaired or rerun during this documentation assignment. The earlier execution session was already closed; it was not resumed. Retain this failure as an unresolved M8 issue for a future authorized implementation task.

No Managed Application Database functionality is complete, tested or enabled. Exact quotas/retention/provider details are not invented, and the target layout does not imply relocation of implemented storage. Existing M7 source-build isolation and M8 browser verification/security gaps persist. A progress-only repository contains no runnable source; earlier local implementation remains outside this delivery.

### Exact next recommended task and resume commands

Review the new section 9.1 and M10-DB scope. Future development requires an explicit assignment; do not start implementing Managed Application Databases, resume M8 code work or start another milestone as a side effect of this documentation change. On an authorized future session, read these records, inspect actual local source/state and reconcile the retained M8 failure before claiming new verification.

```powershell
Get-Content -LiteralPath AGENTS.md -Encoding UTF8
Get-Content -LiteralPath PROGRESS.md -Encoding UTF8 -TotalCount 130
Get-Content -LiteralPath IMPLEMENTATION_PLAN.md -Encoding UTF8
Get-Content -LiteralPath PROJECT_REQUIREMENTS.md -Encoding UTF8
git status --short
git diff --check
```

Progress-only delivery remains origin/main at https://github.com/logtranwanek-spec/IAH-Progress-Tracking-.git. Pre-delivery branch/remote inspection and git fetch confirmed main and baseline bdb369bc30c25a7646e439350ddcba2e42fec4fa equal to origin/main. Verify staged/committed paths contain exactly the four official records; never stage source, other documents, data, credentials or artifacts. The final handoff reports the delivered commit and remote verification; Git metadata is the authoritative commit identity.

```powershell
git add PROJECT_REQUIREMENTS.md AGENTS.md IMPLEMENTATION_PLAN.md PROGRESS.md
git diff --cached --check
git diff --cached --name-only
git commit -m "Define managed application database architecture and future roadmap"
git push origin main
git ls-remote origin refs/heads/main
git rev-parse HEAD
```

## Preserved pre-task progress

The complete pre-task progress text follows unchanged. Earlier active assignments, pending-test statements and resume commands are historical; this documentation-only record is authoritative. The surrounding longer fence preserves all previously nested history.

```````````````text

## Active M8 browser-authentication follow-up — 2026-10-27

Core candidate implemented: opt-in localhost management-host boundary, single-use challenge/P256 request proofs/nonces, bounded explicit operation map, memory-only client, default-off discovery, shorter native M11 issuer and same M11 server authorization. First real Gateway/API protocol run: 13/20; failures exposed cleared nonce headers on API denials plus two test-fixture issues (missing Origin in method-tamper case; expiry violating SQLite's created/expiry constraint). Fixed without relaxing authorization. Rerun passed 20/20, including real trusted Node upload/approval/preparation/start/proxy/stop and DOM simulation through signed requests. Added archive/large-upload/source/issuer tests and scoped-session capacity; these and full regression are pending. No real-browser success is claimed and production opt-in remains off.

Latest user explicitly authorizes resolving browser authentication, superseding the previous no-bridge wait only for a reviewed restricted boundary. Read repository instructions/current progress/plan, authentication/dashboard/Gateway code and security requirements. Baseline bdb369b and 451 checks. Direct browser access is denied by unchanged M11 guard; shared /admin/application origin and cookie port/path scope require care. Design: optional localhost:5050 management origin on existing IPv4 listener; hosted apps only on 127.0.0.1 when enabled; no cookies/storage tokens, memory-only nonextractable signing key with single-use challenges/nonces; fixed allowlisted Gateway operations backed by existing M11 sessions/authorization. Loopback HTTP is not TLS/OS isolation. No approved real-browser tool is connected; never launch a browser exe or claim DOM acceptance. Implement/test first; feature defaults off, M8/M7 remain IN PROGRESS pending evidence. No M9/source builds/user-data or system-setting change.

**Updated:** 2026-10-27 (Asia/Bangkok)

## Current milestone and handoff

- **Current milestone:** M8 — React Dashboard, **IN PROGRESS**, explicitly assigned despite unfinished M7. No M9/other milestone started.
- **Previous milestones:** M1–M6 and M11 remain COMPLETED. **M7 remains IN PROGRESS**: its trusted-prebuilt subset is verified; source templates/build logs still require approved execution isolation and remain disabled.
- **Last completed work:** Responsive React dashboard/views, guarded typed API contract, real public health, optional built /admin/ Gateway hosting and security/DOM/native-client regression. Production browser authentication/private actions remain disabled.
- **Final verification:** `npm.cmd run verify` exited 0: **451 passing checks**, zero failures/skips, plus real local stack smoke. This is the previous 418 plus 28 dashboard checks and five Gateway checks. All twelve .NET publish roots passed no-.exe scans.
- **M8 completion blocker:** Unchanged M11 rejects browser Origin and Sec-Fetch-Site other than none. No approved browser authentication/session/transport/origin design exists. Native-client DOM test success is not browser login or authenticated browser deployment evidence.
- **Exact next task:** Resolve the reviewed M8 browser-session/CSRF/transport/management-origin design and safe actual-browser verification boundary before enabling private UI or declaring M8 complete. Keep existing guard, source-build prohibition and M7 status intact. Do not start M9.
- **Git scope:** Only PROJECT_REQUIREMENTS.md, AGENTS.md, IMPLEMENTATION_PLAN.md and PROGRESS.md may be committed/pushed to `https://github.com/logtranwanek-spec/IAH-Progress-Tracking-.git`, main. Source/tests/additional docs/SQLite/credentials/artifacts remain local/untracked. A progress-only clone cannot reconstruct/run the code.

## Policy and authorized scope

SQLite is the only allowed real database. SQL Server/PostgreSQL/MySQL remain metadata-only references with an accepted policy verification gap; no installation/connection/protocol test occurred. Every real listener remains 127.0.0.1, Gateway 5050 only. Published framework-dependent .NET DLLs use UseAppHost=false and run through dotnet; no apphost/.exe/testhost/VSTest, browser executable/driver, native Prisma engine, Docker, Administrator/security/ACL/firewall change, production access, corporate deployment/listening or existing application modification occurred. Installed node/dotnet/npm.cmd/git hosts follow the established project convention.

The user explicitly authorized M8 while preserving M7 IN PROGRESS. The earlier M7 wait is superseded only for M8. No source-build authorization/acceptance waiver is inferred. Public health/static hosting does not grant browser management or modify API authorization. Verification used fresh owned SQLite registries/users/packages/sample copies and cleaned owned processes/storage; actual configured data was not migrated or bootstrapped.

## Implemented features and actual evidence

| Area                          | Evidence and practical boundary                                                                                                                                                                                                                                                            |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Layout/navigation             | PASS in React DOM: sidebar/header/breadcrumb/hash pages, navigation and mobile toggle/Escape/focus; dark React/TypeScript/Tailwind/local shadcn-style Radix/CVA primitives/Lucide; responsive/reduced-motion CSS contracts. Real browser pixel/screen-reader acceptance remains unverified |
| Browser authentication policy | PASS: production login form disabled, no credential collection/token-paste/storage/cookies/override; all privileged BrowserApi methods reject before any fetch. Only two credential-free public health paths are used                                                                      |
| Authentication integration    | PASS using owned test-only native client against real M11: failed/successful login, current role/user, logout/revocation/401 expiration and clearing private records; late responses cannot repopulate private state. **Not approved browser authentication**                              |
| Application management        | PASS native-client DOM/API: scoped list/detail, ownership/read-only labels, server search/runtime filtering, empty states, current-page sorting/model contracts, two-step real owned registration; Viewer writes denied server-side and UI controls hidden                                 |
| Packages/deployments          | PASS native-client DOM/API: real trusted ZIP inspection/pending state, metadata/hash/history, source-build warning and preparation queued without claiming READY/RUNNING; existing M7 suite verifies actual approved Node/.NET execution/routing                                           |
| Approval management           | PASS native-client DOM/API: exact metadata/file hashes, distinct Administrator/trusted-code attestation gating, real approval/rejection/history and no auto-execution                                                                                                                      |
| Routing/audit                 | PASS native-client DOM/API: actual stored disabled intent/safe loopback link, confirmed removal, controlled rejection when no approved runtime; real Administrator audit events and Viewer denial. Stored intent is not reachability                                                       |
| Loading/errors/revocation     | PASS: safe health failure/malformed/oversized identity response, safe exceptions/no raw secret values, 401 expiration, revoked grants clearing detail, request-identity/stale-response handling                                                                                            |
| Gateway hosting               | PASS managed tests: opt-in /admin/ and allowlisted JS/CSS, GET/HEAD, redirect, CSP/no-store, forbidden publish files/source maps/methods and all other protected paths denied. No generic privileged API proxy                                                                             |
| Existing behavior             | PASS: final full gate retained all M1–M7/M11 checks, twelve published roots and real built /admin/ health/security/shutdown smoke. M7 remains IN PROGRESS; no acceptance waiver                                                                                                            |

Production has no demo application rows, fake runtime status or fabricated CPU/RAM/monitoring data. Unknown application totals display an em dash. Capability pages distinguish implemented, blocked, disabled and planned features. No M9 collector/log aggregation or M10 online upgrade/rollback is introduced.

## Architecture and security decisions

- **Blocked production client:** main.tsx constructs only BrowserApi. It permits same-origin GET /_platform/health and /_platform/api/system/health with credentials omitted, timeout/identity/size checks. Login and every private operation fail before network access. There is no browser feature flag, secret storage or native-token bridge.
- **Typed view contract:** DashboardApi describes safe users, applications, package metadata/events/hashes, approvals, route intent, audit pages and existing actions. It does not expose tokens/binding to React. A Node native harness in test/ privately owns temporary M11 credentials and calls the real secured API; it is not imported into the bundle or provided as a server/production adapter.
- **Permissions:** Administrators manage all apps/review others' packages; Developers manage owned apps; Viewers/read grants are read-only. UI permission checks never replace server enforcement. No ownership/user/policy escalation fields are submitted by ordinary registration.
- **State correctness:** 202 queue admission never becomes successful/healthy deployment. Actual package state/events drive views; API authorization is required for execution controls. Same-user/request-identity refresh retains current records while loading, preserving feedback; identity changes mask old data immediately and pending results cancel/ignore after navigation/logout. API 401 expires the session view; safe 403/404 deny without raw input/error rendering. Logout is reported only after revocation succeeds; failures do not pretend revocation.
- **Review:** Exact M7 archive/config/file/owner fingerprint is computed for explicit trusted-code review. Self-review/non-Administrator/historical decisions are unavailable; server checks remain authoritative. Archive review still requires the approved native download workflow. No code executes by opening/inspecting UI or the dashboard's ZIP tests.
- **Limited data:** Existing package history, registry status, stored routes and safe security audit events are displayed. Registry status is labeled separately from runtime health; missing general legacy-release/runtime endpoints remain unavailable. Sorting is explicitly of the loaded page. No arbitrary API host/path/port/start/build/env/manifest command is accepted.
- **Static hosting only:** IHP_DASHBOARD_ENABLED=1 opts Gateway into a fixed repository src/Dashboard/dist snapshot. LocalStack supplies it only to Gateway; direct Gateway startup without opt-in retains former /admin reservation. Only index.html and assets/<name>.js/.css are loaded; links/unexpected files/source maps rejected; maximum 32 files/4 MiB each/16 MiB total. Client requests use dictionary keys, never filesystem traversal.
- **Response policy:** GET/HEAD /admin/ with self-only CSP, no framing/base/form action, no-store, nosniff, no-referrer and same-origin resources; /admin redirects, unknown assets 404/writes 405. Application route validators/reserved prefixes remain unchanged.
- **Public health projection:** Exact GET/HEAD /_platform/api/system/health sends only a fixed loopback API health request without incoming credentials/cookies/headers/query, proxy or redirects; two-second/2-KiB budgets; only safe component/status output. All other API paths remain reserved at Gateway. Existing Vite proxy preserves Origin/Fetch Metadata; real smoke confirms browser-origin login still returns controlled 403.
- **Scope:** No API source, Prisma schema/migration, Agent runtime policy or original sample application change is required. SQLite total remains 20 models/four reviewed migrations. M8 does not enable source builds, corporate access, alternate execution identities or OS isolation.

## Files created or modified

**Created locally:**

- src/Dashboard/src/app.tsx, api.ts and model.ts.
- src/Dashboard/src/components/ui/button.tsx and card.tsx; src/Dashboard/components.json and tsconfig.test.json.
- src/Dashboard/test/dashboard.test.mjs and native-harness.mjs.
- src/Gateway/DashboardAssets.cs and tests/Gateway.Tests/DashboardChecks.cs.
- DASHBOARD.md and docs/verification/M8-2026-10-27.md; local artifacts/m8-dashboard-first-failure.log, m8-dashboard.log, m8-gateway.log, m8-verify-450.log and m8-verify.log plus continuity drafts.

**Modified locally:**

- src/Dashboard/src/main.tsx/style.css, vite.config.ts and package.json.
- src/Gateway/GatewayHost.cs/Program.cs and tests/Gateway.Tests/Program.cs.
- tools/local.mjs, tasks.mjs and smoke.mjs; root package.json/package-lock.json, .gitignore and .prettierignore (test output exclusions).
- README.md, ARCHITECTURE.md, SECURITY.md, AUTHENTICATION.md, GATEWAY.md and INSTALLATION.md.
- The four tracked records PROJECT_REQUIREMENTS.md, AGENTS.md, IMPLEMENTATION_PLAN.md and PROGRESS.md.

Pinned dependencies installed with npm.cmd --ignore-scripts: lucide-react 1.62.0, @radix-ui/react-slot 1.5.0, class-variance-authority 0.7.1, clsx 2.1.1, tailwind-merge 3.7.0 and dev-only jsdom 30.1.3. No remote generator/browser installer/native tool ran. API/Agent/sample source and existing SQL migrations were not modified. Only the four project records are delivered to Git; implementation/additional docs remain local as required.

## Commands, failures and corrections

| Actual command/iteration                                                                                              | Result                                                                                                                                                                                      |
| --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| npm.cmd view pinned dependency versions; npm.cmd install --workspace @ihp/dashboard --save-exact --ignore-scripts ... | Installed exact runtime packages above; dev jsdom installed separately with --save-dev/--save-exact/--ignore-scripts; zero audit vulnerabilities                                            |
| Initial npm.cmd run build --workspace @ihp/dashboard                                                                  | Failed unused Package type and unknown-valued JSX truthiness; removed unused import/explicit boolean conditions; then build passed                                                          |
| npm.cmd run test:dashboard initial                                                                                    | Built API/dashboard/published PackageInspector; 20/23 tests passed, failures were asynchronous nav assertion and approval/rejection feedback lost when parent refreshed                     |
| Corrections                                                                                                           | Wait for rendered navigation; retain only same-user/request-identity data during refresh, preserve component feedback, cancel stale results; guard identity render. No API security relaxed |
| Focused dashboard reruns                                                                                              | 23/23 then 27/27 passed. Real async assertions wrapped in act; final focused 27/27 had no act warnings                                                                                      |
| npm.cmd run test:gateway                                                                                              | Exit 0, 113/113 including five new dashboard checks/setup; all existing Gateway tests retained                                                                                              |
| npm.cmd run format; dotnet format InternalHostingPlatform.sln --no-restore                                            | Exit 0; full format/type/.NET checks passed in first complete gate                                                                                                                          |
| First npm.cmd run verify                                                                                              | Exit 0: 450 checks, zero failures/skips, plus real built /admin/ assets/CSP/public-health/browser-origin rejection smoke, owned shutdown/port/storage cleanup                               |
| Final accessibility correction                                                                                        | Skip link focuses current main content without changing the SPA route; dedicated check passed in final 28/28 dashboard suite                                                                |
| Final npm.cmd run verify                                                                                              | **Exit 0: 451 checks, zero failures/skips, plus real local smoke; no act warnings in final dashboard results**                                                                              |
| npm.cmd run doctor                                                                                                    | Exit 0: Windows 22631 x64, Node 26.12.1, npm 11.20.0, .NET SDK 8.0.425, .NET/ASP.NET 8.0.31                                                                                                 |
| npm.cmd audit --json                                                                                                  | Exit 0, zero vulnerabilities                                                                                                                                                                |
| Current principal                                                                                                     | Administrator=False                                                                                                                                                                         |
| Published output scan                                                                                                 | Twelve project publish roots, zero .exe artifacts                                                                                                                                           |
| git fetch origin main/base comparison                                                                                 | Local HEAD/origin/main both c553e0102f64146481f58459e508bdc77eeb9948 before delivery; exact progress remote/main confirmed                                                                  |

Some patch contexts were rejected before execution and corrected; no partial external action occurred. Actual verification uses Tee-Object logs and propagated LASTEXITCODE. All .NET runtime/verifier launches come from UseAppHost=false Release framework-dependent publish, no VSTest/testhost/dotnet run/test or apphost.

### Full gate results

Final documentation checks: npm.cmd run check exited 0 after the handoff update; git diff --check passed. Complete prior progress text and every original requirements line were verified preserved. Published output scan again found twelve roots and zero .exe artifacts. Only the four authorized records are tracked for delivery.

| Suite                                          |  Passed | Failed/skipped |
| ---------------------------------------------- | ------: | -------------: |
| Node foundation/lifecycle/sample configuration |      25 |              0 |
| API HTTP/reference/policy/DPAPI/routing/auth   |      81 |              0 |
| Guarded Prisma/SQLite/migration                |      13 |              0 |
| React DOM/model/owned-native-client dashboard  |      28 |              0 |
| Managed Foundation                             |      27 |              0 |
| Managed Gateway                                |     113 |              0 |
| Managed Samples                                |      82 |              0 |
| Managed Deployment Agent                       |      27 |              0 |
| Managed routing/auth integration               |      18 |              0 |
| Managed packages                               |      37 |              0 |
| **Total**                                      | **451** |          **0** |

The first 450-check full gate was the previous 418 plus 27 dashboard and five Gateway checks. The final skip-link regression passed, producing 451. Smoke is separate from the assertion total. It verified literal loopback Gateway/API/Vite listeners, real React module/assets, CSP/public health, preserved Origin rejection, structured logs, successful four-owned-child shutdown/released ports and owned temporary storage cleanup. No actual configured registry was modified.

## Remaining issues and acceptance gaps

1. **M8 browser authentication blocked:** unchanged M11 rejects browser Origin/Fetch Metadata. Need an explicitly reviewed browser-session/CSRF/transport/management-origin design; no native credential bridge/CORS/header stripping is authorized. Private production UI remains disabled; full authenticated browser upload/review/deploy is not verified.
2. **M8 actual visual/accessibility verification incomplete:** Node/jsdom semantic/focus/navigation and responsive CSS contracts were tested. No browser .exe/driver launched. Actual browser viewport/pixel/contrast/screen-reader measurements remain unverified, not inferred from CSS presence. This independently prevents full acceptance claims.
3. **Incomplete original M8 page scope:** available registry/package/approval/route/audit views exist, but the full original deployment wizard, user/grant/config/policy editors, legacy M5 release/runtime-health read contracts and real authenticated browser workflows remain unfinished/disabled. No new backend endpoints were invented to mask this.
4. **M7 remains IN PROGRESS:** approved isolated source build templates/build logs are unavailable; no uploaded build scripts/dependency installation commands run. Prebuilt success is not full M7 completion. M9 monitoring/M10 rollback remain NOT STARTED.
5. **Operational/trust limits unchanged:** local HTTP and same-user code/storage are not TLS/OS isolation; hashes do not prove code safety; M5 ownership/job window and recovery, M6 polling/LKG/offline revocation, M7 orphan/retention/upgrade gaps and M11 native-client assumptions persist. No arbitrary-code, instant revocation or unattended reboot/logout guarantee is claimed.
6. **Data/UX scope:** registry status and stored route intent are not live process/reachability probes; per-page sorting is explicit; polling applies only to selected available package views. Unknown totals/health remain unavailable rather than fabricated. Build/restart refreshes static Gateway assets; Vite remains development-only.

## Exact next task and safe resume commands

Read AGENTS/current PROGRESS/M8 plan/relevant requirements; inspect actual source/tests/storage/config before relying on records. Preserve M7 IN PROGRESS/source builds disabled. Resolve M8's approved browser authentication/session/CSRF/transport/management-origin design and permitted real-browser verification method before enabling private features or claiming completion. Do not start M9 or weaken any security policy. With policy unchanged, retain disabled browser management and accurate IN PROGRESS status.

```powershell
Get-Content -LiteralPath AGENTS.md -Encoding UTF8
Get-Content -LiteralPath PROGRESS.md -Encoding UTF8 -TotalCount 220
Get-Content -LiteralPath IMPLEMENTATION_PLAN.md -Encoding UTF8
Get-Content -LiteralPath DASHBOARD.md -Encoding UTF8
git status --short
npm.cmd run doctor
npm.cmd run test:dashboard
npm.cmd run test:gateway
npm.cmd run verify
```

Tests create owned fresh SQLite registries. Do not migrate actual user data merely to resume checks; M8 adds no migration. After existing authorized development registry setup, npm.cmd run build / npm.cmd run start:local serves http://127.0.0.1:5050/admin/ and Vite 5173/admin/, public health only. Stop with Ctrl+C. Do not seed actual credentials or enable execution automatically.

For the explicitly authorized progress-only delivery:

```powershell
git diff --check
git add PROJECT_REQUIREMENTS.md AGENTS.md IMPLEMENTATION_PLAN.md PROGRESS.md
git diff --cached --name-only
git commit -m "Record M8 dashboard and browser authentication limitation"
git push origin main
git ls-remote origin refs/heads/main
git rev-parse HEAD
```

Require staged/committed paths to be exactly the four named records and compare remote main with HEAD. Final handoff confirms push identity/result; no source/data/credentials/artifact is included. Complete earlier progress/intermediate failures are preserved below as history; obsolete statuses/waits do not override this snapshot.

## Preserved progress history

The complete prior record, including intermediate M8 work and all earlier sessions, follows as history. Its obsolete milestones/waits/results do not override the current snapshot.

``````````````text
# Project Progress

## Active M8 session — 2026-10-27

Expanded dashboard tests passed 27/27 without act warnings, Gateway 113/113, and the first complete verify gate passed 450 checks plus real /admin/ bundle/CSP/public-health/browser-origin-rejection smoke and cleanup. Final accessibility review found skip-link hash navigation resetting the page; corrected to focus current main content and added a dedicated check. Final rerun is pending; do not infer browser authentication or visual acceptance from those results. M7/M8 remain IN PROGRESS.

The latest user instruction assigns M8 only, superseding the M7 wait. Read AGENTS/progress/M8 plan and dashboard/security requirements, inspected existing React/Vite/Tailwind shell and API contracts. M7 remains IN PROGRESS with source builds disabled. Confirmed the existing M11 native-client guard rejects browser Origin/Fetch Metadata; there is no approved browser session transport. Do not strip headers, expose/paste native tokens, enable CORS or invent a credential bridge. Proceed with real responsive components, safe public health and contract/owned-native-client verification, keeping browser management disabled and M8 unfinished. Starting baseline c553e01, 418 passing checks; implementation and tests remain local and only four records may be pushed.

Implemented responsive React/Tailwind dashboard, local shadcn-style primitives/Lucide, guarded application/package/approval/routing/audit views and explicit blocked browser authentication. Added optional fixed /admin/ static hosting and fixed public health projection to Gateway; no protected API relay or API guard change. First TS build found unused type/unknown JSX conditions, corrected. First DOM/native-client test run 20/23 passed: one async navigation assertion and two feedback losses during parent refresh. Fixed test timing and retained only same-user/request-identity data during refresh, with stale-response cancellation. Focused rerun 23/23 and Gateway 113/113 passed. Added further login/revocation/stale-response/route tests and production-host smoke assertions; not yet claiming those or full regression. No browser binary launched; DOM/CSS checks do not prove pixel layout. M8 and M7 remain IN PROGRESS.

**Updated:** 2026-10-27 (Asia/Bangkok)

## Current milestone and handoff

- **Current milestone:** M7 — ZIP Upload and Approved Source Builds, **IN PROGRESS**. The latest attached request authorized M7 after M11; earlier instructions to wait are historical.
- **Completed milestones:** M1–M6 and M11 remain COMPLETED. Their full regression passed alongside M7.
- **Last completed task:** Implemented and verified the secure trusted-prebuilt upload/review/preparation/runtime/routing workflow; completed full build/check/test/smoke and operating documentation.
- **Final verification:** `npm.cmd run verify` exited 0: **418 passing checks**, zero failures/skips, plus real local platform smoke. This is the prior 381 plus 37 package checks.
- **Why M7 remains unfinished:** Source builds/Node.js–React–ASP.NET build templates cannot safely execute under the available same-user boundary. No approved isolation exists. Source builds remain disabled, and the user's explicit instruction requires IN PROGRESS/BLOCKED rather than a completion claim. No source build or build-log acceptance success is recorded.
- **Next task:** Resolve the documented approved source-build/isolation requirement within M7 before enabling any builder or changing its status. Review PACKAGES.md, M7 acceptance and the actual source/tests first. Do not start M8 or another milestone. With policy unchanged, preserve the disabled build path and this accurate status.
- **Git scope:** Commit/push only PROJECT_REQUIREMENTS.md, AGENTS.md, IMPLEMENTATION_PLAN.md and PROGRESS.md to `https://github.com/logtranwanek-spec/IAH-Progress-Tracking-.git`, branch main. Source, tests, extra docs, packages, SQLite/vault data and artifacts remain local/untracked. A progress-only clone cannot reconstruct/run the implementation.

## Mandatory policy and execution boundary

Only **SQLite** is supported/allowed for real operations. SQL Server/PostgreSQL/MySQL labels and protected connection references remain metadata-only; server installation, connection attempts, real authentication/protocol tests are prohibited. Their missing connectivity evidence remains the accepted company-policy gap. No such operation ran in M7.

All real listeners use literal 127.0.0.1; Gateway remains 127.0.0.1:5050. .NET applications/verifiers are framework-dependent publishes with UseAppHost=false, run only as `dotnet <published>.dll`; all twelve publish roots passed no-.exe scans. No apphost/testhost/VSTest, uploaded executable, native Prisma engine, Docker, elevation, security-policy/ACL/firewall change, production access or corporate listening/deployment was used. Installed node/dotnet/npm.cmd/git hosts follow the existing project convention. No new external package/native dependency was added.

Verification created fresh owned temporary profile registries/users/archives/release copies and cleaned its own storage/processes afterward. It did not migrate/bootstrap configured user data or modify original samples/existing applications. Real execution used only copied trusted platform sample code and an owned deterministic exit-17 fixture. Malicious paths/scripts/native-looking bytes were inert inspection inputs, never executed.

## Implemented features and verified evidence

| Requirement                  | Actual evidence                                                                                                                                                                                                                                                                                          |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Authenticated ZIP upload     | PASS: real M11 sessions/current RBAC/ownership; anonymous, Viewer and other Developer denied; raw streaming upload creates owned UUID storage and pending review                                                                                                                                         |
| ZIP safety and budgets       | PASS: malformed/CRC/encrypted/oversized ZIP, traversal/absolute/drive/backslash/percent/device/alternate-stream/case aliases, symlink modes, count/ratio limits and forbidden scripts/native artifacts reject; oversized HTTP body rejects before a deployment record                                    |
| Pure package inspection      | PASS: Node ESM and framework-dependent ASP.NET Core 8 metadata/entrypoint detection, SHA-256 archive/canonical metadata/per-file manifest; inert throwing Node entrypoint is inspected without running; source/script/runtime/port/path overrides reject                                                 |
| Manifest compatibility       | PASS: strict platform.json and JSON-compatible platform.yaml subset; bounded normal dotnet web.config accepted and unsafe IIS command rejected without invoking IIS                                                                                                                                      |
| Exact M11 approval           | PASS: automatic PENDING_APPROVAL, exact fingerprint and explicit trusted-code Administrator attestation, current separate reviewer, rejection history, unapproved action denial and tamper invalidation                                                                                                  |
| Immutable preparation        | PASS: both reviewed prebuilt runtimes prepare to new release directories without process execution; duplicate version/collision rejected; previous healthy release and marker file preserved; hashes rechecked                                                                                           |
| M5/M6 trusted execution      | PASS: approved Node/.NET start concurrently on distinct owned loopback ports; matching health succeeds; new dynamic routes proxy through the real unchanged Gateway; stop withdraws route and approved restart works                                                                                     |
| Failure/recovery             | PASS: owned exit-17 startup tracked, leases released and previous release/stopped selection preserved; revoked approval stops only its package/removes route; interrupted preparation fails closed and cleans its owned staging; default Agent gate refuses even valid approval with no process instance |
| Persistent states/audit/data | PASS: SQL immutable metadata/state guards, insertion-ordered events, foreign-key/model parity, package actions/audits and current deployment status; credential values absent from captured API/Gateway logs                                                                                             |
| Existing functionality       | PASS: all previous M1–M6/M11 suites, WebSockets/dynamic updates/auth/SQLite/DPAPI/process recovery and real platform smoke                                                                                                                                                                               |
| Approved source builds       | **NOT IMPLEMENTED/NOT RUN**: disabled due to unavailable approved isolation; no uploaded scripts/npm install/build/shell/manifest commands executed                                                                                                                                                      |

The managed package suite contains **37 checks** (pure archive/metadata checks plus real HTTP/API/Agent/Gateway scenarios). It publishes/runs production managed DLLs, not VSTest. The full M6/M11 integration still has 18 checks and Gateway still has 108. New package tests verify M7 behavior, not an assertion that arbitrary applications are safe.

## Architecture, permissions and operational decisions

- **M11 integration:** Upload requires Administrator or owned Developer write authority; scoped reads require ownership/read grant or Administrator. Viewers cannot mutate. Administrator-only attachment download supports offline review; the former operator capability cannot upload or bypass initialized sessions. Existing native-client binding/User-Agent/Origin/Host policy stays in force; no browser login/UI is added.
- **Streaming transaction boundary:** Raw ZIP reception runs outside long SQLite transactions; current session/ownership are checked before reception, after streaming and before approval creation. Mutation/audit operations still join atomic authorization transactions. No uploaded body is executed or logged.
- **Fixed helper boundary:** The API allows only published CredentialProtection and metadata-only PackageInspector DLL subprocesses through dotnet, with scrubbed helper environments/bounded outputs/timeouts. Inspector reads ZIP/files/PE metadata only; no imports, entrypoint execution, runtime probe, database connection or build commands occur there.
- **Immutable identity:** Fingerprint is lowercase SHA-256 of UTF-8 `m7\n<deploymentId>\n<archiveHash>\n<canonicalMetadataHash>\n<fileManifestHash>\n<ownerId>`. M5 sorted per-file SHA-256 manifest is reused. Distinct current Administrator must submit the exact fingerprint and trustedCodeReviewed=true. Archive bytes/metadata/files/owner bind the decision; review and launch recheck actual data and current authority. Approval alone is not proof of safety.
- **Operator gate:** API and Agent independently default package execution disabled; both need literal IHP_TRUSTED_PACKAGE_EXECUTION=1 in their own environments after trust review. Preparation executes no code and does not need this flag. Runtime environments omit platform credentials and this flag. No direct Agent HTTP/unauthenticated process control is added; legacy M5 stdin remains separately opt-in.
- **SQLite queue:** Authenticated prepare/deploy/stop stores the session ID/action, not a token. Agent singleton polls and checks current session expiry/revocation, current requester/reviewer roles/active state, owner and fingerprint before promotion/start. Queued admission is not completion or indefinite authorization. SQL guards constrain state transitions and immutable review data; Agent atomically records state/event/audit/general deployment status.
- **Preparation:** Extract approved bytes into a fresh owned inspection directory, validate again, then move without overwrite to Applications/<application UUID>/releases/<version> and record the M5 release. Failure cleanup targets only owned new staging/releases, preserving existing versions and active applications.
- **Execution/routing:** M5 retains process/job ownership, per-app gates, persistent port leases, listener/health identity, exits/log capture and bounded recovery. Packaged launch guard also applies to manual/recovery paths. Only healthy selected packages create/enable strict M6 prefix route intent; current approval is checked in API snapshots as well. A different version cannot deploy while an application runs: stop explicitly first. Prior stopped selection can be restored after failure, but no automatic previous runtime restart/online rollback/M10 is introduced.
- **Revocation:** Agent samples authority/archive integrity every second and release files for running packages, then stops owned invalid packages/withdraws routes. M6 polling/last-known-good staleness still applies; revocation is not instantaneous or guaranteed while the source is offline.
- **Trust limit:** Human code review plus hashes/loopback/jobs is a reviewed-trusted-code policy, not a same-user sandbox. Same-user code can access user files/network and writable registry; filesystem and launch checks cannot eliminate every race. No Windows identity isolation, signature authority, antivirus/CDR integration or corporate-network protection claim is made.

## Supported package contract and limits

Exactly one root platform.json or platform.yaml is required. Both use strict JSON syntax only (JSON-compatible YAML subset), <=16 KiB, depth eight, known required fields/no duplicates. Required schemaVersion=1, name, slug, runtime/framework (node/node or dotnet/aspnetcore), deploymentType=prebuilt, version, build, entryPoint, basePath=/<slug>, healthPath=/<slug>/health, contract=ihp-loopback-v1 and port=auto. Name/slug/runtime must match the registry and version the upload URL. Safe version/slug/prefix/device-name rules apply; ports come only from M5's approved 6100–6999 range/configured subset.

Node requires reviewed ESM files/package.json with no scripts (or empty scripts), no native addons, and preincluded reviewed dependencies within budgets; no install runs. ASP.NET requires published managed DLL/deps/runtimeconfig, net8.0/framework-dependent ASP.NET Core 8; no self-contained/native payloads. Optional root web.config is narrowly validated XML (DTD/resolver prohibited, matching dotnet DLL, stdout files disabled), never used by IIS. Normal M5 shutdown/health contract is mandatory; arbitrary web applications are not automatically compatible.

Budgets: raw application/zip with Content-Length 1–8 MiB, no Content-Encoding, 15-second stream deadline, two concurrent uploads and 50 stored records/uploader; ZIP <=256 entries, <=8 MiB/file expanded, <=32 MiB total expanded, <=100:1 ratio with 4-KiB small-file allowance. ZIP supports stored/deflated entries and validated descriptors, strict central/local/CRC layout; no ZIP64/encryption/extra fields/comments/multidisk/SFX/trailing payloads. Paths are bounded ASCII with strict segments/no links or aliases. Only managed/runtime metadata and allowlisted web/text assets are admitted. General ZIP exporter compatibility and production-sized dependency bundles are limited by these deliberate budgets.

API prefix remains /_platform/api. POST /applications/:id/packages/:version accepts the raw ZIP; GET list/detail exposes scoped metadata/state/events; GET /applications/:id/packages/:deploymentId/archive is Administrator-only. Existing /approval-requests/:approvalId/decision handles exact review/rejection. Empty-JSON POST .../:deploymentId/prepare, /deploy and /stop queue actions (202); poll the scoped detail endpoint. No client filesystem path, destination host/port, start command, environment or build command is accepted. PACKAGES.md contains the full manifest/example/table and operating requirements.

Validated states: UPLOADED → VALIDATING → PENDING_APPROVAL → APPROVED → PREPARING → READY → DEPLOYING → RUNNING; pending review may become REJECTED and active workflow states may become terminal FAILED. Approved stop returns RUNNING → READY. Events/audits retain fixed codes/IDs, not credential/body values. Failed recorded uploads occupy their immutable version/quota; retry with a new version rather than deleting history.

## Schema, files created or modified

Appended reviewed src/Api/prisma/migrations/202610270004_packages/migration.sql; earlier migration SQL is unchanged. New DeploymentPackage/deployment_packages and DeploymentEvent/deployment_events models/tables have indexed ownership/state/request-session/history links, foreign keys, immutable metadata and transition triggers. Existing deployment/user/session relations include package references and previous stopped selection. Current total **20 models/four migrations**. Apply migrations only through the explicit reviewed Node SQLite runner to authorized data; API startup never migrates it. Agent verifies its copied package migration checksum. This session verified fresh owned registries/reapplication/checksum/parity/rollback, not an actual user-data upgrade.

**Created:**

- src/DeploymentAgent/PackageArchive.cs and PackageWorkflow.cs.
- src/PackageInspector/PackageInspector.csproj, Program.cs and generated pinned packages.lock.json.
- src/Api/src/package-files.ts, packages.ts and package-controllers.ts; the package migration above.
- tests/Packages.Tests/Packages.Tests.csproj, Program.cs, Integration.cs, Fixtures.cs, Checks.cs and packages.lock.json.
- PACKAGES.md and docs/verification/M7-2026-10-27.md.
- Local evidence artifacts/m7-packages.log, m7-verify-first-failure.log, m7-verify.log and preserved intermediate progress artifacts.

**Modified:**

- src/DeploymentAgent/ReleaseInspector.cs (shared pure directory inspection), RuntimeEngine.cs (packaged launch/recovery preflight), EngineStore.cs (internal atomic helpers), EngineOptions.cs (default-disabled gate), Program.cs (queue/policy) and DeploymentAgent.csproj (reviewed package SQL content).
- src/Api/prisma/schema.prisma; src/Api/src/app.ts, registry.ts, access.ts, approvals.ts, routing.ts and native-policy.ts.
- src/Api/test/api.test.mjs and database.test.mjs; tests/DeploymentAgent.Tests/Program.cs and AgentProcess.cs (model/migration counts and explicit owned-fixture execution flag).
- InternalHostingPlatform.sln, package.json, tools/publish.mjs and tasks.mjs (two new publish roots/focused and full package verification; no external dependency additions).
- README.md, API.md, ARCHITECTURE.md, AUTHENTICATION.md, DEPLOYMENT_ENGINE.md, DYNAMIC_ROUTING.md, SECURITY.md and INSTALLATION.md for current M7 boundary/operations.
- Four tracked records PROJECT_REQUIREMENTS.md, AGENTS.md, IMPLEMENTATION_PLAN.md and PROGRESS.md.

Generated Prisma/TypeScript/dashboard/managed build/publish output was refreshed locally. Production Gateway source, dashboard UI and original sample application sources are unchanged. Implementation/additional documentation remains local as instructed; only four records belong to the progress Git repository.

## Actual commands, failures, corrections and results

| Command/iteration                                                          | Actual result                                                                                                                                                                                        |
| -------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Initial PackageInspector publish/API build                                 | Passed metadata helper publish and guarded WASM generation/TypeScript compile                                                                                                                        |
| Initial Agent publish                                                      | Failed four CS0819 multiple-var declarators; changed to explicit string declarations before running                                                                                                  |
| Initial focused rebuild                                                    | Failed CS8604 after stricter slug validation; marked throwing validation helper DoesNotReturn                                                                                                        |
| First running npm.cmd run test:packages                                    | 17 passed/3 failed: standard ASP.NET web.config rejected; later integration assertions cascaded from that rejection. Corrected with restricted nonexecuting XML acceptance; never invoked IIS        |
| Focused rerun                                                              | Exit 0: 32/32 including real Node/.NET lifecycle/route, hash/approval/collision/exit/revocation checks                                                                                               |
| Expanded npm.cmd run test:packages                                         | Exit 0: 37/37 including HTTP oversize/rejection/default gate/interrupted staging/JSON-YAML subset checks                                                                                             |
| Status consistency correction                                              | Synchronized general deployment record/package states and added actual RUNNING/READY assertions; included in final full gate                                                                         |
| npm.cmd run format; dotnet format InternalHostingPlatform.sln --no-restore | Exit 0; final format/type/.NET format checks also passed in verify                                                                                                                                   |
| First npm.cmd run verify                                                   | Failed at API 80/81 after successful full build/check/25 Node checks: old OpenAPI assertion required every deploy path absent. SQLite/managed/smoke were not reached in that run                     |
| OpenAPI correction/API rebuild                                             | Exposed accurate M7 default-disabled trusted execution policy; test allows exactly the package deploy path with user-session security and still excludes legacy deploy endpoints. API rebuild passed |
| Final npm.cmd run verify                                                   | **Exit 0: 418 passing checks, zero failures/skips, plus real local smoke**                                                                                                                           |
| npm.cmd run doctor                                                         | Exit 0: Windows 22631 x64, Node 26.12.1, npm 11.20.0, SDK 8.0.425, .NET/ASP.NET runtime 8.0.31                                                                                                       |
| npm.cmd audit --json                                                       | Exit 0: zero vulnerabilities                                                                                                                                                                         |
| Current principal check                                                    | Administrator=False                                                                                                                                                                                  |
| Recursive artifacts/publish scan                                           | Twelve project roots; zero .exe artifacts                                                                                                                                                            |
| git fetch origin main / base comparison                                    | Local HEAD and origin/main both 94c59dbb5e9ae6f0f8b244c434df69b7766b298c before delivery; no incoming conflict                                                                                       |

Commands used PowerShell Tee-Object for artifacts/m7-packages.log and artifacts/m7-verify.log and propagated LASTEXITCODE. tools/publish.mjs invoked dotnet publish in Release/net8.0, --no-self-contained, --disable-build-servers, -p:UseAppHost=false, -p:UseSharedCompilation=false and project-specific artifacts/publish output, scanned output, then invoked the DLL with dotnet. No dotnet run/test or blocked runner was used. A few rejected patch contexts/orchestration attempts were corrected before execution; no external action or policy bypass resulted. Intermediate counts above describe those revisions, not final completion evidence.

Final handoff checks: npm.cmd run check exited 0 after the documentation update. Full previous progress text and every original requirements line were verified preserved; git diff --check passed and the recursive publish scan again found zero .exe artifacts. Only the four authorized Markdown records are tracked for delivery.

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
| Managed packages                               |      37 |              0 |
| **Total**                                      | **418** |          **0** |

Separate smoke applied/verified fresh reviewed SQLite migrations, checked actual Gateway/API/React and structured Gateway logs, inspected literal loopback 5050/5065/5173 listeners, stopped four owned components successfully, verified released ports and deleted only owned temporary storage. Existing dynamic-routing WebSocket tests passed; M7's own test additionally proxied both newly admitted releases. There was no Gateway restart during those package route changes.

## Known limitations, unresolved issues and blockers

- **M7 source-build acceptance remains unfulfilled:** no safe approved isolated build environment. Node.js/React/ASP.NET source templates/build logs/dependency install/custom build commands are not implemented or tested. Do not enable them under the same ordinary identity. This is the reason for IN PROGRESS; the verified prebuilt subset is not full M7 completion.
- **Restricted shapes:** JSON-compatible YAML subset only; no general source manifest/standalone React build/static-only template. Standard ZIP exporters/large apps/native dependencies may exceed the strict formats/budgets. Exact Node 24 compatibility/large-package throughput remains unverified on installed Node 26.12.1.
- **Crash/retention gaps:** PREPARING/DEPLOYING restart fails closed. A crash between filesystem promotion and database recording can leave an orphaned version directory; collision protection prevents overwrite and requires explicit operator reconciliation. API death during receive/validation can leave owned upload/orphan records; no automatic retention/forensic recovery service or quota reclamation is implemented. These are documented limits, not successful crash-atomicity claims.
- **No online upgrades/rollback:** new deploy requires stopped application; failed startup preserves files and may restore stopped selection, not service continuity or automatic prior runtime restart. M10 is not started.
- **Same-user/transport/staleness:** local HTTP/native-client trust and writable same-user filesystem/registry cannot isolate malicious code or eliminate TOCTOU. M5 job-assignment window, best-effort redaction and bounded recovery, M6 polling/LKG/offline revocation limits and M11 session transport assumptions remain. No arbitrary package safety, instant revocation, OS identity isolation, TLS/SSO/MFA or unattended boot/logout operation is claimed.
- **User registry upgrade not exercised:** fresh owned migrations/parity/checksum/rollback passed; no actual configured registry/vault/user data was changed. Authorized backup/upgrade/restore/retention procedures remain future work.
- No reproducible failure remains in the tested trusted-prebuilt subset after fixes. The limitations above are not hidden by passing tests. No new IT approval request, stronger isolation, network deployment or later milestone has been inferred as authorized.

## Exact next task and commands to resume

Read AGENTS.md, this current snapshot, M7 in IMPLEMENTATION_PLAN.md and relevant requirements; inspect the actual local source/output/state. Confirm M1–M6/M11 remain complete and M7 IN PROGRESS. Review PACKAGES.md and the source-build limitation before doing any new execution work. The first unfinished requirement is an approved safe build boundary/allowlisted source templates with genuine build-log evidence. With the current policy, retain source builds disabled; an explicit reviewed isolation/policy or acceptance decision is needed before implementing those templates or declaring M7 complete. Do not start M8/another milestone.

To verify the existing local implementation safely:

```powershell
Get-Content -LiteralPath AGENTS.md -Encoding UTF8
Get-Content -LiteralPath PROGRESS.md -Encoding UTF8 -TotalCount 220
Get-Content -LiteralPath IMPLEMENTATION_PLAN.md -Encoding UTF8
Get-Content -LiteralPath PACKAGES.md -Encoding UTF8
git status --short
npm.cmd run doctor
npm.cmd run test:packages
npm.cmd run verify
```

Do not migrate actual configured data just to resume tests: verifiers create their own SQLite registry. Apply reviewed migrations to configured development data only through an explicitly authorized backup/review process. Do not copy test credentials or turn on trusted execution automatically. No .exe artifacts, Docker, Administrator/security changes or forbidden database/network operations are needed for these checks.

For authorized progress-only delivery after documentation checks:

```powershell
git diff --check
git add PROJECT_REQUIREMENTS.md AGENTS.md IMPLEMENTATION_PLAN.md PROGRESS.md
git diff --cached --name-only
git commit -m "Record M7 trusted package workflow and source-build limitation"
git push origin main
git ls-remote origin refs/heads/main
git rev-parse HEAD
```

Require the staged/committed paths to be exactly those four records and compare remote main to local HEAD. Push result/commit identity is confirmed in the session handoff; source/data/artifacts are never included. Historical progress below is preserved in full; older milestone statuses/wait instructions/results are history and do not override this snapshot.

## Preserved progress history

The following complete prior log includes intermediate M7 work and all earlier sessions. Its obsolete status and wait instructions are historical.

`````````````text
# Project Progress

## Active M7 session — 2026-10-27

M7 is IN PROGRESS under the attached user instruction, superseding the previous wait. Read AGENTS/current progress/plan and ZIP/manifest/deployment/security requirements; inspected actual M11 authorization/approval, M5 inspection/runtime and M6 route boundaries. Baseline tracked docs are clean at 94c59db; source exists locally. Plan: bounded strict managed ZIP inspection, authenticated owned package records, immutable exact-fingerprint reviews, safe promotion and controlled SQLite Agent queue for reviewed trusted prebuilt releases. Source builds remain disabled because same-user execution is not an approved sandbox; do not run uploaded scripts/builds. Per user instruction this limitation prevents claiming full M7 completion. No M8+, corporate deployment or production/user data change is authorized.

Core implementation step: added managed ZIP/CRC/metadata inspection (8-MiB archive/32-MiB expansion/256-entry budgets), a fixed published inspector helper, authenticated raw ZIP streaming with post-upload reauthorization, immutable package/event schema (20 models/four migrations), exact fingerprint/trusted Administrator review and a controlled Agent preparation/deploy/stop queue. M5 inspection is shared without running entrypoints; original trusted flows remain separate. The Agent preflight runs under its singleton lock before recovery; packaged launches check policy/approval/hashes and M6 additionally checks package authority. API compiles; the initial Agent publish found four C# multiple-var declarators, corrected before execution. No M7 lifecycle behavior is yet verified. Next: real managed ZIP/HTTP/runtime fixtures, failure cleanup/recovery checks and full regression. Source builds remain disabled; M7 is not complete.

**Updated:** 2026-10-27 (Asia/Bangkok)

M7 verification step: the first running package suite reported 17 passed/3 failed because standard ASP.NET publish web.config was excluded. Added narrow nonexecuting root XML validation (no IIS/config changes), then verified 32/32 checks including real Node/.NET upload/review/promotion/start/proxy/stop, archive tampering, promotion collision, exit-17 cleanup, prior stopped-selection restoration and authority revocation. An intermediate compile nullability error was corrected with the throwing helper's DoesNotReturn annotation. Added size/rejection/default-gate/interruption and YAML-subset checks; these are running and are not yet claimed passing. Source builds remain disabled and M7 IN PROGRESS. Full regression is next.

Expanded package gate passed 37/37. Added synchronization of general deployment status with package states and corresponding RUNNING/READY assertions. Full verification built/scanned all 12 published roots and passed format/type checks and 25 Node checks; API stopped at 80/81 because an old OpenAPI assertion prohibited every deploy path. Updated it to allow exactly the M7 packaged route with user-session security and default-disabled policy text, retaining legacy endpoint absence. No managed regression/SQLite/smoke completion is claimed for that failed run. Next: rerun full verification after formatting.

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
`````````````
``````````````
```````````````
````````````````
