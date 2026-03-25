<p align="center">
  <h1 align="center">🎓 World University Rankings — Power BI Report</h1>
  <p align="center"><em>A star-schema data model analyzing 592 universities across 73 countries with 29 DAX measures, tracking year-over-year ranking performance from 2025 to 2026.</em></p>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black" />
  <img src="https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white" />
  <img src="https://img.shields.io/badge/Star%20Schema-2E7D32?style=for-the-badge&logo=databricks&logoColor=white" />
  <img src="https://img.shields.io/badge/Version-2.2-6c757d?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Compatibility-Level%201600-003087?style=for-the-badge&logo=microsoft&logoColor=white" />
  <img src="https://img.shields.io/badge/Universities-592-0097A7?style=for-the-badge" />
  <img src="https://img.shields.io/badge/DAX%20Measures-29-7B1FA2?style=for-the-badge" />
</p>

---

## Table of Contents

- [Project Overview](#project-overview)
- [Key Insights](#key-insights)
- [Data Model Architecture](#data-model-architecture)
- [DAX Measures Showcase](#dax-measures-showcase)
- [Report Pages](#report-pages)
- [Tools & Skills Demonstrated](#tools--skills-demonstrated)
- [Data Sources](#data-sources)
- [Project Structure](#project-structure)
- [How to Open & Use](#how-to-open--use)
- [Version History](#version-history)
- [Connect](#connect)

---

## Project Overview

Higher education institutions, ranking agencies, and policy analysts face a fragmented landscape when trying to understand university performance dynamics at a global scale. Year-over-year ranking shifts, regional disparities, and the interplay between research output, internationalization, and sustainability demand a structured, decision-ready analytical environment — not a spreadsheet.

This Power BI report delivers an enterprise-grade analytical model for the **QS World University Rankings**, covering **592 universities across 73 countries and 5 global regions** for the years **2025 and 2026**. The model is built on a clean **star schema** with a central fact table and 6 dimension tables, enabling fast, reliable analysis across 11 academic performance dimensions without ambiguous filter propagation or calculation errors common in flat-file Power BI designs.

Stakeholders can use this report to benchmark institutional performance against global and regional averages, identify ranking movers and consistent performers, analyze the relationship between research intensity and internationalization, and evaluate sustainability commitments across institution types — all through a 5-page interactive report with cross-filtered slicers, drill-through hierarchies, and calculated classification columns that pre-segment the data for immediate insight.

---

## Key Insights

> These findings emerge from the 2025→2026 dataset and reflect patterns that are analytically meaningful beyond surface-level ranking tables.

| # | Finding | Analytical Commentary |
|---|---------|----------------------|
| 📊 | **49.7% of universities improved their ranking** (294 of 592) | The near-equal split between improvers (294) and decliners (295) suggests systemic ranking methodology adjustments rather than isolated institutional improvements — a single-year snapshot would miss this nuance entirely |
| 🌍 | **73 countries represented across 5 regions** | Europe and Asia dominate in volume, but regional average rank reveals that representation ≠ performance — key for policy benchmarking |
| 📈 | **Average ranking volatility of 23.4 positions** | High volatility signals that QS rankings are sensitive to short-term metric shifts, making multi-year tracking far more meaningful than any single edition |
| 🔬 | **International Index (60.1) outpaces Research Excellence (49.8)** | Universities are more internationally connected than they are research-productive — a strategic gap that elite institutions actively close |
| 🏆 | **Only 16.7% of universities are ranked in the Top 100** (99 of 592) | The Top 100 acts as a natural benchmark denominator; country/region comparisons against this threshold reveal true dominance vs. participation |
| 🌱 | **170 universities are Sustainability Leaders** (score ≥ 80) | Nearly 29% of all ranked institutions hold sustainability leadership status — a meaningful structural shift in higher education priorities post-2023 |
| 🔒 | **Only 26% are Consistent Performers** (≤5 position change) | The majority of institutions are in active ranking flux, making this dataset particularly valuable for longitudinal tracking |
| ⚡ | **Average Overall Score: 50.3 with StdDev of 18.4** | High standard deviation confirms a wide performance spread — average-based benchmarks alone are misleading without percentile context |

---

## Data Model Architecture

### Schema Design

The model follows a **star schema** — the gold standard for analytical Power BI models. A single central fact table connects to 6 dimension tables via Many-to-One relationships. All cross-filter directions are set to **single direction** (dimension → fact) to prevent circular dependency risks and ensure predictable filter context in DAX calculations.

```
                    DimCountry  ──────┐
                    DimRegion   ──────┤
                    DimStatus   ──────┼──→  FactUniversity  ←──  DAX Measures
                    DimSize     ──────┤          (592 rows)       (0 data rows)
                    DimResearch ──────┤
                    DimFocus    ──────┘
```

> **Design decision**: A dedicated `DAX Measures` table (with no data rows) centralizes all 29 measures into a single, organized repository. This prevents measure sprawl across tables and makes the model substantially easier to maintain and hand off.

---

### Tables

| Table | Type | Rows | Purpose |
|-------|------|------|---------|
| `FactUniversity` | Fact | 592 | Central table — rankings, scores, foreign keys, calculated columns |
| `DimCountry` | Dimension | 73 | Geographic country classification |
| `DimRegion` | Dimension | 5 | Continental region grouping (Americas, Europe, Asia, Oceania, Africa) |
| `DimStatus` | Dimension | 4 | Institutional public/private status |
| `DimSize` | Dimension | 4 | Student population size bands with min/max thresholds |
| `DimResearch` | Dimension | 4 | Research output intensity classification with level indicator |
| `DimFocus` | Dimension | 4 | Institutional specialization type with focus level |
| `DAX Measures` | Measures | 0 | Centralized measure storage — no data rows |

---

### Relationships

All 6 relationships are **active**, **Many-to-One**, with **single-direction cross-filtering**.

| From (Fact) | To (Dimension) | Join Column | Cardinality |
|-------------|----------------|-------------|-------------|
| `FactUniversity[CountryKey]` | `DimCountry[CountryKey]` | CountryKey | Many → One |
| `FactUniversity[RegionKey]` | `DimRegion[RegionKey]` | RegionKey | Many → One |
| `FactUniversity[StatusKey]` | `DimStatus[StatusKey]` | StatusKey | Many → One |
| `FactUniversity[SizeKey]` | `DimSize[SizeKey]` | SizeKey | Many → One |
| `FactUniversity[ResearchKey]` | `DimResearch[ResearchKey]` | ResearchKey | Many → One |
| `FactUniversity[FocusKey]` | `DimFocus[FocusKey]` | FocusKey | Many → One |

✅ **100% referential integrity** — zero orphaned foreign keys, zero nulls, all 592 fact rows have valid dimension matches.

---

### Calculated Columns (7)

All calculated columns are stored on `FactUniversity` and enable slicer-based segmentation without runtime DAX overhead.

| Column | Type | Categories | Analytical Use |
|--------|------|------------|----------------|
| `Ranking Tier` | String | Top 10 / Top 50 / Top 100 / Top 200 / Top 500 / Beyond 500 | Ranking band grouping for tier-based analysis |
| `Rank Direction` | String | Improved / Declined / Stable | Row-level YoY movement direction |
| `Performance Category` | String | High Performer / Mid Performer / Emerging | Overall score segmentation (≥75 / 50–74 / <50) |
| `Rank Change Magnitude` | String | No Change / Small / Medium / Large / Very Large | Magnitude of position shift (0 / 1-10 / 11-50 / 51-100 / 100+) |
| `Research Intensity` | String | Research Intensive / Teaching Focused | Dual-threshold classification (Academic Rep + Citations both ≥80) |
| `International Level` | String | Highly International / Moderately International / Locally Focused | Composite of 3 international scores (≥70 / 40–69 / <40) |
| `Sustainability Level` | String | Leader / Good / Fair / Emerging | Sustainability score banding (≥80 / 60–79 / 40–59 / <40) |

---

### Hierarchies (4)

| Hierarchy | Levels | Primary Use Case |
|-----------|--------|-----------------|
| **Performance Profile** | Performance Category → Research Intensity → International Level | Segmentation analysis — understand what drives high performance |
| **Ranking Movement** | Rank Direction → Rank Change Magnitude | Volatility analysis — characterize movers vs. stayers |
| **University Excellence** | Ranking Tier → Performance Category | Identify underperformers within top-tier bands |
| **Sustainability Focus** | Sustainability Level → Performance Category | Correlate ESG commitment with academic achievement |

---

## DAX Measures Showcase

29 measures organized across 6 analytical categories. Collapsible sections below contain the full inventory; 4 spotlight measures are featured with annotated DAX to demonstrate advanced calculation techniques.

### Categories at a Glance

| Category | Count | Primary Use |
|----------|-------|-------------|
| Core Metrics | 9 | Fundamental KPIs: counts, averages, top-N counts |
| Benchmarking | 8 | Comparative performance vs. global and regional averages |
| Composite | 2 | Custom composite indexes from multiple raw dimensions |
| Statistical | 3 | Distribution analysis, outlier detection, volatility |
| Movement & Trend | 5 | YoY ranking dynamics, consistency, improvement rates |
| Specialty | 2 | Sustainability leadership and research citation analysis |

---

### Spotlight Measures

#### 1. `Universities Above Avg` — Filter Context with Variable Isolation

```dax
Universities Above Avg =
VAR GlobalAvg = CALCULATE(AVERAGE(FactUniversity[Overall]), ALL(FactUniversity))
RETURN
CALCULATE([Total Univerisities], FactUniversity[Overall] > GlobalAvg)
```

> **Why this matters**: A VAR captures the global average *before* the CALCULATE filter is applied. Without the VAR, using a measure reference inside a filter argument would cause the inner measure to evaluate under the same filter context as the outer CALCULATE — producing a circular or incorrect result. The variable locks the global average as a scalar before any row-by-row filtering occurs.

---

#### 2. `International Index` — AVERAGEX Over a Composite Expression

```dax
International Index =
AVERAGEX(
    FactUniversity,
    (FactUniversity[International Faculty] +
     FactUniversity[International Students] +
     FactUniversity[International Research Network]) / 3
)
```

> **Why this matters**: `AVERAGE` can only operate on a single column. Using `AVERAGEX` iterates over each row, computes the three-column composite expression per row, and then averages the results — producing a true row-level composite average rather than an average of column averages, which would yield different (and incorrect) results when filters are applied.

---

#### 3. `Overall vs Average` — Removing Filter Context for True Global Benchmarking

```dax
Overall vs Average =
AVERAGE(FactUniversity[Overall]) -
CALCULATE(AVERAGE(FactUniversity[Overall]), ALL(FactUniversity))
```

> **Why this matters**: `ALL(FactUniversity)` strips all active filters from the table — including user slicer selections — to return the true global average regardless of what a user has selected. This ensures the benchmark is always anchored to the complete dataset, making this measure reliable as a KPI comparison card in any filter context.

---

#### 4. `Ranking Volatility` — Statistical Measure with AVERAGEX + ABS

```dax
Ranking Volatility =
AVERAGEX(
    FactUniversity,
    ABS(FactUniversity[RankingY2026] - FactUniversity[RankingY2025])
)
```

> **Why this matters**: `ABS` ensures that both improvements and declines contribute positively to volatility — a university moving from rank 50 to 100 is just as volatile as one moving from 100 to 50. Using `AVERAGEX` with row-level `ABS` produces a true mean absolute deviation, which is the correct statistical measure for ranking stability analysis.

---

<details>
<summary><strong>Core Metrics — 9 measures</strong></summary>

These measures power the fundamental KPI cards and summary statistics across all 5 report pages.

| Measure | Returns | Current Value | Technique |
|---------|---------|---------------|-----------|
| `Total Univerisities` | Count of universities in context | 592 | `COUNTROWS` |
| `Total Countries` | Distinct countries in context | 73 | `DISTINCTCOUNT` |
| `Avg Overall Score` | Average overall performance score | 50.3 | `AVERAGE` |
| `Top 100 Univerisity` | Count of universities ranked ≤100 | 99 | `CALCULATE` + filter |
| `Rank Change` | Sum of YoY position change (positive = improved) | — | `SUM` arithmetic |
| `Rank Change Category` | Text: Improved / Declined / Stable | — | `SWITCH(TRUE())` |
| `Universities Improved` | Count with better 2026 rank | 294 | `CALCULATE` + row comparison |
| `Universities Declined` | Count with worse 2026 rank | 295 | `CALCULATE` + row comparison |
| `Universities Stable` | Count with identical rank | 3 | `CALCULATE` + row comparison |

</details>

<details>
<summary><strong>Benchmarking — 8 measures</strong></summary>

These measures support comparative analysis, allowing users to evaluate selections against global or regional reference points.

| Measure | Returns | Current Value | Technique |
|---------|---------|---------------|-----------|
| `Median Overall Score` | Median overall score | 45.9 | `MEDIAN` |
| `Overall vs Average` | Delta from global avg | — | `ALL()` for context removal |
| `Regional Avg Rank` | Average 2026 rank in context | 299.6 | `AVERAGE` |
| `Top 100 Percentage` | % of universities in Top 100 | 16.7% | `DIVIDE` |
| `Top 500 Universities` | Count ranked ≤500 | 499 | `CALCULATE` + filter |
| `Universities Above Avg` | Count above global avg | 249 | `VAR` + `CALCULATE` |
| `Best Rank in Context` | Minimum (best) rank in selection | — | `MIN` |
| `Above Avg Percentage` | % above global avg | 42.1% | `DIVIDE` |

</details>

<details>
<summary><strong>Composite Metrics — 2 measures</strong></summary>

Custom composite indexes built from multiple raw dimension columns, enabling cross-dimensional performance comparison.

| Measure | Returns | Current Value | Technique |
|---------|---------|---------------|-----------|
| `International Index` | Avg of 3 international dimension scores | 60.1 | `AVERAGEX` over composite expression |
| `Research Excellence` | Avg of Academic Reputation + Citations per Faculty | 49.8 | `AVERAGEX` over composite expression |

</details>

<details>
<summary><strong>Statistical — 3 measures</strong></summary>

Statistical measures for distribution analysis and outlier detection.

| Measure | Returns | Current Value | Technique |
|---------|---------|---------------|-----------|
| `StdDev Overall` | Population standard deviation of overall scores | 18.4 | `STDEV.P` |
| `Top Quartile Threshold` | 75th percentile of overall scores | 62.3 | `PERCENTILE.INC` |
| `Ranking Volatility` | Mean absolute YoY ranking change | 23.4 | `AVERAGEX` + `ABS` |

</details>

<details>
<summary><strong>Movement & Trend — 5 measures</strong></summary>

These measures quantify ranking dynamics and are the backbone of Page 3 (Ranking Movement & Trends).

| Measure | Returns | Current Value | Technique |
|---------|---------|---------------|-----------|
| `Consistent Performers` | Count with ≤5 position change | 154 | `CALCULATE` + `ABS` filter |
| `Avg Rank Improvement` | Mean YoY position gain | -0.4 | `AVERAGEX` |
| `Improvement Rate` | % of universities that improved | 49.7% | `DIVIDE` |
| `Consistency Rate` | % of consistent performers | 26.0% | `DIVIDE` |
| `Avg Employment Score` | Average employment outcomes score | 48.4 | `AVERAGE` |

</details>

<details>
<summary><strong>Specialty — 2 measures</strong></summary>

Targeted measures for sustainability and research citation analysis, powering Page 5.

| Measure | Returns | Current Value | Technique |
|---------|---------|---------------|-----------|
| `Sustainability Leaders` | Count with sustainability score ≥80 | 170 | `CALCULATE` + filter |
| `High Citation Count` | Count in top 10% for citations | 61 | `CALCULATE` + `PERCENTILE.INC` |

</details>

---

## Report Pages

The report is structured as a 5-page analytical narrative, each page answering a distinct business question for a different audience need.

---

### Page 1 — Executive Overview

> **Business question**: *What is the state of global university rankings in 2026, and how has the performance landscape shifted year-over-year?*

Designed for leadership and non-technical stakeholders who need the big picture at a glance.

- **6 KPI Cards**: Total Universities · Top 100 % · Avg Overall Score · Universities Improved · International Index · Research Excellence
- **Donut Chart**: Ranking movement split (Improved / Declined / Stable)
- **Clustered Bar Chart**: Top 10 countries by university count
- **Gauge Chart**: Improvement Rate vs. 50% target
- **Sparkline Card**: Ranking Volatility indicator
- **Slicers**: Region, Performance Category

---

### Page 2 — Geographic Analysis

> **Business question**: *Which regions and countries dominate the rankings, and where is academic performance concentrated versus distributed?*

Enables geographic benchmarking for institutional strategy and regional policy analysis.

- **Map Visual**: Countries sized by university count, colored by average overall score
- **Matrix**: Region → Country drill-down with rank, score, and movement metrics
- **Clustered Column Chart**: Regional performance comparison across key dimensions
- **Scatter Chart**: Country size (university count) vs. average quality score
- **Slicers**: Region, Ranking Tier

---

### Page 3 — Ranking Movement & Trends

> **Business question**: *Which universities experienced the most significant ranking shifts, and what patterns distinguish movers from consistent performers?*

The volatility and momentum analysis page — critical for year-over-year narrative reporting.

- **Waterfall Chart**: Ranking movement breakdown by direction
- **100% Stacked Bar**: Rank change distribution by region
- **Clustered Bar**: Rank change magnitude by performance category
- **Line Chart**: Volatility by country (Top 20)
- **KPI Cards**: Improved / Declined / Stable counts with rates
- **Table**: Biggest movers (top gainers and fallers)
- **Slicers**: Rank Direction, Region

---

### Page 4 — Performance Deep Dive

> **Business question**: *What drives high-performer status — research excellence, internationalization, sustainability, or all three?*

The analytical core of the report — multi-dimensional performance decomposition using all 11 QS scoring dimensions.

- **Hierarchy Slicer**: Performance Profile drill-down (Performance → Research → International)
- **Radar Chart**: All 11 performance dimensions — shape reveals institutional profile
- **Scatter Plot**: Research Excellence vs. International Index — identify quadrant positioning
- **Funnel Chart**: Performance category distribution (High → Mid → Emerging)
- **Ribbon Chart**: Top performers by key dimension, ranked
- **KPI Cards**: StdDev Overall · Top Quartile Threshold · Median Overall Score
- **Slicers**: Research Intensity, International Level, Sustainability Level

---

### Page 5 — Sustainability & Specialization

> **Business question**: *How do institutional focus and sustainability commitment correlate with academic ranking performance?*

Examines whether sustainability leadership translates to ranking advantage, and how institutional characteristics (size, research type, focus) segment performance.

- **Tree Map**: Universities grouped by Sustainability Level
- **Stacked Area**: Sustainability score distribution across the full dataset
- **Clustered Column**: Institutional size distribution by performance category
- **Matrix**: Performance breakdown by Research Type × Institutional Focus
- **Donut Charts**: Research Type · International Level · Focus Type splits
- **KPI Cards**: Sustainability Leaders · High Citation Count · Avg Employment Score
- **Slicers**: Status, Size Category, Sustainability Level

---

## Tools & Skills Demonstrated

| Tool / Skill | Application in This Project |
|---|---|
| **Power BI Desktop** | End-to-end report authoring — data model, DAX, visualization, layout |
| **DAX** | 29 measures across 6 categories; advanced use of CALCULATE, AVERAGEX, VAR, SWITCH, ALL, PERCENTILE.INC, STDEV.P |
| **Star Schema Design** | 1 fact table + 6 dimensions + measures table; deliberate single-direction cross-filtering |
| **Power Query (M)** | Data ingestion from CSV and XLSX, type enforcement, column normalization |
| **Dimensional Modeling** | 4 drill-through hierarchies, 7 row-level calculated columns for analytical segmentation |
| **Data Modeling** | Cardinality management, referential integrity validation, foreign key mapping |
| **Statistical Analysis** | Standard deviation, percentile thresholds, mean absolute deviation (volatility) |
| **Microsoft Excel** | Dimension workbook authoring — Status, Size, Research, Focus lookup tables |
| **Analytical Storytelling** | 5-page report structured as a progressive analytical narrative by audience type |

**DAX Functions Used**:
`CALCULATE` · `AVERAGEX` · `COUNTROWS` · `DISTINCTCOUNT` · `DIVIDE` · `SWITCH` · `IF` · `VAR` / `RETURN` · `ALL` · `ALLSELECTED` · `MIN` · `AVERAGE` · `MEDIAN` · `STDEV.P` · `PERCENTILE.INC` · `SUM` · `ABS`

---

## Data Sources

| File | Format | Records | Role in Model |
|------|--------|---------|---------------|
| `2026-world-university-rankings.csv` | CSV | 592 rows | Primary fact data — university names, rankings for 2025 and 2026, all 11 performance dimension scores |
| `Dimension Workbook.xlsx` | XLSX | Multiple sheets | Pre-built dimension tables for Status, Size, Research, and Focus classifications with surrogate keys |

**Data origin**: QS World University Rankings dataset covering ranking years 2025 and 2026. All data reflects publicly available institutional ranking information. No personal or sensitive data is contained in this dataset.

> **Note**: If you plan to refresh this report, ensure both source files are placed in the same directory as the `.pbix` file and update the data source paths via **File → Options → Data Source Settings** in Power BI Desktop.

---

## Project Structure

```
10-World-University-Ranking-Project/
│
├── world-university-ranking-report.pbix          # Main Power BI report file
├── 2026-world-university-rankings.csv            # Primary data source (fact data)
├── Dimension Workbook.xlsx                        # Dimension reference data (lookup tables)
├── world-university-ranking-model-documentation.md  # Full technical model documentation
├── README.md                                      # This file
│
└── assets/                                        # Screenshots for README preview
    ├── model-diagram.png                          # Power BI model view screenshot
    ├── page-1-executive-overview.png
    ├── page-2-geographic-analysis.png
    ├── page-3-ranking-movement.png
    ├── page-4-performance-deepdive.png
    └── page-5-sustainability.png
```

> **Note**: The `assets/` folder requires report screenshots to be exported from Power BI Desktop. Open the `.pbix` file, navigate to each page, and use **File → Export → Export to PDF** or take screenshots directly from the report canvas for embedding in this README.

---

## How to Open & Use

### Prerequisites

- [Power BI Desktop](https://powerbi.microsoft.com/desktop/) — free download from Microsoft
- Minimum version: **Power BI Desktop 2.96+** (Compatibility Level 1600, released April 2021 or later)

### Steps

1. **Clone or download** this repository to your local machine
2. Ensure `2026-world-university-rankings.csv` and `Dimension Workbook.xlsx` are in the **same folder** as the `.pbix` file
3. Open `world-university-ranking-report.pbix` in Power BI Desktop
4. If prompted about data source paths, go to **File → Options and Settings → Data Source Settings** and update the file paths to your local directory
5. Click **Refresh** in the Home ribbon to reload data
6. Navigate report pages using the tabs at the bottom of the canvas

### Viewing Without Power BI Desktop

- A **PDF export** of the report can be generated via **File → Export → Export to PDF** in Power BI Desktop for offline or cross-platform viewing
- If this report is published to **Power BI Service**, a live interactive link will be added here

### Compatibility Note

This report is built at **Compatibility Level 1600**. Opening in significantly older versions of Power BI Desktop may produce compatibility warnings. Upgrading to the latest version of Power BI Desktop is always recommended.

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-12-26 | Initial model creation — fact table, 6 dimensions, core relationships |
| 1.1 | 2026-01-08 | Core DAX measures added (9 measures) |
| 2.0 | 2026-03-25 | Major release — 20 new measures, 7 calculated columns, 4 hierarchies, 5-page report layout |
| 2.1 | 2026-03-25 | Fixed 5 DAX measures (`AVERAGE` → `AVERAGEX`); resolved filter context issue in `Universities Above Avg` |
| 2.2 | 2026-03-25 | Full technical documentation · README |

---

## Connect

I build data models and analytical reports that turn raw data into decisions. If you found this project useful or want to discuss Power BI, DAX, or data modeling, feel free to connect.

<p>
  <a href="https://www.linkedin.com/in/your-linkedin-handle"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" /></a>
  &nbsp;
  <a href="https://github.com/your-github-handle"><img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" /></a>
</p>

---

<p align="center">
  <sub>Built with Power BI Desktop · Star schema · 29 DAX measures · 5 analytical report pages</sub>
</p>
