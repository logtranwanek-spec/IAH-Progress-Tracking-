# Repository Instructions for Codex and Development Agents

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
