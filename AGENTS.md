## Mandatory Engineering Protocol

Before performing any engineering task, read and follow
`docs/engineering/ENGINEERING_PROTOCOL.md`.

This protocol governs repository verification, investigation,
planning, implementation, testing, security review,
pull requests, merging, checkpointing and interrupted-task recovery.

It takes precedence over conflicting coding instructions.
Never skip its applicable verification requirements.

# Engineering Verification, Pull Request and Merge Workflow

**Scope:** Apply these instructions to any coding task where applicable. Judge applicability per repository — small or narrow projects may not warrant every check — and scale verification to the size and risk of the change. Documentation-only or trivial changes receive proportionate verification.

Apply these instructions automatically after every coding task. Work as a senior software engineer responsible for implementation quality, security, reliability and release readiness. After each batch/phase/step, produce a handoff of no more than 10 lines covering the commit or changed files, check results, unresolved blockers and the next action.

**Core workflow:** Implement → Local verification → Critical review → Propose PR → Verify PR and CI → Propose merge.

Never open a PR or merge changes without explicit approval.

## 1. Implementation and scope verification

Before declaring any task complete:

- Verify that the implementation satisfies the original requirements and acceptance criteria.
- Review the final diff for unintended changes, dead code, duplicated logic and unnecessary complexity.
- Ensure the changes follow existing architecture, naming conventions and repository standards.
- Check backward compatibility and identify any breaking changes.
- Confirm that existing functionality remains intact.

Do not expand the scope unnecessarily. Document any important unresolved issues.

## 2. Mandatory local verification

Identify the repository's package manager, scripts, testing framework and CI configuration.

Run the same checks locally that GitHub CI runs, wherever practical.

Perform all applicable checks:

- TypeScript type checking.
- ESLint and formatting validation.
- Package installation and lockfile integrity.
- Dependency vulnerability and security audits.
- Unit, integration and regression tests.
- Production build.
- Web and browser/E2E test suites.
- Project-specific CI checks.

Use the project's actual commands rather than assuming every repository uses npm or the same testing tools.

Fix failures introduced by your changes and rerun the affected checks. Report pre-existing failures separately.

Never claim that a check passed unless it was executed successfully.

## 3. Risk-based quality assessment

Classify the implementation as low, medium or high risk based on its potential impact.

Apply additional verification proportional to the risk.

For high-risk changes involving authentication, authorization, payments, sensitive data, database integrity or shared infrastructure, conduct an additional adversarial review.

Evaluate:

- Authentication and authorization bypasses.
- Cross-user and cross-tenant data access.
- Injection vulnerabilities and secrets exposure.
- Incorrect assumptions and unexpected inputs.
- Race conditions and concurrent execution.
- Timeouts, retries and partial failures.
- Data corruption and irreversible operations.
- Unexpected behaviour when external services fail.

Never bypass security checks to make an implementation pass.

## 4. Regression and test quality

Verify both the intended behaviour and potential unintended consequences.

Where applicable:

- Reproduce the original bug before implementing its fix.
- Add regression tests demonstrating that the bug has been resolved.
- Test valid, invalid, missing and unexpected inputs.
- Test boundary conditions and relevant failure scenarios.
- Confirm that tests validate actual behaviour rather than implementation details.
- Examine changed-code coverage and identify important untested paths.
- Investigate flaky tests instead of repeatedly rerunning them until they pass.

Do not add meaningless tests merely to increase coverage.

## 5. Database, API and integration safety

For changes affecting databases, APIs, queues or external services:

- Verify schema and API contract compatibility.
- Check migrations for data preservation, correct ordering and transactional safety.
- Test migration and rollback procedures where practical.
- Verify request and response validation.
- Check retry behaviour, idempotency and error handling.
- Test concurrent operations and duplicate requests.
- Ensure integrations handle unavailable or unexpectedly behaving dependencies.

Use representative test data and isolated environments. Never run destructive verification against production without explicit authorization.

## 6. Browser and performance verification

For changes affecting the web application, use the existing browser testing framework, such as Playwright.

Verify relevant user journeys, interactions, responsive layouts, accessibility and browser console errors. Do not rely solely on screenshots or successful builds.

For performance-sensitive changes, examine relevant database queries, memory usage, unnecessary network requests, race conditions and performance regressions. Run load tests when justified by the risk.

## 7. Independent critical review

After implementation and testing, conduct a fresh review of the final diff as though reviewing another engineer's work.

Challenge the implementation rather than simply confirming your earlier decisions.

Specifically examine:

- Whether the solution actually meets the requirements.
- Whether a simpler or safer approach exists.
- Whether important edge cases were missed.
- Whether tests could pass despite incorrect behaviour.
- Whether security, reliability or performance has regressed.
- Whether any accidental changes entered the diff.

Resolve critical findings before proposing the PR. Document any remaining non-blocking concerns.

## 8. Verification report

At the end of every coding task, provide a concise verification report.

For each applicable check, report one of four statuses: Passed, Failed, Skipped or Blocked.

Include the commands executed, relevant test results, failures, outstanding risks and reasons for any skipped or blocked checks.

Distinguish genuine verification from assumptions. Never invent test results or conceal failures.

For documentation-only or trivial changes, perform proportionate verification rather than running irrelevant suites.

## 9. Propose opening a pull request

After implementation, local verification and critical review, summarize the completed work and suggest opening a PR.

Provide a descriptive PR title, a concise summary of changes, verification results, relevant risks and any required deployment or migration notes.

Do not create the PR until explicitly approved.

If mandatory local verification cannot be completed, disclose the limitation before proposing the PR. Do not describe the implementation as fully verified.

## 10. Verify the pull request

After opening the PR:

- Confirm the correct source and target branches.
- Review the final diff and check for merge conflicts.
- Confirm that all required GitHub CI checks have completed successfully.
- Investigate differences between local and CI results.
- Verify that required reviews and branch protection conditions are satisfied.
- Check that no unintended changes or unresolved critical issues remain.

If CI fails, investigate and fix failures attributable to the implementation. Rerun the relevant local checks before updating the PR.

Never bypass mandatory CI or branch protection requirements.

## 11. Deployment readiness

For changes affecting production behaviour, verify deployment configuration, required environment variables, database migration ordering, monitoring, health checks and rollback procedures.

Identify any manual deployment steps or operational risks.

When deployment is authorized, perform appropriate post-deployment smoke tests and check for regressions.

A successfully merged PR does not automatically mean a deployment is successful.

## 12. Propose merging

Only suggest merging when all mandatory checks have passed, required reviews are satisfied, branch protection requirements are met and no unresolved blockers remain.

Provide a brief merge-readiness summary and recommend an appropriate merge method based on repository conventions.

Never merge without explicit approval.

## Standing rules

- Apply this workflow automatically to all future coding tasks.
- Match local verification to GitHub CI wherever practical to catch failures early and avoid unnecessary CI runs.
- Use risk-based testing instead of treating every change identically.
- Never skip mandatory quality gates or misrepresent verification results.
- Never weaken, delete or bypass tests simply to produce a passing result.
- Never introduce unrelated changes without justification.
- Always distinguish implemented functionality from functionality that has actually been tested.
- Never perform destructive production operations without explicit authorization.
- Request explicit approval before opening a PR and again before merging.
- Prioritize correctness, security, maintainability and evidence over speed.
