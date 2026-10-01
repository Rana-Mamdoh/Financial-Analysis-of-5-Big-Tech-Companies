# Financial Analysis of 5 Big Tech Companies

This project looks at annual financial data (2017–2026) for five big tech companies — Microsoft, Apple, Alphabet (Google), Meta, and Amazon. The data is pulled directly from the SEC EDGAR API (not a pre-made dataset), cleaned, checked for consistency, and then used to calculate profitability, debt and cash flow ratios.

Note: the most recent fiscal year for each company should be double-checked against its latest 10-K before being quoted anywhere else.

## Question I wanted to answer

**Is the AI infrastructure boom putting pressure on Big Tech's free cash flow?**

Short answer: yes, but not for everyone. Microsoft's CapEx went from 8% to 35% of revenue between 2017 and 2026, and its FCF margin dropped from 33% to 20% over the same period. Meta shows a similar pattern. Apple is the outlier — its CapEx barely changed, and its FCF margin stayed close to 25% the whole time. So the pressure seems to be concentrated in the companies that are actually building AI infrastructure, not across the board. More detail on this is in the Key Findings section.

## Files

```
├── Financial Data Collection.ipynb   
├── EDA_.ipynb                        
├── companies_annual.csv            
└── README.md
```

## How the data was collected

- Source: SEC EDGAR's company-facts API (XBRL data), 10-K filings only.
- Years covered: 2017 through 2025 or 2026, depending on each company's fiscal year end.
- Companies sometimes restate old numbers in later filings. To avoid mixing numbers from different filings, each year's Assets, Liabilities and Equity come from the same 10-K — the latest one that reports all three together.
- Debt = long-term debt (current + non-current) + short-term borrowings. Lease liabilities are not included. For a couple of companies (Google, for example) the current portion of debt only shows up in the following year's filing, so for debt specifically the latest available value is used even if it's from a different filing than the balance sheet.
- CapEx = cash spent on property and equipment, excluding finance leases.

The full validation checks (no duplicate years, no missing required fields, balance sheet has to balance) are in the comments inside `Financial Data Collection.ipynb`.

## Ratios calculated

| Metric | Formula | What it shows |
|---|---|---|
| Net Margin | Net Profit / Revenue | profit per dollar of revenue |
| ROE | Net Profit / Equity | return on shareholders' equity |
| ROA | Net Profit / Assets | return on total assets |
| Debt to Equity | Debt / Equity | how much debt vs. equity |
| Free Cash Flow (FCF) | Operating Cash Flow − CapEx | cash left after capital spending |
| CapEx Intensity | CapEx / Revenue | share of revenue spent on infrastructure |
| FCF Margin | FCF / Revenue | share of revenue left as free cash |

## Key findings

1. Microsoft and Meta's CapEx grew a lot as a share of revenue (Microsoft: 8% → 35% between 2017 and 2026).
2. FCF margin dropped as CapEx rose — Microsoft went from 33% to 20%. Part of this is just how the math works, since CapEx is subtracted when calculating FCF, but the size of the drop is what actually matters here, and it's biggest for Microsoft and Meta.
3. Apple's FCF margin barely moved (~25% the whole time), because its CapEx barely moved either (5% → 3% of revenue). It shows the same negative CapEx-FCF correlation as the other companies (-0.78), but since CapEx didn't really change, neither did FCF.
4. Apple's ROE looks extreme (up to 197%) but this isn't really comparable to the other companies — it's mostly a result of Apple's stock buybacks shrinking its equity base, not higher profitability. ROA is a fairer comparison.
5. Amazon had negative FCF in 2021 and 2022, driven by heavy investment during that period and a loss in 2022 connected to its investment in Rivian.
6. Debt levels are pretty different across companies — Apple carries relatively high debt (some of it funding buybacks), while Google and Meta have carried very little for most of the period.
7. The companies' fiscal years don't line up — Microsoft's ends in June, Apple's in September, and the rest in December — so a "2024" for one company isn't exactly the same 12 months as another's.

## Things to keep in mind

- This isn't meant to rank the companies — their business models are too different for that to mean much. The point is to find a shared trend and see who doesn't follow it (Apple, mainly).
- Debt excludes lease liabilities and CapEx is cash-only, so both likely understate real infrastructure spending for some companies.
- 2026 figures are the most recent and least double-checked — verify against the actual 10-K before relying on them.

## Running it yourself

1. Run `Financial Data Collection.ipynb` top to bottom. You'll need to put your own name and email in `SEC_USER_AGENT` — the SEC requires this on every request. This builds `companies_annual.csv`.
2. Run `EDA_.ipynb` top to bottom to reproduce the cleaning, ratios, charts and findings above.

## Some of charts

![CapEx Intensity over time](images/capex_intensity_trend.png)

![FCF Margin over time](images/fcf_margin_trend.png)

![CapEx vs FCF Margin](images/capex_vs_fcf_scatter.png)

![ROE over time](images/roe_apple_outlier.png)

## Tools

Python, pandas, matplotlib, seaborn, and the SEC EDGAR API.
