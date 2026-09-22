# Financial Statements Analytics Dashboard (Power BI)

An interactive Power BI dashboard that benchmarks a sector's companies on one screen, built from live SEC filings. Pick a sector and a company and see who grows fastest, who turns revenue into profit and cash most efficiently, who carries more leverage, and how each has moved over five years.

Every figure ties back to the 10-K, so the view is decision-grade, not directional.

## What's in this repo

| File | What it is |
|------|------------|
| `Financial_Statements_Dashboard.pbix` | The Power BI file. Open in Power BI Desktop. |
| `Dashboard_Details.pdf` | Full page-by-page export of the report. View without Power BI. |
| `README.md` | This file. |

## Coverage

Payments sector: Visa, Mastercard, PayPal, fiscal years 2021 to 2025. The model scales to more companies and sectors without redesign.

## Data source

SEC EDGAR XBRL API (company facts), pulled by company CIK. No manual data entry. The dashboard reads each company's filings straight from the API, maps the XBRL tags to statement lines, and loads a star schema.

- Visa: CIK 0001403161
- Mastercard: CIK 0001141391
- PayPal: CIK 0001633917

## The six pages

1. **Executive Summary** — headline KPIs, five-year revenue and margin trends, peer comparison table.
2. **Income Statement** — revenue, operating income and net income by company, margin trend, reconciling P&L matrix.
3. **Balance Sheet** — asset, liability and equity structure, current-ratio trend, a balance sheet that ties out each year.
4. **Cash Flow** — operating and free cash flow, capex, free-cash-flow conversion by company.
5. **Ratio and Peer Comparison** — growth, margins, returns, liquidity and leverage side by side, strongest company highlighted.
6. **Company Deep-Dive** — one company's full three-statement history, driven by a single-select company filter.

## Architecture

- **Model:** star schema. A long `Fact_Financials` table with `Dim_Company` (company, ticker, sector) and `Dim_Date`.
- **Ingestion:** Power Query (M) calls the SEC company-facts endpoint per company. Adding a company is one CIK. Adding a sector is one row in `Dim_Company`.
- **Measures:** DAX resolves XBRL tag differences across companies (the same statement line is tagged differently by different filers), so totals stay correct as the company set grows.

## Data accuracy

- Sourced from SEC filings through the company-facts API, so the figures are the filed numbers by construction.
- Every balance sheet balances (assets = liabilities + equity), and cash flow ties (free cash flow = operating cash flow minus capex).
- FY2023 and FY2024 figures for all three companies cross-checked against each company's 10-K.

## How to open

1. Install Power BI Desktop (free).
2. Open `Financial_Statements_Dashboard.pbix`.
3. Refresh to pull the latest filings from the SEC API. No credentials required; the query sets a User-Agent header as the SEC requests.

If you do not have Power BI Desktop, `Dashboard_Details.pdf` shows every page.

## Tools

Power BI Desktop, Power Query (M), DAX, SEC EDGAR XBRL API.

---

Built entirely from public SEC filings (EDGAR). All figures are reported GAAP; no confidential data is used. Portfolio demonstration.

**Ankit Kale** · contact@ankitkale.comm
