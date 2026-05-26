# Wealth Platform

Personal wealth management platform with AI-powered analysis.

**Built from:**
- [xmayukx/dimex](https://github.com/xmayukx/dimex) — UI shell (Next.js + Prisma + Zustand + shadcn/ui)
- [Caesarcph/financial-llm-prompts](https://github.com/Caesarcph/financial-llm-prompts) — Financial AI prompts
- [kennethleungty/Finance-LLMs](https://github.com/kennethleungty/Finance-LLMs) — Enterprise architecture reference

---

## Features

| Section | What it does |
|---|---|
| **Dashboard** | Net worth, portfolio allocation pie, spending bar chart, top holdings |
| **Portfolio** | Track stocks, ETFs, crypto, bonds — P&L per holding |
| **Transactions** | Income/expense log, monthly cash flow summary |
| **Budget** | Category limits with progress bars, over-budget alerts |
| **AI Advisor** | Risk assessment, market analysis, trade planner, daily briefing |

---

## Setup

### 1. Install Node.js
Download from https://nodejs.org (LTS version recommended).

### 2. Install dependencies
```bash
cd "C:\Users\Candy Fan's laptop\wealth-platform"
npm install
```

### 3. Configure environment
Copy `.env.example` to `.env` and fill in your API key:

```env
# Use "mock" for demo (no API key needed)
LLM_PROVIDER="mock"

# For real AI (pick one):
LLM_PROVIDER="anthropic"
ANTHROPIC_API_KEY="sk-ant-..."

# or:
LLM_PROVIDER="openai"
OPENAI_API_KEY="sk-..."
```

### 4. Set up the database
```bash
npm run db:generate   # generates Prisma client
npm run db:push       # creates SQLite database
npm run db:seed       # loads sample data
```

### 5. Start the app
```bash
npm run dev
```

Open http://localhost:3000

---

## LLM Providers

The platform is **provider-agnostic** — swap without changing any app code:

| `LLM_PROVIDER` | Key needed | Notes |
|---|---|---|
| `mock` | None | Instant fake responses, great for UI dev |
| `anthropic` | `ANTHROPIC_API_KEY` | Uses `claude-sonnet-4-6` by default |
| `openai` | `OPENAI_API_KEY` | Uses `gpt-4o` by default |

---

## AI Analysis Modules

All prompts sourced from `Caesarcph/financial-llm-prompts`:

| Module | Endpoint | Prompt file |
|---|---|---|
| Portfolio Risk | `POST /api/ai/risk-assessment` | `prompts/risk_assessment/portfolio_risk.md` |
| Market Analysis | `POST /api/ai/market-analysis` | `prompts/market_analysis/trend_analysis.md` |
| Trade Planner | `POST /api/ai/trade-plan` | `prompts/trade_planning/entry_exit_plan.md` |
| Daily Briefing | `POST /api/ai/daily-brief` | `prompts/report_generation/daily_brief.md` |

---

## Project Structure

```
src/
  app/
    (dashboard)/          # Main pages (dashboard, portfolio, transactions, budget, ai-advisor)
    api/                  # REST endpoints
      holdings/           # CRUD portfolio holdings
      transactions/       # CRUD income/expenses
      budgets/            # Budget management
      ai/                 # AI analysis endpoints
  lib/
    llm/                  # Provider-agnostic LLM adapter
      provider.ts         # Router: picks anthropic | openai | mock
      anthropic.ts        # Anthropic SDK wrapper
      openai.ts           # OpenAI SDK wrapper
      mock.ts             # Offline demo mode
    prompts/              # Financial prompt templates (from financial-llm-prompts)
  store/                  # Zustand state
  types/                  # TypeScript interfaces
prisma/
  schema.prisma           # SQLite schema (Transaction, Holding, Budget, AIInsight)
  seed.ts                 # Sample data
```
