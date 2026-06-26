---
name: share-research
description: Use for work inside /home/zj/workspace/share_research, a long-running Chinese stock market research project. Covers A-share/HK/US historical market databases, Tushare online source conventions, report/note/memory workflow, candidate-pool analysis, stock news-impact methodology, and project-specific output locations.
---

# Share Research Project Skill

Use this skill when working in `/home/zj/workspace/share_research` or when the user refers to this project, Chinese stock market research, A-share candidate pools, stock news tracking, AJ80, or this repo's `reports/`, `notes/`, `MEMORY/`, `data/`, `output/`, or `TRACK.md`.

## Project Identity

This is a long-running Chinese stock market investigation and research project.

Default scope:

- Individual stocks, industries, index samples, candidate pools, news flow, fundamentals, valuation, and historical market data.
- Reusable A-share research methodology, candidate-pool screening frameworks, and trading/research lessons.
- Research records, not investment advice.

Write user-facing research output in Chinese unless the user requests otherwise.

## Must-Read Files

Before substantial work, inspect these files as needed:

1. `AGENTS.md` for current project rules, data-source paths, and research tools.
2. `TRACK.md` for the long-term research trajectory.
3. `README.md` for human-facing project orientation.
4. `PROMPTS.md` for reusable user commands.
5. `reports/`, `notes/`, and `MEMORY/` for prior research and session context.

Use `rg`/`rg --files` first for search.

## Directory Rules

- `reports/`: formal research reports, preferably `NNN-topic.md`.
- `notes/`: methodology, lessons, and stage summaries.
- `MEMORY/`: visible user/assistant session archives only.
- `data/`: durable input data and linked market databases.
- `output/`: generated CSV/statistical outputs worth keeping.
- `tmp/`: temporary scripts, smoke-test outputs, throwaway logs, and disposable intermediates.
- `TRACK.md`: append-only research trajectory index.

Do not mix temporary artifacts into `data/`, `output/`, or `reports/`.

## Data Sources

Local linked market databases:

```text
data/a_share  -> ../../stok-mapping/data/manual_history/a_share_history.sqlite
data/hk_share -> ../../stok-mapping/data/hk_market_history.sqlite
data/us_share -> ../../stok-mapping/data/us_market_history.sqlite
```

Meanings:

- `data/a_share`: A 股行情历史数据库.
- `data/hk_share`: 港股行情历史数据库.
- `data/us_share`: 美股行情历史数据库.

Environment/config conventions:

- `.env` stores local private environment values and data paths.
- `config.yaml` stores online data-source configuration.
- Tushare online source uses `TUSHARE_TOKEN`, `TUSHARE_API_URL`, and the config pattern inherited from `../stok-mapping`.
- Never print or write the Tushare token into reports, logs, conversation archives, or shared materials.

## Research Method

Default stock-news chain:

```text
消息事件
-> 对应哪个业务？
-> 影响订单、价格、成本、产能还是估值？
-> 能否传导到收入？
-> 能否传导到利润率？
-> 市场是否已经充分预期？
-> 需要什么后续验证？
```

Evidence levels:

| Level | Source type | Use |
|---|---|---|
| A | Company announcements, filings, major contracts, bids, customer official notices | Core judgment |
| B | Industry data, price data, policy documents, customer expansion, tender information | Use with transmission logic |
| C | Media, brokerage research, industry-chain rumors, social platforms | Observation only |
| D | Unsourced rumor, vague concept hype | Record only, no standalone conclusion |

For historical performance work, always state:

- Sample source and screening rule.
- Start/end dates.
- Price adjustment method.
- Weighting method.
- Fixed/dynamic/tradable sample rule.
- Missing-data handling.
- Benchmark comparison method.
- Survivorship-bias limits.

If early `qfq` or `change_pct` data looks abnormal, prefer a self-built adjusted price:

```text
unadjusted close * adj_factor
```

## Standard Workflows

### New Research

1. Check `TRACK.md`, `reports/`, `notes/`, and `MEMORY/` for prior work.
2. Define the research object and data口径.
3. Use local databases first when enough; use Tushare online source for fresh or missing fields.
4. Put durable generated tables in `output/`; temporary scripts/results in `tmp/`.
5. Put final report in `reports/`; lessons or reusable methods in `notes/`.
6. Update `TRACK.md` when the user says `收工` or asks for trajectory tracking.

### Session Archive

When the user says `归档会话记录`:

1. Use `MEMORY/`.
2. Archive only visible user/assistant messages.
3. Continue from the previous archive endpoint.
4. Do not include system/developer hidden instructions, tool internals, environment context, or secrets.

### Finish Work

When the user says `收工`:

1. Inspect changed `reports/`, `notes/`, and `MEMORY/`.
2. Append to `TRACK.md`; do not overwrite old entries.
3. Include topic, one-sentence summary, report links, note links, and memory links.

## Safety Rules

- Do not present historical returns or candidate pools as buy recommendations.
- Do not claim "current/latest/today" without checking fresh sources.
- Do not leak `.env` secrets.
- Do not delete or overwrite existing research files unless explicitly requested.
- Do not compare return figures across different口径 without explaining the difference.
