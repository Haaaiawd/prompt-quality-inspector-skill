---
name: prompt-quality-inspector
description: 'Industrial prompt review and optimization. Use when: you want a credible, explainable score (0–100) for a prompt/spec, then guided clarification and a rewritten, execution-ready integrated prompt (product + tech stack + data + security + operations). Keywords: prompt review, prompt quality, scoring rubric, spec prompt, PRD-to-implementation, requirements, architecture, offline/on-prem, security, observability.'
argument-hint: 'Paste your prompt/idea. Optional: score-only / optimize-only / score+optimize. If you already know the target tech stack, environment (offline/on-prem/cloud), and deliverables, include them.'
user-invocable: true
---

# Prompt Quality Inspector

## What this skill does
- **Scoring diagnostics (primary)**: produces an explainable quality score (0–100) with sub-scores, concrete deductions, risks, and reproducible reasoning.
- **Guided optimization (secondary)**: asks 3–7 prioritized clarification questions, then outputs an **execution-ready integrated prompt** (requirements + tech + data + security + ops), clearly marking what is confirmed vs assumed.

Design goal: make prompt quality measurable, repeatable, and iteratively improvable.

## When to use
- “Score this prompt/spec and tell me what’s missing.”
- “Turn my idea into an industrial-grade prompt that an AI can implement.”
- You have a long requirement dump and worry it is too big / too vague / not executable.
- You want an offline/on-prem, security-heavy system prompt that includes requirements + engineering constraints.

## Output contract (pseudo UI)
Always start with a “console panel” that includes two tabs: **SCORE** and **OPTIMIZE**.

## Terminal dashboard output spec (Futuristic Console Edition)
Style target: future-modern, high-contrast, operator-console feel.
Hard constraints (aligned with `terminal-logo-ui` principles): decide output type first, then width tiering, then graceful degradation; meaning must be readable without color; provide an ASCII-safe fallback for logs/CI.

### Output types
- Default output type: `header` (high-frequency, information-dense).
- If the user explicitly asks for “welcome screen / startup banner”: output `welcome screen` (lower-frequency, more context).

### Width tiers and fallback
- `>= 80`: `primary`
- `60–79`: `compact`
- `< 60` OR `NO_COLOR`/logs/CI: `plainTextFallback` (ASCII-safe)

### Characters and borders
- `primary/compact`: may use box-drawing characters (e.g., `╔═╗`, `┌─┐`, `◆`, `▌`), but **never rely on color for comprehension**.
- `plainTextFallback`: ASCII-safe characters only (`A-Z0-9-_+=|/\\` etc.). Must be stable for copy/paste and log ingestion.

### Language rule (hard)
- Panel labels, verdicts, questions, suggestions, and the final rewritten prompt: **English only**.
- If the user’s input is Chinese, you may understand it internally, but outputs remain English.

### Must provide three renderings
When you output the pseudo UI, always include:
- `primary`
- `compact`
- `plainTextFallback`

Callers can pick based on terminal width.

### Tab content constraints
- **[SCORE]** must include: total score/grade, a sub-score matrix, top 3 issues, categorized findings (Blocking / High risk / Later), plus “why” (reproducible rationale).
- **[OPTIMIZE]** must include:
  - 3–7 clarification questions with options (or example answers), ordered by impact
  - a “draft prompt you can already write now” (mark unknowns as ⚠️)
  - a “next minimal info list” (<= 5 items)

### Pseudo UI templates (switch by width only)
Rule:
- width `>= 80` → `primary`
- width `60–79` → `compact`
- width `< 60` OR logs/no-color → `plainTextFallback`

#### Primary (>= 80)
```
╔══════════════════════════════════════════════════════════════════════════════╗
║ ◢ PROMPT QUALITY INSPECTOR ◣                            PROFILE: NEON-FUTURE ║
╠══════════════════════╦══════════════════════╦════════════════════════════════╣
║ SCORE      84 / 100  ║ GRADE      A         ║ VERDICT   SHIP WITH SCOPING    ║
║ RISK       MEDIUM    ║ CONFIDENCE HIGH      ║ MODE      SCORE + OPTIMIZE     ║
╠══════════════════════╩══════════════════════╩════════════════════════════════╣
║ SCORE MATRIX                                                                 ║
║ Goal & Acceptance   8/10   Scope Control      12/15   Roles & Access      9/10║
║ Flow & State       13/15   Data Boundaries     8/10   NFR & Constraints  13/15║
║ Validation & Edges  8/10   Tech Fit            9/10   Operability         4/5 ║
╠══════════════════════════════════════════════════════════════════════════════╣
║ PRIORITY SIGNALS                                                             ║
║ [B1] MVP slice missing: pick one production-grade end-to-end loop first      ║
║ [H1] Retry/idempotency gap: key strategy and TTL not explicit                ║
║ [L1] Audit export gap: watermark schema and retention not pinned             ║
╠══════════════════════════════════════════════════════════════════════════════╣
║ NEXT ACTIONS                                                                  ║
║ Q1 Pick MVP loop: A) Author->Review->Publish  B) Assemble->Practice  C) Ops  ║
║ Q2 Environment: A) Offline single-box  B) On-prem LAN  C) Cloud              ║
║ Q3 Audit retention: A) 1y  B) 3y  C) 7y                                      ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

#### Compact (60–79)
```
┌──────────────────────────────────────────────────────────────────┐
│ PROMPT QUALITY INSPECTOR                              FUTURE OPS │
├──────────────────────────────────────────────────────────────────┤
│ score 84/100 | grade A | risk MEDIUM | verdict SHIP+SCOPING     │
├──────────────────────────────────────────────────────────────────┤
│ matrix: goal8 scope12 role9 flow13 data8 nfr13 val8 tech9 ops4   │
├──────────────────────────────────────────────────────────────────┤
│ top: [B1] no MVP slice; [H1] idempotency gap; [L1] audit gap     │
│ next: Q1 loop? Q2 env? Q3 retention?                             │
└──────────────────────────────────────────────────────────────────┘
```

#### PlainTextFallback (< 60 / logs / NO_COLOR)
```
PROMPT_CHECK score=84/100 grade=A risk=MEDIUM verdict=SHIP_WITH_SCOPING
scorecard=goal:8 scope:12 role:9 flow:13 data:8 nfr:13 val:8 tech:9 ops:4
top=[B1]mvp_slice_missing; [H1]idempotency_gap; [L1]audit_gap
next=Q1:mvp_loop Q2:environment Q3:audit_retention
```

## Scoring rubric (0–100)

## Integrated “industrial prompt” structure (extracted from strong samples)
This skill is **not** trying to force a pure PRD.
It targets an integrated, implementation-oriented prompt that bundles:
requirements + tech stack + data + security + reliability + operations, in one coherent spec.

### The 8-part backbone (recommended order)
1. **One-line positioning**: what system, for whom, what problem.
2. **Roles & access boundaries**: 3–7 roles; who can do/see what; why separation exists.
3. **UX flows (task-first)**: key screens/actions/feedback; states, timers, validation banners.
4. **Business rules (quantified)**: thresholds, windows, caps, examples; exception handling.
5. **State machines & closed loops**: lifecycle, timeouts, rollback, idempotency expectations.
6. **System architecture**: offline/on-prem assumptions; decoupling; avoid unnecessary complexity.
7. **Data & auditability**: core entities, retention, exportability, immutability where needed.
8. **Security & reliability**: auth, encryption/masking, rate limits, upload allowlists, retries/degrade.

### Reusable thinking angles from strong prompts
- Start from closed loops: every module has a complete lifecycle.
- Make “invisible engineering” explicit: concurrency, idempotency, conflicts, validation, abnormal flows.
- Quantify rules so they’re testable: e.g., `30 min auto-cancel`, `60 req/min`, `p95 < 800 ms`.
- Front-load governance: audit, least privilege, masking, export watermark, dual control.
- Be realistic for offline/on-prem: no SMS/maps/cloud; local queues; deterministic fallback modes.

### Common weak patterns (deductions)
- Feature pile-up without a main loop.
- Treating unknowns as known (demanding “smart analytics” without definitions/data).
- Demanding high-difficulty capabilities without constraints or acceptance.
- Contradictory assumptions (e.g., “fully offline” + third-party online dependencies).

### Dimensions and weights
1. **Goal & success criteria (10)**: what outcome, what artifacts, how to verify.
2. **Scope control & executability (15)**: MVP boundary; staged plan if large.
3. **Users/roles & access boundaries (10)**: who uses it, who administers, data visibility.
4. **Core flows & state machines (15)**: closed loop; states; timeout/rollback expectations.
5. **Data model & system boundaries (10)**: entities; relationships; storage; inputs/outputs.
6. **Constraints & NFRs (15)**: security, privacy, offline/on-prem assumptions, audit, retention, perf.
7. **Validation & edge cases (10)**: input checks; conflict handling; abuse limits; failure modes.
8. **Tech stack & coherence (10)**: stack choices match requirements; no gratuitous complexity.
9. **Operability & verifiability (5)**: monitoring/alerts/runbook checkpoints; test points.

Scoring principles:
- Specific + testable + executable earns points.
- “Hand-wavy but expensive engineering” loses points.
- Contradictions and hidden assumptions are heavily penalized.

### Calibration rules (human-friendly, sample-aligned)
- If the prompt covers **>= 6 out of the 8 backbone sections**, it should **not** score below **B (70)** unless it is contradictory or critically incomplete.
- “Scope too large” typically drops **at most one grade**, and the output must include a clear scoping/MVP recommendation.
- Do **not** penalize missing endpoint-level API contracts at early stages. Treat detailed API specs as **optional** unless the user explicitly asks for them or says “generate code now.”

### Grades
- **S (90–100)**: ready to execute; gaps are small and controlled.
- **A (80–89)**: executable; a few critical clarifications needed.
- **B (70–79)**: good direction; needs a focused scoping pass.
- **C (60–69)**: scattered or missing key rules; split scope first.
- **D (40–59)**: many unstated assumptions; hard to implement safely.
- **F (0–39)**: non-executable (contradictory or massively incomplete).

### Red flags (automatic grade drop)
- No MVP slice / no iteration plan for a large scope.
- “Fill in all missing parts” while forbidding questions.
- Missing key facts for settlement/audit/risk but demanding correctness.
- Offline requirement while depending on online third parties.
- Unverifiable outcomes (“build a powerful system”) without testable rules.

## Workflow (Score + Optimize)

### Step 0: Detect user intent
- If user wants score-only: output SCORE panel + verdict + minimal actions.
- If user wants optimize: show diagnosis summary, then clarification.
- If unclear: default to **score + optimize**.

### Step 1: Structured extraction
Extract and list:
- goals and user value
- roles/objects/permissions
- core flows (including states)
- quantified rules (thresholds/windows/caps)
- data/storage/integration boundaries
- NFRs and compliance assumptions
- confirmed vs unknown

### Step 2: Score with reproducible “why”
- Total score + sub-scores.
- Concrete deductions with evidence (“what is missing and what risk it creates”).
- Categorize findings:
  - **Blocking**: cannot proceed without answers
  - **High risk**: likely to cause rework
  - **Later**: safe to defer for MVP

### Step 3: Scope shaping (if too large)
When scope is exploding:
- propose **one MVP slice**: a single end-to-end closed loop
- park the rest into Phase 2/3 with dependencies
- output a “what I removed” list so nothing feels silently dropped

### Step 4: Guided clarification (core of optimization)
Ask **3–7 questions per round**, prioritized, with options.

Default question set (trim as needed):
1) Primary users (1–2 types)?
2) Target environment (offline single-box / on-prem LAN / cloud)?
3) Target tech stack constraints (frontend/backend/DB), or “no preference”?
4) What is the single MVP closed-loop flow?
5) What are the top 5 quantified rules (timeouts, caps, thresholds)?
6) Data boundaries and permissions (who can read/write/export)?
7) Compliance/security expectations (encryption, retention, audit immutability)?

### Step 5: Output the final integrated prompt
Produce:
- **A. Quick orientation** (one-line + deliverables)
- **B. Integrated implementation spec** (requirements + tech + data + security + ops)
- **C. Constraints and acceptance** (NFRs + rejection rules + acceptance tests)

Explicitly mark:
- ✅ confirmed by user
- ⚠️ proposed assumptions (needs confirmation)

## Integrated prompt template (copy/paste)
Replace `{...}`. Do not state unknowns as facts.

```text
You are a senior product architect and engineering lead.
Convert the following idea into an execution-ready integrated specification and coding prompt.
The output must be structured, testable, offline/on-prem realistic (if applicable), and operationally safe.
If critical information is missing, ask up to 7 clarification questions with selectable options.

[Business Idea]
{user_raw_idea}

[Objectives]
- Target users: {user_types}
- Problem to solve: {core_problem}
- Success criteria (testable):
  1) {acceptance_criteria_1}
  2) {acceptance_criteria_2}

[Scope]
- MVP must include exactly one production-grade end-to-end loop:
  - {one_closed_loop_flow}
- Explicitly out of scope for MVP:
  - {excluded_items}
- Phase plan (optional): {phase_2_phase_3}

[Roles & Permissions]
- Roles: {role_A, role_B, role_C}
- Data visibility and allowed actions per role: {rules}

[Core Flows & State Machines]
- Primary flow steps: {trigger_to_completion_steps}
- State machine (at least one core object):
  - Object: {order/ticket/document/...}
  - States: {S1 -> S2 -> S3}
  - Allowed/forbidden transitions: {rules}
  - Timeouts, rollback expectations, and idempotency expectations: {rules}

[Data Model & Storage]
- Core entities and key fields: {entities}
- Relationships, uniqueness, and indexing hints: {constraints}
- Audit/event logging: {what_to_log}
- Retention and deletion rules: {retention_policy}

[Rules & Validation]
- Input validation: {required/format/range}
- Concurrency/conflict handling: {locks/idempotency_key/optimistic_versioning}
- Abuse protection: {rate_limits/captcha/lockouts}

[Tech Stack & Deployment]
- Frontend: {stack_or_constraints}
- Backend: {stack_or_constraints}
- Database: {db}
- Deployment topology: {single_machine/on_prem_lan/cloud}
- Forbidden dependencies (if any): {no_sms/no_external_map/no_third_party_payments/...}

[Security & Privacy]
- Authentication: {username_password/session/jwt} with password rules and lockout
- Encryption at rest: {fields} and key management assumptions
- Masking in UI/logs: {rules}
- Input/output safety: {xss/csrf/sql_injection_defenses}

[Operations & Observability]
- Background jobs/queues: {where_needed} with retry and backoff policy
- Metrics and alerts: {latency/error_rate/queue_depth/...} with thresholds
- Backups and recovery: {schedule/retention/restore_steps}
- Degradation strategy: {what_to_disable_or_cache_under_stress}

[Required Output]
1) MVP backlog (prioritized)
2) Data model draft (tables/fields/indexes)
3) Acceptance tests (>= 8, >= 10 preferred) for the MVP loop (happy + edge + failure)
4) Risk & tradeoff matrix (blocking/high/later)
5) Optional: API surface sketch (only if building a decoupled client/server, otherwise omit)
```

## Quality gates (before presenting the final rewritten prompt)
- At least one end-to-end MVP loop is fully specified (trigger → process → done/fail).
- At least one core object has an explicit lifecycle/state machine.
- Acceptance criteria/tests are present and measurable (>= 8, >= 10 preferred).
- MVP vs Phase 2/3 is explicit.
- Tech stack + environment assumptions are explicit.
- Any critical unknowns are marked as ⚠️ assumptions or asked as questions.

## Writing rules (hard)
- No “etc.” / “and so on.” / “fill in the rest.”
- Do not invent major modules as if they were obvious.
- For numeric rules: use user-provided numbers or mark as ⚠️ needs confirmation.
- If scope is too big: scope down first; do not output an impressive but non-executable fantasy spec.
- The final prompt output must be English.

## Gold standard example (English)
Purpose: a reusable, production-grade prompt blueprint.

### Example prompt: Offline Question Bank Governance Console
```text
You are a senior product architect and full-stack engineering lead.
Transform the following idea into an execution-ready specification and coding prompt.
Your output must be structured, measurable, auditable, and offline-first.
If critical information is missing, ask up to 7 clarification questions with selectable options.

[Business Idea]
Build an offline Question Bank Governance Console for internal curriculum and operations teams.
Core modules include question authoring, review/publish workflow, exam assembly, practice delivery,
CSV import/export, and immutable audit trails.

[Goals]
- Primary users: Content Editor, Reviewer, Operations Admin, Instructor, Learner (read/practice only)
- Success criteria:
  1) End-to-end traceable lifecycle from create -> review -> publish -> assemble -> practice -> archive
  2) Every export is watermark-stamped (actor, timestamp, scope) and audit-searchable
  3) Fully functional in offline/on-prem mode without SMS, cloud storage, or third-party login

[Scope]
- MVP (single production-grade loop first):
  1) Question CRUD + review state machine
  2) Exam assembly with tag/difficulty/type filters
  3) CSV import/export with preview and row-level validation errors
- Out of scope (Phase 2): auto-question generation, knowledge graph, ML ranking

[Roles and Access]
- Content Editor: create/edit Draft, submit for review, cannot publish
- Reviewer: approve/reject with required reason code and notes
- Operations Admin: manage taxonomy, sensitive-term dictionary, export policy, user access, full audit view
- Instructor/Learner: read-only for published assets, scoped practice records

[Core State Machines]
1) Question lifecycle: Draft -> InReview -> Published/Rejected -> Archived
2) Exam lifecycle: Draft -> Locked -> Published -> Archived
Rules:
- No in-place edit after Published; create a new version and re-review
- Locked exams are immutable; changes require a new version
- Rejected items return to Draft with a new revision record

[Data and Audit]
- Define core entities, relationships, uniqueness constraints, and required indexes
- Include immutable event logs for publish/archive/export/permission changes
- Audit retention policy must be explicit (default proposal: 7 years)

[Security and Reliability]
- Local username/password auth only, minimum 12 characters, lockout after 5 failures / 15 minutes
- Encrypt sensitive fields at rest; mask fields in UI/logs by default
- Enforce idempotency expectations for critical write actions and conflict-safe concurrency rules
- Validate uploads with allowlist, MIME/header checks, size limits, and SHA-256 fingerprinting

[Required Output]
1) Prioritized MVP backlog
2) Data model draft (tables/fields/indexes)
3) At least 12 acceptance tests (happy path + edge/failure cases)
4) Risk and tradeoff matrix (blocking/high/later)
5) Optional: API surface sketch (only if needed for your implementation plan)
```
