# Multi-Agent Financial Analysis System

This project implements an agentic AI financial news analysis system using the three required workflow patterns: **Prompt Chaining**, **Routing**, and **Evaluator-Optimizer**. It also has the four agent functions: **Planning**, **Tool Use**, **Self-Reflection**, and **Learning/Memory** across runs.

The notebook starts with a focused prompt chain:

```text
Ingest News -> Preprocess -> Classify -> Extract -> Summarize
```

## Project Scope

This project is an Investment Research Agent for stock symbols. It shows how financial news moves through a prompt chain, how extracted content is routed to specialist analyzers, and how generated analysis is evaluated and refined. A planner decides which steps to run for each symbol, the agent reflects on its own work, and it saves notes and lessons that the next run reads.

The notebook is made for Google Colab and uses Claude through the Anthropic API. It also runs without paid APIs. It includes bundled CSV datasets at `data/sample_financial_news.csv` and `data/sample_market_data.csv`, and each LLM step falls back to a rule-based version if no key is set. NewsAPI.org, Yahoo Finance RSS, and `yfinance` give live news and market data.

## Tools

- Python / Jupyter Notebook
- NewsAPI.org for live financial news ingestion (finance sites only)
- Yahoo Finance RSS for headlines about one stock symbol
- yfinance for stock price, one-month return, volume, market cap and P/E
- LangChain (langchain-anthropic) and LangGraph for the evaluator-optimizer loop
- Prompt chaining workflow: Ingest → Preprocess → Classify → Extract → Summarize
- Routing workflow: direct each article to an earnings, macro, market, or company-news specialist
- Evaluator-Optimizer workflow: score generated analysis and refine weak outputs
- LLM-agent classifier, extractor, router, analyst, evaluator, optimizer, and summarizer
- Deterministic fallbacks for offline reproducibility when no LLM API key is available
- Planning: the agent picks and orders its research steps for each symbol
- Self-reflection: the agent reviews its evaluation results and saves lessons
- Memory/Data storage: CSV input + JSON-style structured outputs, plus notes and lessons saved between runs
- Colab or local Jupyter for execution
- GitHub for submission/version control

## Notebook

- `financial_prompt_chaining_news_analysis.ipynb`

## Workflow Stages

1. **Ingest News**  
   Loads financial news from NewsAPI.org, Yahoo Finance RSS, the bundled CSV, or built-in sample data.

2. **Preprocess**  
   Cleans text, removes HTML/URLs, normalizes whitespace, and deduplicates articles.

3. **Classify**  
   Categorizes each article into financial topics such as earnings, macro, market movement, company news, or general financial news.

4. **Extract**  
   Pulls out investment-relevant signals including tickers, sentiment, catalysts, risks, and source metadata.

5. **Summarize**  
   Produces a concise financial news research brief from the extracted signals.

## Workflow Patterns Implemented

### 1. Prompt Chaining

The notebook implements:

```text
Ingest News → Preprocess → Classify → Extract → Summarize
```

The prompt-chaining section is built end to end with no missing stage. Its decision stages are designed to use LLM agents when an API key is available, with deterministic fallbacks so the notebook still runs for grading. It now includes:

- Bundled CSV news data for reproducible runs
- Optional live Yahoo Finance RSS ingestion for real-news testing without an API key
- NewsAPI.org ingestion limited to finance sites
- Stage-by-stage memory logging to `memory/prompt_chain_memory.jsonl`
- A final prompt-chain report table showing stage status, counts, classes, tickers, sentiment, and summary length
- Assertions that test the full chain against real Yahoo Finance RSS headlines
- LLM-agent prompts for classification, signal extraction, and summarization

### 2. Routing

After extraction, each article is routed to a specialist analyzer:

- Earnings analyzer
- Macro analyzer
- Market movement analyzer
- Company news analyzer

### 3. Evaluator-Optimizer

The notebook generates an initial analysis, evaluates it for completeness and grounding, and then refines the output with missing details such as tickers, catalysts, risks, and market context.

The evaluator-optimizer stage is also LLM-agent based when credentials are available. If no key is configured, the notebook uses transparent fallback scoring so the workflow remains executable. The loop is a LangGraph graph that stops when an analysis passes or after the round limit.

## Agent Functions

- **Planning:** a planner picks which tools to run for a symbol and in what order, and re-plans if something is missing (for example, no news found).
- **Tool Use:** the agent chooses between news sources, market data, the prompt chain, routing, the evaluator-optimizer, and memory.
- **Self-Reflection:** after the evaluator-optimizer runs, the agent looks at its own scores and feedback and writes lessons.
- **Learning/Memory:** notes and lessons are saved to the `memory/` folder and read back on the next run.

Section 10 of the notebook runs all of this together for one symbol.

## Memory

The agent writes to the `memory/` folder:

- `research_notes.jsonl`: one short note per stock, plus an open question
- `evaluator_lessons.jsonl`: lessons from self-reflection, with average scores for each run
- `prompt_chain_memory.jsonl`: a log of each prompt-chain stage

Colab deletes this folder when the runtime resets. To see the second run use what the first one saved, run the notebook again in the same session without restarting the runtime.

## LLM Configuration

The notebook uses Claude Haiku 4.5 (`claude-haiku-4-5-20251001`) through the Anthropic API. In Colab, add a secret named `ANTHROPIC_API_KEY` and turn on Notebook access. The first code cell reads it.

Running locally, you can set it as an environment variable instead:

```powershell
$env:ANTHROPIC_API_KEY = "your_key_here"
```

Without these keys, the notebook clearly marks outputs as `deterministic_fallback`.

## Optional Data Sources

The notebook runs without credentials using the included CSV files:

- `data/sample_financial_news.csv`
- `data/sample_market_data.csv`

It also supports optional external data sources if you want to replace the included sample data.

### yfinance

Install dependencies if you want live stock market data:

```powershell
pip install -r requirements.txt
```

If `yfinance` is unavailable or cannot reach live data, the notebook automatically uses the bundled market-data CSV.

### Yahoo Finance RSS

No key needed. Used for headlines about one stock symbol.

### NewsAPI.org

In Colab, add a secret named `NEWSAPI_KEY` and turn on Notebook access. The notebook then pulls live news from finance sites. Without it, the notebook uses Yahoo Finance RSS or the bundled CSV.

```powershell
$env:NEWSAPI_KEY = "*******"
```
