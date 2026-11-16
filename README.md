# CCAR-P Exam Cheat Sheet

Minimum decision rules for **Claude Certified Architect – Professional (CCAR-P)**.

Unofficial study aid. Not affiliated with, endorsed by, or sponsored by Anthropic.

**Exam facts (confirm on official guide):** ~63 questions · 120 minutes · pass ~720/1000

---

## Meta rule

Find the **binding constraint** first (SLA / safety / cost / data / ownership). Then read the options.

Correct answer is almost always a **systems control** — rarely “bigger model” or “better prompt”.

---

## 1) Solution Design

| Situation | Pick |
|---|---|
| Ambiguous “AI chatbot” ask | Discovery first → journeys, owners, acceptance (capability / error / data / HITL / boundary) |
| What Claude should own | Language + judgment → Claude; single-right-answer rules → code; irreversible risk → human |
| Pattern choice | Augmented LLM (1 call + tools) < Workflow (fixed graph) < Agent (path uncertain, bounded autonomy) — simplest that meets the bar |
| Multi-agent? | Only if measured win: parallelism / context isolation > coordination cost |
| Orchestration | 1 responsibility = 1 owner; 1 shared field = 1 writer |
| Handoff | state + evidence + done + recommended action |
| Failed branch | keep verified partials; never accept incomplete as final |
| Value pillars | efficiency / transformation / productivity / cost / SLA — each own metric + baseline + owner |
| Cost | full ledger: tokens + retries + tools + evals + humans + lifecycle |
| SLA | user-journey targets (p95, quality %, availability) — not model latency alone |

---

## 2) Models / Prompts / Context

| Situation | Pick |
|---|---|
| Model pick | task eval evidence: quality vs latency vs cost; route hard cases up |
| Thinking / effort | raise only where measured quality gain > latency/cost |
| System prompt | standing policy only; request data in labeled slots |
| Untrusted content | isolate as data; never as instructions |
| Grounding | answer only from sources; abstain if unsupported |
| Prompt complexity | zero-shot → few-shot → CoT only if held-out lift proven |
| Context budget | allocate before admit; drop noise; keep decisive evidence |
| Long chat | compaction / tool-clear / memory with provenance — don’t treat notes as truth |
| Cache | **static first** (system + tools + policy), then dynamic; measure hit rate |
| Reuse | cache vs modular prompts vs Skills by change cadence + governance |

---

## 3) Integration / Tools / RAG

| Situation | Pick |
|---|---|
| Capability bloat | inventory by role → remove unused / high-risk tools (least privilege) |
| New tool | need + authz + eval (select / deny / recover) before ship |
| Auth | user vs service identity clear; creds never in prompt; authorize *before* context |
| Accuracy vs latency | match lever to measured error; verification must earn its delay |
| Observability | 1 **trace_id** across model → tools → subagents → side effects |
| Metrics | latency / error / cost / quality / termination — **with denominators** |
| RAG wrong after doc update | suspect **stale index/chunks**, not the model |
| Retrieval vs full doc vs live tool | by freshness, permissions, answer shape |
| Measure RAG | retrieval quality separate from answer quality |
| MCP vs API/CLI vs A2A | reuse + identity + observability + failure isolation decide |
| Progressive discovery | load on demand when catalog huge; keep policy + audit after discover |

---

## 4) Eval / A-B / Optimize / Monitor

| Situation | Pick |
|---|---|
| Scorecard | accuracy + latency + cost + safety + security + **counter-metric** |
| Graders | **code > human > LLM**; prefer volume of auto-graded cases |
| Dataset | prod traffic + rare + adversarial + synthetic; hold-out untouchable |
| A/B | freeze baseline; **one** change; comparable traffic; staged rollout + rollback |
| “Is it better?” | n + variance + interval; random LLM noise ≠ lift |
| Diagnose fail | isolate layer (prompt / retrieval / tool / model / agent hop) before changing prod |
| Hallucination vs bad RAG | claim vs retrieved evidence; stale/missing/irrelevant vs unfaithful gen |
| Optimize cost | baseline first; cache/trim/fewer turns; never silently cut quality bar |
| Prod logs | version labels (prompt/model/index/tools) + usage + termination + tool outcomes |
| Alerts | separate error / cost / staleness / drift; named owners |

---

## 5) Governance / Safety

| Situation | Pick |
|---|---|
| Guardrail placement | **before harmful effect**: input screen → output screen → tool auth |
| Risky action | approval + **fail closed** if reviewer/policy down |
| Classifiers | price false block vs false allow; deterministic rules for must-always |
| Prompt injection | isolate content, shrink tools, constrain effects — don’t “trust the prompt” |
| HITL trigger | impact + evidence + policy — **not** model confidence |
| Risk register | likelihood × severity × detectability × reversibility; owner + residual acceptance |
| GDPR | purpose, minimize, retention, deletion across prompts/logs/memory/outputs |
| HIPAA | BAA + eligible config; don’t blindly stack ZDR unless required |
| FedRAMP | provider boundary ≠ your system authorization |
| Bias/fairness | measure by group; don’t treat aggregate accuracy as fairness |
| Ethics | notice + explain + appeal ownership for AI-assisted decisions |

---

## 6) Stakeholders / Lifecycle

| Situation | Pick |
|---|---|
| Discovery | journeys + conflict + who can accept/block |
| Output of discovery | bounded problem + prioritized acceptance + owned assumptions |
| Present options | comparable options; mandatory vs preference; recommend + consequences |
| Audience | exec = value/risk/cost; security = trust boundaries; eng = interfaces/SLO |
| Uncertainty | say variable model + bounded eval claims before promising prod |
| Feedback / SLA | cadence + owners; SLA includes exclusions, provider deps, human handoff, fallback |
| Pilot | predefined exit criteria before scale funding |
| Docs | versioned behavior config; ADR with reopen conditions |
| Handoff | ops can deploy/monitor/rollback **without** architect |
| Releases | model/prompt/index/tool as one release train + regression + rollback |
| Iterate | signals + outcomes → improve / reopen discovery / retire |

---

## 7) Claude Code / Team Enablement

| Situation | Pick |
|---|---|
| Team setup | cloneable bundle: instructions + permissions + hooks + MCP + skills |
| Org policy | **managed settings** enforce; local can’t override musts |
| Permissions | narrow allowlist + sandbox; secrets out of agent reach |
| Distribute skills/plugins | review source + bound updates before broad roll |
| AI-assisted PR | require tests/review/merge gates before accept |
| CI Claude | analysis ≠ mutation; side effects need owned policy |
| Debug Claude Code | diagnose **loaded** config (instructions/hooks/MCP) before relaxing controls |
| Incidents | read-only diagnose first; remediate only with human approval |

---

## Instant traps

1. Extra tools unused → **remove**, don’t “prompt harder”.
2. Huge static system prompt every call → **reorder + cache**.
3. Confident wrong after content refresh → **index/retrieval**, not model.
4. Multi-agent by default → usually **wrong**; justify cost.
5. HITL on “low confidence” only → **wrong**; use impact/policy.
6. Risky tool when checker down → **fail closed**.
7. A/B with many simultaneous changes → **invalid**.
8. Success = 200 from model → **wrong**; need outcome + side-effect truth.
9. Bigger model first → last resort after layer isolation.
10. Stakeholder (~14%) + governance (~14%) ≈ **28%** — scored architecture, not soft skills.

---

## 10-second decision tree

```
Safety/compliance break?     → fail closed / gate / shrink tools
Wrong or stale facts?        → retrieval/index/authz path
Too many / dangerous tools?  → remove & least privilege
Slow/expensive but OK quality? → cache, trim, fewer turns, route tiers
Unclear if better?           → frozen eval / one-change A/B
Path uncertain, need explore? → bounded agent (else workflow)
Many specialists needed?     → multi-agent only with contracts + trace
Stakeholder “build AI”?      → discovery → acceptance → pillar metrics
Team Claude chaos?           → managed settings + shared cloneable config
```

---

## Official sources

Always prefer Anthropic’s published CCAR-P exam guide and docs over third-party notes:

- Anthropic certification / partner learning portals
- Claude API docs: evals, prompt caching, tool use, MCP
- Claude Code docs: managed settings, hooks, permissions

Domain weights (approx.): Integration 19% · Solution Design 17% · Evaluation 16% · Governance 14% · Stakeholders 14% · Models/Prompts 13% · Developer Productivity 7%.

---

## License

[MIT](LICENSE)
