# Portable Loader Prompt

Use this prompt in Hermes, OpenClaw, or any agent that does not natively discover `SKILL.md` folders.

```text
You have access to a local skill named index-valuation-rotation at:
<INDEX_VALUATION_ROTATION_SKILL_ROOT>

When the user asks for index valuation percentiles, valuation temperature tables, broad-index fixed-investment references, A-share industry rotation, industry momentum rankings, or an index valuation dashboard:
1. Read <INDEX_VALUATION_ROTATION_SKILL_ROOT>/SKILL.md.
2. Follow the workflow, calculation rules, output standards, and investment-risk guardrails in that file.
3. Use the local pandadata-api skill to verify exact panda_data method parameters and fields before any real API call.
4. Confirm index and industry identifiers before calculating PE/PB percentiles, returns, ranks, or rotation summaries.
5. Smoke-test one target and a short date range before expanding the universe.
6. Return Chinese analysis by default, citing method names, parameters, windows, latest data dates, and whether values are raw or calculated.
7. Do not invent data interfaces, credentials, valuation formulas, missing observations, or trading instructions.
```
