# House of Hunterz: Planned Features, Current Baseline, and Gaps

**Assessment basis:** source-code review and checks performed during the November 2026 development session. This is not a production-readiness certification. Features marked as present have implementation evidence in the repository; they have not all been exercised end to end.

## Status key

- **Present** — supporting code or UI exists; end-to-end operation may still need verification.
- **Partial** — some supporting code/UI exists, but the specified workflow or important behavior is incomplete.
- **Missing** — no substantive implementation was identified in the reviewed source.
- **Blocked / unverified** — cannot be confirmed operationally with the current local configuration or test coverage.

## Executive summary

The repository contains a basic web security-bounty platform foundation: account registration and email verification, role-based screens, programs/opportunities, findings, moderation, reputation, notifications, and payout records.

It does **not** yet deliver several key requirements in the product plan: KYC/KYB, a complete duplicate-report and dispute process, real payment execution and escrow, DAO governance, or wallet/blockchain integration. The existence of a UI, route, or database field is not proof that the user journey works.

Local end-to-end operation was blocked during the previous startup checks because PostgreSQL was unavailable at `localhost:5432` and the configured `JWT_SECRET` did not satisfy the backend's requirements. The frontend production build and Prisma schema validation passed after a frontend JSX correction, but backend integration tests could not be run against a live database. No application server was started for this document.

## Planned feature checklist

### Company, program, and report features

| Planned requirement | Baseline found | Status | Work still needed |
|---|---|---|---|
| Company bounty/program details view | Company and opportunity detail routes/pages and supporting API code exist. | Present / unverified | Verify correct company permissions, saved program data, and every detail/action against a running API. |
| Bounty reports list page | Finding/report screens and list endpoints exist across researcher, company, admin, and moderator areas. | Present / unverified | Verify filtering, pagination, role visibility, and that each view returns the intended records. |
| Bounty report details page | Finding detail pages and API handlers exist. | Present / unverified | Exercise the full report lifecycle, attachments, comments, decisions, and notifications. |
| Company program lifecycle | Project, bounty, and competition screens/routes exist. | Partial | Verify create/edit, approval, launch/pause/close, scope changes, and audit history as a coherent workflow. |
| Super-admin all screens and actions | A substantial admin area exists. | Partial / unverified | Build a role-by-role screen/action inventory and test authorization and every supported operation. |

### Duplicate vulnerability report policy

The plan requires a fair and transparent “first valid report” policy, not only a duplicate flag.

| Planned requirement | Baseline found | Status | Work still needed |
|---|---|---|---|
| Submission timestamping | Findings have creation timestamps. | Partial | Preserve an auditable submission/decision timeline and use it in triage. |
| Triage and duplicate determination | Findings can be assigned duplicate fields/status through the finding moderation code. | Partial | Define and implement criteria, permissions, evidence capture, and validation of the original report. |
| Reward the first valid report | No complete first-valid-report reward workflow was verified. | Missing / unverified | Define validity, priority, payout eligibility, and how the first report is determined. |
| Exact duplicate handling | Duplicate status and linkage fields exist. | Partial | Implement reporter/company notifications, rationale, and reward outcome. |
| Similar reports, same vulnerability | No complete decision procedure was verified. | Missing | Add triage guidance and a recorded decision distinguishing same issue from materially different issues. |
| Different vulnerabilities in the same area | No complete decision procedure was verified. | Missing | Ensure separate issues can be assessed independently, with clear evidence and rationale. |
| Incomplete first report edge case | No verified response window or priority-retention workflow was found. | Missing | Define required report information, clarification requests/deadline, and effect on priority. |
| Dispute/appeal when duplicate is challenged | Moderator dispute endpoints are placeholders; there is no dispute model in the Prisma schema. | Missing | Add appeal submission, evidence, assignments, states, reviewer decision, rationale, and notifications. |
| Transparency and continuous improvement | No complete policy/audit/feedback workflow was verified. | Missing / partial | Record decisions and policy versions; review outcomes and collect feedback. |

Relevant starting points: [Finding model](backend/prisma/schema.prisma), [finding status controller](backend/src/controllers/findingController.js), and [moderator routes](backend/src/routes/moderator.js).

### Hunter and company verification

| Planned requirement | Baseline found | Status | Work still needed |
|---|---|---|---|
| Hunter personal/contact details and identity documents | No KYC submission/provider workflow or identity-document model was identified. | Missing | Decide the minimum required fields/documents and implement secure, access-controlled submission and review. |
| Hunter proof of address and skills/experience | Profile/skills fields exist, but no complete verification workflow was identified. | Partial / missing | Decide what is collected and what is verified; record verification evidence and status. |
| Company organization/contact/business documents | Organization profiles and basic verification flags exist, but not a full KYB workflow. | Partial | Add business documents, reviewer workflow, statuses, and decision history. |
| Financial information, if required | No verified collection/verification flow was identified. | Missing / undecided | Decide whether financial verification is required and define appropriate data minimization/security. |
| Automated KYC/KYB provider (e.g. Sumsub/Veriff) | No provider integration was identified. | Missing | Select a provider, define API/webhook handling, and ensure failures/manual review are supported. |
| AI-powered verification | No implementation was identified. | Missing / undecided | Define acceptable use, human review, privacy, error handling, and whether it is in scope. |
| Blockchain verification | No implementation was identified. | Missing / undecided | Clarify what is to be verified on-chain and whether this means identity, credentials, or transactions. |
| Secure identity-data storage | No KYC-specific storage/retention/access controls were identified. | Missing | Specify encryption, least-privilege access, retention/deletion, audit logging, and incident handling before collecting documents. |
| Verification status and gated access | Email verification and account roles exist; no KYC/KYB status/access flow was identified. | Partial | Define which actions require verified identity/business status and enforce it on the backend. |
| Ongoing monitoring and re-verification | No complete process was identified. | Missing | Define triggers, review queues, user notice, and status changes. |
| MFA | No MFA/second-factor implementation was identified. | Missing | Select supported factors, recovery flow, and enforcement policy. |
| Fraud detection, regular audits, user education | No complete system/process was verified. | Missing / partial | Define operational controls, audit cadence, fraud response, and user-facing guidance. |

Email verification in [auth routes](backend/src/routes/auth.js) and [authentication controller](backend/src/controllers/authController.js) verifies control of an email address; it is not KYC or KYB.

### Disputes, moderation, and communications

| Planned requirement | Baseline found | Status | Work still needed |
|---|---|---|---|
| Moderator review screens | Moderator dashboard, findings, reports, and review routes exist. | Partial / unverified | Test each operation with real moderator/admin roles and verify authorization on every route. |
| Duplicate appeals/dispute inbox | Dispute UI/API surfaces exist, but the API behavior is placeholder code. | Missing | Implement persistent disputes and a complete resolution workflow. |
| Evidence and decision history | Audit-log and finding fields exist. | Partial | Link all dispute evidence, communications, decisions, and changes to an immutable/auditable timeline. |
| Clear user/company communications | Email and notification infrastructure exists. | Partial / unverified | Connect each triage, duplicate, appeal, and resolution state transition to clear notifications. |
| Report handling | Some report/moderation behavior exists. | Partial | Verify reports are persisted, assigned, acted upon, and visible to permitted users. |

### Reputation and ranking

| Planned requirement | Baseline found | Status | Work still needed |
|---|---|---|---|
| Reputation score and history | Reputation fields, logs, service logic, profile/dashboard UI exist. | Present / unverified | Test score changes, bounds, audit behavior, and consistency across all views. |
| Researcher rankings/leaderboard | Public leaderboard-related routes/UI exist. | Present / unverified | Confirm ranking rules, tie handling, privacy controls, and database-backed behavior. |
| Duplicate-aware reputation policy | Duplicate data and reputation logic exist, but the policy has not been verified against all edge cases. | Partial | Define and test whether duplicate/incomplete/disputed reports affect reputation and when. |

Starting points: [reputation service](backend/src/services/reputationService.js) and [Prisma schema](backend/prisma/schema.prisma).

### Payouts, escrow, DAO, and Web3

| Planned requirement | Baseline found | Status | Work still needed |
|---|---|---|---|
| Payout record and earnings views | A `Payout` model and earnings/admin/public reporting code exist. | Present as records only | Reconcile status values and verify all totals/statuses against actual payout records. |
| Actual payout transfer | No payment processor or transfer execution was identified. | Missing | Choose provider/currency, implement execution, idempotency, failure/retry/reconciliation, receipts, and audit. |
| Escrow funding and balance | No escrow model, contract, or lifecycle was identified. | Missing | Decide whether escrow is optional/default; define funding, custody, release, refunds, fees, and disputes. |
| Smart contracts/on-chain escrow | No Solidity contracts or chain integration were identified. | Missing | Define chain/security requirements, implement contracts, test/audit them, and plan deployment/monitoring. |
| Wallet connection and transaction verification | No wallet or transaction integration was identified. | Missing | Decide supported wallets/chains and implement signature/transaction verification if required. |
| DAO governance | No proposals/voting/governance implementation was identified. | Missing | Define membership, proposal rights, voting, quorum, execution, and governance security. |

**Known payout consistency issue:** the schema/public reporting use `COMPLETED`, while some researcher earnings code checks `PAID`. Resolve status definitions centrally before relying on totals. Finding status handling also notes payout creation is separate and does not execute a payment. See [Payout model](backend/prisma/schema.prisma), [finding controller](backend/src/controllers/findingController.js), and [researcher routes](backend/src/routes/researcher.js).

### Other platform capabilities

| Planned capability | Baseline found | Status | Work still needed |
|---|---|---|---|
| Notifications | Notification model/routes, email, web-push, and Socket.IO-related code exist. | Partial / unverified | Test delivery, preferences, unread counts, reconnects, and failure handling end to end. |
| File uploads | Upload routes and S3-related code exist. | Partial / environment-dependent | Verify provider configuration, access control, limits, malware/content handling, and safe downloads. |
| Analytics/admin monitoring | Analytics and admin surfaces exist; some areas are mock/placeholder or not verified against real data. | Partial | Replace placeholders, test authorization, and verify calculations against persisted records. |
| Product policies/terms | Terms-related page/documentation exists. | Partial | Finalize policy decisions and align terms with real duplicate, dispute, KYC/KYB, payout, and escrow behavior. |

## Engineering and operational blockers

| Issue | Observed baseline | Required action |
|---|---|---|
| PostgreSQL unavailable locally | Previous startup attempt could not connect to `localhost:5432`; API then exited before listening. | Start/configure a database and verify the backend's local database URL privately. |
| Invalid JWT secret | Previous startup check rejected the configured secret because it was not a unique random value of at least 32 characters. | Set a strong, unique local secret; do not commit it or share it. |
| Schema setup for new environments | Prisma schema validation passed, but no Prisma migration directory/files were found in the repository. | Create and review repeatable migrations (or an explicitly documented safe development setup) before relying on a fresh database. |
| Backend automated test command | `backend/package.json` defines `test` as an error placeholder. | Add a maintained test runner and automated unit/integration tests; keep database tests isolated. |
| Existing test scripts are ad hoc | There are standalone `test-*.js` scripts, not a cohesive automated suite. | Review, isolate, and integrate suitable tests into the test runner. |
| Risk in `backend/test-auth.js` | The script falls back to a known weak JWT secret, prints user emails/token excerpt, and overwrites the first user's push subscription with dummy data without restoring it. | Do not use against real data; remove the weak-secret fallback and sensitive output, use an isolated test database/fixture, and clean up test mutations. |
| API/socket integration | The API and Socket.IO ports were not listening during the previous check because backend startup stopped at configuration/database checks. | After configuration, test health, auth, CORS, API flows, and socket authentication without exposing secrets. |
| Frontend build warnings | Production build passed after fixing three malformed Markdown preview expressions; lint/accessibility warnings remain. | Triage warnings and add a documented lint/type-check command to CI. |
| CI/release confidence | No complete CI-backed test/build workflow was verified. | Add CI for formatting/lint, frontend build, backend tests, Prisma validation/migrations, and security checks. |

## Recommended implementation order

1. **Make local development reproducible:** configure PostgreSQL and a valid local JWT secret; add reviewed Prisma migrations; document safe setup without committing credentials.
2. **Add reliable tests:** replace the placeholder test command; isolate and fix risky ad-hoc tests; cover authentication/authorization, programs, findings, duplicate decisions, reputation, and payout records.
3. **Finish the core bounty lifecycle:** company program management, report triage, status communication, duplicate decisions, first-valid-report policy, evidence, audit history, and appeals.
4. **Build complete dispute resolution:** persistence, submission, evidence, assignment, decision, notifications, and appeal history.
5. **Choose verification requirements:** decide collected data, provider, review responsibilities, access gates, MFA, privacy/retention, fraud controls, and re-verification; then implement KYC/KYB.
6. **Implement payment operations:** settle status semantics, choose payment providers/currencies, and build transfer/reconciliation behavior. Define escrow rules before selecting or writing a custody/contract design.
7. **Decide Web3/DAO scope:** confirm whether DAO, wallets, and on-chain escrow are required for launch; define chains and governance/security requirements before implementation.
8. **Operationalize launch readiness:** verify every role's screens and permissions, complete notification/upload checks, replace mocks, add CI, and test deployment/monitoring/recovery.

## Priority, implementation location, and effort

These are planning estimates, not commitments. Engineering effort is in **person-weeks** (one engineer working full time for one week). Calendar estimates assume two experienced engineers, product decisions made promptly, and no major vendor or legal delays. Work can overlap, so person-weeks do not add directly to calendar weeks.

| Gap / workstream | Priority and why it matters | Where the work belongs | Rough engineering effort |
|---|---|---|---|
| Reproducible environment, migrations, tests, and CI | **P0 — release blocker.** Without these, changes cannot be applied reliably to fresh environments or safely verified. The current backend test command is a placeholder, and the previous local startup was blocked. | Backend setup docs and scripts; `backend/prisma/` migrations; backend test runner; CI configuration at repository root. Keep secrets in environment/secret storage, never in source. | **2–4 person-weeks** |
| Program and finding lifecycle | **P0 — core product.** Researchers and companies need a coherent, permission-checked path from program creation through report triage, resolution, and closure. Existing screens and endpoints do not establish that the full lifecycle works. | Backend domain rules in finding/program services and controllers, API routes, Prisma models/migrations; matching company/researcher/moderator pages and API hooks in `frontend/`. | **4–7 person-weeks** |
| Duplicate policy, appeals, moderation, and decision history | **P0 — trust and fairness.** Incorrect duplicate decisions can deny valid rewards and damage researcher/company trust. Placeholder disputes and an undefined first-valid-report policy leave this unresolved. | Define policy first; persist disputes, evidence references, policy version, assignments, decisions, and event history in Prisma; enforce transitions and permissions server-side; add moderator and reporter views and notifications in the frontend. | **3–6 person-weeks** |
| Authorization, account protection, and verification gates | **P0/P1 — security and abuse prevention.** A visible role-specific screen is not an authorization boundary. Email verification proves mailbox access, not identity or business legitimacy. | Backend auth middleware and route/service checks; tests for role/ownership boundaries; frontend access states. Add MFA in auth flows. Add provider-backed KYC/KYB only after deciding what actions require it. | **2–4 person-weeks** for authorization review/tests and MFA; **3–6** for a provider-backed KYC/KYB workflow |
| Payout execution and reconciliation | **P0 if the platform promises rewards; otherwise P1 before paid launch.** A payout record is not a transfer. Inconsistent `COMPLETED`/`PAID` semantics can also make earnings inaccurate. | Normalize payout states in the backend schema/service first; isolate provider calls behind a payment service; add idempotency, retries, reconciliation, receipts, and admin review. Frontend shows authoritative statuses and failure states. | **3–6 person-weeks**, provider and country dependent |
| Secure uploads and sensitive data handling | **P0 before accepting sensitive evidence or identity documents.** Unsafe access, retention, or file handling can expose private reports or identity data. | Backend upload authorization, validation, storage policy, and download access; frontend upload UX. Keep KYC documents with the identity provider where possible instead of storing copies. | **2–4 person-weeks** to harden and verify the existing upload path; KYC privacy/retention design is additional |
| Notifications and communications | **P1 — operationally important.** Users need to know about triage, requests for clarification, duplicate decisions, appeals, and payment outcomes; silent transitions create avoidable disputes. | Backend domain-event/state-transition handlers and notification/email services; frontend preferences, unread state, and actionable messages. | **1–3 person-weeks** to complete and test the core lifecycle notifications |
| Production readiness and operations | **P0 before public launch.** A functioning feature is not enough without monitoring, backups/restore, error reporting, deployment checks, and incident procedures. | Deployment/CI configuration, backend health/structured logs, database backup and restore runbooks, monitoring/alerts, and release documentation. | **2–4 person-weeks**, partly parallel with feature work |
| Analytics, ranking, and admin completeness | **P1/P2 depending on operational use.** Incorrect metrics can mislead operators; rankings need explicit, tested rules and privacy decisions. | Backend aggregation and authorization; admin/researcher UI; tests for calculations. Separate real persisted metrics from mock/demo data. | **2–4 person-weeks** for launch-critical admin reporting; more for product analytics |
| Escrow, wallet, smart contracts, and DAO | **P2 — optional product direction, not a prerequisite for a credible web bounty MVP.** Custody, irreversible transfers, contract flaws, legal obligations, and governance abuse materially expand risk. | Separate architecture and threat/legal review first; isolated payment/chain integration and contract repositories/services if approved. Do not mix wallet/DAO assumptions into core finding state transitions. | **At least 8–16+ person-weeks** for an initial escrow/on-chain slice, excluding independent audits, legal work, and operational approvals; DAO is a separate scope |
| Product policy, privacy, and legal review | **P0/P1 wherever rewards, disputes, or identity checks are involved.** Engineering cannot safely invent definitions for report validity, appeal windows, custody, data retention, or regional eligibility. | Product/legal decisions and published policy; reflect approved rules in backend state transitions, audit records, and user-facing explanations. | **Not reliably estimable as engineering work.** Allow several weeks for decisions/review; vendor and jurisdiction choices may extend this. |

### What “full-fledged” should mean for the first release

A credible first public release should complete the core program/report lifecycle, enforce server-side authorization, support evidence and fair duplicate/appeal decisions, communicate every important state change, and make any advertised rewards traceable through real payout execution. It also needs migrations, automated tests, backups, monitoring, and an operator workflow. KYC/KYB should be added only to the actions and jurisdictions that require it, using a provider and privacy-reviewed process.

**Indicative calendar:** with two experienced engineers and timely product decisions, budget roughly **10–16 weeks** for a focused, launchable web marketplace without escrow/DAO, assuming existing UI and APIs can be reused. A more complete general-availability release including payment integration, verification-provider workflow, operational hardening, and acceptance testing is more realistically **3–5 months**. One engineer working alone could take roughly **6–10 months**. These ranges exclude substantial redesign, vendor onboarding delays, legal review, and independent security audits.

If on-chain escrow and DAO governance are mandatory for launch, treat them as a separate program: expect **6–12+ months** for a responsibly scoped product with external contract/security review, and do not treat the estimate as a substitute for an audit. The recommended path is to launch and validate the core marketplace first, then make an explicit product decision on Web3.

## Validation record and limits

- Frontend production build: **passed** after correcting malformed JSX in the comment edit/reply/new-comment Markdown previews; existing warnings remain.
- Prisma schema validation: **passed**.
- Backend JavaScript syntax checks: **passed** for 52 source files.
- Backend runtime/API/database workflows: **not verified** because PostgreSQL and valid JWT configuration were unavailable during the previous startup checks.
- User-facing product features: presence in source does not prove the full workflow works; verify with integration and acceptance tests once the backend is configured.

