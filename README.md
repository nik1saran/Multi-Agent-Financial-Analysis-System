# Multi-Agent Financial Analysis System

This project implements an agentic AI financial news analysis system using the three required workflow patterns: **Prompt Chaining**, **Routing**, and **Evaluator-Optimizer**.

The notebook starts with a focused prompt chain:

```text
Ingest News -> Preprocess -> Classify -> Extract -> Summarize
```

## Project Scope

This version covers the workflow-pattern portion of the larger agentic AI project. It shows how financial news moves through a prompt chain, how extracted content is routed to specialist analyzers, and how generated analysis is evaluated and refined.

The project is complete and runnable without paid APIs. It includes bundled CSV datasets at `data/sample_financial_news.csv` and `data/sample_market_data.csv`. NewsAPI.org, Kaggle CSV imports, and `yfinance` are optional ways to replace the bundled data with live or larger datasets.

## Tools

- Python / Jupyter Notebook
- NewsAPI.org for live financial news ingestion
- Kaggle Financial News Dataset via CSV import
- yfinance for stock price and market context
- Prompt chaining workflow: Ingest → Preprocess → Classify → Extract → Summarize
- Routing workflow: direct each article to an earnings, macro, market, or company-news specialist
- Evaluator-Optimizer workflow: score generated analysis and refine weak outputs
- Rule-based classifier and extractor for offline reproducibility
- Memory/Data storage: CSV input + JSON-style structured outputs
- Colab or local Jupyter for execution
- GitHub for submission/version control

## Notebook

- `financial_prompt_chaining_news_analysis.ipynb`

## Workflow Stages

1. **Ingest News**  
   Loads financial news from a Kaggle-style CSV, NewsAPI.org, or built-in sample data.

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

The prompt-chaining section is built end to end with no missing stage. It now includes:

- Bundled CSV news data for reproducible runs
- Optional live Yahoo Finance RSS ingestion for real-news testing without an API key
- Optional NewsAPI.org and Kaggle CSV ingestion
- Stage-by-stage memory logging to `memory/prompt_chain_memory.jsonl`
- A final prompt-chain report table showing stage status, counts, classes, tickers, sentiment, and summary length
- Assertions that test the full chain against real Yahoo Finance RSS headlines

### 2. Routing

After extraction, each article is routed to a specialist analyzer:

- Earnings analyzer
- Macro analyzer
- Market movement analyzer
- Company news analyzer

### 3. Evaluator-Optimizer

The notebook generates an initial analysis, evaluates it for completeness and grounding, and then refines the output with missing details such as tickers, catalysts, risks, and market context.

## Optional Data Sources

The notebook runs without credentials using the included CSV file:

- `data/sample_financial_news.csv`
- `data/sample_market_data.csv`

It also supports optional external data sources if you want to replace the included sample data.

### yfinance

Install dependencies if you want live stock market data:

```powershell
pip install -r requirements.txt
```

If `yfinance` is unavailable or cannot reach live data, the notebook automatically uses the bundled market-data CSV.

### Kaggle CSV

Set an environment variable pointing to your downloaded financial news CSV:

```powershell
$env:KAGGLE_FINANCIAL_NEWS_CSV = "C:\path\to\financial_news.csv"
```

### NewsAPI.org

NewsAPI is optional. Only set this environment variable,  and can be used to have the notebook to pull live news. 
NOTE: Do not commit real API keys to GitHub.

```powershell
$env:NEWSAPI_KEY = "*******"
```

## ipynb file

The final output is a structured financial news brief summarizing article classes, sentiment, tickers, catalysts, risks, routed specialists, market context, and evaluator-optimizer refinements.
