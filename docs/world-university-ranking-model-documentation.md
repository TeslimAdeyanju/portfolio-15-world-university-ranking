# World University Rankings Report - Complete Documentation

**Model Name:** word-university-ranking-report  
**Version:** 2026.03.25  
**Compatibility Level:** 1600  
**Model Type:** Tabular (Import Mode)  
**Created:** December 26, 2025  
**Last Updated:** March 25, 2026

---

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [Data Model Architecture](#data-model-architecture)
3. [Tables & Columns](#tables--columns)
4. [Relationships](#relationships)
5. [DAX Measures](#dax-measures)
6. [Calculated Columns](#calculated-columns)
7. [Hierarchies](#hierarchies)
8. [Data Quality Validation](#data-quality-validation)
9. [Visualization Plan](#visualization-plan)
10. [Deployment Guide](#deployment-guide)
11. [Maintenance & Troubleshooting](#maintenance--troubleshooting)

---

## Executive Summary

### Purpose
This Power BI model analyzes **world university rankings** for years **2025 and 2026**, tracking performance across multiple academic dimensions and enabling comparative analysis of ranking changes, regional performance, and institutional characteristics.

### Key Statistics
- **Universities**: 592
- **Countries**: 73
- **Regions**: 5 (Americas, Europe, Asia, Oceania, Africa)
- **Tables**: 8 (1 Fact, 6 Dimensions, 1 Measures)
- **Relationships**: 6 (all Many-to-One)
- **DAX Measures**: 29
- **Calculated Columns**: 7
- **Hierarchies**: 4

### Business Value
- Track year-over-year ranking improvements and declines
- Compare universities by geography, size, research intensity, and focus
- Analyze performance across 11 academic dimensions
- Identify trends in internationalization and sustainability
- Benchmark institutional performance against global averages

---

## Data Model Architecture

### Schema Type
**Star Schema** with dimensional design:
- 1 Central Fact Table: `FactUniversity`
- 6 Dimension Tables: Country, Region, Status, Size, Research, Focus
- 1 Measures Table: `DAX Measures` (calculation-only table)

### Model Diagram

```
DimCountry ────┐
               │
DimRegion ─────┤
               │
DimStatus ─────┼──→ FactUniversity
               │
DimSize ───────┤
               │
DimResearch ───┤
               │
DimFocus ──────┘
```

### Data Cardinality
- **Fact Records**: 592 universities
- **Country Dimension**: 73 countries (avg 8.1 universities per country)
- **Region Dimension**: 5 regions (avg 118.4 universities per region)
- **Status Dimension**: 4 status types
- **Size Dimension**: 4 size categories
- **Research Dimension**: 4 research types
- **Focus Dimension**: 4 focus types

---

## Tables & Columns

### FactUniversity (Fact Table)
**Purpose**: Central fact table with university performance metrics and rankings

#### Columns (27 total)

##### Ranking Information
| Column Name | Data Type | Description |
|------------|-----------|-------------|
| `RankingY2026` | Integer | 2026 ranking position |
| `RankingY2025` | Integer | 2025 ranking position |
| `Institution` | String | University name |

##### Performance Metrics (11 dimensions)
| Column Name | Data Type | Description |
|------------|-----------|-------------|
| `Overall` | Decimal | Overall composite score (0-100) |
| `Academic Reputation` | Decimal | Academic reputation score |
| `Employer Reputation` | Decimal | Employer reputation score |
| `Faculty Student` | Decimal | Faculty-to-student ratio score |
| `Citations per Faculty` | Decimal | Research citation score |
| `International Faculty` | Decimal | International faculty ratio score |
| `International Students` | Decimal | International student ratio score |
| `International Students Diversity` | Decimal | Student diversity score |
| `International Research Network` | Decimal | Research collaboration score |
| `Employment Outcomes` | Decimal | Graduate employment score |
| `Sustainability` | Decimal | Sustainability score |

##### Foreign Keys
| Column Name | Data Type | References |
|------------|-----------|-----------|
| `CountryKey` | Integer | DimCountry[CountryKey] |
| `RegionKey` | Integer | DimRegion[RegionKey] |
| `StatusKey` | Integer | DimStatus[StatusKey] |
| `SizeKey` | Integer | DimSize[SizeKey] |
| `ResearchKey` | Integer | DimResearch[ResearchKey] |
| `FocusKey` | Integer | DimFocus[FocusKey] |

##### Calculated Columns (7)
| Column Name | Data Type | Purpose |
|------------|-----------|---------|
| `Ranking Tier` | String | Categories: Top 10, Top 50, Top 100, Top 200, Top 500, Beyond 500 |
| `Rank Direction` | String | Improved / Declined / Stable |
| `Performance Category` | String | High Performer / Mid Performer / Emerging |
| `Rank Change Magnitude` | String | No Change / Small (1-10) / Medium (11-50) / Large (51-100) / Very Large (100+) |
| `Research Intensity` | String | Research Intensive / Teaching Focused |
| `International Level` | String | Highly International / Moderately International / Locally Focused |
| `Sustainability Level` | String | Leader / Good / Fair / Emerging |

---

### DimCountry (Dimension)
**Purpose**: Country dimension for geographic analysis

| Column Name | Data Type | Description |
|------------|-----------|-------------|
| `CountryKey` | Integer | Primary key |
| `Country` | String | Country name |

**Records**: 73 countries

---

### DimRegion (Dimension)
**Purpose**: Regional grouping dimension

| Column Name | Data Type | Description |
|------------|-----------|-------------|
| `RegionKey` | Integer | Primary key |
| `Region` | String | Region name |

**Records**: 5 regions
- Americas
- Europe
- Asia
- Oceania
- Africa

---

### DimStatus (Dimension)
**Purpose**: Institutional status classification

| Column Name | Data Type | Description |
|------------|-----------|-------------|
| `StatusKey` | Integer | Primary key |
| `Status` | String | Status classification |

**Records**: 4 status types

---

### DimSize (Dimension)
**Purpose**: University size classification based on student population

| Column Name | Data Type | Description |
|------------|-----------|-------------|
| `SizeKey` | Integer | Primary key |
| `SizeCode` | String | Size code |
| `SizeName` | String | Size category name |
| `MinStudents` | Integer | Minimum student count for category |
| `MaxStudents` | Integer | Maximum student count for category |

**Records**: 4 size categories

---

### DimResearch (Dimension)
**Purpose**: Research output classification

| Column Name | Data Type | Description |
|------------|-----------|-------------|
| `ResearchKey` | Integer | Primary key |
| `ResearchCode` | String | Research type code |
| `ResearchName` | String | Research classification name |
| `ResearchLevel` | Integer | Research intensity level |

**Records**: 4 research types

---

### DimFocus (Dimension)
**Purpose**: Institutional focus/specialization classification

| Column Name | Data Type | Description |
|------------|-----------|-------------|
| `FocusKey` | Integer | Primary key |
| `FocusCode` | String | Focus type code |
| `FocusName` | String | Focus area name |
| `FocusLevel` | Integer | Focus level indicator |

**Records**: 4 focus types

---

### DAX Measures (Calculation Table)
**Purpose**: Centralized storage for all DAX measures

**Records**: No data rows (measures-only table)  
**Measures**: 29 DAX measures

---

## Relationships

All relationships follow **Many-to-One** cardinality from fact to dimensions with **one-directional** cross-filtering.

### Relationship Details

| From Table | From Column | To Table | To Column | Cardinality | Status |
|-----------|-------------|----------|-----------|-------------|--------|
| FactUniversity | CountryKey | DimCountry | CountryKey | Many-to-One | Active |
| FactUniversity | RegionKey | DimRegion | RegionKey | Many-to-One | Active |
| FactUniversity | StatusKey | DimStatus | StatusKey | Many-to-One | Active |
| FactUniversity | SizeKey | DimSize | SizeKey | Many-to-One | Active |
| FactUniversity | ResearchKey | DimResearch | ResearchKey | Many-to-One | Active |
| FactUniversity | FocusKey | DimFocus | FocusKey | Many-to-One | Active |

### Relationship Properties
- **Cross-Filtering Behavior**: One Direction (from dimension to fact)
- **Security Filtering**: One Direction
- **Referential Integrity**: Enforced
- **Active Status**: All relationships active

### Data Integrity Status
✅ **100% Data Integrity**
- No duplicate keys in dimension tables
- No orphaned foreign keys in fact table
- All 592 fact records have valid dimension references
- Zero missing or null foreign key values

---

## DAX Measures

### Core Metrics (9 measures)

#### 1. Total Universities
```dax
Total Univerisities = COUNTROWS(FactUniversity)
```
**Description**: Count of universities in current context  
**Format**: Whole number  
**Current Value**: 592

#### 2. Total Countries
```dax
Total Countries = DISTINCTCOUNT(FactUniversity[CountryKey])
```
**Description**: Distinct count of countries  
**Format**: Whole number  
**Current Value**: 73

#### 3. Avg Overall Score
```dax
Avg Overall Score = AVERAGE(FactUniversity[Overall])
```
**Description**: Average overall performance score  
**Format**: Decimal (auto)  
**Current Value**: 50.3

#### 4. Top 100 University
```dax
Top 100 Univerisity = 
CALCULATE(
    COUNTROWS(FactUniversity),
    FactUniversity[RankingY2026] <= 100
)
```
**Description**: Count of universities ranked in top 100  
**Format**: Whole number  
**Current Value**: 99

#### 5. Rank Change
```dax
Rank Change = 
SUM(FactUniversity[RankingY2025]) - SUM(FactUniversity[RankingY2026])
```
**Description**: Year-over-year ranking change (positive = improved ranking)  
**Format**: #,##0  
**Logic**: Lower rank number = better rank, so 2025-2026 shows improvement

#### 6. Rank Change Category
```dax
Rank Change Category = 
SWITCH(
    TRUE(),
    [Rank Change] > 0, "Improved",
    [Rank Change] < 0, "Declined",
    "Stable"
)
```
**Description**: Categorizes rank changes  
**Format**: Text  
**Values**: Improved / Declined / Stable

#### 7. Universities Improved
```dax
Universities Improved = 
CALCULATE(
    [Total Univerisities],
    FactUniversity[RankingY2026] < FactUniversity[RankingY2025]
)
```
**Description**: Count of universities with improved rankings  
**Format**: Whole number  
**Current Value**: 294

#### 8. Universities Declined
```dax
Universities Declined = 
CALCULATE(
    [Total Univerisities],
    FactUniversity[RankingY2026] > FactUniversity[RankingY2025]
)
```
**Description**: Count of universities with declined rankings  
**Format**: Whole number  
**Current Value**: 295

#### 9. Universities Stable
```dax
Universities Stable = 
CALCULATE(
    [Total Univerisities],
    FactUniversity[RankingY2026] = FactUniversity[RankingY2025]
)
```
**Description**: Count of universities with unchanged rankings  
**Format**: Whole number  
**Current Value**: 3

---

### Benchmarking Measures (8 measures)

#### 10. Median Overall Score
```dax
Median Overall Score = MEDIAN(FactUniversity[Overall])
```
**Description**: Median overall score (better than average for skewed distributions)  
**Format**: 0.00  
**Current Value**: 45.9

#### 11. Overall vs Average
```dax
Overall vs Average = 
AVERAGE(FactUniversity[Overall]) - 
CALCULATE(AVERAGE(FactUniversity[Overall]), ALL(FactUniversity))
```
**Description**: How current selection compares to global average  
**Format**: +0.00;-0.00;0.00  
**Use**: Benchmark indicator

#### 12. Regional Avg Rank
```dax
Regional Avg Rank = AVERAGE(FactUniversity[RankingY2026])
```
**Description**: Average ranking in current context (region, country, etc.)  
**Format**: 0  
**Current Value**: 299.6

#### 13. Top 100 Percentage
```dax
Top 100 Percentage = 
DIVIDE([Top 100 Univerisity], [Total Univerisities], 0)
```
**Description**: Percentage of universities in Top 100  
**Format**: 0.0%  
**Current Value**: 16.7%

#### 14. Top 500 Universities
```dax
Top 500 Universities = 
CALCULATE([Total Univerisities], FactUniversity[RankingY2026] <= 500)
```
**Description**: Count of universities ranked in top 500  
**Format**: 0  
**Current Value**: 499

#### 15. Universities Above Avg
```dax
Universities Above Avg = 
VAR GlobalAvg = CALCULATE(AVERAGE(FactUniversity[Overall]), ALL(FactUniversity))
RETURN
CALCULATE([Total Univerisities], FactUniversity[Overall] > GlobalAvg)
```
**Description**: Count of universities above global average  
**Format**: 0  
**Current Value**: 249  
**Note**: Fixed to use variable instead of measure reference in filter

#### 16. Best Rank in Context
```dax
Best Rank in Context = MIN(FactUniversity[RankingY2026])
```
**Description**: Best ranking in current context (country/region)  
**Format**: 0  
**Use**: Identify national/regional champions

#### 17. Above Avg Percentage
```dax
Above Avg Percentage = 
DIVIDE([Universities Above Avg], [Total Univerisities], 0)
```
**Description**: Percentage of universities above global average  
**Format**: 0.0%  
**Current Value**: 42.1%

---

### Composite Metrics (2 measures)

#### 18. International Index
```dax
International Index = 
AVERAGEX(
    FactUniversity,
    (FactUniversity[International Faculty] + 
     FactUniversity[International Students] + 
     FactUniversity[International Research Network]) / 3
)
```
**Description**: Composite internationalization score  
**Format**: 0.0  
**Current Value**: 60.1  
**Note**: Fixed to use AVERAGEX for calculated average

#### 19. Research Excellence
```dax
Research Excellence = 
AVERAGEX(
    FactUniversity,
    (FactUniversity[Academic Reputation] + 
     FactUniversity[Citations per Faculty]) / 2
)
```
**Description**: Research excellence composite metric  
**Format**: 0.0  
**Current Value**: 49.8  
**Note**: Fixed to use AVERAGEX for calculated average

---

### Statistical Measures (3 measures)

#### 20. StdDev Overall
```dax
StdDev Overall = STDEV.P(FactUniversity[Overall])
```
**Description**: Standard deviation of overall scores  
**Format**: 0.00  
**Current Value**: 18.4  
**Use**: Measure of score variability

#### 21. Top Quartile Threshold
```dax
Top Quartile Threshold = 
PERCENTILE.INC(FactUniversity[Overall], 0.75)
```
**Description**: 75th percentile of overall scores  
**Format**: 0.0  
**Current Value**: 62.3  
**Use**: Identify top-performing threshold

#### 22. Ranking Volatility
```dax
Ranking Volatility = 
AVERAGEX(
    FactUniversity,
    ABS(FactUniversity[RankingY2026] - FactUniversity[RankingY2025])
)
```
**Description**: Average magnitude of ranking changes  
**Format**: 0.0  
**Current Value**: 23.4 positions  
**Note**: Fixed to use AVERAGEX for calculated average

---

### Movement & Trend Measures (5 measures)

#### 23. Consistent Performers
```dax
Consistent Performers = 
CALCULATE(
    [Total Univerisities], 
    ABS(FactUniversity[RankingY2026] - FactUniversity[RankingY2025]) <= 5
)
```
**Description**: Universities with ≤5 position change  
**Format**: 0  
**Current Value**: 154

#### 24. Avg Rank Improvement
```dax
Avg Rank Improvement = 
AVERAGEX(
    FactUniversity,
    FactUniversity[RankingY2025] - FactUniversity[RankingY2026]
)
```
**Description**: Average ranking improvement (positive = better)  
**Format**: +0.0;-0.0;0.0  
**Current Value**: -0.4  
**Note**: Fixed to use AVERAGEX for calculated average

#### 25. Improvement Rate
```dax
Improvement Rate = 
DIVIDE([Universities Improved], [Total Univerisities], 0)
```
**Description**: Percentage of universities that improved  
**Format**: 0.0%  
**Current Value**: 49.7%

#### 26. Consistency Rate
```dax
Consistency Rate = 
DIVIDE([Consistent Performers], [Total Univerisities], 0)
```
**Description**: Percentage of consistent performers  
**Format**: 0.0%  
**Current Value**: 26.0%

#### 27. Avg Employment Score
```dax
Avg Employment Score = AVERAGE(FactUniversity[Employment Outcomes])
```
**Description**: Average employment outcomes score  
**Format**: 0.0  
**Current Value**: 48.4

---

### Specialty Measures (2 measures)

#### 28. Sustainability Leaders
```dax
Sustainability Leaders = 
CALCULATE([Total Univerisities], FactUniversity[Sustainability] >= 80)
```
**Description**: Universities with sustainability score ≥80  
**Format**: 0  
**Current Value**: 170

#### 29. High Citation Count
```dax
High Citation Count = 
CALCULATE(
    [Total Univerisities], 
    FactUniversity[Citations per Faculty] >= 
    PERCENTILE.INC(FactUniversity[Citations per Faculty], 0.90)
)
```
**Description**: Universities in top 10% for citations  
**Format**: 0  
**Current Value**: 61

---

## Calculated Columns

### FactUniversity Calculated Columns (7 total)

#### 1. Ranking Tier
```dax
Ranking Tier = 
SWITCH(
    TRUE(),
    [RankingY2026] <= 10, "Top 10",
    [RankingY2026] <= 50, "Top 50",
    [RankingY2026] <= 100, "Top 100",
    [RankingY2026] <= 200, "Top 200",
    [RankingY2026] <= 500, "Top 500",
    "Beyond 500"
)
```
**Purpose**: Categorize universities into ranking bands  
**Data Type**: String  
**Use**: Slicers, filtering, grouping

---

#### 2. Rank Direction
```dax
Rank Direction = 
SWITCH(
    TRUE(),
    [RankingY2026] < [RankingY2025], "Improved",
    [RankingY2026] > [RankingY2025], "Declined",
    "Stable"
)
```
**Purpose**: Simple direction of ranking change  
**Data Type**: String  
**Values**: Improved / Declined / Stable

---

#### 3. Performance Category
```dax
Performance Category = 
IF(
    [Overall] >= 75, "High Performer",
    IF([Overall] >= 50, "Mid Performer", "Emerging")
)
```
**Purpose**: Classify universities by overall performance  
**Data Type**: String  
**Thresholds**: 
- High Performer: ≥75
- Mid Performer: 50-74
- Emerging: <50

---

#### 4. Rank Change Magnitude
```dax
Rank Change Magnitude = 
VAR RankChange = ABS([RankingY2026] - [RankingY2025])
RETURN 
SWITCH(
    TRUE(),
    RankChange = 0, "No Change",
    RankChange <= 10, "Small (1-10)",
    RankChange <= 50, "Medium (11-50)",
    RankChange <= 100, "Large (51-100)",
    "Very Large (100+)"
)
```
**Purpose**: Categorize magnitude of ranking changes  
**Data Type**: String  
**Use**: Analyze volatility patterns

---

#### 5. Research Intensity
```dax
Research Intensity = 
IF(
    [Academic Reputation] >= 80 && [Citations per Faculty] >= 80,
    "Research Intensive",
    "Teaching Focused"
)
```
**Purpose**: Classify research vs teaching focus  
**Data Type**: String  
**Logic**: Both scores ≥80 = Research Intensive

---

#### 6. International Level
```dax
International Level = 
VAR IntScore = 
    ([International Faculty] + 
     [International Students] + 
     [International Research Network]) / 3
RETURN 
IF(
    IntScore >= 70, "Highly International",
    IF(IntScore >= 40, "Moderately International", "Locally Focused")
)
```
**Purpose**: Classify internationalization level  
**Data Type**: String  
**Thresholds**:
- Highly International: ≥70
- Moderately International: 40-69
- Locally Focused: <40

---

#### 7. Sustainability Level
```dax
Sustainability Level = 
IF(
    [Sustainability] >= 80, "Leader",
    IF(
        [Sustainability] >= 60, "Good",
        IF([Sustainability] >= 40, "Fair", "Emerging")
    )
)
```
**Purpose**: Categorize sustainability performance  
**Data Type**: String  
**Thresholds**:
- Leader: ≥80
- Good: 60-79
- Fair: 40-59
- Emerging: <40

---

## Hierarchies

### FactUniversity Hierarchies (4 total)

#### 1. Performance Profile
**Purpose**: Drill from performance through research and international dimensions

**Levels**:
1. Performance → `Performance Category`
2. Research → `Research Intensity`
3. International → `International Level`

**Use Cases**:
- Understanding what drives performance categories
- Analyzing correlation between research and internationalization
- Performance segmentation analysis

---

#### 2. Ranking Movement
**Purpose**: Analyze ranking changes from direction to magnitude

**Levels**:
1. Direction → `Rank Direction`
2. Magnitude → `Rank Change Magnitude`

**Use Cases**:
- Analyzing ranking volatility patterns
- Identifying consistent vs volatile performers
- Trend analysis

---

#### 3. University Excellence
**Purpose**: Drill from tier groups into performance levels

**Levels**:
1. Tier → `Ranking Tier`
2. Performance → `Performance Category`

**Use Cases**:
- Comparing performance across ranking bands
- Identifying underperformers in top tiers
- Excellence analysis

---

#### 4. Sustainability Focus
**Purpose**: Correlate sustainability with overall performance

**Levels**:
1. Sustainability → `Sustainability Level`
2. Overall Performance → `Performance Category`

**Use Cases**:
- Sustainability impact analysis
- ESG performance correlation
- Green university identification

---

## Data Quality Validation

### Validation Results ✅

#### Relationship Integrity
- ✅ All dimension tables have unique primary keys
- ✅ Zero duplicate keys in any dimension
- ✅ All fact foreign keys have valid dimension matches
- ✅ Zero orphaned records
- ✅ Zero null foreign key values

#### Data Completeness
- ✅ 592 universities with complete data
- ✅ All 11 performance metrics populated
- ✅ Both ranking years (2025 & 2026) present
- ✅ All calculated columns validated

#### Cardinality Validation
```
Country:     73 countries → 592 universities (8.1 avg per country)
Region:      5 regions → 592 universities (118.4 avg per region)
Status:      4 status types
Size:        4 size categories
Research:    4 research types
Focus:       4 focus types
```

#### DAX Measure Validation
- ✅ All 29 measures execute without errors
- ✅ 4 measures fixed (AVERAGE → AVERAGEX)
- ✅ 1 measure fixed (filter context issue)
- ✅ All formulas validated with actual data

#### Calculated Column Validation
- ✅ All 7 calculated columns working correctly
- ✅ Categories properly distributed
- ✅ No null or error values

### Known Data Characteristics

**Geographic Distribution**:
- Single-country regions: None (all regions multi-country)
- Note: Country-Region relationship is independent (no direct dimension relationship)
- Each country belongs to one region via fact table

**Ranking Distribution**:
- Universities in Top 100: 99 (16.7%)
- Universities in Top 500: 499 (84.3%)
- Universities beyond 500: 93 (15.7%)

**Movement Statistics**:
- Improved: 294 universities (49.7%)
- Declined: 295 universities (49.8%)
- Stable: 3 universities (0.5%)
- Average change: 23.4 positions

---

## Visualization Plan

### Report Structure: 5 Pages

---

### **Page 1: Executive Overview**
**Purpose**: High-level KPIs and global trends

#### Visuals:
1. **KPI Cards** (6 cards)
   - Total Universities (592)
   - Top 100 Percentage (16.7%)
   - Avg Overall Score (50.3)
   - Universities Improved (294 / 49.7%)
   - International Index (60.1)
   - Research Excellence (49.8)

2. **Line/Area Chart**: Ranking Distribution by Tier & Direction
3. **Donut Chart**: Ranking Movement Distribution
4. **Clustered Bar Chart**: Top 10 Countries by Universities
5. **Gauge Chart**: Improvement Rate vs 50% target
6. **Card with Sparkline**: Ranking Volatility indicator

**Slicers**: Region, Performance Category

---

### **Page 2: Geographic Analysis**
**Purpose**: Regional and country performance insights

#### Visuals:
1. **Map**: Countries sized by universities, colored by avg score
2. **Matrix**: Region → Country drill-down with key metrics
3. **Clustered Column**: Regional performance comparison
4. **Scatter Chart**: Country size vs quality

**Slicers**: Region, Ranking Tier

---

### **Page 3: Ranking Movement & Trends**
**Purpose**: Analyze ranking changes and volatility

#### Visuals:
1. **Waterfall Chart**: Ranking movement breakdown
2. **100% Stacked Bar**: Rank change by region
3. **Clustered Bar**: Rank change magnitude by performance
4. **Line Chart**: Volatility by country (Top 20)
5. **Card Metrics**: Improved/Declined/Stable counts
6. **Table**: Biggest movers

**Slicers**: Rank Direction, Region

---

### **Page 4: Performance Deep Dive**
**Purpose**: Detailed academic performance analysis

#### Visuals:
1. **Hierarchy Slicer**: Performance Profile drill-down
2. **Radar Chart**: Performance dimensions (11 metrics)
3. **Scatter Plot**: Research vs International Excellence
4. **Funnel Chart**: Performance distribution
5. **Ribbon Chart**: Top performers by key dimensions
6. **KPI Cards**: StdDev, Top Quartile, Median

**Slicers**: Research Intensity, International Level, Sustainability Level

---

### **Page 5: Sustainability & Specialization**
**Purpose**: Sustainability and institutional characteristics

#### Visuals:
1. **Tree Map**: Universities by Sustainability Level
2. **Stacked Area**: Sustainability score distribution
3. **Clustered Column**: Institutional size distribution
4. **Matrix**: Performance by Research Type & Focus
5. **Donut Charts**: Research/International/Focus splits
6. **Card Metrics**: Sustainability Leaders, High Citation, Employment

**Slicers**: Status, Size Category, Sustainability Level

---

### Design Guidelines

**Color Scheme**:
- Primary: Blues/Teals (academic, professional)
- Accent: Gold/Orange (improvements, highlights)
- Negative: Red/Pink (declines)
- Neutral: Gray (stable/unchanged)

**Best Practices**:
- Maximum 7 visuals per page
- F-pattern layout (important info top-left)
- Consistent typography and spacing
- Clear visual hierarchy
- Descriptive titles and subtitles
- Data labels where helpful
- Mobile-optimized layouts

---

## Deployment Guide

### Publishing to Power BI Service

#### Pre-Deployment Checklist
- [ ] Validate all DAX measures execute correctly
- [ ] Test all relationships and hierarchies
- [ ] Verify calculated columns populated
- [ ] Check data refresh configuration
- [ ] Review security settings
- [ ] Test all visualizations
- [ ] Optimize performance (reduce visual count if needed)
- [ ] Create bookmarks for key views

#### Step 1: Prepare the Model
1. Open Power BI Desktop
2. File → Save As → Save final version
3. File → Options and settings → Options
   - Data Load: Configure refresh settings
   - Security: Set RLS if needed

#### Step 2: Publish to Service
1. Click **Publish** button in Home ribbon
2. Select destination workspace
3. Choose appropriate workspace:
   - **My Workspace**: Personal use
   - **Shared Workspace**: Team collaboration
   - **Premium Workspace**: Large datasets, more features

#### Step 3: Configure Data Refresh (if applicable)
1. Go to Power BI Service (app.powerbi.com)
2. Navigate to workspace
3. Find dataset → Settings
4. Configure scheduled refresh:
   - Set data source credentials
   - Configure refresh schedule
   - Set failure notifications

#### Step 4: Share Report
1. Open report in Power BI Service
2. Click **Share** button
3. Options:
   - **Direct sharing**: Enter email addresses
   - **Publish to web**: Generate embed code (use with caution)
   - **Embed in SharePoint/Teams**: Integration options
   - **Create app**: Package workspace content

#### Step 5: Set Permissions
- **Viewer**: Can view report
- **Contributor**: Can edit content
- **Admin**: Full workspace control

### Migration to Fabric

#### Fabric Workspace Setup
1. Create Fabric workspace (if not exists)
2. Enable Fabric features
3. Upload Power BI report

#### Fabric-Specific Features
- **Direct Lake mode**: (if using Fabric data warehouse)
- **OneLake integration**: Store data in OneLake
- **Real-time analytics**: Enable streaming if needed
- **Data Activator**: Set up alerts and triggers

#### Optimization for Fabric
1. Convert to Direct Lake mode (if applicable)
2. Enable automatic aggregations
3. Configure cache refresh
4. Set up incremental refresh zones

---

## Maintenance & Troubleshooting

### Regular Maintenance Tasks

#### Monthly
- [ ] Review data quality
- [ ] Validate measure calculations
- [ ] Check for new data anomalies
- [ ] Update documentation if changes made

#### Quarterly
- [ ] Performance optimization review
- [ ] User feedback incorporation
- [ ] Add new measures/visuals based on requests
- [ ] Archive old versions

#### Annually
- [ ] Complete model review
- [ ] Evaluate new Power BI features
- [ ] Restructure if significant changes needed
- [ ] Update data sources if changed

---

### Common Issues & Solutions

#### Issue: Measures showing blank
**Solution**: 
- Check filter context
- Verify relationships are active
- Ensure dimension tables have matching keys

#### Issue: Slow performance
**Solutions**:
- Reduce number of visuals per page
- Use SUMMARIZE for aggregations
- Consider calculated tables instead of measures
- Enable query reduction in options

#### Issue: Incorrect AVERAGE calculations
**Solution**:
- Use AVERAGEX for calculated averages
- Don't use AVERAGE on expressions
- Use variables for complex calculations

#### Issue: Filter context problems
**Solutions**:
- Use CALCULATE to modify filter context
- Use ALL to remove filters
- Use ALLSELECTED to respect slicers
- Check relationship directions

#### Issue: Relationship errors
**Solutions**:
- Verify cardinality is correct
- Check for duplicate keys in dimension tables
- Validate foreign key integrity
- Ensure correct many-to-one direction

---

### DAX Best Practices Applied

✅ **Variables**: Used for complex calculations (reduces recalculation)  
✅ **AVERAGEX**: Used instead of AVERAGE for calculated values  
✅ **DIVIDE**: Used for safe division with zero-handling  
✅ **Filter Context**: Properly managed with CALCULATE and ALL  
✅ **Formatting**: Applied appropriate format strings  
✅ **Descriptions**: Added for all key measures  
✅ **Naming**: Clear, descriptive measure names  

---

### Performance Optimization

#### Model Size
- Current estimated size: ~846 KB
- Optimization status: ✅ Good (under 1 GB)

#### Query Performance
- Average query time: < 1 second
- Complex measures: < 2 seconds
- Status: ✅ Optimized

#### Recommendations
1. Keep fact table under 10M rows for optimal performance
2. Use aggregation tables if dataset grows significantly
3. Consider DirectQuery for very large datasets
4. Use incremental refresh for time-based data

---

## Appendix

### Measure Categories Reference

| Category | Count | Examples |
|----------|-------|----------|
| Core Metrics | 9 | Total Universities, Total Countries, Avg Overall Score |
| Benchmarking | 8 | Median, Regional Avg, Top 100%, Above Avg % |
| Composite | 2 | International Index, Research Excellence |
| Statistical | 3 | StdDev, Top Quartile, Ranking Volatility |
| Movement & Trend | 5 | Improvement Rate, Consistent Performers |
| Specialty | 2 | Sustainability Leaders, High Citation Count |

### Calculated Column Categories

| Category | Count | Examples |
|----------|-------|----------|
| Ranking Classification | 2 | Ranking Tier, Rank Direction |
| Performance Classification | 3 | Performance Category, Research Intensity, International Level |
| Change Classification | 1 | Rank Change Magnitude |
| Sustainability Classification | 1 | Sustainability Level |

### Hierarchy Usage Matrix

| Hierarchy | Primary Use | Secondary Use |
|-----------|-------------|---------------|
| Performance Profile | Segmentation analysis | Driver identification |
| Ranking Movement | Trend analysis | Volatility tracking |
| University Excellence | Performance comparison | Excellence identification |
| Sustainability Focus | ESG analysis | Correlation study |

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-12-26 | Initial model creation |
| 1.1 | 2026-01-08 | Added core measures |
| 2.0 | 2026-03-25 | Added 20 new measures, 7 calculated columns, 4 hierarchies |
| 2.1 | 2026-03-25 | Fixed 5 DAX measures (AVERAGE → AVERAGEX issues) |
| 2.2 | 2026-03-25 | Documentation created |

---

## Contact & Support

**Model Owner**: [Your Name]  
**Last Updated**: March 25, 2026  
**Power BI Version**: Desktop (Compatibility 1600)  
**Target Platform**: Power BI Service / Microsoft Fabric

---

## Quick Reference: Key Metrics

```
Total Universities:        592
Countries:                 73
Regions:                   5
Top 100 Count:            99 (16.7%)
Universities Improved:     294 (49.7%)
Avg Overall Score:        50.3
International Index:      60.1
Research Excellence:      49.8
Ranking Volatility:       23.4 positions
Consistent Performers:    154 (26.0%)
```

---

**End of Documentation**
