# Concept C (THIRD CHOICE): PayPal Agent Trust Workbench

**Project status:** Third choice. The original pitch ("the missing PayPal policy/approval/audit layer") is **false**: closely overlapping public projects exist (PayLeash, Bursar, Pulse Safe Commerce Agent, BoundPay, others). This file describes the only defensible reworking: a **credential-isolated, adversarial conformance workbench** for PayPal's agent tooling. That niche is **not verified empty** (RefundLab advertises agent fault injection). Choose C only after comparing it to PayLeash and RefundLab, validating a specific user, and passing the spike. Primary option is A (`01-concept-a-seller-money-state-copilot.md`); fallback is B (`02-concept-b-receivables-workbench.md`).
**Prepared:** October 10, 2026 (Phoenix, AZ). **Team:** 2 people. **Deadline:** Thu Nov 12, 2026, noon Pacific (1:00 p.m. Phoenix). **Judge access through:** Dec 15, 2026.

## 0. How to use this file (instructions for Claude Code)

1. **Run the spike (Section 12) first.** Do not build on assumptions about PayPal's agent tooling.
2. **Do not claim novelty you haven't verified.** Compare against PayLeash and RefundLab (read their code and READMEs) before positioning.
3. **The model is fully untrusted.** It proposes actions; deterministic code decides and executes. The model never sees a PayPal token.
4. **Sandbox only, explicitly bound.** Library defaults can point at production if `context.sandbox` is unset. Fail startup unless the environment and every outbound PayPal host are sandbox.
5. **Say "AP2-inspired," never "AP2 on PayPal,"** unless native support is verified.
6. Label fixtures and recorded attack traces as such; keep a real working AI path alongside them. Keep AI spend low (Section 10).

---

## 1. One-liner

A gateway and test harness that holds the PayPal sandbox credentials, exposes a fixed allowlisted set of PayPal tools to an untrusted AI agent, requires exact-action human approval, and ships a reproducible red-team suite showing exactly which attacks are blocked and which residual risks remain.

## 2. User and promise

**User hypothesis (unvalidated, needs validation):** a developer or operator building agents on PayPal's Agent Toolkit or MCP server who needs evidence that the integration can't be talked into the wrong action. Demand for this specific tool is not established.

**Promise:** "Run an AI agent against PayPal's sandbox without it ever holding credentials, and see which attacks the gateway blocks, with approvals bound to exact actions and a reproducible test report."

**Explicit non-promises:**
- Universal prompt-injection prevention.
- Payment-network-level enforcement, native AP2, Mastercard Agent Pay, or Agent Ready integration (unverified).
- Production-ready financial security or an independent security audit.
- That a budget or signed mandate proves item quality, necessity, delivery, or merchant honesty.
- Buyer-wallet autonomous spending (merchant credentials are not a consumer's spending wallet).
- Being the first or only PayPal governance layer.

## 3. Hackathon constraints

Same as the other concepts: PayPal sandbox central plus a real AI component; working app; public repo with detectable license; video under 3 minutes on YouTube; free judge access through Dec 15; judges bring no AI keys (key server-side). Five equal criteria. Prize fit: strong conceptual fit for Best Use of Agentic Commerce and PayPal + AI, but the category is likely the most saturated and Most Impactful is harder to evidence. Do not choose this for an assumed agentic scoring bonus (no organizer statement establishes one).

## 4. Research basis

### 4.1 Verified from official code (inspected, not executed)
- **Toolkit packages differ:** `paypal/agent-toolkit` is the framework library (TypeScript registry, commit a3aa963...); `paypal/paypal-mcp-server` is the local MCP server; `paypal/AI-Toolkit` is currently a coding-agent plugin (sandbox MCP bearer-token config, an informational hook that never blocks).
- **47 tools in the pinned TypeScript registry.** Money-moving or liability-affecting ones include `pay_order` (captures an order), `create_refund` (refunds a captured payment), `accept_dispute_claim`. Invoice/subscription tools create communications or future obligations (`send_invoice`, `create_subscription`, `cancel_subscription`, etc.). Categories: invoices/series (22), catalog (4), plans/subscriptions (8), shipment (3), orders (3), disputes (3), payments (2), reporting (2). No payouts tool in this registry.
- **No native spending/approval/audit controls** found in the published code. `isToolAllowed` filters which tools register; handlers call `PayPalAPI.run` directly. No amount, merchant, per-agent policy, or approval queue. The auth path sends only `grant_type=client_credentials` (no requested OAuth `scope`).
- `get_merchant_insights` is explicitly unsupported in sandbox. Three remote commerce tools (`search_product`, `create_cart`, `checkout_cart`) require `x-feature-flags: commerce:true` and are remote-only (untested).
- Library defaults: `context.sandbox ?? false` means production unless set. The plugin's MCP URL (`https://mcp.sandbox.paypal.com/sse`) is a separate safe default.
- npm `@paypal/mcp` depends on `@paypal/agent-toolkit: latest`, so **pin versions and use a lockfile**.

### 4.2 UNVERIFIED (resolve in the spike)
- Hosted remote MCP and Agent Ready internals and controls (cannot be observed from code).
- Whether a credentialed `tools/list` matches the source registry.
- Exact OAuth scope restriction support (can credentials be limited to capture-only or refund-only?).
- **Native AP2 or Mastercard Agent Pay sandbox support.** PayPal announced a Mastercard Agent Pay integration and pilot ("will integrate", "will pilot"); AP2 backers include PayPal; no public sandbox contract was found. PayPal developer pages were unavailable during research.
- Participant reports (single reports, not confirmed outages): remote sandbox MCP `create_invoice` error ("Unsupported cache mode"), and wallet-vault `NOT_ENABLED_TO_VAULT_PAYMENT_SOURCE` errors for platform apps. Do not make hosted remote MCP or platform vaulting your only demo path. Keep a pinned local toolkit or ordinary REST path.
- Third-party multi-merchant access needs partner approval; assume a single sandbox merchant.

### 4.3 Standards context
- **AP2** (Agent Payments Protocol): Google-originated, donated to FIDO (announced April 28, 2026; official FIDO announcement). v0.2.0 (April 28) adds Human Not Present flows; the current spec uses **Checkout and Payment mandates** (not just intent/cart). Verification must be deterministic code on a non-agentic surface. An official Python SDK documents `MandateClient` create/present/verify with ES256/SD-JWT; reference samples need Google credentials; docs and spec examples differ in places. A small pinned local signing demo is plausible; full interoperability is not a two-person scope.
- Agent-payment threats: Anthropic's Project Vend experiment (hallucinated payment account, selling at a loss, talked into discounts) is real but physical and not PayPal. AgentDojo and the AP2 "Whispers of Wealth" prototype are simulated/synthetic. **No verified PayPal-specific agent payment incident was found.** Do not imply fraud incidents or universal defenses have been demonstrated.

### 4.4 Competition (why the original pitch fails)
- **PayLeash:** README plus code show signed mandates, deterministic policy/provenance checks, approvals, budgets, replay guards, kill switch, backtesting, hash-chained audit. Its pinned `Guard.authorize` verifies signature, unknown tool, freeze, step-up, policy, budget reservation, audit. It discloses v0 gaps: cooperative provenance registration, no per-user model, no mandate revocation list, limited rate limiting, single-node SQLite, model sees decision facts. Test counts and sandbox success are self-reported.
- Bursar (buyer mandates, Vault, approvals, ledger), Pulse (exact consent binding, replay prevention, receipts), BoundPay (policy-bound procurement, approvals, verified capture), PayGuard (scaffold, no PayPal calls), OpenAgentPay (multi-rail SDK), plus metadata leads: RefundLab (fault injection), IntentLock, Procure Agent, Payagent Sentinel.
- Outside PayPal: Payman, Crossmint (lists PayPal as a checkout method), Skyfire, Stripe Issuing for agents and Shared Payment Tokens, Nevermined, Visa/Mastercard programs already cover limits, scoped delegation, merchant restrictions, approvals, revocation, and visibility.
- **Implication:** "limits + approvals + audit on PayPal" is not novel. Any differentiation has to be a specific, demonstrated failure analysis or workflow that these projects don't already show, and that needs validation.

## 5. Scope (narrowed)

### 5.1 In the MVP
1. **Credential-isolated gateway.** Holds sandbox credentials server-side. Agent talks only to the gateway's fixed tool schema (a small allowlisted subset, e.g. read order, create order, capture approved order, create capture-bound refund). No raw REST escape hatch; no token ever enters model context or the browser.
2. **Exact-action approval.** Human approval is bound to tool, resource id, amount, currency, merchant, and policy version. Mutating any bound value requires a new decision. One-use, short expiry.
3. **Budget with concurrency safety.** Transactional budget reservation so concurrent requests cannot exceed the cap.
4. **Inventory and drift check.** Compare the pinned toolkit's registry (and live `tools/list` if observable) against the gateway's classification; unclassified tools are blocked by default; report new/changed tools.
5. **Red-team harness.** Reproducible scripted attacks (Section 15) with a generated report of blocked, allowed, and residual cases.
6. **One real happy path.** A buyer-approved sandbox order, gateway-approved capture, and verified order retrieval (or a capture-bound refund). Show an actual successful sandbox API result, not only policy fixtures.
7. **AI role.** An untrusted agent (using Nebius) that proposes actions and explains outcomes. Keep AI outside authorization.

### 5.2 Out of the MVP
Native AP2/Agent Ready/Mastercard dependency; all 47 tools; payouts; consumer wallet spending; multi-tenant onboarding; full AP2 role implementation; claims of standards compliance; production claims.

## 6. Rules (mandatory)

1. **The model is untrusted.** Treat its output, tool arguments, customer emails, product descriptions, and tool results as untrusted.
2. **The agent never sees a PayPal token.** The gateway owns credentials and exposes a fixed tool schema. Tool filtering alone is not a capability; it only works because the gateway owns the credential exclusively. (`actions.orders.capture=false` is not an OAuth restriction.)
3. **Resolve authoritative facts server-side** (order, capture, amount, currency, merchant) rather than trusting arguments. Re-validate the exact approved action after the human decision.
4. **Sandbox-bound startup guard** (environment and all outbound hosts). Library `context.sandbox` must be explicitly true.
5. **A refund is not an arbitrary payout.** The refund endpoint refunds a captured payment; do not demo "refund to attacker email" as if it were a transfer to any address.
6. **A signed mandate does not solve prompt injection or bad purchases.** Budgets can't prove item quality, necessity, delivery, or honesty. Say so in the docs and the demo.
7. **Hash-chained audit is tamper-evident, not immutable.** Independent checkpoints are required to detect rewriting by a compromised DB admin.
8. Money is integer minor units with explicit currency; budget reservation is transactional.
9. Idempotency: one-use approvals and stable action records. PayPal response ambiguity requires reconciliation (fetch state), not blind retry. Use `PayPal-Request-Id` where supported.
10. Pin package versions and lockfile; never depend on `latest` for the toolkit.
11. Recorded adversarial traces must be labeled as recorded; keep a live AI path too.
12. Merchant capture credentials are not a consumer's general spending wallet; do not present the demo that way.

## 7. System design

### 7.1 Components
- **Gateway service:** fixed tool schema, policy engine, approval verification, budget reservation, PayPal client (sandbox-bound), audit writer.
- **Approval inbox UI:** shows authoritative action details (not an LLM-only explanation), approve/deny, expiry.
- **Agent runner:** AI agent (Nebius) that calls gateway tools; untrusted.
- **Red-team harness:** scripted attack cases plus runner and report generator.
- **Inventory checker:** diff of pinned toolkit registry vs gateway classification (and live `tools/list` if available).

### 7.2 Domain model (suggested)
`tools_inventory`, `policies` (versioned), `action_requests`, `approvals` (action hash, expiry, consumed_at), `budget_reservations` (transactional), `executions`, `attack_runs` and `attack_results`, `audit_log` (hash chain), `ai_usage`.

### 7.3 Key flows
1. **Propose:** agent requests a gateway tool; gateway classifies it (read vs money-moving) and checks policy.
2. **Approve:** money-moving requests create a pending action with authoritative details; human approves; one-use signed token issued.
3. **Execute:** verify token, recompute action hash, resolve authoritative facts, reserve budget transactionally, call PayPal, reconcile, audit.
4. **Attack run:** harness replays scripted attacks and records outcome per case.

### 7.4 Approval token (suggested)
Signed payload: `tool, resource_id, amount_minor, currency, merchant_id, policy_version, nonce, iat, exp`. Store consumed nonces; execute atomically with marking consumption.

## 8. Architecture and repo layout

```
/apps/gateway   gateway API: tools, policy, approvals, budget, audit
/apps/web       approval inbox + attack report viewer
/apps/agent     untrusted agent runner (Nebius)
/packages/paypal sandbox-bound client, pinned toolkit adapter
/packages/core  money, policy, approvals, budget reservation, audit
/packages/redteam attack cases, runner, report
/docs           architecture.md, threat-model.md, spike-notes.md, comparison-vs-payleash.md
/fixtures       redacted payloads (labeled), recorded attack traces (labeled)
```

## 9. Tech stack

TypeScript (strict), Node LTS; a small API framework (Hono/Fastify) or Next.js; Tailwind + shadcn/ui; PostgreSQL (UNVERIFIED host expiry through Dec 15); Drizzle or Prisma; pg-boss if jobs are needed; Zod; `jose`; pinned `@paypal/agent-toolkit` (or ordinary REST via generated types); Pino; Vitest + fast-check + Playwright; gitleaks + GitHub Actions; Docker Compose; Render or similar (UNVERIFIED credit math). **AI provider: Nebius Token Factory via `openai` SDK** (Section 10). Sponsors: Render, Postman only.

## 10. AI layer (Nebius Token Factory, budget-limited)

### 10.1 Documented
OpenAI-compatible API at `https://api.tokenfactory.nebius.com/v1/`; docs pages exist for structured output/JSON and function calling. Third-party calculators list per-1M-token prices (GLM-5 about $1.00/$3.20, MiniMax-M3 about $0.30/$1.20, INTELLECT-3 about $0.20/$1.10); not official, so verify. Reasoning models may add reasoning-token overhead.

### 10.2 UNVERIFIED
$50 credit expiry/restrictions (from another hackathon, shared with another challenge); available model IDs; tool-calling and structured-output reliability on the chosen model; current official prices; dashboard caps; rate limits.

### 10.3 Budget design
Suggested cap about $8 to $10 total, about $1/day, enforced in code with an `ai_usage` ledger; per-IP rate limit; `max_tokens` cap; cache; cheap non-reasoning model preferred. **Agent loops are the main cost risk:** cap steps per run and tokens per run, and stop runaway loops. The red-team suite should run **deterministically from recorded traces** by default (no model cost) with a small number of live-model runs on demand. If the budget is exhausted, show "live agent paused; recorded traces available" (clearly labeled). Key stays server-side; sandbox data only; rotate after the hackathon.

### 10.4 AI contract
The agent only emits tool requests through the gateway schema. It is never trusted for authorization, amounts, or targets. An optional explanation model summarizes outcomes using gateway-provided facts only, with validators like in the other concepts.

### 10.5 Evaluation
Not model accuracy but **security outcomes**: for each attack case, record whether the gateway blocked it, and compare with a naive baseline (agent given the raw toolkit with direct credentials) to show the delta. Report residual cases honestly.

## 11. PayPal API surface (sandbox base `https://api-m.sandbox.paypal.com`)

OAuth `POST /v1/oauth2/token`. Candidate operations via toolkit or REST: `POST /v2/checkout/orders` (create), `GET /v2/checkout/orders/{id}`, `POST /v2/checkout/orders/{id}/capture` (needs buyer approval), `POST /v2/payments/captures/{capture_id}/refund`, `GET /v2/payments/captures/{id}`. Dispute `accept-claim` is available in the toolkit but should stay out of scope. Scope strings and entitlements for restricting capture vs refund are **UNVERIFIED**.

## 12. Gating spike (target about 2 days; comparison first)

| # | Task | Record |
|---|---|---|
| S1 | Read PayLeash and RefundLab code/READMEs; write `comparison-vs-payleash.md` | What they already do, what they don't; is the workbench niche real |
| S2 | Sandbox accounts; pin toolkit version; obtain live `tools/list` if possible | Live inventory vs source registry |
| S3 | Ordinary buyer-approved checkout, gateway-approved capture, verified order retrieval (or capture-bound refund) | Real successful sandbox responses |
| S4 | Check credential scope restriction options | Can capture/refund be restricted at token level |
| S5 | Deterministic mutations: amount/currency/resource mismatch, expired/reused approval, unknown tool, direct token access attempt, concurrent budget race, ambiguous response/retry | Blocked vs residual, per case |
| S6 | Compare a naive baseline agent (raw toolkit) against the gateway | Demonstrated delta |
| S7 | Nebius agent test: tool-calling reliability, step/cost per run | Model id, cost per run |
| S8 | Test remote MCP and local toolkit paths | Which works reliably; fallback decision |

**Go only if** the API happy path works, the agent cannot retrieve or bypass credentials, adversarial cases are reproducible, and the team can articulate a narrower user benefit that PayLeash, RefundLab, and the others don't already deliver. **No-go/pivot** if it is mainly an approvals dashboard, depends on unavailable AP2/Agent Ready APIs, needs unstated paid keys, or can't show real PayPal centrality with a working AI path. Local cryptographic proofs alone are insufficient.

## 13. Build plan (proposal; assumes pivot around Oct 13 or later)

| When | Focus |
|---|---|
| Spike days | Comparison, happy path, attack feasibility |
| Next 10 days | Gateway, policy, approvals, budget reservation, audit |
| Following 10 days | Approval inbox, agent runner, red-team harness and report |
| Last week | Hosting, judge access, docs, video, submission |

## 14. Testing

Property tests for money and budget math; concurrency tests (N parallel requests vs cap); approval mutation/expiry/reuse tests; unknown/unclassified tool tests; credential-access attempt tests; sandbox-guard startup tests; inventory drift tests; contract tests on redacted payloads; Playwright for the approval inbox.

## 15. Threat model and attack suite

**Trusted:** gateway, DB transactions and reservations, key management, authenticated approval surface, PayPal REST responses over TLS. **Untrusted:** the LLM and its conversation, customer emails, product descriptions, tool-supplied text, client-side arguments. **Out of scope:** compromised gateway/admin/keys, PayPal/network compromise, a malicious authorized owner, physical fulfillment, semantic truth of recommendations.

| Attack case | Honest protection claim | Limitation |
|---|---|---|
| Email tells support agent to refund an unrelated valid capture | Rejected: capture not bound to the approved support case/merchant records | Refund of a captured payment is not an arbitrary-address transfer |
| Approved $20 action mutated to $200 or different currency/resource | Action hash mismatch blocks before any API call | Owner must see authoritative details, not an LLM summary |
| 20 concurrent $10 requests against a $50 cap | Transactional reservation admits at most the cap | Only credible if actually tested under concurrency and ambiguous outcomes |
| Retried approval or repeated capture/refund | One-use approval and stable action record prevent duplicate application execution | Ambiguous PayPal responses need reconciliation |
| Dependency adds a new financial tool | Inventory snapshot rejects unclassified tools | PayLeash already fails closed on unknown tools; not novel alone |
| Product description pressures wrong item within budget | Explicit category/SKU constraints can reject | A budget or mandate cannot judge taste, price fairness, or description truthfulness |
| Agent tries to read the token or call raw REST | No token in model context; no REST escape hatch | Compromised gateway defeats this |

## 16. Demo plan (under 3 minutes)

1. Show the naive baseline agent (raw toolkit) being talked into a wrong refund or capture in sandbox.
2. Same attack through the gateway: blocked, with the reason.
3. A legitimate action: approval inbox shows authoritative details; approve; real sandbox capture succeeds.
4. Concurrency race against the budget; show it stays within the cap.
5. Show the generated attack report with blocked and residual cases.
State plainly: sandbox, not production-approved, not a universal defense.

## 17. Compliance checklist

Submit before the deadline and verify receipt; public repo with detectable license and no secrets in history; YouTube video under 3 minutes; README with run/test access and honest limitations; hosted demo or Docker with server-side AI key working through Dec 15 (recorded-trace mode must be labeled); confirm hosting, DB, and Nebius credit expiry; English materials.

## 18. Risks and open questions

1. Differentiation: PayLeash, Bursar, Pulse, BoundPay, RefundLab overlap. 2. No validated user. 3. Native PayPal controls may arrive (Agent Ready, roadmap: Copilot Checkout launched Jan 8, 2026; Hey Savi UK launch June 2, 2026). 4. Hosted MCP and vault issues from participant reports. 5. Credential scope restriction unknown. 6. AP2/Mastercard native support unknown. 7. Agent loop cost on shared Nebius credits. 8. This prize category is likely the most competitive. 9. Hosting/DB/Nebius credit longevity. 10. Team skills unspecified (this concept leans heavily on backend and security skills).

## 19. Definition of done

Comparison doc written and the niche argued honestly; real sandbox happy path working through the gateway; attack suite reproducible with a generated report including residual cases; baseline-vs-gateway delta shown; budget guard enforced; threat model documented; limitations stated; video and submission complete before the deadline.

## 20. Sources

PayPal official: `paypal/agent-toolkit` (pinned commit a3aa963..., `tools.ts`, `api.ts`, `client.ts`, `functions.ts`), `paypal/paypal-mcp-server`, `paypal/AI-Toolkit` (plugin config, hooks, mcp-tools reference), MCP registry and npm/PyPI metadata; PayPal newsroom (Oct 27/28, 2025 announcements; Jan 8 and June 2, 2026 launches). Standards: FIDO announcement (April 28, 2026), AP2 spec and release v0.2.0, AP2 SDK README. Competitors: PayLeash (README plus `guard.ts`), Bursar, Pulse, BoundPay, PayGuard, OpenAgentPay; Payman, Crossmint, Skyfire, Stripe Issuing, Nevermined docs. Threat research: Anthropic Project Vend, AgentDojo, "Whispers of Wealth." Nebius docs (docs.tokenfactory.nebius.com) with third-party price calculators to verify. Full reports: `paypal-hackathon-research-brief.md` and the verification report.
