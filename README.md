# AtomicVal: Global Nuclear Fleet Valuation Model

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![Model](https://img.shields.io/badge/Model-Asset%20Level%20DCF-green)
![Status](https://img.shields.io/badge/Status-Production%20Ready-brightgreen)
![License](https://img.shields.io/badge/License-Old%20Economy%20(Private)-red)

**AtomicVal** is a bottom-up Discounted Cash Flow (DCF) model that calculates the intrinsic value of the global nuclear energy industry.

Unlike traditional top-down models that rely on corporate financial statements, this project builds a valuation from the **asset level up**. It utilizes physical engineering data (IAEA PRIS) to model the remaining life, revenue potential, and operational costs of over 400 individual reactors, aggregating them into a global industry valuation.

> **RESTRICTED ACCESS:** This project is protected under the **Old Economy License**. It is intended for proprietary internal use only.

---

## The Investment Thesis

The valuation of the nuclear industry is traditionally opaque, buried in the conglomerate structures of large utilities. This model creates transparency by treating every reactor as a standalone business unit.

**Key Capabilities:**
* **Physical Reality Check:** Valuation is based on *actual* reactor ages and performance data, not management guidance.
* **Uranium Sensitivity:** Quantifies exactly how rising fuel costs impact industry EBITDA margins.
* **Asset Cliff Analysis:** Visualizes the "Revenue Cliff" as the current fleet ages out in the 2030s and 2040s.
* **Interactive Stress Testing:** A dashboard to adjust WACC, Power Prices, and Uranium costs in real-time.

---

## Model Architecture

The project is structured as a sequential data pipeline across 5 Jupyter Notebooks.

### 1. Physical Asset Segmentation (01_fleet_roster.ipynb)
* **Input:** Raw IAEA PRIS Database.
* **Logic:** Filters for operational status; calculates reactor age; applies "Life Extension" logic (60-80 years) to determine remaining economic life.
* **Output:** `fleet_roster.csv` (The Asset Ledger).

### 2. Revenue Modeling (02_revenue_schedule.ipynb)
* **Logic:** "Explodes" the roster into an annual time-series schedule (~12,000 reactor-years).
* **Calculation:** `Capacity (MW) * 8760 * Load Factor * Power Price`.
* **Scenarios:** Bear ($50/MWh), Base ($75/MWh), Bull ($100/MWh).
* **Output:** `fleet_revenue_projections.csv`.

### 3. Cost & EBITDA Modeling (03_cost_structure.ipynb)
* **Logic:** Applies industry-standard cost curves to the revenue schedule.
* **Inputs:** Fixed O&M ($/MW), Variable O&M ($/MWh), and Fuel Cycle Costs (Uranium + Enrichment).
* **Output:** `fleet_financial_projections.csv` (EBITDA Ledger).

### 4. The Valuation Engine (04_dcf_valuation.ipynb)
* **Logic:** Discounts future EBITDA to Present Value (PV) using a dynamic WACC (Weighted Average Cost of Capital).
* **Result:** Calculates the Net Present Value (NPV) of the entire industry and identifies the "Top 10" most valuable assets globally.

### 5. Interactive Risk Dashboard (05_risk_dashboard.ipynb)
* **Tech:** `ipywidgets` + `matplotlib`.
* **Features:** Real-time sliders for Uranium Price, Electricity Price, and Discount Rate. Instantly re-calculates the global NPV.

---

## Key Assumptions

| Parameter | Bear Case | Base Case | Bull Case | Unit |
| :--- | :--- | :--- | :--- | :--- |
| **Power Price** | $50 | $75 | $100 | USD / MWh |
| **Uranium Price** | $50 | $85 | $125 | USD / lb U3O8 |
| **Discount Rate** | - | 8.0% | - | WACC |
| **Load Factor** | - | 5-Yr Avg | - | % |

---

## Installation & Usage

1.  **Environment Setup:**
    ```bash
    git clone [https://github.com/yourusername/AtomicVal-DCF.git](https://github.com/yourusername/AtomicVal-DCF.git)
    cd AtomicVal-DCF
    pip install pandas numpy matplotlib seaborn ipywidgets
    ```

2.  **Data Prerequisite:**
    Ensure `Reactor Database - master(pris.iaea.xlsx - in.csv` is in the root directory.

3.  **Execution:**
    Run the notebooks in sequential order (01 -> 05). The final notebook will launch the interactive GUI.

---

## Visualization Previews

* **The Asset Cliff:** A histogram showing the massive retirement wave approaching the industry.
* **The Revenue Slide:** Projected top-line revenue decline under current fleet conditions.
* **Valuation sensitivity:** Bar charts contrasting the Industry NPV under Bear vs. Bull commodity cycles.

---

## Disclaimer & License

**Disclaimer:** This project is a simulation based on public engineering data. It does not constitute financial advice, investment recommendations, or a solicitation to buy/sell any securities.

**License: OLD ECONOMY LICENSE (Private)**
* **Strictly Confidential.**
* **No Public Distribution.**
* **Internal Use Only.**

Copyright (c) 2025. All Rights Reserved.
