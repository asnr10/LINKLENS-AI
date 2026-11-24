# LINKLENS AI — Remaining Operations & Implementation Roadmap

Updated: 2026-11-24
Repository: `asnr10/LINKLENS-AI`
Branch: `main`

This document is a forward-looking task register. Items below are **pending unless explicitly marked otherwise**. It is not evidence that the features have been implemented or tested.

## Phase 0 — Synchronize the local project

- [ ] Open the local workspace at `E:\LINKLENS-AI` in VS Code.
- [ ] Inspect `git status`, current branch, and configured remote before changing anything.
- [ ] Compare the local tree with the GitHub repository.
- [ ] Review and preserve the private-repository setting.
- [ ] Add and commit the verified local source tree, including the API, tests, and web app configuration.
- [ ] Exclude virtual environments, caches, secrets, local databases, and generated files using an appropriate `.gitignore`.
- [ ] Verify the pushed files and commit on GitHub.
- [ ] Never commit API keys, passwords, access tokens, or real personal/financial data.

**Acceptance criteria:** the intended source files are visible in the private GitHub repository; no environment folder, secret, or real sensitive dataset is committed; the commit is traceable.

## Phase 1 — Stabilize the current API

- [ ] Run the complete local pytest suite after repository synchronization.
- [ ] Add API-level tests for `GET /health` and `POST /analyze`.
- [ ] Test empty, whitespace-only, oversized, Arabic, English, benign, and suspicious inputs.
- [ ] Confirm consistent response schemas and validation errors.
- [ ] Add malformed URL and URL-extraction edge-case tests.
- [ ] Handle malformed ports safely so URL parsing cannot cause an unhandled exception.
- [ ] Review URL punctuation trimming, IPv4/IPv6 parsing, hostname normalization, and Punycode handling.
- [ ] Record test results; do not describe previously reported local tests as GitHub CI results.

**Acceptance criteria:** automated tests pass locally and API responses remain schema-consistent for valid and invalid input.

## Phase 2 — Domain Intelligence

- [ ] Define a normalized domain data model.
- [ ] Implement safe hostname and internationalized-domain normalization.
- [ ] Add lookalike/typosquatting indicators with explainable matching evidence.
- [ ] Design optional DNS, domain-age, TLS, and reputation enrichment behind explicit, controlled provider interfaces.
- [ ] Record source, retrieval time, evidence, and confidence for every external indicator.
- [ ] Treat unavailable or stale intelligence as unknown, not as proof of safety.
- [ ] Do not fetch arbitrary user URLs as part of the current static analyzer.

**Security gate:** any future network lookup must prevent SSRF, DNS rebinding, redirects to localhost/private/link-local/reserved IP ranges, unsafe schemes, excessive response sizes, and unbounded timeouts. Use controlled egress and strict validation.

**Acceptance criteria:** domain indicators are reproducible, source-attributed, test-covered, and safe by design.

## Phase 3 — Unified Explainable Risk Engine

- [ ] Define the relationship between message findings and link/domain findings.
- [ ] Prevent duplicate counting when the same URL signal appears in multiple analyzers.
- [ ] Separate raw indicators, severity, confidence, and final risk classification.
- [ ] Document score weights, thresholds, limitations, and versioning.
- [ ] Add unit tests for score boundaries and duplicate-indicator cases.
- [ ] Keep the output explainable; do not present uncalibrated scores as fraud probabilities.

**Acceptance criteria:** every final score can be traced to distinct evidence and documented scoring rules.

## Phase 4 — Data and Investigation Foundation

- [ ] Design PostgreSQL entities for messages, links, domains, indicators, evidence sources, cases, and audit events.
- [ ] Use synthetic records for development and demonstrations.
- [ ] Define evidence provenance, timestamps, retention, and deletion requirements.
- [ ] Add role-based access control and audit logging before multi-user case access.
- [ ] Consider graph relationships using relational tables first; defer a dedicated graph database until justified.

**Acceptance criteria:** data relationships and evidence lineage are documented, access is controlled, and synthetic test data is available.

## Phase 5 — Web Application

- [ ] Inspect the existing Next.js app under `apps/web`.
- [ ] Define Arabic and English UI behavior, including RTL/LTR layout.
- [ ] Build a message/link analysis submission screen.
- [ ] Display risk level, evidence, findings, recommendations, and limitations clearly.
- [ ] Add an investigation/case view only after the API and data model are stable.
- [ ] Connect the frontend to the API through environment-based configuration.
- [ ] Test loading, validation, API failure, and mobile layouts.

**Acceptance criteria:** the web interface accurately reflects API output and does not imply certainty or capabilities the backend does not have.

## Phase 6 — Quality, Security, and Delivery

- [ ] Add CI to run linting and automated tests on pushes and pull requests.
- [ ] Add dependency and secret scanning.
- [ ] Review authentication, authorization, input validation, rate limiting, CORS, logging, and error handling.
- [ ] Add synthetic end-to-end test scenarios.
- [ ] Document local setup, architecture, API contracts, and known limitations.
- [ ] Prepare a demo with clearly labeled synthetic data.
- [ ] Conduct a security and privacy review before any pilot involving real users or data.

**Acceptance criteria:** reproducible setup, passing CI, documented limitations, and a reviewed security baseline.

## Product scope and non-claims

- Supported languages: Arabic and English only.
- The current implementation is an early proof of concept, not a production fraud-detection service.
- A risk indicator is not proof of fraud, identity theft, or impersonation.
- No operational connection to police, banks, telecom providers, payment rails, or transaction-freezing systems is established.
- Any future institutional integration requires authorization, agreements, security review, and applicable legal/privacy controls.
- AI may assist with summarization and analyst workflows; high-impact decisions must not rely solely on an automated model.

## Immediate next action

Resume in VS Code at `E:\LINKLENS-AI`. First inspect the Git branch, remote, and working-tree status; then synchronize the verified local source files with the private GitHub repository. Do not push blindly before reviewing the status and ignore rules.
