<a id="readme-top"></a>

<div align="center">

# 🎯 Prompt Quality Inspector
## 提示词质量巡检器

Industrial prompt diagnostics & rewrite for **project-building prompts**.

[中文](#zh) · [English](#en)

![Skill](https://img.shields.io/badge/Skill-prompt--quality--inspector-6f42c1)
![Mode](https://img.shields.io/badge/Mode-score%20%2B%20optimize-0ea5e9)
![Output](https://img.shields.io/badge/Output-execution--ready-16a34a)

</div>

<details>
  <summary><strong>目录 / Table of Contents</strong></summary>

- [🇨🇳 中文](#zh)
  - [适用场景](#zh-when)
  - [你将获得](#zh-what)
  - [安装](#zh-install)
  - [使用](#zh-usage)
  - [面板预览](#zh-preview)
  - [仓库结构](#zh-layout)
  - [检索关键词](#zh-keywords)
- [🇺🇸 English](#en)
  - [When to use](#en-when)
  - [What you get](#en-what)
  - [Install](#en-install)
  - [Usage](#en-usage)
  - [Panel preview](#en-preview)

</details>

---

## 🇨🇳 中文 <a id="zh"></a>

### 适用场景 <a id="zh-when"></a>

- 你在写“做一个系统/做一个产品”的提示词，但 **范围太大、规则不量化、缺状态机/边界/验收**。
- 你希望把想法变成 **可实现、可测试、可运维** 的一体化 Prompt。

### 你将获得 <a id="zh-what"></a>

1. **可解释评分（0–100）**：总分 + 分项分 + 扣分依据 + 风险分级（Blocking / High risk / Later）。
2. **高优先级澄清（3–7 个问题/轮）**：带选项，按影响力排序。
3. **最终一体化 Prompt**：把需求 + 技术栈 + 数据边界 + 安全 + 可观测性/运维打包成可执行规格，并标注 ✅ 已确认 / ⚠️ 假设项。

> 说明：该 skill 的面板标签与最终 Prompt **强制英文输出**（来自 `SKILL.md` 的硬规则）。

### 安装 <a id="zh-install"></a>

推荐：

```bash
npx skills add Haaaiawd/prompt-quality-inspector-skill
```
### 使用 <a id="zh-usage"></a>

把你的提示词/PRD 草稿直接贴给 agent，然后说：

```text
Score this prompt only.
```

```text
Optimize this prompt into an execution-ready on-prem specification.
```

```text
Score + optimize this project PRD prompt. Ask clarification questions first.
```

### 面板预览 <a id="zh-preview"></a>

<details>
  <summary><strong>点击展开：Primary / Compact / PlainTextFallback（来自 SKILL.md）</strong></summary>

#### Primary (>= 80 columns)

```text
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

#### Compact (60–79 columns)

```text
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

#### PlainTextFallback (< 60 columns / logs / NO_COLOR)

```text
PROMPT_CHECK score=84/100 grade=A risk=MEDIUM verdict=SHIP_WITH_SCOPING
scorecard=goal:8 scope:12 role:9 flow:13 data:8 nfr:13 val:8 tech:9 ops:4
top=[B1]mvp_slice_missing; [H1]idempotency_gap; [L1]audit_gap
next=Q1:mvp_loop Q2:environment Q3:audit_retention
```

</details>

### 仓库结构 <a id="zh-layout"></a>

本仓库按“可发布 skill 仓库”组织：

```text
skills/
  prompt-quality-inspector/
    SKILL.md
    README.md
```

（如果你是把 skill 内嵌到其它仓库，也可以使用 `.github/skills/<skill>/SKILL.md` 的布局。）

### 检索关键词 <a id="zh-keywords"></a>

`prompt review`, `prompt quality`, `scoring rubric`, `spec prompt`, `PRD-to-implementation`, `requirements`, `architecture`, `offline`, `on-prem`, `security`, `observability`

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

## 🇺🇸 English <a id="en"></a>

### When to use <a id="en-when"></a>

- Your project prompt/spec is broad, under-specified, or contradictory.
- You want a measurable, repeatable prompt quality gate before implementation.

### What you get <a id="en-what"></a>

- **Explainable 0–100 score** with sub-scores and evidence-based deductions.
- **3–7 high-impact clarification questions** (with options) per iteration.
- **An execution-ready integrated prompt** (requirements + stack + data + security + operations), with ✅ confirmed vs ⚠️ assumed.

> Note: per `SKILL.md`, the UI labels, questions, suggestions, and final rewritten prompt are English-only.

### Install <a id="en-install"></a>

```bash
npx skills add Haaaiawd/prompt-quality-inspector-skill
```
### Usage <a id="en-usage"></a>

Paste your prompt/spec, then ask for:

- `score-only`
- `optimize-only`
- `score+optimize` (default)

### Panel preview <a id="en-preview"></a>

See the bilingual section above (same templates).

<p align="right">(<a href="#readme-top">back to top</a>)</p>
