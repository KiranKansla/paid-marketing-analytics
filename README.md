# 📊 B2B Paid Marketing Intelligence Dashboard
### 5,000+ Records · Multi-Account · Multi-Market · Power BI · DAX

> **Tools:** Power BI · DAX · SQL · Excel · Multi-Currency Normalization
> **Domain:** B2B Paid Marketing Analytics
> **Context:** Independent consulting project — large B2B industrial organization
> **Scale:** 5,000+ performance records · 25+ markets · Multi-currency (EUR/GBP/SEK)
> **Year:** 2026

---

## 📸 Dashboard Preview

![Paid Marketing Performance Dashboard](Project_snapshort.png)

*Interactive Power BI dashboard — 8 visuals · 6 KPI cards · 6 DAX measures · Dynamic color coding*

---

## 🧭 Project Overview

A large B2B organization running paid marketing across multiple Google Ads accounts, markets, and business areas had accumulated 5,000+ performance records with no consolidated analytics framework. Each account was managed independently — leadership had no unified view of portfolio efficiency, spend distribution, or data quality.

**I was engaged to:**
- Consolidate 5,000+ decentralized multi-currency records into a unified analytics framework
- Clean, normalize, and model the data for reliable cross-account comparison
- Calculate and interpret paid marketing KPIs across the full portfolio
- Identify efficiency risks and opportunities hidden within the aggregate numbers
- Deliver a stakeholder-ready executive summary leadership could act on immediately

---

## 📊 Portfolio KPIs — From The Dashboard

| Metric | Value |
|---|---|
| Total Spend | 10.51M kr (SEK normalized) |
| Total Conversions | 185.37K |
| Click-Through Rate (CTR) | 1.58% |
| Cost Per Click (CPC) | 3.23 kr |
| Conversion Rate (CVR) | 5.69% |
| Cost Per Acquisition (CPA) | 56.68 kr (portfolio avg) |
| Impressions | 206.40M |
| Clicks | 3.26M |

---

## 🔑 Key Findings

### 🔴 Finding 1 — Critical: Google Ads Trucks Netherlands
The single most important finding in the dataset:

| Metric | Netherlands Account | Portfolio Average |
|---|---|---|
| Total Spend | 1.16M kr | — |
| Clicks | 87,812 | — |
| Conversions | 526 | — |
| CVR | 0.60% | 5.69% |
| CPA | 2,206.84 kr | 56.68 kr |
| Gap | — | **39× above average** |

**Diagnosis:** Traffic is arriving (87K clicks) but almost none is converting (526 conversions). This is a **post-click problem** — the ad creative is working, but the landing page experience or audience fit is broken. Redirecting budget here will compound losses, not fix them.

---

### 🟢 Finding 2 — Benchmark: Construction Equipment
The strongest performer in the portfolio:

| Metric | Value |
|---|---|
| CVR | 21.62% |
| CPA | 49.88 kr |
| Status | Portfolio benchmark |

This account's strategy should be studied and its budget scaled. Every krona moved here from Netherlands generates significantly more return.

---

### 📊 Finding 3 — Full Account Breakdown

| Account | Spend | Clicks | Conversions | CVR | CPA | Status |
|---|---|---|---|---|---|---|
| Google Ads Trucks NL | 1.16M kr | 87,812 | 526 | 0.60% | 2,206 kr | 🔴 Critical |
| Trucks Germany | 1.26M kr | 877,536 | 10,231 | 1.17% | 122.91 kr | 🔵 Average |
| Renault Trucks France | 1.25M kr | 358,470 | 14,078 | 3.93% | 89.08 kr | 🔵 Average |
| Construction Equipment | 2.21M kr | 204,767 | 44,262 | 21.62% | 49.88 kr | 🟢 Benchmark |

---

### 🌍 Finding 4 — Spend by Market (Top 5)

| Market | Approx Spend |
|---|---|
| France | ~2.0M kr |
| Germany | ~1.6M kr |
| Netherlands | ~1.2M kr |
| Finland | ~0.8M kr |
| Italy | ~0.8M kr |

3 markets drive the majority of spend — concentration risk if any market underperforms.

---

### 📈 Finding 5 — Campaign Type Efficiency

| Campaign Type | CVR Performance |
|---|---|
| Search | ✅ Highest CVR |
| Performance Max | High CVR |
| Video | Mid CVR |
| Display | Low CVR |
| Demand Gen | 🔴 Lowest CVR |

Budget allocation should reflect this efficiency ranking — not be distributed evenly.

---

### 🔽 Finding 6 — Conversion Funnel

```
Impressions   206.40M  ████████████████████████████████  100%
Clicks          3.26M  ████                               1.58% CTR
Conversions     0.19M  █                                  5.69% CVR overall
                                                          0.60% CVR — Netherlands only
```

Overall portfolio CVR of 5.69% is reasonable. But Netherlands at 0.60% confirms a post-click breakdown that is dragging down what could otherwise be a strong portfolio.

---

## 🛠️ Technical Approach

### Data Cleaning & Normalization
- Standardized multi-currency data (EUR, GBP, SEK) → all normalized to SEK
- Resolved inconsistent account and campaign naming conventions
- Documented all structural data quality issues before analysis

### DAX Measures Created

```dax
-- Core KPIs
CPA = DIVIDE(SUM('Data'[Cost In SEK]), SUM('Data'[Conversions]), 0)
CVR = DIVIDE(SUM('Data'[Conversions]), SUM('Data'[Clicks]), 0)
CTR = DIVIDE(SUM('Data'[Clicks]), SUM('Data'[Impr.]), 0)
CPC = DIVIDE(SUM('Data'[Cost In SEK]), SUM('Data'[Clicks]), 0)

-- Portfolio spend share
Spend % of Total =
DIVIDE(
    SUM('Data'[Cost In SEK]),
    CALCULATE(
        SUM('Data'[Cost In SEK]),
        ALLSELECTED('Data'[Country/Territory])
    ), 0
)

-- Data-driven color coding (no hardcoding)
CPA Color =
IF([CPA] > 500, "#E24B4A",
    IF([CPA] < 50, "#639922", "#378ADD"))

Market Color =
IF(
    SUM('Data'[Cost In SEK]) = MAXX(
        ALLSELECTED('Data'[Country/Territory]),
        CALCULATE(SUM('Data'[Cost In SEK]))
    ),
    "#E24B4A", "#378ADD"
)
```

### Color Logic
| Color | Hex | Threshold | Meaning |
|---|---|---|---|
| 🔴 Red | `#E24B4A` | CPA > 500 kr | Critical — immediate action |
| 🔵 Blue | `#378ADD` | CPA 50–500 kr | Average — monitor |
| 🟢 Green | `#639922` | CPA < 50 kr | Benchmark — scale |

### Dashboard Structure
```
┌─────────────────────────────────────────────────────────────┐
│  KPI Cards: Spend · Conversions · CTR · CPC · CVR · CPA    │
├──────────────────────────┬──────────────────────────────────┤
│  CPA by Account          │  Spend vs Conversions            │
│  Horizontal Bar · Color  │  Scatter (bubble size = CPA)     │
│  coded · Log scale       │                                  │
├────────────┬─────────────┴──────────┬───────────────────────┤
│ Spend by   │ CVR by Campaign Type   │ Conversion Funnel     │
│ Market     │ Search → Demand Gen    │ Impr→Clicks→Conv      │
│ Top 5      │ Color coded            │                       │
├────────────┴────────────────────────┴───────────────────────┤
│  Monthly Spend & CVR Trend — Combo Chart · Jan–Dec          │
├─────────────────────────────────────────────────────────────┤
│  🔴 Alert: NL Trucks 2,206 kr CPA · 0.60% CVR · Audit now │
└─────────────────────────────────────────────────────────────┘
```

---

## ⚠️ Data Limitations

| Limitation | Impact | Mitigation |
|---|---|---|
| Fixed proxy FX rates | Spend figures directionally accurate, not exact | Noted in all reporting |
| No revenue/conversion value data | ROI/ROAS not calculable | Focused on efficiency metrics |
| Conversion definition variance | Cross-account CVR comparison requires caution | Flagged in stakeholder summary |
| No audience/segmentation data | Targeting hypothesis unverifiable from data alone | Flagged as recommended investigation |

---

## 📋 Recommendations

### Immediate
1. **Audit Netherlands account** — 0.60% CVR with 87K clicks = post-click breakdown
2. **Reduce NL budget by 30%** → redirect to Construction Equipment
3. **Review landing page + audience targeting** for Trucks Netherlands

### Strategic
4. **Build live automated dashboard** with daily FX normalization
5. **Standardize naming conventions** across all accounts
6. **Align conversion definitions** for reliable cross-account comparison

### Additional Data Required
- Conversion value / revenue → ROAS calculation
- Customer segmentation → targeting validation
- Budget targets per account → pacing analysis

---

## 📁 Repository Files

| File | Description |
|---|---|
| `Kirankansla_dashboard.pbix` | Interactive Power BI dashboard |
| `Project_snapshort.png` | Dashboard screenshot |
| `Insights.pdf` | Business insights report |
| `Stakeholder_Summary.pdf` | 1-page executive summary |
| `README.md` | This file |

---

## 👩‍💼 About

**Kiranjot Kaur Kansla**
Data Analyst · 9+ years across Banking, Retail & Logistics
SQL · Power BI · DAX · Python · Snowflake · Advanced Excel

🔗 [LinkedIn](https://linkedin.com/in/kirankansla) · 📧 kirankansla94@gmail.com
