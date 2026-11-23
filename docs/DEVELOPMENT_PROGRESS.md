# LINKLENS AI — Development Progress

Last documented milestone: 2026-11-23

## Project objective

LINKLENS AI is an early-stage Fraud Intelligence & Early Prevention Platform. The current implementation is a local proof of concept built around explainable, rule-based message analysis and static URL inspection.

## Implemented locally

### 1. Project foundation

- Repository: `asnr10/LINKLENS-AI`
- Local workspace: `E:\\LINKLENS-AI`
- API framework: FastAPI
- Request validation: Pydantic
- Runtime: Python virtual environment
- Local API: `http://127.0.0.1:8000`
- API documentation: `http://127.0.0.1:8000/docs`

### 2. Message Intelligence

Local module: `apps/api/engine/fraud_analyzer.py`

Engine version reported by the API: `0.4.1`

Implemented rule indicators:

| Indicator | Weight |
|---|---:|
| Credential or OTP request | 35 |
| Money request | 25 |
| Urgency or threat language | 15 |
| Electronic link | 10 |
| Financial or official entity mention | 20 |

Risk thresholds:

- CRITICAL: 70 or higher
- HIGH: 45–69
- MEDIUM: 20–44
- LOW: below 20

The score is capped at 100. The engine returns bilingual English/Arabic findings and recommendations. Mentioning a financial institution is only an indicator; it does not prove impersonation or fraud.

### 3. Link Intelligence — initial implementation

Local module: `apps/api/engine/link_analyzer.py`

The analyzer currently:

- Extracts HTTP/HTTPS and `www.` URLs from message text.
- Parses the hostname and URL scheme.
- Flags direct IP-address hosts.
- Flags unencrypted HTTP.
- Flags embedded URL credentials.
- Flags non-standard ports.
- Flags internationalized-domain/Punycode indicators.

The current analyzer performs static parsing only. It does not resolve DNS, follow redirects, fetch pages, assess domain age, or query reputation feeds. An empty findings list does not establish that a URL is safe.

### 4. FastAPI integration

Local module: `apps/api/main.py`

The `POST /analyze` endpoint calls both analyzers and returns:

- `message_analysis`
- `link_analysis`

The `GET /health` endpoint remains available.

The combined endpoint was tested locally and returned `status: success` with both analysis sections.

## Verified local test scenarios

| Scenario | Observed result |
|---|---|
| English message with OTP and a URL | Score 45, HIGH |
| Arabic message with bank-account threat, OTP, and URL | Score 80, CRITICAL |
| Benign package-delivery message | Score 0, LOW |
| Direct IP URL using HTTP and port 8080 | Detected `ip_address_used`, `unencrypted_http`, and `unusual_port` |
| HTTPS `example.com` URL | Parsed; no configured link indicators detected |

The local pytest suite previously reported **7 passed** for `tests/test_fraud_analyzer.py`. These are reported test results from the local development session, not a CI run on GitHub.

## Current limitations and safety requirements

- The message score and link findings are currently separate. Link findings are not yet incorporated into a unified risk score.
- A detected indicator is not proof of fraud.
- The analyzer must not automatically visit arbitrary user-supplied URLs.
- Any future network-based URL inspection must defend against SSRF, DNS rebinding, redirects to localhost/private/link-local IP ranges, oversized responses, and timeouts.
- Use synthetic data for demonstrations and testing.
- Do not claim operational integration with police, banks, telecom providers, or transaction systems.
- Keep Arabic and English as the supported project languages.

## Next development steps

1. Mirror the verified local source files and tests into this repository.
2. Add tests for URL parsing edge cases and malformed URLs.
3. Design Domain Intelligence with evidence and source attribution.
4. Add safe domain normalization and lookalike-domain analysis.
5. Define a calibrated, explainable combined-risk model without double-counting the same URL indicator.
6. Re-run automated tests and add CI before treating the repository snapshot as release-ready.

## Repository synchronization note

At the time this progress record was added, the GitHub repository did not yet contain the local `apps/api` source tree. This document records the completed local milestone; it does not claim that the entire local project has been pushed.
