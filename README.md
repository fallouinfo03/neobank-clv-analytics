# 📊 NeoBank CLV Analytics — Customer Lifetime Value Dashboard

> **Power BI · DAX · Data Modeling · Banking Analytics**

A complete business intelligence solution analyzing **Customer Lifetime Value (CLV)** for a fictional retail bank. Built to answer a core business question: *Which customers generate the most value — and how do we keep them?*

---

## 🎯 Business Problem

Retail banks allocate marketing and commercial resources without always knowing which clients are truly profitable. This project builds an end-to-end CLV analytics framework to:

- **Identify** the most valuable customer segments
- **Prioritize** cross-sell and up-sell opportunities
- **Forecast** revenue trends over the next 6 months
- **Understand** product co-purchase patterns (market basket analysis)

---

## 📸 Dashboard Preview

### Page 1 — RFM Segmentation
*Customers scored on Recency, Frequency, and Monetary dimensions and grouped into actionable segments (Champions, Standard, At-Risk, New Clients, Lost).*

> <img width="1372" height="731" alt="image" src="https://github.com/user-attachments/assets/70a0dfdd-696c-4a72-b5c9-eb653394e59b" />


**Key metrics:** Average basket size · Annual purchase frequency · Average CLV

---

### Page 2 — 20/80 Rule (Pareto Analysis)
*Which product sub-categories drive 80% of revenue? Visual Pareto curve with cumulative CA breakdown.*

> <img width="1349" height="749" alt="image" src="https://github.com/user-attachments/assets/2ee1c4bf-4b43-4a69-9236-f091f1235c45" />



**Insight:** Top 4 sub-categories (BC, BB, BA, CA) account for ~46% of total revenue (23.9M).

---

### Page 3 — CLV Per Client
*Detailed client-level table: basket size, purchase frequency, estimated lifetime, and CLV score — filterable by segment.*

> <img width="1291" height="743" alt="image" src="https://github.com/user-attachments/assets/d685a502-3311-4200-9921-76a6942eae76" />


**Global KPIs:** Panier Moyen: 2,510 · Fréquence Annuelle: 8.23 · CLV Globale: 120,694

---

### Page 4 — Basket Analysis (Product Correlation Matrix)
*Heatmap showing how often pairs of products appear in the same transaction — enabling targeted cross-sell recommendations.*

> <img width="1359" height="775" alt="image" src="https://github.com/user-attachments/assets/bd285daa-228c-49d1-8046-92bac068aa41" />


---

### Page 5 — Sales Forecast (6 months)
*Time series forecast using Power BI's built-in analytics — monthly transaction volume from 2020 to mid-2026 with confidence interval.*

> <img width="1324" height="726" alt="image" src="https://github.com/user-attachments/assets/17868e93-2a9b-4884-a985-21d822fa717f" />


---

### Page 6 — Under-Exploited Clients (Opportunities)
*Clients with high CLV potential but low product penetration — ranked by opportunity score with recommended next product.*

> <img width="1380" height="850" alt="image" src="https://github.com/user-attachments/assets/205b9f9c-6be0-419c-a496-f3972ffe16d0" />


---

##  Data Model

Star schema with 8 tables:

```
FactTransaction (central fact table)
    ├── DimAccount       — Account type, balance, open/close dates
    ├── DimCustomer      — Demographics: DOB, Gender, Region, Join Date
    ├── DimProduit       — Product catalog
    ├── DimProductSubCategory
    ├── DimProductCategory
    ├── Table_RFM        — Calculated RFM scores + CLV per customer
    └── Produits_Comparaison — Pre-computed product co-purchase matrix
```

>  <img width="1448" height="840" alt="image" src="https://github.com/user-attachments/assets/9c4537f1-59f8-401f-a505-d7d4dabc128f" />


---

##  Key DAX Measures

```dax
-- Customer Lifetime Value
CLV_Globale =
    [Panier Moyen_fait] * [Fréquence Achat Annuelle] * [Durée de Vie Moyenne (Années)]

-- RFM Recency Score (1-5)
R_Score =
    SWITCH(TRUE(),
        [Recence] <= 30,  5,
        [Recence] <= 60,  4,
        [Recence] <= 90,  3,
        [Recence] <= 180, 2,
        1
    )

-- Pareto Cumulative Revenue %
% CA Cumulé_fait =
    DIVIDE([CA Cumulé_fait], CALCULATE([Total Ventes_fait], ALL(DimProductSubCategory)))

-- Product Co-purchase Matrix
Matrice de corrélation de produits =
    CALCULATE(
        COUNTROWS(FactTransaction),
        FILTER(FactTransaction, FactTransaction[ProductID] = MIN(Produits_Comparaison[ProductID]))
    )
```

---

##  Tech Stack

| Tool | Usage |
|------|-------|
| **Power BI Desktop** | Dashboard development & visualization |
| **DAX** | All KPIs, RFM scoring, CLV calculation, forecasting |
| **Power Query (M)** | Data cleaning & transformation |
| **Star Schema** | Data modeling (8 tables) |

---

##  Repository Structure

```
neobank-clv-analytics/
│
├── README.md
├── report/
│   └── neobank_clv_dashboard.pbix     ← Main Power BI file
├── screenshots/
│   ├── 01_rfm_segmentation.png
│   ├── 02_pareto_20_80.png
│   ├── 03_clv_per_client.png
│   ├── 04_basket_analysis.png
│   ├── 05_forecast.png
│   ├── 06_opportunities.png
│   └── data_model.png
└── data/
    └── README_data.md                 ← Data source description
```

---

## How to Open

1. Download [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free)
2. Clone this repo: `git clone https://github.com/fallouinfo03/neobank-clv-analytics`
3. Open `report/neobank_clv_dashboard.pbix`
4. Explore the 6 pages using segment slicers

---

## What I Learned

- **RFM modeling in DAX**: Building scored segmentation tables from raw transaction data without Python or SQL
- **Market basket analysis** without ML: Replicating co-purchase logic purely in DAX with a bridge table
- **Pareto visualization**: Combining bar + line chart with dual axes and dynamic cumulative measures
- **CLV formula design**: Translating a business formula (basket × frequency × lifetime) into a reusable DAX measure
- **Star schema discipline**: Keeping fact and dimension tables clean for maximum filter context performance

---

## Context

The dataset is **fully synthetic** — no real customer data was used. It has since been refactored as a personal portfolio project.

---

*Built with Power BI · Inspired by real banking analytics challenges*
