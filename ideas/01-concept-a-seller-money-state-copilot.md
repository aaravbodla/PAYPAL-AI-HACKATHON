# Concept A (SELECTED): Seller Money-State and Dispute Action Copilot

**Project status:** Provisional selection. Becomes final only if the gating spike (Section 12) passes. Fallback is Concept B (`02-concept-b-receivables-workbench.md`).
**Prepared:** October 10, 2026 (Phoenix, AZ). **Team:** 2 people. **Hackathon deadline:** Thu Nov 12, 2026, noon Pacific (1:00 p.m. Phoenix). **Judge access required through:** Dec 15, 2026.

## 0. How to use this file (instructions for Claude Code)

This file is the single source of truth for what is being built and why. Read it fully before writing code.

1. **Do the spike (Section 12) before building features.** Do not build UI or AI features on assumptions about PayPal behavior. Save redacted real responses to `/fixtures` and record findings in `/docs/spike-notes.md`.
2. **Respect the data semantics rules (Section 6). They are product requirements, not suggestions.** They exist because past research found specific traps.
3. **Anything marked UNVERIFIED must be tested before relying on it.** Do not silently resolve a conflict between documents. Record it in `/docs/spike-notes.md` and ask.
4. **Never invent PayPal behavior.** If the API doesn't supply a value, the UI shows "unknown".
5. **The AI model never touches the PayPal client and never executes actions.** It reads ledger facts and returns cited drafts. Deterministic code decides eligibility and execution.
6. **Sandbox only.** The PayPal client must refuse to start against a live host. No production secrets anywhere in the repo.
7. **Label fixture data as fixture data** in the UI, logs, and audit log. Never present fixtures as real sandbox responses.
8. Keep AI spend low (Section 10). The AI credits are shared with another project.

---

## 1. One-liner

A tool for small PayPal sellers that shows what PayPal tells you about your money, what is still unknown, and the next dispute action you can safely take. AI drafts grounded responses; the seller approves every action.

## 2. User and promise

**User hypothesis (unvalidated):** a small seller who uses PayPal directly, has no dispute team, and can't tell which cases need attention or which money figures to trust. Market size and willingness to pay were not established by research.

**Promise:** "See what PayPal tells us about your money, what is still unknown, and the next dispute action you can safely take."

**Explicit non-promises (never claim these in UI, README, or demo):**
- A complete explanation of why PayPal placed a hold, reserve, or limitation.
- Reserve release date forecasts presented as PayPal facts.
- Releasing or accelerating the release of funds.
- Improved dispute win rates or recovered revenue.
- Autonomous refunds, offers, claim acceptance, or appeals.
- A production-approved financial service.

## 3. Hackathon constraints that shape the design

- PayPal sandbox must be central, plus a real AI component.
- Working app only (no mockups). Public repo with a detectable open-source license. No secrets committed.
- Demo video under 3 minutes, public on YouTube, showing the real working app.
- Hosted demo or full run instructions, with sandbox test access documented for judges.
- Must stay free and working for judges through **Dec 15, 2026**.
- Judges do not bring their own AI keys (organizer confirmed for Anthropic). The model key must live server-side on the hosted demo.
- Judging: five equal criteria in tiebreak order: Technological Implementation, Design, Potential Impact, Innovation/Idea, Presentation.
- Prize fit: Best Use of PayPal + AI, Most Impactful. (Agentic Commerce fit is weaker for this concept.)

## 4. Research basis

### 4.1 Verified from official schemas and docs (documented support, not executed)
- **Reporting API** exposes per-currency `total_balance`, `available_balance`, `withheld_balance`, plus `as_of_time` and `last_refresh_time`. Only `currency` and `total_balance` are required, so optional fields may be absent.
- **Transaction Search** exposes gross `transaction_amount`, `fee_amount`, event code, reference IDs, dates. No explicit `net_amount`. Limits: 31-day query window, `page_size` 1 to 500, history up to 3 years, up to 3 hours before data appears.
- **Disputes API** exposes states, `HELD` as a disputed-transaction status, `dispute_amount`, `seller_response_due_date`, requested evidence, `fund_movements`, and action links (HATEOAS).
- **Capture details** (`GET /v2/payments/captures/{id}`) expose selected `PENDING`/`DENIED` reasons and a net breakdown (not available while pending).
- **Webhooks:** signature verification, event listing, resend, and simulation endpoints exist. Dispute events: `CUSTOMER.DISPUTE.CREATED`, `.UPDATED`, `.RESOLVED`.
- **Chargeback Protection** and Seller Protection exist at PayPal, with a monthly loss cap.

### 4.2 Not exposed in the reviewed public schemas (design around it)
- A complete per-hold inventory with reasons.
- Reserve policy (percentage and duration), reserve release dates.
- Account limitation status.

These are bounded "not found" results, not proof they don't exist elsewhere.

### 4.3 UNVERIFIED (resolve in the spike)
- **Dispute creation access conflict:** the official Disputes schema labels create-dispute as limited release (contact an account manager). An earlier PayPal doc describes a sandbox-only create-dispute endpoint with scripted scenarios. Test which applies to a new app. This decides whether the demo exists.
- Whether a new sandbox app has Reporting entitlement, and whether `withheld_balance` is ever populated in sandbox.
- Whether "with hold" dispute scenarios actually produce `HELD` status or a nonzero withheld balance.
- Evidence request body format: the schema's operation prose says JSON in places, but the request-body definition and PayPal's guide indicate multipart. Verify with a real call.
- `make-offer` stage rules (documentation conflict on claim-stage offers). Use live `allowed_response_options` and links, not a hardcoded chart.
- The exact current webhook event list.
- Third-party access to other merchants' accounts requires PayPal partner network membership (Reporting schema). Do not assume multi-merchant access; scope the demo to one controlled sandbox merchant.

### 4.4 Competition (why the differentiator is provenance, not features)
- **Chargeflow:** automates PayPal inquiries and chargebacks, appeals, rules, and offers a free multi-processor Insights dashboard. Success-based pricing reported at 25% of recovered chargebacks. Vendor claims, not independently validated.
- ChargePay (Shopify-centric flat monthly tiers), Chargeback Gurus (non-Shopify breadth, forecasting claims), HeldFunds (complaint archive, not a product).
- A2X and Synder handle accounting reconciliation.
- **Implication:** do not compete on generic evidence generation or a plain dashboard. Differentiate on honest provenance, withheld-vs-exposure separation, deadline and unknown-state handling, and approval-bound actions.

## 5. Scope

### 5.1 In the MVP
1. **Money-state overview.** One merchant, one currency. Available and withheld balances, each with as-of time, source, and missing-data state. Dispute exposure shown separately.
2. **Action queue.** Per-case response deadline, state, requested evidence, allowed actions (from live links). Missing deadlines and unavailable actions shown explicitly.
3. **Evidence-grounded AI.** Explains which source fields it used, flags missing evidence, drafts a factual response or checklist, abstains rather than invent reasons, delivery proof, or dates.
4. **Approval-bound execution.** Read-only first. If sandbox allows, execute one supported action (evidence submission preferred) after explicit approval and a fresh state check.
5. **Engineering core.** Verified webhook ingestion, deduplication, decimal-safe money, tamper-evident audit log, adverse-case tests, AI eval harness.

### 5.2 Out of the MVP
- All-account hold, reserve, and limitation explanations; release date forecasts.
- Broad accounting reconciliation, multiple processors or currencies, bank settlement forecasts.
- Autonomous refunds, claim acceptance, offers, or appeals.
- Invoice and subscription features (Concept B).
- AP2, Agent Ready, Mastercard dependencies, or a full governance platform (Concept C).
- Claims of recovered revenue or win-rate improvement.
- Multi-tenancy (keep a `merchant_id` column everywhere, but only one merchant is supported).

## 6. Data semantics rules (mandatory)

**Money and state**
1. Never add disputed amounts to withheld balances. They can overlap, and disputed amount is exposure, not proof of a hold. Show as separate metrics.
2. A missing optional field is **unknown**, never zero.
3. `PENDING`, `HELD`, delayed disbursement (`disbursement_mode=DELAYED`), and account limitation are different concepts. Do not conflate them.
4. Reporting can lag up to 3 hours. Store `observed_at` (when we fetched) and the provider's `as_of_time`/`last_refresh_time` for every figure. The AI cannot make stale data fresh.
5. Reporting timestamps are not bank settlement dates. Do not present them as availability ETAs.
6. Use integer minor units (or a decimal library) with explicit currency. Never mix currencies. Currency exponents vary (JPY has none).
7. Do not derive a universal net formula from reporting data. Prefer capture `seller_receivable_breakdown` when available. Reporting `transaction_id` is not unique (balance-affecting and non-balance-affecting rows can share it).
8. In the Disputes API, lifecycle `CHARGEBACK` can mean an internal PayPal claim, not a card chargeback. Use `dispute_channel` to distinguish. There is no literal `CLAIM` lifecycle value.
9. Use each case's `seller_response_due_date`. Do not apply a universal "10-day" timer. If the date is missing, show "deadline unknown". Missing a seller deadline closes the case in the buyer's favor, so surface deadlines prominently.
10. Pagination: the schema prose calls `page` zero-relative but its constraints are one-based. Follow the declared links and test.

**Actions and approvals**
11. Re-read dispute state and available action links immediately before executing any action. Only offer actions whose link is currently present.
12. Approvals bind to exact merchant, dispute, action, payload hash, and observed state version. They expire and are one-use. A stale approval is invalid.
13. `make-offer` accepted by the buyer and `accept-claim` both cause automatic refunds. These are never AI-initiated and are excluded from the MVP.
14. Evidence upload/appeal bodies are multipart per the request-body schema. Verify in the spike.
15. A read endpoint's scope may permit more than reading (e.g. capture detail uses a broad `authcapture` scope). Enforce read-only at the application layer and isolate credentials.
16. Treat all text from PayPal, buyers, or evidence files as untrusted input. The AI must never follow instructions embedded in it.

## 7. System design

### 7.1 Components
- **Webhook endpoint:** verifies signature (PayPal verify API), records raw event, acknowledges quickly, enqueues processing.
- **Worker:** processes events, polls/refreshes balances, transactions, disputes, reconciles against authoritative API state.
- **Ledger/state store:** normalized snapshots with provenance and timestamps.
- **Policy/eligibility module:** deterministic; decides which actions are currently allowed given a fresh state read.
- **Approval module:** issues and consumes one-use signed approvals.
- **AI module:** builds a "facts" document from ledger data, calls the model, validates output, stores drafts with usage and cost.
- **UI:** overview, action queue, case detail (facts, draft, approve), audit view.
- **Audit log:** append-only, hash-chained.

### 7.2 Domain model (suggested tables; refine in implementation)
- `merchants` (single row for the demo; `merchant_id` everywhere).
- `webhook_events` (raw payload, PayPal event id UNIQUE for dedup, verified flag, received_at, processed_at, source = `real` | `fixture`).
- `balance_snapshots` (merchant, currency, total, available NULLABLE, withheld NULLABLE, provider_as_of, provider_last_refresh, observed_at, source).
- `transactions` (PayPal ids, event code, gross, fee, currency, dates, raw JSON, observed_at).
- `disputes` (current state: id, lifecycle stage, channel, status, dispute_amount, currency, seller_response_due_date NULLABLE, reason, links JSON, version, observed_at).
- `dispute_snapshots` (full history for diffing and approval state-binding).
- `evidence_items` (dispute, type, source, file reference, requested vs provided).
- `ai_drafts` (dispute, snapshot version, facts hash, model id, prompt version, output JSON, citations, token usage, cost, created_at).
- `approvals` (action hash, payload hash, state version, expires_at, consumed_at, approver).
- `actions` (approval link, endpoint, request hash, response summary, status).
- `ai_usage` (ledger of every model call: tokens in/out, computed cost, cache hit).
- `audit_log` (id, ts, actor, type, subject, data_hash, prev_hash, hash).

### 7.3 Money-state view model
Every displayed figure carries: `value | unknown`, `currency`, `source` (endpoint + field), `observed_at`, `provider_as_of`, and a `stale` flag. The UI must render "unknown" and "stale" as deliberate designed states.

### 7.4 Key flows
1. **Ingest:** webhook (verified) or poll, then store raw, then enqueue, then fetch authoritative state (never trust the event body alone), then write snapshot, then diff and audit.
2. **Overview:** latest balance snapshot per currency, separate dispute-exposure panel, freshness indicators.
3. **Action queue:** disputes sorted by nearest known deadline, "deadline unknown" group clearly marked, each with currently allowed actions.
4. **Draft:** build facts, call model, validate output, store with citations. UI shows draft beside its sources.
5. **Approve and execute:** user approves, system issues one-use token bound to the exact payload and state version, execution re-reads state and links, rejects on mismatch/expiry/reuse, calls PayPal, re-fetches state, audits.

### 7.5 Approval token spec (suggested)
Signed (`jose`, HS256 or ES256) payload: `merchant_id, dispute_id, action, payload_hash, state_version, nonce, iat, exp` (short expiry). Store consumed nonces. On execution: verify signature, expiry, nonce unused, recompute payload hash, re-fetch dispute, confirm state version matches and the action link still exists, then execute atomically with marking the nonce consumed.

### 7.6 Audit log spec (suggested)
`hash = SHA-256(prev_hash || canonical_json(entry))`. DB trigger and permissions block UPDATE/DELETE. Describe it as **tamper-evident, not immutable**. Optionally export periodic checkpoint hashes outside the database. Log AI calls (by id, not full prompts with sensitive data), approvals, executions, rejections, and fixture/real source.

## 8. Architecture and repo layout

```
/apps/web        Next.js (App Router) UI + API routes + webhook endpoint
/apps/worker     pg-boss jobs (can run inside web process to save hosting cost)
/packages/paypal typed PayPal client (types generated from official OpenAPI), sandbox guard
/packages/core   money helpers, ledger types, policy/eligibility, approvals, audit
/packages/ai     prompts, output schemas, validators, budget guard, evals
/docs            architecture.md, threat-model.md, spike-notes.md
/fixtures        redacted real payloads (labeled), plus clearly labeled synthetic ones
```
Keep `core` free of framework code so money and policy logic is easy to test.

## 9. Tech stack

| Aspect | Choice | Notes |
|---|---|---|
| Language | TypeScript (strict), Node LTS | No `any` on money or PayPal payloads |
| App | Next.js App Router as a long-running Node server | Do not deploy serverless-only (worker needs a persistent process) |
| UI | Tailwind CSS + shadcn/ui | Design unknown/stale/missing states first |
| Database | PostgreSQL | UNVERIFIED: host free-tier expiry vs Dec 15 |
| ORM/migrations | Drizzle or Prisma | Raw SQL for audit triggers |
| Jobs | pg-boss | No Redis; idempotent job IDs |
| PayPal client | Thin typed wrapper; types from official OpenAPI via `openapi-typescript` | Sandbox-bound, fails at startup on live host |
| Validation | Zod | Validate every PayPal payload and every AI output |
| Approvals | `jose` signed one-use tokens | |
| Audit | Append-only Postgres table, hash chain | |
| AI provider | **Nebius Token Factory** via the official `openai` SDK | See Section 10 |
| Testing | Vitest, fast-check (money properties), Playwright | Contract tests on redacted real payloads |
| Logging | Pino structured logs | Never log tokens, secrets, or full customer data |
| Secrets | env vars, `.env.example`, gitleaks in CI + pre-commit | Repo is public |
| CI | GitHub Actions: lint, typecheck, test, build, secret scan | Tests run on fixtures, not live sandbox |
| Packaging | Docker + docker-compose (app, worker, Postgres) | Reproducible local run for judges |
| Hosting | Render or similar | UNVERIFIED: credits ($50 on Render per sponsor page) and DB expiry through Dec 15 |
| License/docs | MIT or Apache-2.0; README, ARCHITECTURE, THREAT_MODEL | License visible in repo About area |

**Sponsor tools:** Render (hosting, if credit math works), Postman (spike; official PayPal collection). AG Grid only if a heavy table is truly needed; note its 45-day trial would expire about Nov 24 if started now. Skip the rest.

## 10. AI layer (Nebius Token Factory, budget-limited)

### 10.1 What is documented
- Token Factory exposes an **OpenAI-compatible API**; base URL `https://api.tokenfactory.nebius.com/v1/` with the standard `openai` SDK and bearer API key. Its docs include "Structured output & JSON" and "Function calling" pages. A third-party project uses `json_schema` response format against it with an open model.
- Model IDs and prices per 1M tokens, from **third-party calculators (not Nebius official; verify on the Nebius pricing page):** GLM-5 about $1.00 in / $3.20 out; MiniMax-M3 about $0.30 / $1.20; INTELLECT-3 about $0.20 / $1.10. A Pipecat integration lists `openai/gpt-oss-120b` as its default model.
- Third-party commentary notes reasoning models may add reasoning-token overhead to cost.

### 10.2 UNVERIFIED (check before building)
- The $50 credit's **expiry date and any usage restrictions** (it came from another hackathon). It must cover development and judging through Dec 15, while leaving enough for the other challenge.
- Which exact model IDs are available now, and whether the chosen model **reliably honors `json_schema` structured output**. If not, use JSON mode plus Zod validation with one retry.
- Actual current prices (official page) and whether usage can be capped in the Nebius dashboard.
- Rate limits for the account.

### 10.3 Budget design (the user wants to conserve credits)
Suggested defaults (user to confirm):
- **Total project AI budget cap: about $8 to $10** of the $50, enforced in code, not just intended. Daily cap about $1.
- Pick a **cheap, non-reasoning (or low reasoning effort) model** that passes the eval. Benchmark two candidates on the eval set before choosing.
- Rough cost intuition (my arithmetic from the unverified third-party prices above): a draft using about 3,000 input and 1,000 output tokens on a $0.30/$1.20 model costs roughly $0.002; on a $1.00/$3.20 model roughly $0.006. Even 1,000 calls would stay in single dollars, but recompute with verified prices.
- **Budget guard (in `packages/ai`):** before each call, check the `ai_usage` ledger against total and daily caps and a per-IP rate limit (suggested 10 calls/hour per visitor); cap `max_tokens`; cache by facts hash so repeated views never re-call the model; log tokens and computed cost from response usage fields.
- **When the budget is exhausted:** the UI shows "AI drafting paused" and falls back to the deterministic checklist. Do not fake AI output.
- Pre-generate and cache drafts for the demo scenarios so judging does not burn credits unpredictably, but make clear in the UI which drafts are cached versus fresh.
- Keep the API key server-side only. Rotate it after the hackathon.
- Only sandbox data is sent to the model. Never send real customer data.
- Because open models may follow injected instructions more readily, keep untrusted text clearly delimited in prompts, and rely on code (not the prompt) for safety.

### 10.4 AI contract
**Input:** a `facts` JSON built by code. Each fact has a stable id, value, source field, and observed_at. Untrusted free text (buyer messages) is included only in a delimited block and marked untrusted.
**Output schema (Zod-validated):**
```
{ summary, cited_fact_ids[], missing_evidence[], draft_response, uncertainties[], refusals[] }
```
**Validators (deterministic, must pass or draft is rejected/flagged):**
- Every `cited_fact_id` exists in the input.
- Any date or monetary amount in `draft_response` appears in the facts.
- No claims about hold reasons, release dates, or delivery proof unless present in facts.
- `refusals` must be used when facts are insufficient.

The AI never calls PayPal and never sees credentials.

### 10.5 Evaluation
- Build 20 to 30 hand-labeled cases (synthetic sandbox-derived) covering: complete evidence, missing evidence, missing deadline, conflicting amounts, injected instructions in buyer text, stale snapshot.
- Metrics: citation validity, fabricated date/amount rate, missing-evidence recall, correct abstention rate.
- Compare against a deterministic-only baseline (checklist from requested evidence). If the AI does not beat the baseline on useful dimensions, narrow its role (e.g. phrasing only).

## 11. PayPal API surface (sandbox base `https://api-m.sandbox.paypal.com`)

OAuth: `POST /v1/oauth2/token` with `client_credentials`. Scope prefix `https://uri.paypal.com/services/`.

| Purpose | Endpoint | Scope / note |
|---|---|---|
| Balances | `GET /v1/reporting/balances` | `reporting/balances/read`; `withheld_balance` optional |
| Transactions | `GET /v1/reporting/transactions` | `reporting/search/read`; 31-day window, page_size up to 500 |
| List disputes | `GET /v1/customer/disputes` | supports `disputed_transaction_id` |
| Dispute detail | `GET /v1/customer/disputes/{id}` | fields depend on access; use links |
| Provide evidence | `POST /v1/customer/disputes/{id}/provide-evidence` | multipart; needs `WAITING_FOR_SELLER_RESPONSE` and a link |
| Make offer / appeal / accept claim | `.../make-offer`, `.../appeal`, `.../accept-claim` | excluded from MVP; refund-causing |
| Sandbox helpers | `.../require-evidence`, `.../adjudicate` | sandbox only; need matching links |
| Create dispute (sandbox) | `POST /v1/customer/disputes` | UNVERIFIED: limited release vs sandbox-only |
| Capture detail | `GET /v2/payments/captures/{id}` | broad `authcapture` scope; read-only by policy |
| Webhook verify | `POST /v1/notifications/verify-webhook-signature` | needs webhook_id and headers |
| Webhook resend/simulate | `POST /v1/notifications/webhooks-events/{id}/resend`, `.../simulate-event` | testing |

Dispute webhook events: `CUSTOMER.DISPUTE.CREATED`, `CUSTOMER.DISPUTE.UPDATED`, `CUSTOMER.DISPUTE.RESOLVED`. Enable Disputes in the app's feature options when creating the sandbox app.

## 12. Gating spike (target about 3 days; kill date Tue Oct 13)

Use team-owned sandbox accounts only. Save redacted real responses to `/fixtures` and log everything in `/docs/spike-notes.md`.

| # | Task | Record |
|---|---|---|
| S1 | Create sandbox seller (business) and buyer (personal) accounts; create an app with Disputes enabled; obtain credentials | Which features were selectable |
| S2 | Call balances and transactions | Scopes granted, whether `withheld_balance`/`available_balance` appear, freshness fields, currency |
| S3 | Create a sandbox transaction as buyer; fetch capture detail | Fee/net breakdown present or not |
| S4 | Try to create a dispute (both the sandbox-only endpoint and the buyer-side flow) | **Resolves the creation-access conflict.** Exact error if denied |
| S5 | Fetch dispute detail | Deadline field present? action links? state? `HELD` anywhere? |
| S6 | Provide evidence via multipart | Body format that works, response, resulting state |
| S7 | Use sandbox helpers (`require-evidence`, `adjudicate`) if linked | Which transitions are reachable |
| S8 | Register webhook; receive real dispute events; verify signature; test resend | Delivery delay, missing events, verification result |
| S9 | Try a "with hold" scenario if available | Does `HELD` or a withheld balance ever appear |
| S10 | Small AI test with the chosen Nebius model: structured output compliance, latency, token counts | Model id, whether `json_schema` is honored, measured cost per call |

**Go** if: real reporting data plus at least one real end-to-end dispute transition with accessible deadline/action fields. If balances stay empty, keep the dispute assistant and drop the live held-funds showcase. **No-go for A** if dispute creation or actions are unavailable and no independent working path exists. Do not substitute fixtures for a missing dispute. Pivot to Concept B.

Also test adverse cases: stale snapshot, absent withheld field, multiple currencies, duplicate reporting IDs, no appeal/evidence link, expired approval, missing seller deadline, partial dispute amount.

## 13. Build plan (proposal, not a verified estimate)

| When | Focus |
|---|---|
| Oct 10 to 13 | Spike and go/no-go |
| Oct 14 to 24 | Ingestion, ledger, money-state overview, audit log |
| Oct 25 to Nov 3 | Action queue, AI drafting with budget guard, approval flow, one real action |
| Nov 4 to 8 | Adverse-case tests, AI eval, hosting, judge-access check, threat model doc |
| Nov 9 to 11 | Video, README, submission, verify Devpost received the entry |

Suggested order inside the build: `core` (money, states, approvals, audit) before UI; `paypal` client with sandbox guard first.

## 14. Testing

- Unit and property tests for money (fast-check): no float errors, currency never mixed.
- Contract tests against redacted real PayPal payloads.
- Approval tests: mutated payload, expired, reused, state changed, link gone.
- Webhook tests: duplicate, out-of-order, unverified signature, replayed fixture kept away from action paths.
- AI tests: validators reject fabricated dates/amounts and uncited claims; injected buyer text does not change behavior.
- Playwright end-to-end of the demo path.
- CI uses fixtures, not live sandbox.

## 15. Threat model (summary; expand in `/docs/threat-model.md`)

**Trusted:** server code, database transactions, key management, authenticated approval surface. **Untrusted:** the LLM and its output, buyer messages, evidence files, webhook bodies until verified, client input.

| Threat | Mitigation | Limit |
|---|---|---|
| Prompt injection in buyer text | Delimited untrusted block; code-enforced validators; AI cannot act | Draft quality can still be poor; seller reviews |
| Approval replay or mutation | One-use signed token bound to payload hash and state version | Compromised server defeats it |
| Stale state action | Re-read state and links before executing | PayPal latency/ambiguity needs reconciliation |
| Forged webhook | Signature verification; fetch authoritative state | Sandbox delivery reliability unknown |
| Credential leakage | Env only, gitleaks, server-side only, sandbox-bound client | Public repo discipline |
| Audit tampering | Hash chain, DB triggers, optional external checkpoints | DB admin can rewrite without checkpoints |
| Over-broad token scope | Read-only enforced in app; isolated credentials | Scope names are broader than "read" |
| AI cost abuse | Budget guard, rate limits, cache | Shared hosted demo exposure |

Out of scope: compromised host or PayPal, a malicious authorized owner, legal/compliance determinations.

## 16. Demo plan (under 3 minutes)

1. Overview: balances with timestamps and an explicit "unknown" where PayPal gives no reason.
2. A real sandbox dispute in the queue with its real deadline.
3. AI draft appears with cited sources and flagged missing evidence.
4. Approve; evidence submitted; the case state changes live.
5. One adverse case: a stale approval rejected, or a missing deadline shown honestly.
Say plainly: sandbox, production-minded, not production-approved.

## 17. Compliance checklist

- [ ] Submit before Thu Nov 12, 1:00 p.m. Phoenix; verify Devpost received it.
- [ ] Public repo, detectable license, no secrets in history.
- [ ] YouTube video under 3 minutes, public.
- [ ] README with run instructions, test access, architecture, honest limitations.
- [ ] Hosted demo or Docker run; AI key server-side; works through Dec 15.
- [ ] Confirm hosting credits, DB expiry, and Nebius credit expiry cover Dec 15.
- [ ] English materials; no unlicensed music or trademarks in the video.
- [ ] If targeting a sponsor prize, verify real tool use.

## 18. Risks and open questions

1. Dispute creation access (biggest). 2. Reporting entitlement and populated `withheld_balance`. 3. Differentiation versus Chargeflow Insights. 4. Unvalidated user demand: ask a few Etsy/eBay sellers whether deadlines, held funds, and next steps are confusing. 5. Webhook reliability in sandbox. 6. Hosting/database/AI credit longevity through Dec 15. 7. Nebius model choice and structured-output reliability. 8. Third-party access limits (single-merchant demo only). 9. PayPal developer docs were largely unreachable during research, so several P0 behaviors are unresolved, not disproven. 10. Team skill mix not yet specified.

## 19. Definition of done

- Spike notes show real sandbox evidence for each gate.
- Overview, action queue, AI draft, approval, and one real action work end to end on the hosted demo.
- Unknown/stale/missing states are designed and tested.
- Money, approval, audit, webhook, and AI-validator tests pass in CI.
- AI eval compares against the baseline and results are in the README.
- Budget guard enforced and verified by test.
- Threat model and limitations documented honestly.
- Video, README, and submission complete before the deadline.

## 20. Sources

PayPal official: Reporting, Disputes, and Payments v2 OpenAPI specs (github.com/paypal/paypal-rest-api-specifications); Webhooks Management OpenAPI; PayPal AI-Toolkit disputes guide; Chargeback Protection docs (developer.paypal.com/expanded/chargeback-protection). Vendors: Chargeflow (pricing, integrations catalog), ChargePay, Chargeback Gurus, HeldFunds, A2X vs Synder. Case: UK Financial Ombudsman DRN-5996562. Hackathon: official rules, FAQs, API-key-for-judges thread (paypalaihackathon.devpost.com). Nebius: docs.tokenfactory.nebius.com (OpenAI-compatible API, structured output pages); model prices from third-party calculators, to verify. Full research reports: `paypal-hackathon-research-brief.md` and the verification report.
