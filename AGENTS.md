# Repository Instructions for Codex and Development Agents

## Current ZIP upload-limit assignment — 2026-10-28

The latest user authorizes only raising the compressed ZIP cap to 50 MiB (52,428,800 bytes), its boundary/security tests and documentation. This supersedes the previous documentation-only/operator-wait scope for this task, not any security rule. Preserve milestone statuses, M11/M7 controls, default-off production browser management and the 53 NOT RUN browser scenarios. Run focused upload checks and npm.cmd run verify; use published DLLs only. Never modify an applied checksummed migration: prepare a reviewed additive migration and test it only on owned disposable data, leaving configured Registry migration as an explicit operator step.

## Permanent ZIP size rule

- Compressed ZIP uploads are limited to **52,428,800 bytes (50 MiB), inclusive**; Dashboard label is **Maximum ZIP size: 50 MB**. Keep client/API/Gateway/inspector/SQLite constraints consistent and test below/exactly/one-byte-above boundaries. Do not increase the independent 32 MiB expanded-total, 8 MiB per-file, 256-entry or 100:1 ratio budgets, weaken any ZIP validation/approval/fingerprint check, widen ordinary proxy/JSON limits or enable source execution. API uploads remain streamed with bounded concurrency; review archive-sized allocations/copies when changing limits. No automatic migration of configured data.

## Current workflow-configuration assignment — 2026-10-28

The latest user assigns workflow configuration/documentation only. Configure lean optional delegation and update AGENTS.md/PROGRESS.md; do not implement application features, run application tests/builds, start applications, migrate databases or start another milestone. A minimum supported repository-local Codex configuration may be prepared; do not change global settings or install extensions. This assignment supersedes earlier active implementation directives for this task only. Preserve all security rules, milestone statuses, the M7 source-build restriction, default-off production browser management and all 53 NOT RUN M8 browser scenarios. Use documentation/configuration consistency checks and git diff --check. Commit/push only changed files within the four authorized Markdown tracking paths; local configuration stays out of that progress repository.

## Current M8 continuation — 2026-10-28

The latest user explicitly resumes M8 implementation and verification, superseding the preceding documentation-only assignment for this session. Diagnose API readiness, secure and verify the existing browser candidate, integrate authorized Dashboard operations and run focused/full regression. Earlier task-specific scope statements are historical when superseded by an explicit user assignment. Preserve M7 IN PROGRESS/source-build restrictions and M10-DB NOT STARTED; do not start M9/M10/M10-DB/M12. Actual-browser acceptance requires permitted real-browser evidence; do not launch browser executables/drivers or infer acceptance from HTTP/WebCrypto/DOM simulation. Keep private production browser operations disabled if that gate remains unmet. Push only the four official records to progress origin/main.

## Current documentation-only assignment — 2026-10-27

The latest user request is documentation only and supersedes the earlier M8 implementation assignment for this task. Modify only PROJECT_REQUIREMENTS.md, AGENTS.md, IMPLEMENTATION_PLAN.md and PROGRESS.md. Do not implement/modify/build/test application code, change database schemas/migrations, execute applications or start another milestone. Review documentation consistency and run git diff --check; application tests/builds are not authorized for this task. Preserve every existing milestone status and historical progress. M7/M8 remain IN PROGRESS; M10-DB is future NOT STARTED work and requires a separate explicit implementation assignment. Earlier implementation/test/resume instructions below are historical or conditional and do not override this scope.

## Session continuity

At the start of every development session:

1. Read this `AGENTS.md`.
2. Read `PROGRESS.md`.
3. Identify the current milestone and status in `IMPLEMENTATION_PLAN.md`.
4. Read the relevant sections of `PROJECT_REQUIREMENTS.md`.
5. Inspect the actual repository state and relevant files before relying on prior notes.
6. Verify that documented progress matches the code and recorded test evidence.
7. Resume from the first unfinished task. Do not repeat completed work without a reason.

At the end of every development session:

1. Update `PROGRESS.md` with completed and incomplete work, files changed, actual test results, known bugs, unresolved issues, blockers, and the exact next task.
2. Update `IMPLEMENTATION_PLAN.md` whenever a milestone status changes.
3. Record commands and results accurately. Never record untested functionality as completed or a test as passing if it did not run and pass.
4. Preserve prior progress history; do not remove earlier entries without a valid reason.
5. Provide a concise handoff summary.

The user approved the documentation and authorized local implementation on 2026-10-26. Continue within the current milestone without requesting that approval again. Corporate-network deployment and production access still require separate explicit authorization.

## Lean Multi-Agent Orchestration

### Root ownership and delegation decision

- Use the current Root Agent alone by default. Orchestration and integration are two responsibilities of the same accountable Root, not two automatically spawned agents. No sub-agent is required for a routine edit, documentation task, simple lookup or already-understood fix.
- Delegate only when a bounded investigation, implementation or independent review provides a clear benefit that exceeds coordination/context cost. State the benefit, owner, exact scope and expected result before spawning. Do not delegate the same investigation twice or automatically invoke every role.
- This section authorizes optional delegation within the user's currently assigned task when the active host permits applicable repository instructions to authorize it. It never overrides a higher-priority no-delegation instruction, a user request to work alone, missing tool/model access or project security limits. If unavailable, Root performs the work directly.
- Root owns scope/architecture, task decomposition, acceptance criteria, integration/conflict resolution, final verification, milestone decisions, official progress records, Git delivery and the final report. Delegate results are evidence to inspect, not automatic completion.

### Optional roles and model preferences

| Role                | Preferred model / native identifier | Preferred reasoning | Assigned responsibility and access                                                                                                                                                   |
| ------------------- | ----------------------------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Root / Orchestrator | GPT-6.1 Sol / gpt-6.1-sol           | medium              | Decide whether delegation helps; own the task and security boundaries                                                                                                                |
| Explorer            | GPT-6 Luna / gpt-6-luna             | high                | Investigate one specific module/defect; read-only; concise findings and relevant file references                                                                                     |
| Worker              | GPT-6.1 Sol / gpt-6.1-sol           | high                | Implement an explicitly bounded task in assigned files; focused relevant tests; report changes/results/unfinished work                                                               |
| Researcher          | GPT-6 Luna / gpt-6-luna             | high                | Answer one narrow technical question; prefer local project documentation, then authoritative sources when needed; read-only; concise evidence/recommendation                         |
| Reviewer            | GPT-6 Astra / gpt-6-astra           | low                 | Optional independent review justified by security risk, architectural complexity or significant changes; concrete findings with evidence; read-only unless separately assigned edits |
| Root / Integrator   | GPT-6.1 Sol / gpt-6.1-sol           | medium              | Same Root integrates/reviews changes, runs the final gate and reports verified outcomes/blockers                                                                                     |

These are preferences, not claims about the currently executing model, account entitlements or guaranteed routing. Check available tools/model IDs/reasoning values at the beginning of a session; do not invent aliases, configuration keys or model availability. Avoid delegation/high reasoning for trivial work. Use the listed high levels for suitably complex delegated work; choose a supported lighter level when the actual bounded task warrants it. Do not automatically escalate to extra-high/max/ultra. If an override/model is unavailable, continue on the current Root or a verified supported choice; disclose the fallback rather than repeatedly probing models or installing tools.

### Native capability and local configuration boundary

- The inspected native collaboration interface supports spawn_agent, follow-up, messaging, listing, interruption and waiting. Its spawn arguments include model and reasoning_effort; gpt-6.1-sol, gpt-6-luna and gpt-6-astra and the requested effort levels are advertised. This is interface evidence, not a live model/entitlement test. Recheck future-session capabilities before use.
- On this host, a model/reasoning override requires fork_turns="none". Supply a concise self-contained task with repository path, current authorized milestone, file scope, acceptance/verification instructions and security constraints; tell the child to read this AGENTS.md and relevant progress/requirements. Do not fork the whole conversation just to repeat broad exploration. When using another host, use its actual supported controls instead of assuming these argument names.
- Repository .codex/config.toml prepares the documented model=gpt-6.1-sol, model_reasoning_effort=medium and agents.max_concurrent_threads_per_session=2 settings for clients that load trusted project configuration. It does not switch an already-running Root or prove that this session's host loaded it. If project configuration is ignored/not trusted, retain current Root and report the unmet preference; do not change trust/global settings as a workaround.
- Specialized model/effort choices are supplied explicitly on justified native spawn calls; no custom-role TOML files or speculative role keys are needed. AGENTS.md controls workflow intent; it does not implement a permission sandbox or force host model selection. No global Codex setting, extension, provider, authentication, network or security setting may be changed by this policy.
- Prefer one delegated agent at a time. At most two may run concurrently for genuinely independent, non-overlapping scopes; always obey any lower runtime limit. The inspected session advertises four total slots, but the local policy is deliberately leaner. Do not spawn children solely to test that spawning works, and do not allow recursive delegation without an explicit bounded Root assignment and available capacity.

### Task contract, file ownership and integration

- Every task has one accountable owner. Before delegating, specify role, precise question/change, allowed files, read-only versus edit authority, required focused verification, expected deliverable and stopping condition. Send only necessary context; reuse already-verified findings instead of rereading every large document.
- Assign non-overlapping file scopes where possible. Never allow simultaneous edits to the same file. If overlap becomes necessary, stop/wait for the existing owner and explicitly transfer ownership before edits. Shared workspace changes are immediately visible; do not assume branch or filesystem isolation.
- Keep AGENTS.md, PROGRESS.md, IMPLEMENTATION_PLAN.md, PROJECT_REQUIREMENTS.md, model/workflow configuration, final staging/commit/push and milestone statuses under Root control. A worker may propose documentation text but must not edit official tracking records or deliver Git changes without an explicit scope transfer.
- Explorer/Researcher/Reviewer do not modify files, launch applications or run builds/tests by default. A Worker edits only assigned files and performs only authorized focused checks. All roles stop at their scope boundary and report blockers rather than starting another milestone, changing security or expanding execution authority.
- Required return: concise findings/changes, file references, exact commands/results, unresolved risks/blockers and next action. No raw logs or secrets. Root inspects changes, resolves conflicts, checks acceptance/security and integrates before reporting success. Do not repeat an investigation or passing test without a concrete unresolved concern.

### Verification, quota and security inheritance

- During authorized implementation, Workers run relevant focused tests; Root runs the required project final gate at the appropriate integration point. Do not run the whole suite separately in every child. Repeat/broaden only after changes, failures or unresolved concerns. Never skip required security/regression acceptance to save quota.
- For documentation/configuration-only tasks, perform read-only consistency/configuration checks and git diff --check; do not run application builds/tests or represent the historical 520-check result as a new run. Real-browser acceptance still needs permitted actual-browser evidence, never Node/jsdom substitution.
- Delegation grants no additional authority. Every child inherits the user's scope and all IHP restrictions: SQLite-only real operations; external engines metadata-only; no .exe artifacts; .NET UseAppHost=false publish then dotnet <published>.dll; literal 127.0.0.1 listeners/Gateway 5050; no Docker/elevation/firewall/Windows-policy changes/corporate exposure; no deployed-application changes; no secrets in logs/source control. Preserve M11 authorization, M7 approval/fingerprint/trusted-prebuilt/source-build restrictions and Managed Application Database requirements; M10-DB is not implicitly assigned.
- Read-only instructions/file ownership are behavioral controls, not OS-level isolation. All agents remain subject to current host permissions and company policy. Keep production browser management default disabled until its required acceptance is verified. Root validates that a progress commit contains only changed allowed Markdown records and verifies remote main after push; source, tests, configuration, credentials, databases, artifacts/logs and extra docs stay local.

### Invocation

For normal tasks, Root handles the work directly. To use the optional workflow, ask: "Use the lean workflow for [authorized task]; delegate only if it helps." Root then chooses only necessary roles. A bounded explicit request can say: "Use one read-only Explorer on [module/question], return references, then have Root decide the next step." Implementation and independent review each require their own justified bounded assignment. Merely invoking the workflow never authorizes a new milestone, unavailable browser tooling or security bypass.

Capability/configuration references (checked 2026-10-28): [official OpenAI subagent documentation](https://learn.chatgpt.com/docs/agent-configuration/subagents) and [project configuration reference](https://learn.chatgpt.com/docs/config-file/config-reference). Runtime tool metadata governs the actual session; configuration loading/model selection remains host- and access-dependent.

## Source of truth and architecture

- Read `PROGRESS.md` and the relevant sections of `IMPLEMENTATION_PLAN.md` before continuing development.
- Consult `PROJECT_REQUIREMENTS.md` whenever requirements, architecture, environment, security, deployment, or acceptance constraints are relevant. It contains the complete project specification and is the canonical requirements source.
- Follow the existing architecture and coding conventions. Confirm the proposed source layout in `IMPLEMENTATION_PLAN.md` during the foundation milestone; update planned command paths if the layout changes.
- Keep implementation aligned with the project requirements. Do not silently skip requirements or make unsupported claims about completed behavior.
- Update `IMPLEMENTATION_PLAN.md` when milestone status changes. Mark a milestone `COMPLETED` only after every acceptance criterion has been verified and its evidence is recorded in `PROGRESS.md`.

## Mandatory environment and security restrictions

- Never use Docker, Docker Desktop, Docker Compose, Kubernetes, or Docker-based deployment.
- Never require Windows Administrator privileges for development or normal operation.
- Never bypass company IT restrictions or disable/alter endpoint protection, PowerShell Execution Policy, firewall rules, registry settings, or security policies.
- Use `npm.cmd` when invoking npm from PowerShell on Windows.
- Do not modify existing applications or their ports.
- Do not expose application runtime ports to the corporate network. Application processes must bind to localhost/loopback; port 5050 is the only intended network-facing entry point.
- Start and verify locally at `127.0.0.1:5050`. Do not enable network listening, deploy to the corporate network, access production systems, or modify security settings without explicit authorization.
- Do not expose the Deployment Agent directly to the company network.
- Do not execute arbitrary uploaded scripts, manifest commands, builds, or application code without an approved deployment policy and security boundary. If safe isolation cannot be achieved with available permissions, restrict deployment to reviewed trusted applications and document the limitation.
- Validate upload archives, paths, route paths, and proxy destinations. Never allow a package to overwrite platform files or a user to register an arbitrary proxy destination.
- Protect credentials and redact secrets from API responses and logs. Do not store plaintext passwords.
- Do not assume applications support path-based routing; verify base paths and report incompatibility clearly.
- Keep filesystem paths, ports, and runtime settings configurable. Do not hardcode the Windows username.
- Do not assume unattended startup after logout or reboot is available without IT-approved configuration.

## Development and verification

- Make changes in independently testable milestones from `IMPLEMENTATION_PLAN.md`.
- Execute relevant tests after code changes and record their actual commands and results in `PROGRESS.md`.
- Do not run or claim tests for projects that do not exist. Record when tests were not run and why.
- Keep changes focused, preserve existing applications, and avoid introducing infrastructure dependencies not required by the specification.
- Record blockers, failures, security limitations, and unfinished tasks accurately. Do not weaken security controls merely to make a test or demonstration pass.

## Authorization boundary

Do not access production systems, deploy to the corporate network, change network exposure, modify Windows security settings, or alter existing applications without explicit authorization. Local development, read-only inspection, and project-owned documentation/source changes may proceed within the approved milestone and repository scope.

## Mandatory rules after every task

After completing any task, the Agent must:

1. Update `PROGRESS.md` in detail (what was done, actual test results, remaining issues, and the exact next task).
2. Update `IMPLEMENTATION_PLAN.md` whenever a milestone status changes.
3. Commit and push **only** the following 4 files to the progress tracking repository:
   - `PROJECT_REQUIREMENTS.md`
   - `AGENTS.md`
   - `IMPLEMENTATION_PLAN.md`
   - `PROGRESS.md`

**Progress repository:**

- Remote: `https://github.com/logtranwanek-spec/IAH-Progress-Tracking-.git`
- Branch: `main`

Use these exact commands (or equivalent):

```powershell
git add PROJECT_REQUIREMENTS.md AGENTS.md IMPLEMENTATION_PLAN.md PROGRESS.md
git commit -m "Update progress after completing [task name]"
git push origin main
```

Do not commit any source code or other project files into this progress repository.

### Company policy restrictions

- Company policy strictly prohibits running `.exe` artifacts. Do not attempt to bypass this policy.
- For .NET applications and verification programs, publish framework-dependent output with `UseAppHost=false`, then run the published DLL using `dotnet <filename>.dll`. Never execute apphosts or testhost executables directly.
- Do not use `dotnet run`, VSTest, or `dotnet test` if they require an executable runner. A dedicated published managed assertion program may verify production Foundation code and must return nonzero on failure; it is not a rehosted VSTest runner.
- The user authorizes starting M2 if M1 remains impossible to verify within policy; record the exact unverified criteria and this exception before proceeding.
- Verify that staged and pushed paths contain only the four named Markdown files. Never stage source code, binaries, credentials, or other documents in this progress repository.

## Gateway development rules

- M2 is explicitly authorized after M1 completion. Finish only the requested milestone and provide a handoff; do not start an unrequested later milestone.
- The current Gateway must bind exclusively to `127.0.0.1:5050`; reject alternate ports/hosts at startup. Do not enable corporate-network listening without separate explicit authorization.
- Keep application route data out of `Program.cs`. Preserve the approved route-source/config-provider boundary, reserved platform paths, loopback destination validation, and explicit request budgets.
- M2 loads an immutable operator-reviewed startup snapshot. Do not describe it as registry-backed live routing; those updates belong to M6.
- Verify Gateway changes using `npm.cmd run test:gateway`, which publishes and runs the managed verification DLL. Trusted test fixtures must bind only to loopback and be stopped afterward. Do not create later-milestone sample deployments during M2.

## Sample development rules

- M3 is explicitly authorized after M1/M2 completion. Implement only the trusted samples and their run/verification support; do not start M4 or later milestones in the M3 session.
- Samples must bind exclusively to `127.0.0.1` using validated configurable ports in `6100–6999`. Reject unsafe settings before binding. Do not modify existing applications.
- Publish DemoDotnet and the managed Samples.Tests verifier with `UseAppHost=false`, then run their published DLLs through `dotnet`. Use Node directly for the dependency-free DemoNode sample.
- Verify real concurrent processes, health/release identity, loopback sockets, prefix compatibility, graceful shutdown, and port release. Stop only processes owned by the verification session.

## M4 database and API rules

- M4 is explicitly authorized after M1–M3 completion; do not start M5 or later milestones in this session.
- Audit Prisma dependency installation, validation, generation, migrations, and runtime separately. Disable dependency lifecycle scripts; never launch a native Prisma engine or silently substitute a different ORM.
- Use only reviewed repository migrations as a separate controlled step, never uploaded SQL or an HTTP-triggered migration. Record any custom migration-runner limitation accurately.
- Management operations must fail closed until explicitly configured local operator access; destructive and deployment operations stay disabled until the authorization milestone.
- Protect database credentials using Windows-supported current-user protection; never return or log plaintext credentials, connection strings, bearer tokens, or raw driver exceptions.
- Only SQLite is allowed for real database operations. Never install, connect to, or run real connectivity checks against SQL Server, PostgreSQL, or MySQL, even on loopback or through protocol fixtures. Keep these engine labels and protected connection-reference metadata APIs; keep their operational connectivity path disabled.
- Supported Prisma commands are the API package's prisma:validate/build scripts, which use guarded pinned WASM/internal generation. Use npm.cmd ci --ignore-scripts; preserve project .npmrc ignore-scripts=true. Do not replace them with bare Prisma CLI/Migrate or enable dependency hooks.
- Registry migrations are explicit db:migrate/db:status steps after building, never automatic API startup. Consult local API.md for the custom Node migration runner, paths, operator capability, DPAPI identity boundary, and limitations.
- The 2026-10-27 SQLite-only clarification accepts missing SQL Server/PostgreSQL/MySQL connectivity verification as a company-policy gap. It supersedes the former M4 blocker and instructions to obtain/test instances. Complete M4 after SQLite/reference/policy checks pass; preserve historical results as history, never as current engine support. Do not start M5 in this session.

## Permanent managed application database rules

- Follow PROJECT_REQUIREMENTS.md section 9.1: one dedicated Platform Registry plus optional independent application databases. NONE, MANAGED_SQLITE and EXISTING_SQLITE are future service modes, not currently verified lifecycle capabilities.
- Platform Registry and application databases must remain separate. Each managed application database belongs to exactly one application; never share its file with another application or alias the Registry as an application database.
- Never expose the Platform Registry, its file/configuration/credentials or internal management capabilities to deployed applications. Give an app only the minimum configuration for its authorized database.
- Store application database files, SQLite sidecars and backups outside every release directory and public/static tree. Use configurable IHP_STORAGE_ROOT and approved application-scoped storage. Treat the section 9.1.3 structure as a target; preserve existing physical paths/configurations unless a future authorized implementation explicitly migrates them safely.
- Validate canonical paths, file identities and authorization at every database operation; reject traversal, symlink/junction/reparse escapes, aliases and cross-application/Registry access through Platform APIs. An EXISTING_SQLITE file requires explicit registration and approved-location/ownership validation; registration alone never authorizes creation, migration or deletion.
- Do not claim that separate SQLite files or filesystem separation under the same Windows identity provide a security sandbox or OS-level isolation against malicious application code. Keep trusted-reviewed-code restrictions and document actual access limits; do not change Windows security settings to claim isolation.
- Never delete, replace or reset application data as an automatic side effect of deployment, update, restart, rollback, release cleanup, mode changes or application deletion. Data retention/deletion requires an explicit authorized decision; application rollback never automatically reverses database schema changes.
- Only SQLite may be used for real database operations under current policy. SQL Server/PostgreSQL/MySQL remain metadata-only; no real connection, authentication, migration, provisioning or connectivity test is permitted.
- Database backup, restore and deletion each require explicit authorization. Restore/deletion and other destructive actions also require explicit confirmation and verified target identity. Use an approved SQLite-consistent backup mechanism, never assume an active-file copy is safe, and coordinate downtime/writers for restore with a recoverable failure plan. Never overwrite an unrelated database.
- Enforce M11 RBAC and current application ownership server-side for database management; Viewer access is read-only. Do not expose raw database files through ordinary browser endpoints or database paths/contents/secrets through public Gateway routes or unauthorized APIs/logs.
- Initialization and schema migrations must be explicit, controlled and separately authorized. Never automatically execute uploaded database migration scripts, SQL files, package hooks or arbitrary commands.
- Audit database lifecycle actions, authorization denials, retention decisions and recovery outcomes without sensitive inputs. Do not claim provisioning/backup/restore works until implemented and verified; M10-DB remains NOT STARTED until separately assigned.

## M5 deployment engine rules

- The user explicitly authorizes M5 after completed M1–M4; this supersedes previous session-specific instructions to wait before M5. Do not start M6 or later milestones.
- Execute only reviewed, fingerprinted releases inside platform-owned Applications storage. Detection must inspect files without executing code. Never run arbitrary manifests, scripts, npm installs/builds, executable artifacts, or uploaded/unreviewed code.
- Use only installed node/dotnet hosts and published managed DLLs. Child environments must omit platform credentials, fix IPv4 loopback, and use the reviewed supervised-shutdown/health contract. Document that environment settings and process separation are not a sandbox.
- Use SQLite for persistent leases, intent, instances, exits and recovery budgets. Keep schema changes in reviewed explicit migrations. Never contact other database engines.
- Serialize Agent ownership and per-application operations, verify PID/start identity and listeners, and never terminate an unrelated port occupant or process discovered only by PID. Recovery must be bounded and persisted across Agent restarts.
- Preserve the API deployment prohibition until M11; M5 control is restricted to the trusted local operator/verification boundary. Do not expose Agent HTTP/network control.

## M6 dynamic routing rules

- The user now authorizes M6 only, superseding the previous session's wait for M6. Do not start M7 or later milestones.
- Preserve the M2 validator, static startup source, fixed 127.0.0.1:5050 listener, request limits and reserved paths. Runtime changes must be complete immutable validated snapshots with last-known-good retention on rejection.
- Load dynamic routes through a fixed loopback API endpoint and a separate read-only routing capability. Never accept arbitrary destination URLs/ports from route-management clients; derive destinations from approved running M5 registry state.
- Route-intent changes require the existing local operator capability. This is not M11 user authorization; application deletion and deployment/process execution APIs remain disabled.
- Verify real add/update/remove, both trusted samples, WebSockets, invalid snapshots, unchanged Gateway process and restart reconciliation using published managed DLLs. Stop only owned fixtures and preserve progress history.
- Use npm.cmd run test:routing for the dedicated managed Gateway/integration gate and npm.cmd run verify for the full regression. Consult local DYNAMIC_ROUTING.md for protocol/capability configuration and stale-source/isolation limits.

## M11 authentication and authorization priority

- The user explicitly prioritizes M11 before M7; this supersedes the M6 handoff's next-milestone order. Implement M11 only; do not start M7 or other milestones.
- Preserve M1–M6, loopback/no-.exe/SQLite-only policy and progress history. Actual upload/deployment/rollback execution remains disabled even when approval exists.
- Use local password hashing and revocable server-side sessions with no native executable dependency. Never log passwords, tokens, client binding, credential material or raw authentication inputs.
- Bootstrap the first Administrator only through the configured local operator capability. Once users exist, that legacy capability must not bypass session/RBAC/ownership/approval enforcement.
- Enforce authorization and sensitive state changes atomically, protect all endpoints by default, scope application reads/writes, prevent self-approval/escalation and audit security actions without sensitive inputs.
- Document loopback HTTP/client trust and same-user limitations accurately; no corporate/SSO/TLS/OS security configuration is authorized by this milestone. Verify real HTTP security behavior and full M1–M6 regression before marking M11 completed.
- Use npm.cmd run test:auth for focused security checks and npm.cmd run verify for the full gate. Read local AUTHENTICATION.md for bootstrap transition, native-client protocol, permission matrix, transaction boundary, approval/execution separation and operational limits.

## M7 package workflow authorization

- The attached user request explicitly authorizes M7, superseding the M11 wait. Do not start M8 or other milestones.
- Require real user sessions/RBAC/ownership for uploads and package actions; the pre-bootstrap maintenance capability must never upload or execute packages.
- Never execute package code during ZIP/metadata inspection. Reject traversal, links, unsafe artifacts, malformed/oversized archives and unsupported commands. Use platform-owned storage and preserve previous releases.
- Source builds remain disabled unless an explicitly reviewed safe build boundary is established; no uploaded scripts/npm installs/shell/build commands are authorized. Same-user processes are not a sandbox.
- Only exact-fingerprint, distinct-Administrator-reviewed trusted prebuilt releases may enter the controlled Agent workflow. Recheck approval, current user authority and hashes before promotion/start; preserve M5 ownership/loopback and M6 validation.
- If safe source builds/execution cannot be implemented, record the exact limitation and leave M7 IN PROGRESS or BLOCKED as appropriate; do not weaken acceptance or invent verification. Update/push only the four records and verify origin/main.

## M7 folder-intake and startup-script rules

- Folder intake is a proposed M7 workstream, not a claim that the Dashboard/API supports it. Keep the existing strict ZIP operation backward-compatible. Do not mark M7 complete; source-build/isolation acceptance remains unresolved.
- Enforce the inclusive 52,428,800-byte folder file-payload limit on bytes actually received; separately bound multipart framing. Preserve the compressed 50 MiB limit and all existing expanded/file/count/ratio/CRC/path/type/ownership/approval budgets. Do not increase extraction or entry limits without a threat analysis and explicit approval.
- Treat browser relative paths, metadata, file sizes, runtime claims and run.cmd as hostile input. Validate and normalize paths server-side inside owned staging; do not trust client UUIDs/manifests or run .gitignore. Do not log secret contents or credentials.
- Never execute run.cmd/.bat, cmd.exe /c, PowerShell, npm hooks, install/build scripts or arbitrary commands. Startup detection is static only. Translate only exact documented startup patterns to validated allowlisted Node/dotnet host arguments; unknown/ambiguous intent stays review-only and deployment-blocked. Administrator review alone is not authority to execute an unsafe command.
- Source-only uploads may be retained only in separately authorized protected review storage. They are not release artifacts and cannot reach Agent preparation/start while approved build isolation is unavailable. Keep upload accepted, package validated, ready, build required and deployment blocked distinct; do not infer backend status.
- Generate application identity, unique package token, manifests, package archives and fingerprints server-side from authenticated Registry state. Node package metadata must contain no scripts; use only already-resolved runtime dependencies and never install them. Preserve M11 ownership/authorization and distinct-Administrator exact-package review. Upload does not approve, build or deploy. Preserve SQLite-only, M10-DB NOT STARTED, no-.exe, no-Docker, no-elevation, loopback and no-existing-application-modification restrictions.

## M8 dashboard assignment

- The latest user request authorizes M8 despite unfinished M7; this supersedes older wait instructions. Preserve M7 IN PROGRESS and its source-build/isolation dependency. Do not start M9 or any other milestone.
- Build against existing API contracts and preserve M11 guards. Never disguise arbitrary browser requests by stripping security headers, forward browser-supplied native credentials, paste native tokens or enable CORS as a workaround. The explicitly authorized follow-up may construct only independently verified, allowlisted Gateway operations with server-held M11 sessions.
- If approved browser authentication is unavailable, disable affected login/management functionality explicitly and keep M8 IN PROGRESS/BLOCKED. Tests with owned fixtures or native clients are not evidence of approved browser authentication.
- Do not invent runtime metrics/history endpoints or M9 monitoring. Show only available registry/package/route/audit evidence; label unavailable capabilities accurately.
- Keep real listeners loopback-only, managed .NET verification/publish policy, SQLite-only operations and four-document-only progress delivery. Run the full regression and verify origin/main after pushing.

## M8 browser authentication follow-up

- Bind queued browser operations to the session identity that scheduled them; do not let an old intent execute under a later login. Discard private responses after session expiry/clearing. Preserve backend authorization, explicit disruptive-action confirmation and honest unsupported-operation labels. Keep startup diagnostics phase/code-only, verify actual API/SQLite health before dependent components, and never kill an unrelated port owner or report an unreproduced historical failure as conclusively resolved.

- The latest user explicitly authorizes designing/implementing a browser authentication boundary. Preserve original M11 native Origin/session/RBAC/ownership checks; no generic proxy or direct native token in the browser is allowed.
- Isolate the management hostname from application routing on the existing 127.0.0.1:5050 listener. Do not use cookies/path separation as an origin boundary or expose session material to application routes.
- Implement only explicit browser operations with independent Host/Origin/Fetch Metadata/CSRF/replay/session checks and server-side M11 authorization. Secrets/keys remain memory-only; no localStorage/sessionStorage/URL credentials.
- Keep new browser flow opt-in/default disabled until verified. Do not launch browser executables/install drivers or claim actual-browser acceptance from Node/DOM tests. Record transport limitations and unfinished acceptance accurately; M7 remains IN PROGRESS and M9 is not assigned.
