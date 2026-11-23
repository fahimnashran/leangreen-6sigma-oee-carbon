# LeanGreen-6σ: OEE, Quality & Carbon Waste Command Centre

> **Bridging Operational Excellence (Lean Six Sigma / DFSS) and ESG Sustainability using Excel, Power BI, and AI.**

## 1. Project Overview & Problem Statement (DMADV — Define)
In many manufacturing plants, **Operations teams** track scrap and downtime in isolation while **Sustainability/ESG teams** only review electricity bills at year-end. As a result, every defective unit creates a hidden double loss: **wasted raw material & energy cost (COPQ)** and **avoidable grid carbon emissions (tCO2e)**.

**LeanGreen-6σ** is an end-to-end analytics model designed around a simulated Malaysian **bio-composite eco-packaging plant** (3 production lines, 2 shifts, 6 months of shift-level production logs). Built under the **Design for Six Sigma (DFSS — DMADV)** framework, it unifies **Overall Equipment Effectiveness (OEE)**, **Six Sigma quality metrics (DPMO, SPC ±3σ)**, and **Scope 2 carbon accounting** calibrated to the official **Peninsular Malaysia Grid Emission Factor (`0.740 kg CO2e/kWh`)** published by [Suruhanjaya Tenaga / MGTC](https://lcosbasic.mgtc.gov.my/).

---

## 2. Headline Baseline Results (6-Month Simulated Plant Data)
| Metric | Baseline Value | Benchmark / Significance |
| :--- | :---: | :--- |
| **Overall OEE** | **81.7%** | Availability `94.8%` × Performance `90.3%` × Quality `95.5%` |
| **Total Scrap Units** | **15,644 units** | Out of `344,837` total units produced (**DPMO: 45,366**) |
| **Cost of Poor Quality (COPQ)** | **RM 30,333.62** | Direct material & wasted electricity cost from defective units |
| **Wasted Scope 2 Carbon** | **2.01 tCO2e** | Calculated at Peninsular Malaysia Grid EF (`0.740 kg CO2e/kWh`) |

## 3. Architecture & Technical Stack
- **Excel (`LeanGreen6Sigma_Model.xlsx`)**: Relational table setup (`tbl_Line`, `tbl_Defect`, `tbl_Production`), `INDEX/MATCH` + `IFERROR` automated lookups, Data Validation dropdowns, OEE calculation engine, Cost of Poor Quality (`COPQ_RM`), and `Wasted_Carbon_tCO2e`.
- **Power BI Desktop (`LeanGreen6sigma_PROJECT_Designed.pbix`)**:
  - **Data Model**: Clean **Star Schema** (`Dim_Date`, `tbl_Line`, `tbl_Defect` → `Fact_Production`, 1-to-Many single-direction relationships).
  - **DAX Measures**: Weighted `OEE %`, `Availability %`, `Quality %`, `DPMO`, `Total COPQ (RM)`, `Wasted Carbon (tCO2e)`, dynamic **Pareto 80/20 Cumulative %**, and **SPC Control Limits (`±3σ` UCL/LCL)** for Mold Temperature (CTQ).
  - **Built-in AI**: Power BI **Key Influencers** machine learning visual for automated root-cause defect diagnostics + **What-If Numeric Parameter** simulator for projected RM & tCO2e savings.

---

## 4. Dashboard Pages & Key Findings (Measure → Analyze → Design → Verify)

### Page 1 — OEE Scorecard & Pareto 80/20
![Page 1 - OEE and Pareto](screenshots/page1_oee_pareto.png)
- Tracks plant-wide **OEE, Availability, Performance, and Quality** against the **85% world-class benchmark**, filterable by Month, Production Line, and Shift.
- **Pareto 80/20 Combo Chart** isolates the top defect drivers contributing to the majority of scrap losses.

### Page 2 — Statistical Process Control (SPC) & Green Waste
![Page 2 - SPC and Green Waste](screenshots/page2_spc_green.png)
- Quantifies **DPMO**, **Total COPQ (RM)**, and **Wasted Carbon (tCO2e)** directly attributable to scrap.
- **Daily SPC Control Chart (±3σ)** and **Mold Temperature vs. Scrap Scatter Plot** reveal thermal instability on **Line 2** when mold temperatures exceed **182°C**.

### Page 3 — AI Root-Cause Diagnostics & DFSS Action Plan
![Page 3 - AI Key Influencers](screenshots/page3_ai_dmadv.png)
- **AI Key Influencers** confirms that **Line 2**, **Night Shift**, and **Mold Temp > 182°C** are the primary statistical drivers behind spikes in **Warping (D01)** scrap.
- **DFSS Green-by-Design Recommendation**: Implement an automated `>180°C` thermal interlock alarm and pre-shift heater calibration on Line 2, paired with a **What-If Simulator** to quantify projected annual RM and tCO2e savings before capital outlay.

---

## 5. Key DAX Measures Implemented
```dax
Availability % =
DIVIDE(
    SUM(Fact_Production[Planned_Min]) - SUM(Fact_Production[Downtime_Min]),
    SUM(Fact_Production[Planned_Min]), 0
)

Quality % =
DIVIDE(SUM(Fact_Production[Good_Units]), SUM(Fact_Production[Total_Units]), 0)

Pareto Cumulative % =
VAR CurrentScrap = SUM(Fact_Production[Scrap_Units])
VAR TotalScrapAll = CALCULATE(SUM(Fact_Production[Scrap_Units]), ALLSELECTED(Dim_Defect))
VAR CumulScrap =
    SUMX(
        FILTER(
            ALLSELECTED(Dim_Defect),
            CALCULATE(SUM(Fact_Production[Scrap_Units])) >= CurrentScrap
        ),
        CALCULATE(SUM(Fact_Production[Scrap_Units]))
    )
RETURN DIVIDE(CumulScrap, TotalScrapAll, 0)
```

---

## 6. Data Transparency & References
- **Operational Dataset**: Simulated 6-month production dataset (180 shift runs across 3 bio-composite packaging lines) created for portfolio demonstration and root-cause modeling.
- **Grid Emission Factor**: Peninsular Malaysia Grid Emission Factor (`0.740 kg CO2e/kWh`) via [MGTC SME Carbon Footprint Calculator / Suruhanjaya Tenaga](https://lcosbasic.mgtc.gov.my/).
- **Sustainability Alignment**: Designed in alignment with Malaysian industrial decarbonisation priorities such as [PETRONAS Net Zero Carbon Emissions 2050 (NZCE 2050)](https://www.petronas.com/sustainability/delivering-net-zero).
