---
name: index-valuation-rotation
description: Pandadata index valuation and A-share industry rotation analysis skill for building PE/PB percentile dashboards, valuation temperature tables, broad-index fixed-investment references, industry momentum rankings, and rotation summaries. Use when the user asks for 指数估值分位, 现在贵不贵, 估值温度计, 宽基指数定投参考, 行业轮动, 行业动量排名, 行业强弱切换, or an index valuation dashboard.
license: GPL-3.0-only
metadata:
  organization: QuantSkills
  organization_url: https://github.com/quantskills
  repository: skill-index-valuation-rotation
  repository_url: https://github.com/quantskills/skill-index-valuation-rotation
  project_type: skill
  collection: index-valuation-rotation
  creator: abgyjaguo
  creator_url: https://github.com/abgyjaguo
  maintainer: abgyjaguo
  maintainer_url: https://github.com/abgyjaguo
---

# Index Valuation & Rotation

Use this skill to turn index valuation and industry-rotation requests into traceable Pandadata calls, reproducible calculations, and a Chinese analytical deliverable.

## Maintenance & Scope

- Maintain this as a QuantSkills community skill in `quantskills/skill-index-valuation-rotation`.
- Creator and maintainer: `abgyjaguo` (`https://github.com/abgyjaguo`).
- Support index valuation snapshots, valuation dashboards, fixed-investment research references, industry momentum rankings, and rotation summaries.
- Depend on the sibling `pandadata-api` skill for method contracts and runtime data access.
- Treat all outputs as research and educational material. Do not present the skill as official, certified, verified, endorsed, production-ready, or as investment advice.

## Core Rules

- Load the local `pandadata-api` skill before making real API calls. Use its method index or search script to verify exact `panda_data` parameters and fields; do not guess method signatures.
- Confirm index and industry identifiers before analysis. Use `get_index_detail` for index candidates and `get_industry_detail` for industry taxonomy.
- Keep every number traceable. Report method names, parameters, data window, latest data date, and whether each value is raw API data or a calculation.
- Separate facts from interpretation. Present valuation percentiles and momentum ranks first, then label any allocation, fixed-investment, or rotation view as research interpretation.
- Write Chinese Markdown by default. Produce HTML/dashboard artifacts only when the user explicitly asks for a visual output.
- Avoid deterministic investment advice. End formal reports with: `本报告基于公开数据与规则化分析生成，仅供研究参考，不构成任何投资建议。`

## Workflow

1. **Classify the request**: index valuation snapshot, valuation dashboard, fixed-investment reference, industry momentum ranking, industry rotation report, or combined valuation-plus-rotation analysis.
2. **Define the universe**: use the user-provided index or industry list. If absent, default broad indexes to 上证50、沪深300、中证500、中证1000、创业板指, then add industry indexes only after coverage is confirmed.
3. **Verify API contracts**: inspect `pandadata-api` for `get_index_detail`, `get_index_indicator`, `get_index_daily`, `get_industry_detail`, `get_industry_constituents`, `get_index_weights`, and `get_stock_daily` as needed.
4. **Smoke-test narrow calls**: query one target and a short date range first. Check row count, columns, date format, symbol format, PE/PB availability, and latest data date before expanding.
5. **Align dates and windows**: use absolute dates, the latest completed trading day, and comparable lookback windows. Mark lagged, missing, or partial histories explicitly.
6. **Compute metrics**: calculate valuation percentiles, recent returns, momentum ranks, rank changes, and coverage diagnostics from raw rows.
7. **Generate the deliverable**: include conclusion, evidence tables, calculation notes, data-quality notes, and method/parameter appendix.

## Valuation Dashboard

- Pull PE/PB series with `get_index_indicator`; use at least 5 years when available. If the usable window is shorter, downgrade the label and explain why.
- Calculate percentile as the share of historical observations less than or equal to the latest valid observation. Treat PE and PB separately.
- Use a simple valuation temperature band unless the user provides another rule: `<20%` low, `20%-80%` neutral, `>80%` high.
- Add recent index performance from `get_index_daily`, preferably 20/60/120 trading-day returns.
- Show compact evidence columns: index, method, latest date, PE, PE percentile, PB, PB percentile, return windows, coverage days or years, and note.

## Industry Rotation

- Prefer direct industry index data when a reliable industry index exists. If unavailable, aggregate constituent stocks from `get_industry_constituents` and `get_stock_daily`.
- State the aggregation rule in the output. Use market-cap weighting only when the required weights or capitalization fields are available; otherwise use equal weighting and label it.
- Calculate 20/60/120 trading-day momentum, current rank, prior rank, rank change, and persistence of improvement or deterioration.
- Use `get_index_weights` when the user asks how rotation affects a broad index; report weight coverage and date before attributing index impact.
- Identify leaders, laggards, improving industries, weakening industries, and representative constituents without turning them into trading orders.

## Output Standards

- Always include a data note with API methods, parameters, date range, latest data date, and missing-coverage handling.
- Explain calculated fields briefly: percentile, return window, rank change, and aggregation rule.
- Use tables for evidence and short bullets for interpretation.
- If data calls fail or return empty rows, preserve useful partial output and add the exact failed method, parameters, queried window, and next diagnostic step.
- For fixed-investment references, frame results as valuation discipline and risk budgeting, not personalized investment advice.

## Cross-Agent Use

- Codex and Claude Code can load this folder directly as a skill named `$index-valuation-rotation`.
- Cursor should use `agents/cursor-rule.mdc` as the project rule adapter and keep the full skill folder under `.cursor/skills/index-valuation-rotation`.
- Hermes and OpenClaw can load the full `SKILL.md` folder when installed into their skill roots. If native discovery is unavailable, paste `agents/portable-loader.md` into the agent as the loader prompt.
- Keep `SKILL.md`, `agents/openai.yaml`, `agents/cursor-rule.mdc`, and `agents/portable-loader.md` consistent when changing triggers, workflows, or guardrails.
