# Multi-Agent Financial Analysis System

This project implements the **Prompt Chaining** workflow for an agentic AI financial news analysis system.

The notebook demonstrates a focused chain:

```text
Ingest News -> Preprocess -> Classify -> Extract -> Summarize
```

## Project Scope

This version focuses only on the prompt-chaining requirement from the larger agentic AI project. It shows how financial news can move through multiple specialized processing stages, where each stage transforms the output for the next stage.

The project is complete and runnable without paid APIs. It includes a bundled sample CSV dataset at `data/sample_financial_news.csv`. NewsAPI.org and Kaggle CSV imports are optional ways to replace the bundled data with live or larger datasets.

## Tools

- Python / Jupyter Notebook
- NewsAPI.org for live financial news ingestion
- Kaggle Financial News Dataset via CSV import
- Prompt chaining workflow: Ingest → Preprocess → Classify → Extract → Summarize
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

## Optional Data Sources

The notebook runs without credentials using the included CSV file:

- `data/sample_financial_news.csv`

It also supports optional external data sources if you want to replace the included sample data.

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

The final output is a structured financial news brief summarizing article classes, sentiment, tickers, catalysts, and risks.
