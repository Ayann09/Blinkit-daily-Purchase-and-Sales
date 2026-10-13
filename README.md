# 🛒 Blinkit Grocery: Daily Purchase Behavior at Scale

### Tracking Best-Sellers, Sales Trends, Ratings, Regional Demand & Inventory

**A Descriptive Analytics Report on Quick-Commerce Grocery Data**

![Type](https://img.shields.io/badge/Type-Descriptive%20Analytics-1f3a6b)
![Data](https://img.shields.io/badge/Data-Synthetic-orange)
![Window](https://img.shields.io/badge/Window-30%20Days-4a90d9)
![SKUs](https://img.shields.io/badge/SKUs-60-4a8c4a)
![Regions](https://img.shields.io/badge/Regions-8%20Metros-b03a3a)
![Status](https://img.shields.io/badge/Purpose-Academic%20%2F%20Demo-lightgrey)

> **⚠️ Disclaimer:** All data in this project is **synthetic**. It was generated to mirror realistic quick-commerce grocery patterns and demonstrate the structure, charts, and metrics a real analytics pipeline would produce. **None of the figures represent actual Blinkit sales data.**

---

## 📑 Table of Contents

1. [Overview](#-overview)
2. [Business Questions](#-business-questions)
3. [Headline Numbers](#-headline-numbers)
4. [Key Findings](#-key-findings)
5. [Detailed Analysis](#-detailed-analysis)
6. [Methodology & Assumptions](#-methodology--assumptions)
7. [KPI Definitions](#-kpi-definitions)
8. [How This Would Work at Real Scale](#-how-this-would-work-at-real-scale)
9. [Data Schema](#-data-schema)
10. [How Each Insight Was Derived](#-how-each-insight-was-derived)
11. [Limitations & Caveats](#-limitations--caveats)
12. [Roadmap: Descriptive → Predictive](#-roadmap-descriptive--predictive)
13. [Suggested Repository Structure](#-suggested-repository-structure)
14. [Glossary](#-glossary)
15. [Author](#-author)

---

## 🔎 Overview

This project builds a **descriptive analytics layer** over daily Blinkit-style grocery purchase transactions. It covers:

| Dimension | Scope |
|---|---|
| **Catalog** | 60 SKUs across 10 grocery categories |
| **Geography** | 8 Indian metro regions |
| **Time window** | 30 days |
| **Volume** | ~31.69 lakh units (~3.17M) |
| **Revenue** | ~₹48.2 Cr |

A single descriptive layer, refreshed **daily** (or **hourly** for operations), is designed to serve merchandising, finance, customer experience, operations, and dark-store supply chain teams from **one consistent source of truth**, rather than separate ad-hoc reports.

---

## ❓ Business Questions

| # | Question | Stakeholder |
|---|---|---|
| 1 | What are today's best-selling grocery items? | Merchandising |
| 2 | How is total order volume trending day over day? | Finance |
| 3 | How are products rated, and where is rating quality slipping? | Customer Experience |
| 4 | Where is demand concentrated geographically across India? | Operations |
| 5 | Which SKUs are at risk of stocking out at dark stores? | Dark-Store Supply Chain |

---

## 📊 Headline Numbers

| Metric | Value |
|---|---|
| Total units sold (30 days) | 31,69,000 (~3.17M) |
| Total revenue (30 days) | ₹48.2 Cr |
| Average Selling Price (ASP) | ₹152 / unit |
| Average daily units sold | ~1,05,633 units/day |
| Average product rating | 4.13 / 5 |
| Top single-day seller | 6,412 units (Amul Toned Milk 500ml) |
| Hero SKU share of daily volume | ~6.1% |
| Top 2 metros' share of total demand | ~41.0% |
| Metro regions tracked | 8 |
| Grocery categories tracked | 10 |
| Total catalog size | 60 SKUs |
| Peak single-day revenue | ~₹1.98 Cr |
| Lowest single-day revenue | ~₹1.27 Cr |
| Core rating range | 3.5 – 4.9 / 5 |
| In-Stock SKUs | 51 of 60 (~85.0%) |
| SKUs at Low Stock or below | 9 of 60 (~15.0%) |
| Long-tail (#2–#10) combined volume | 26,013 units |

---

## 💡 Key Findings

1. **Hero SKU dependency.** Amul Toned Milk 500ml sold 6,412 units in a day, nearly **46% ahead** of the runner-up (Banana Robusta, 6pc). A small set of daily essentials (milk, bread, staples) drives a disproportionate share of volume, a Pareto pattern worth prioritizing for dark-store inventory.
2. **Weekly demand cycle.** Sales follow a **mid-week dip / weekend peak** rhythm layered on gradual growth. Staffing, delivery-partner capacity, and replenishment should be planned around this cycle rather than a flat daily average.
3. **Regional concentration.** **Delhi NCR (~697K) and Mumbai (~602K)** together account for ~41% of demand. Dark-store density in these two metros is the binding operational constraint.
4. **Healthy ratings, visible tail.** The 4.13/5 average hides that ~8% of product-days fall below 3.5, which matters for freshness-sensitive categories (dairy, produce, bakery).
5. **Inventory mostly healthy, but at-risk SKUs matter disproportionately.** 9 of 60 SKUs are Low Stock or Out of Stock; if any are hero SKUs, the revenue and trust impact is far larger than the count suggests.
6. **Revenue tracks volume closely.** Units and revenue move almost in lockstep, with a stable ASP of ~₹152, indicating stable pricing and category mix.
7. **A diversified long tail.** SKUs ranked #2–#10 combined (26,013 units) sell **over 4×** the #1 SKU (6,412 units), so the catalog isn't overly reliant on a single hero product.

### Additional Insights

- **Category diversity among best-sellers:** the top 10 span dairy, produce, bakery, staples, snacks, and personal care, so demand is broad-based.
- **Long-tail metros still matter:** Kolkata (~222K) and Ahmedabad (~158K) each move far more volume in 30 days than the top SKU sells in a day.
- **Rating stability under scale:** the 4.13 average held across ~31.7 lakh units, a sign QC is keeping pace with growth.
- **Tiered reorder cadence:** near-real-time checks for hero and at-risk SKUs; weekly for the stable majority.
- **Growth beneath seasonality:** a gradual upward drift independent of the weekly cycle, worth isolating with trend decomposition.
- **Revenue concentration likely mirrors volume concentration** (inference, not measured).
- **Metro-wide cadence alignment:** a shared weekly rhythm likely holds platform-wide (hypothesis to validate).

---

## 📈 Detailed Analysis

### 1. Daily Sales Trend

- ~31.69 lakh units and ~₹48.2 Cr revenue over 30 days.
- A clear **~7-day cycle**, with each weekly peak trending mildly upward.
- **Cumulative revenue** climbs steadily to ~₹48.2 Cr by day 30, indicating broad-based growth rather than one or two outlier days.
- **Operational implication:** the mid-week trough is a natural window for restocking before the weekend surge.

### 2. Best-Selling Products (Most Recent Day)

Ranking is by **units sold**, not revenue. A ₹25 item outselling a ₹400 item matters differently to warehousing than to finance, so both cuts should exist as separate dashboard views.

| Rank | Product | Units |
|---:|---|---:|
| 1 | Amul Toned Milk 500ml | 6,412 |
| 2 | Banana Robusta 6pc | 4,380 |
| 3 | Britannia Bread 400g | 3,865 |
| 4 | Tata Salt 1kg | 3,210 |
| 5 | Lays Classic Salted 52g | 2,940 |
| 6 | Onion 1kg | 2,705 |
| 7 | Maggi Noodles 4-pack | 2,588 |
| 8 | Amul Butter 100g | 2,340 |
| 9 | Surf Excel 1kg | 2,110 |
| 10 | Colgate Strong Teeth 150g | 1,875 |

The drop-off from #1 to #10 is steady rather than a cliff, suggesting healthy demand across the catalog.

### 3. Category-Wise Sales Breakdown

| Category | Units (30 days) |
|---|---:|
| Dairy & Bakery | 636K |
| Fruits & Vegetables | 537K |
| Staples & Grains | 456K |
| Snacks & Beverages | 396K |
| Household Essentials | 340K |
| Personal Care | 277K |
| Instant & Frozen Foods | 225K |
| Beauty & Wellness | 153K |
| Baby Care | 84K |
| Pet Care | 65K |

Dairy & Bakery and Fruits & Vegetables lead as the highest-frequency categories; Baby Care and Pet Care form a long tail of lower-frequency, higher-loyalty purchases. This view informs **category-level assortment and shelf-space** decisions, as distinct from SKU-level inventory decisions.

### 4. Rating Distribution

Average rating across **1,800 product-days** (60 SKUs × 30 days): **4.13 / 5**.

| Rating Bucket | Product-Days |
|---|---:|
| 1.0 – 1.9 | 0 |
| 2.0 – 2.9 | 11 |
| 3.0 – 3.4 | 133 |
| 3.5 – 3.9 | 532 |
| 4.0 – 4.4 | 757 |
| 4.5 – 5.0 | 367 |

**What to monitor:**
- Products trending downward in **rolling 7-day average rating**, the earliest signal of a quality or fulfillment issue.
- Products with **high volume but sub-4.0 rating**, the highest-risk combination, since volume amplifies the damage of poor reviews.

A tight distribution is good news but leaves little room to absorb a real quality problem; a cluster of 1–2 star ratings on even one high-volume, freshness-sensitive SKU would pull the aggregate down quickly. Segment-level (not just aggregate) tracking matters more as volume grows.

A **Rating vs. Sales Volume** scatter (all 60 SKUs) with a trend line shows a mildly downward slope, so high-volume SKUs are not, on the whole, rated meaningfully worse than low-volume ones.

### 5. Regional Demand

| Region | Units (30 days) | Share |
|---|---:|---:|
| Delhi NCR | 697K | 22.0% |
| Mumbai | 602K | 19.0% |
| Bangalore | 539K | 17.0% |
| Hyderabad | 349K | 11.0% |
| Pune | 317K | 10.0% |
| Chennai | 285K | 9.0% |
| Kolkata | 222K | 7.0% |
| Ahmedabad | 158K | 5.0% |

This view drives dark-store allocation by micro-market, localized pricing and promotions, and delivery SLA risk planning in regions with limited local stock. Real-world demand typically concentrates even more heavily in 2–3 metro cores than this balanced simulation shows.

### 6. Inventory Status

| Tier | Meaning | SKUs | Action |
|---|---|---:|---|
| 🟢 In Stock | Comfortable buffer at current velocity | 51 | None |
| 🟡 Low Stock (< 200 units) | Reorder-trigger zone; replenish within 24–48 hrs | 7 | Flagged |
| 🔴 Out of Stock | Lost-sale risk | 2 | Cross-reference against best-seller list |

> **Recommended upgrade:** cross-tab inventory against sales velocity into a **"days of supply remaining"** metric per SKU, instead of relying on a static stock count.

---

## 🧪 Methodology & Assumptions

Because live transaction data isn't accessible, a **synthetic dataset** was generated using the following assumptions:

- **60 SKUs** across **10 categories**, sized to resemble a realistic dark-store catalog (not the full platform).
- **8 metro regions**, each with a baseline demand share plus random daily noise; regional totals sum exactly to the 30-day unit total **by construction**.
- **30-day window** with weekly seasonality (mid-week dips, weekend peaks) on top of gradual growth.
- **Ratings** drawn from a distribution centered at **4.13/5** across six buckets (not uniform).
- **Inventory tiers** assigned so most SKUs hold a healthy buffer, with a small minority at Low Stock or Out of Stock.

All figures illustrate pipeline output and chart structure, **not actual Blinkit sales**.

---

## 📐 KPI Definitions

| KPI | Definition |
|---|---|
| **Daily units sold** | Sum of `quantity` across all completed orders that day |
| **Daily revenue** | Sum of `unit_price × quantity`, net of returns/cancellations |
| **ASP** | Total revenue ÷ total units (₹48.2 Cr ÷ 31,69,000 ≈ ₹152) |
| **Days of supply** | Stock on hand ÷ average daily sales rate |
| **Hero SKU share** | Top SKU's single-day units ÷ daily total volume |

---

## 🏗️ How This Would Work at Real Scale

| Layer | Purpose |
|---|---|
| **Stream processing** | Roll up raw events into per-SKU, per-dark-store, per-minute aggregates |
| **Cloud storage** | Hold raw events (for audits) and pre-aggregated tables (for speed) |
| **Batch aggregation** | Nightly rollups: daily sales, rating averages, inventory reconciliation |
| **Investigation** | Ad-hoc root-cause analysis on anomalies (rating drops, demand spikes, stockouts) |
| **Serving / BI layer** | Dashboards refreshed hourly for ops, daily for leadership |

The charts reflect the output of **daily aggregated tables**, not raw event-level data, since no dashboard can usefully render millions of individual rows.

---

## 🗄️ Data Schema

| Group | Fields |
|---|---|
| Identifiers | `order_id`, `sku_id`, `category`, `dark_store_id`, `region` |
| Transaction facts | `unit_price`, `quantity`, `order_timestamp` |
| Quality signal (nullable) | `rating`, `rating_timestamp` |
| Inventory state | `stock_on_hand`, `reorder_threshold` |

---

## 🧮 How Each Insight Was Derived

| Insight | Calculation |
|---|---|
| Hero SKU share | 6,412 ÷ 1,05,633 ≈ **6.1%** |
| Lead over #2 SKU | (6,412 − 4,380) ÷ 4,380 ≈ **46.4%** |
| Top-2 metro share | (697K + 602K) ÷ 3,169K ≈ **41.0%** |
| Low-rating tail | (0 + 11 + 133) ÷ 1,800 ≈ **8.0%** below 3.5 |
| At-risk inventory | 9 ÷ 60 = **15.0%** |
| ASP | ₹48.2 Cr ÷ 31,69,000 ≈ **₹152** |
| Long-tail volume | 4,380 + 3,865 + 3,210 + 2,940 + 2,705 + 2,588 + 2,340 + 2,110 + 1,875 = **26,013** (>4× the #1 SKU) |

**Qualitative reads:** the weekly cycle, underlying growth trend, and revenue–volume correlation come from visual inspection of the daily trend chart.
**Inferences/hypotheses (not measured):** revenue concentration mirroring volume, metro-wide weekly cadence, and the tiered reorder cadence recommendation.

---

## ⚠️ Limitations & Caveats

- **Synthetic data only.** Not suitable for drawing real-world business conclusions.
- **No SKU-level inventory names.** The report doesn't identify *which* SKUs are Low/Out of Stock, so the hero-SKU cross-check is a recommended practice, not a verified result.
- **No per-SKU revenue breakdown**, so revenue concentration is inferred from unit concentration.
- **No category-by-day or metro-by-day data**, so the "shared weekly rhythm" is a hypothesis.
- **Hero share denominator.** The ~6.1% figure divides the top SKU's single-day units by the *average* daily total (~1,05,633), not by that specific day's total.
- **Chart consistency.** In the rating-vs-volume scatter, several SKUs appear at higher single-day volumes (~4,000–5,500) than the #2 SKU in the top-10 chart (4,380), and the scatter's ratings sit mostly below the stated 4.13 mean.
- **Illustrative simulation.** Regional demand is more balanced than real quick-commerce, which concentrates more heavily in 2–3 metro cores.

---

## 🚀 Roadmap: Descriptive → Predictive

- [ ] **Demand forecasting** per SKU / dark store to set automatic reorder points and prevent hero-SKU stockouts
- [ ] **Anomaly detection** for rating drops and sudden demand spikes (e.g., monsoon-driven surges) in near real time
- [ ] **Cohort & retention analysis** of repeat purchase behavior and basket composition
- [ ] **Trend decomposition** to separate underlying growth from weekly seasonality
- [ ] **Days-of-supply metric** per SKU, cross-tabbed with sales velocity
- [ ] **Swap in a real dataset** (CSV/Excel export or database connection); the same pipeline and chart structure applies directly

---

## 📁 Suggested Repository Structure

```
blinkit-purchase-behavior-analytics/
├── README.md
├── report/
│   └── Blinkit_Daily_Purchase_Behavior_At_Scale.pdf
├── images/
│   ├── daily_sales_trend.png
│   ├── cumulative_revenue.png
│   ├── top10_products.png
│   ├── category_breakdown.png
│   ├── rating_distribution.png
│   ├── rating_vs_volume.png
│   ├── regional_demand.png
│   └── inventory_status.png
├── data/
└── LICENSE
```

To display charts, add them with:

```markdown
![Daily Sales Trend](images/daily_sales_trend.png)
```

---

## 📖 Glossary

| Term | Meaning |
|---|---|
| **ASP** | Average Selling Price: total revenue ÷ total units sold over a period |
| **Dark store** | A small, fulfillment-only warehouse for quick-commerce delivery; not open to walk-in customers |
| **Hero SKU** | A single product responsible for a disproportionately large share of daily volume |
| **Long tail** | Lower-ranked SKUs or metros that individually sell less but collectively represent significant volume |
| **Days of supply** | Current stock on hand ÷ average daily sales rate |
| **SKU** | Stock Keeping Unit: a distinct sellable product |
| **SLA** | Service Level Agreement: here, delivery-time commitments |

---

## 👤 Author

**Mohd Ayan**
Roll No: 29
August 2026

*Prepared for academic / demonstration purposes only. Built on a synthetic dataset mirroring real quick-commerce patterns.*

---

## 📄 License

Add your preferred license here (e.g., MIT) and include a `LICENSE` file in the repository.
