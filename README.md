Production-Ready Repository Documentation: UK Road Safety Analytics System
Project Team: Data Pioneers (DEPI R4)
Contributors: Abdallah Ahmed, Yousef Elhwehy, Basel Mohamed, Moaz Elkersh
Tech Stack: PostgreSQL, Power BI, VS Code DB Client
1. Project Planning
Project Overview & Problem Statement
Every year, thousands of lives are severely impacted by road traffic accidents across the United Kingdom. Behind the sprawling tabular registries maintained by emergency services lies an intricate web of hidden patterns, behavioral correlations, and infrastructure systemic failures.
This project addresses the operational challenge faced by public safety authorities and urban planning committees: transforming historical, flat-file accident logs into high-throughput relational data architectures. By establishing an enterprise-grade analytics infrastructure, this system uncovers critical environmental and temporal risk factors. The final system serves as a scalable diagnostic tool designed to optimize law enforcement deployment, evaluate infrastructure modifications, and ultimately implement data-driven policies that save lives.
Objectives & Scope
 * Database Normalization & Optimization: Refactor flat-file legacy source datasets into a highly indexed relational database to mitigate update anomalies and optimize query speeds.
 * Data Quality Assurance: Enforce strict technical type casting, primary/foreign key relationships, and data-imputation techniques directly within the database layer to achieve clean datasets.
 * Enterprise Dimensional Modeling: Design and implement a multi-fact Star Schema that maximizes business intelligence indexing and cross-filtering efficiency.
 * Interactive BI Deployment: Construct a high-fidelity, multi-page executive dashboard detailing micro-level infrastructure correlations and macro-level spatial/temporal trends.
Out of Scope
 * Real-time streaming ingestion pipelines (historical data analysis focus).
 * Predictive machine learning models for real-time accident forecasting.
 * Automated automated alerts or notifications for emergency responses.
Key Deliverables & Milestones
├── Phase 1: E-R Blueprinting & DDL Implementation (Week 1)
│   └── Relational model mapping, strict schema definitions, and type enforcement.
├── Phase 2: Ingestion, Imputation & SQL Validation (Week 2)
│   └── Ingestion of records, zero-NULL metric validation via PostgreSQL queries.
├── Phase 3: Star Schema Generation & DAX Modeling (Week 3)
│   └── Fact/Dimension key linking and deployment of semantic performance expressions.
└── Phase 4: Full Multi-Tab UX/UI Dashboard Deployment (Week 4)
    └── Dynamic visualizations, executive summaries, and cross-filter validation.

2. Stakeholder Analysis
Target Audience / Stakeholders
 * Department for Transport Executives: High-level strategic decision-makers requiring top-line metrics to allocate financial budgets and track macro-level national safety trends over multi-year cycles.
 * Civil & Infrastructure Engineers: Deep analytical specialists focused on structural risk evaluation, analyzing combinations of lighting, road structures, and surface properties to prioritize physical road safety investments.
 * Traffic Operations & Law Enforcement Managers: Regional tactical teams utilizing temporal distribution data to coordinate highway patrol assignments and optimize active traffic monitoring schedules.
User Personas & Requirements
Persona A: Executive Safety Director
> Primary Business Question: What is our nationwide benchmark across key metrics, and where are structural safety initiatives succeeding or falling short?
> 
 * Pain Points: Overwhelmed by massive unorganized flat tables; lacks actionable, dynamic visual representations to present directly to budget committees.
 * Functional Requirement: A single overview tab hosting high-level KPI blocks (Total Accidents, Fatalities, and Casualties) supported by simple filtering to rapidly monitor macro-trends.
Persona B: Infrastructure Risk Analyst
> Primary Business Question: Which specific road structural designs interact poorly with hazardous weather conditions, creating high-lethality risk zones?
> 
 * Pain Points: Inability to efficiently evaluate cross-tabulated variables (e.g., weather types against specific road formats) without running intensive database scripts.
 * Functional Requirement: Comprehensive environment-tracking matrices and cross-filtering visual components allowing deep-dive infrastructure analysis.
Success Metrics (KPIs)
The success of the deployment is benchmarked against both technical performance constraints and enterprise analytics targets:
 * Database Execution Latency: Analytical aggregation queries must complete in under 2.0 seconds directly within the SQL environment.
 * Data Integrity Metric: Critical analytical columns must achieve exactly 0% NULL values across all dimensions following the extraction, transformation, and loading (ETL) pipeline.
 * Core Analytic Baselines Formulated:
   * Total Accidents Monitored: Exactly 2M records analyzed.
   * Total Casualties Monitored: Exactly 3M records tracked.
   * System-wide National Fatality Rate: Calculated accurately at 2.40%.
   * Average Severity Load: Stabilized calculation tracking 1.35 casualties per individual incident.
   * Annual Historical Volume Baseline: Normalized processing at 157.48K accidents per year.
3. Database Design
Data Architecture & Modeling
To transition away from slow, flat-file processing that frequently results in structural modification errors, the system utilizes a robust Star Schema architectural pattern.
The database architecture decouples events into two unique operational resolutions:
 * Incident Resolution Layer: Captures the localized metadata surrounding the single event instance via public.fact_accidents.
 * Participant Resolution Layer: Identifies the technical characteristics of every individual asset or driver involved via public.fact_vehicles.
These dual core transactional tables connect outward to structured dimension layers (dim_weather, dim_roadtype, dim_severity, etc.) using established primary and foreign key constraints. This schema layout decreases file footprint requirements, maintains absolute clean integrity constraints, and ensures top-tier cross-filtering computational speeds within the visualization layer.
Entity-Relationship (ER) Overview
                  +-----------------------+          +-----------------------+
                  |     dim_severity      |          |       dim_make        |
                  +-----------------------+          +-----------------------+
                  | PK | severity_id      |          | PK | make_id          |
                  |    | description      |          |    | description      |
                  +----------+------------+          +-----------+-----------+
                             |                                   |
                             | 1                                 | 1
                             |                                   |
                             | *                                 | *
+---------------+ *  1 +-----+--------------+  1     * +---------+-----------+
|   dim_light   +------+ public.fact_accidents+--------+ public.fact_vehicles|
+---------------+      +-----+--------------+          +-----------------------+
| PK | light_id |      | PK  |事故Index     |          | PK | Accident_Index   |
|    | desc     |      | FK  |severity_id   |          | FK | make_id          |
+---------------+      | FK  |light_id      |          |    | Age_of_Vehicle   |
                       | FK  |day_id        |          |    | Age_Band_Driver  |
+---------------+      | FK  |road_type_id  |          +-----------------------+
|   dim_days    |      | FK  |weather_id    |
+---------------+      |     | Date         |
| PK | day_id   |      +-----+------+-------+
|    | desc     |            |      |
+---------------+          * |      | *
                             |      |
                    1        |      | 1
       +---------------------+      +---------------------+
       |    dim_roadtype     |      |     dim_weather     |
       +---------------------+      +---------------------+
       | PK | road_type_id   |      | PK | weather_id     |
       |    | description    |      |    | description    |
       +---------------------+      +---------------------+

Data Dictionary
1. Table Name: public.fact_accidents
| Column Name | Data Type | Key Type | Constraint / Rationale | Description |
|---|---|---|---|---|
| Accident_Index | VARCHAR(50) | Primary Key | Unique, Indexed | Enterprise system-wide unique identifier for a specific accident incident. |
| severity_id | INT | Foreign Key | Linked to dim_severity | Categorical level of technical severity (Fatal, Serious, Slight). |
| light_id | INT | Foreign Key | Linked to dim_light | Structural code representing lightning status (Daylight, Dark - lights lit, etc.). |
| day_id | INT | Foreign Key | Linked to dim_days | Tracks the day of week index for granular seasonal metrics. |
| road_type_id | INT | Foreign Key | Linked to dim_roadtype | Structural asset classification (Single Carriageway, Roundabout, etc.). |
| weather_id | INT | Foreign Key | Linked to dim_weather | Environmental meteorological code during active event logging. |
| Date | DATE | Attribute | Correct Format Input | Explicit structural date value mapping. |
| Time | TIME | Attribute | Precise Logging | Identifies time value mapping across 24-hour cycles. |
2. Table Name: public.fact_vehicles
| Column Name | Data Type | Key Type | Constraint / Rationale | Description |
|---|---|---|---|---|
| Accident_Index | VARCHAR(50) | Foreign Key | Linked to fact_accidents | Transactional structural bridge parameter tracing back to main accident. |
| make_id | INT | Foreign Key | Linked to dim_make | Numerical dimension map identifying mechanical vehicle origins. |
| Age_of_Vehicle | INT | Attribute | Checked Validated Range | Structural runtime age parameter of asset involved in years. |
| Age_Band_of_Driver | VARCHAR(20) | Attribute | Binned String | Grouped demographical band assignment for target driver (e.g., 26-35). |
| Sex_of_Driver | VARCHAR(10) | Attribute | Explicitly Cleaned | Demographic classification tracking male or female operators. |
4. UI/UX & Dashboard Design
User Flow & Navigation
The end-user application interface operates on an exhaustive 3-tier layout architecture structured by thematic user intent:
[Main Entry Point: Corporate Executive Hub]
       │
       ├──► Tab 1: Road Accidents: Time & Trends Analysis[span_68](start_span)[span_68](end_span)
       │           (Temporal pattern discovery & top-level executive KPIs)
       │
       ├──► Tab 2: Environment & Infrastructure Analysis[span_69](start_span)[span_69](end_span)
       │           (Deep cross-tabulation of asset formats x physical hazards)
       │
       └──► Tab 3: Road Infrastructure & Geographic Insights[span_70](start_span)[span_70](end_span)
                   (Geospatial hotspot discovery & granular demographics)

Global Slicer Engine: Positioned directly along the top global navigation ribbon, synced filter modules (Year, Severity, Area Type, Weather) persist states uniformly across tabs, minimizing repetitive filtering interactions.
Wireframe Layout & Visual Hierarchy
Tab 1: Road Accidents: Time & Trends Analysis
Designed for macro-level tactical management tracking hourly and weekly seasonal trends:
+------------------------------------------------------------------------------------+
| [Logo] Road Accidents: Time & Trends Analysis   [Slicers: Year | Severity | Weather] |
+------------------------------------------------------------------------------------+
| [ 2M ] Total  | [ 3M ] Total  | [ 49K ] Total | [ 26K ] Fatal | [ 286K ] Serious   |
|   Accidents   |  Casualties   |  Fatalities   |   Accidents   |    Accidents       |
+-----------------------------------------------+------------------------------------+
|                                               |                                    |
| [Visual A: Hourly Traffic Accident Trends]     | [Visual B: Daily Volume Breakdown] |
| - High-density area continuous spline chart.  | - Sorted vertical column display.  |
| - Morning Peak: 8:00 AM - 9:00 AM[span_74](start_span)[span_74](end_span)   | - Identifies Friday as highest     |
| - Afternoon Peak: 4:00 PM - 6:00 PM[span_75](start_span)[span_75](end_span) |   operational risk day (>0.3M)     |
|                                               |[span_76](start_span)[span_76](end_span).                       |
|                                               |                                    |
+-----------------------------------------------+------------------------------------+
| [Visual C: Weekly Distribution Patterns]      | [Visual D: Monthly Seasonal Trend] |
| - Multi-segment donut plot tracking relative  | - Linear continuous trend mapping. |
|   percentage load per weekday[span_77](start_span)[span_77](end_span).       | - Tracks systemic crash variations |
| - Friday leads at 16.37%[span_78](start_span)[span_78](end_span).           |   across seasons[span_79](start_span)[span_79](end_span).        |
+-----------------------------------------------+------------------------------------+

Tab 2: Environment & Infrastructure Analysis
Focuses on technical interactions between road infrastructure assets and dynamic environmental variables:
+------------------------------------------------------------------------------------+
| Environment & Infrastructure Analysis           [Slicers: Year | Severity | Weather] |
+-----------------------------------------------+------------------------------------+
| [ 2.40% ] National Systemic   | [ 1.35 ] Casualty Load   | [ 157.48K ] Annual Run  |
|         Fatality Rate         |    per Crash Event       |      Volume Base        |
+-------------------------------+--------------------------+-------------------------+
|                                                          | [Visual F: Visibility]  |
| [Visual E: Environmental Risk Grid Matrix]               | - Donut chart.          |
| - Full cross-tabulation table containing infrastructure  | - Daylight: 73.1%       |
|   vs weather records[span_82](start_span)[span_82](end_span).                          | - Dark (lit): 19.7%     |
| - Key structural insight: Single Carriageway systems     |[span_83](start_span)[span_83](end_span).            |
|   combined with clear weather display highest volume     +-------------------------+
|   exceeding 1.2M individual logs[span_84](start_span)[span_84](end_span).              | [Visual G: Gauge]       |
|                                                          | - Speed/Severity index  |
|                                                          |   metric[span_85](start_span)[span_85](end_span).     |
+----------------------------------------------------------+-------------------------+
| [Visual H: Infrastructure Safety Volume Benchmark]                                 |
| - Extended horizontal bar plot sorting incident footprints across road parameters. |
| - Single Carriageways: 1,528K | Dual Carriageways: 303K | Slip Roads: 22K[span_86](start_span)[span_86](end_span).|
+------------------------------------------------------------------------------------+

Tab 3: Road Infrastructure & Geographic Insights
Coordinates spatial attributes with target operator demographics for precision localization:
+------------------------------------------------------------------------------------+
| Road Infrastructure & Geographic Insights       [Slicers: Year | Severity | Weather] |
+-----------------------------------------------+------------------------------------+
| [Visual I: Urban vs Rural Volumetric Split]   | [Visual J: Surface Condition Logs] |
| - Paired comparative horizontal layout.       | - Column metric chart.             |
| - Urban: 1,322.34K occurrences[span_89](start_span)[span_89](end_span).     | - Dry conditions dominate safety   |
| - Rural: 724.76K occurrences[span_90](start_span)[span_90](end_span).       |   footprint registry at 1.42M logs |
| - Note: Rural contains high lethal rates[span_91](start_span)[span_91](end_span).|[span_92](start_span)[span_92](end_span).                     |
+-----------------------------------------------+------------------------------------+
|                                               |                                    |
| [Visual K: Geospatial Hotspot Mapping Layer]  | [Visual L: Operator Demographic]  |
| - Interactive Bing Maps integration[span_93](start_span)[span_93](end_span).| - Binned vertical clustered matrix.|
| - Dense clusters localized inside major UK    | - Compares age bands split by sex. |
|   metropolitan centers[span_94](start_span)[span_94](end_span).             | - Identifies highest impact        |
|                                               |   segment: Males aged 26-35        |
|                                               |[span_95](start_span)[span_95](end_span).                       |
|                                               |                                    |
+-----------------------------------------------+------------------------------------+

Design Choices
 * Color Palette Philosophy (Corporate Clean High-Contrast):
   * Primary Visual Interface Element: Light Corporate Gray #F2F2F2 background ensures clear separation from workspace widgets, optimizing scannability without visual strain.
   * Accent Metric Identifier: Electric Blue Blue #1E90FF acts as the uniform focus color. It is applied exclusively to trend indicators, active charts, and high-priority metrics to ensure direct focal alignment.
   * Muted Baseline Indicator: Neutral Slate Gray #A9A9A9 maps baseline states, unselected variables, and standard parameters, preserving clear visual contrast.
 * Typography Hierarchy: Clean, non-serif modern system fonts applied with strict sizing constraints:
   * Executive Metric Cards: 28pt Bold.
   * Section Structural Headings: 14pt Semi-Bold.
   * Axis Labeling & Grid Notations: 9pt Regular.
 * Accessibility Implementations:
   * Contextual Tooltips: Hovering over line plots or matrix cells reveals explicit analytical details, mitigating visual density issues in clustered areas.
   * Color-Independent Labeling: Donut and bar visual components do not rely solely on color variants; they are supported by direct numeric tags and layout indicators to remain functional for colorblind users.
   * Predictable Left-to-Right Hierarchy: Core analytical parameters progress logically from broad metrics on the top left to specific diagnostic charts on the bottom right.
Technical Validation Appendix (SQL Data QA Build Sample)
-- Enterprise Quality Check: Ensure complete imputation across target operational columns
SELECT 
    (SELECT COUNT(*) FROM public.fact_vehicles WHERE "Age_of_Vehicle" IS NULL) AS missing_vehicle_age,
    (SELECT COUNT(*) FROM public.fact_vehicles WHERE "Engine_Capacity_.CC." IS NULL) AS missing_engine_cc,
    (SELECT COUNT(*) FROM public.fact_accidents WHERE "Junction_Control" IS NULL) AS missing_junction_control,
    (SELECT COUNT(*) FROM public.fact_vehicles WHERE "Driver_IMD_Decile" IS NULL) AS missing_driver_imd;
-- Verification Output: Exactly 0 records missing across all monitored parameters[span_102](start_span)[span_102](end_span).

