# 🗺️ UK Road Safety Analytics - Advanced Business Intelligence Platform

A production-ready data engineering and business intelligence infrastructure built from scratch to analyze police-reported traffic accidents across the United Kingdom. Migrating from flat CSV structures to a highly optimized PostgreSQL Star Schema, this framework cross-profiles over **2 million incidents** to locate micro-temporal risk trends, spatial population variances, and severe roadway infrastructure vulnerabilities.

## 📋 Table of Contents

* [Quick Overview](https://www.google.com/search?q=%23-quick-overview)
* [System Architecture](https://www.google.com/search?q=%23-system-architecture)
* [Database Schema](https://www.google.com/search?q=%23-database-schema)
* [Installation & Set Up Guide](https://www.google.com/search?q=%23-installation--set-up-guide)
* [Project Structure](https://www.google.com/search?q=%23-project-structure)
* [Data Analysis & Core Discovery](https://www.google.com/search?q=%23-data-analysis--core-discovery)
* [Key Performance Indicators](https://www.google.com/search?q=%23-key-performance-indicators)
* [Testing & Quality Assurance](https://www.google.com/search?q=%23-testing--quality-assurance)
* [Future Roadmap](https://www.google.com/search?q=%23-future-roadmap)
* [Team Members](https://www.google.com/search?q=%23-team-members)
* [Contributing Guidelines](https://www.google.com/search?q=%23-contributing-guidelines)
* [Version History](https://www.google.com/search?q=%23-version-history)

---

## 🚀 Quick Overview

**UK Road Safety Analytics** shifts transport monitoring away from reactive flat-file indexing into multi-dimensional dynamic business intelligence. It delivers deep clarity to urban planners and regional traffic councils through:

* ✅ Strict relational entity isolation that eliminates update anomalies.


* ✅ Granular analysis slicing at both incident and participant levels.


* ✅ Advanced temporal cross-filtering (hourly spikes vs. weekday commute shifts).


* ✅ Weather and roadway infrastructure risk matrices.


* ✅ Demographic profiling (driver gender variations across age cohorts).



### Architecture Matrix

| Architectural Layer | Project Framework Implementation | Legacy Processing |
| --- | --- | --- |
| **Data Layout** | Relational Star Schema (Fact/Dimension) 

 | Flat CSV Files 

 |
| **Update Integrity** | ✅ Safe from update anomalies 

 | ⚠️ High redundancy risks 

 |
| **Granularity Slicing** | Event level + Participant level 

 | Combined un-indexed lines 

 |
| **Coordinate Handling** | Exact `DECIMAL(10,8)` GPS formatting 

 | String/Float conversion lag 

 |
| **BI Cross-Filtering** | Optimized relational primary keys 

 | Heavy calculation runtime columns 

 |

---

## 🏗️ System Architecture

### The Technology Core

| Component | Target Framework Layer | Version/Scope |
| --- | --- | --- |
| **Database Engine** | PostgreSQL 

 | 12.0+ Server |
| **Query Framework** | Data Definition Language (DDL) / SQL Aggregations 

 | Advanced PostgreSQL |
| **BI Architecture** | Power BI Desktop 

 | Star Schema Relationship Engine 

 |
| **Mapping Engine** | Microsoft Bing Maps API 

 | Precision Geospatial Hotspots 

 |

### Quality & Performance Controls

| Optimization Type | Database Protection Method |
| --- | --- |
| **Redundancy Elimination** | Multi-dimensional dimension lookups (`dim_weather`, `dim_roadtype`).

 |
| **Scanning Index Boost** | Hardcoded `PRIMARY KEY` validation and `SERIAL` auto-indexing.

 |
| **Spatial Precision** | <br>`DECIMAL(10,8)` layout constraints preserving coordinates.

 |
| **Join Integrity** | Explicit `FOREIGN KEY` bounds maps preventing orphaned records.

 |
| **BI Processing Optimization** | Database-level preprocessing eliminates conversion lag in Power BI.

 |

---

## 🗄️ Database Schema

### Complete Relational DDL Architecture

```sql
-- 1. Dim Severity Lookups
CREATE TABLE public.dim_severity (
    severity_id SERIAL PRIMARY KEY,
    description VARCHAR(50) NOT NULL
);

-- 2. Dim Weather Configurations
CREATE TABLE public.dim_weather (
    weather_id SERIAL PRIMARY KEY,
    description VARCHAR(100) NOT NULL
);

-- 3. Dim Road Type Classifications
CREATE TABLE public.dim_roadtype (
    road_type_id SERIAL PRIMARY KEY,
    description VARCHAR(100) NOT NULL
);

-- 4. Dim Light Conditions
CREATE TABLE public.dim_light (
    light_id SERIAL PRIMARY KEY,
    description VARCHAR(100) NOT NULL
);

-- 5. Dim Day Tracking Lookups
CREATE TABLE public.dim_days (
    day_id SERIAL PRIMARY KEY,
    description VARCHAR(50) NOT NULL
);

-- 6. Dim Vehicle Make Lookups
CREATE TABLE public.dim_make (
    make_id SERIAL PRIMARY KEY,
    description VARCHAR(100) NOT NULL
);

-- 7. Core Incident Fact Table
CREATE TABLE public.fact_accidents (
    accident_index VARCHAR(50) PRIMARY KEY,
    first_road_class VARCHAR(50),
    first_road_number VARCHAR(50),
    second_road_class VARCHAR(50),
    second_road_number VARCHAR(50),
    carriageway_hazards TEXT,
    date DATE NOT NULL,
    time TIME WITHOUT TIME ZONE,
    latitude DECIMAL(10,8),
    longitude DECIMAL(10,8),
    number_of_casualties INT DEFAULT 0,
    number_of_vehicles INT DEFAULT 0,
    urban_or_rural_area VARCHAR(20),
    road_surface_conditions VARCHAR(100),
    weather_id INT,
    severity_id INT,
    road_type_id INT,
    light_id INT,
    day_id INT,
    FOREIGN KEY (weather_id) REFERENCES public.dim_weather(weather_id),
    FOREIGN KEY (severity_id) REFERENCES public.dim_severity(severity_id),
    FOREIGN KEY (road_type_id) REFERENCES public.dim_roadtype(road_type_id),
    FOREIGN KEY (light_id) REFERENCES public.dim_light(light_id),
    FOREIGN KEY (day_id) REFERENCES public.dim_days(day_id)
);

-- 8. Core Participant Vehicle Fact Table
CREATE TABLE public.fact_vehicles (
    vehicle_id SERIAL PRIMARY KEY,
    accident_index VARCHAR(50) NOT NULL,
    age_band_of_driver VARCHAR(50),
    age_of_vehicle INT,
    driver_home_area_type VARCHAR(50),
    driver_imd_decile INT,
    engine_capacity_cc INT,
    sex_of_driver VARCHAR(20),
    vehicle_manoeuvre VARCHAR(100),
    vehicle_type VARCHAR(100),
    year INT,
    make_id INT,
    FOREIGN KEY (accident_index) REFERENCES public.fact_accidents(accident_index) ON DELETE CASCADE,
    FOREIGN KEY (make_id) REFERENCES public.dim_make(make_id)
);

```

### Logical Model Topology

```
public.dim_severity  ───(1:N)───┐
public.dim_weather   ───(1:N)───┤
public.dim_roadtype  ───(1:N)───┼───> public.fact_accidents ───(1:N)───> public.fact_vehicles
public.dim_light     ───(1:N)───┤                                               │
public.dim_days      ───(1:N)───┘                                               │
                                                                                ▼
                                                                        public.dim_make (1:N)

```

---

## 🔧 Installation & Set Up Guide

### Base System Requirements

* 
**Server Core:** PostgreSQL 12.0+ Environment 


* 
**BI Interface:** Power BI Desktop Application 


* **Storage Profile:** 2GB minimum storage space for historical row execution

### Implementation Blueprint

#### 1. Repository Migration & Directory Setup

```bash
git clone https://github.com/data-pioneers/uk-road-safety-bi.git
cd uk-road-safety-bi
mkdir -p assets/dashboards/ config/sql/

```

#### 2. Local Environment Variable Mapping

Create a `.env` infrastructure file inside your root folder:

```env
DB_HOST=127.0.0.1
DB_PORT=5432
DB_NAME=uk_road_db
DB_USER=postgres
DB_PASS=your_secure_password

```

#### 3. Database Initial Setup & Schema Generation

Execute the schema creation script on your local server terminal:

```bash
psql -h 127.0.0.1 -p 5432 -U postgres -d uk_road_db -f config/sql/schema_blueprint.sql

```

#### 4. Run Verification Suite

Run the quality assurance validation parameters:

```bash
psql -h 127.0.0.1 -p 5432 -U postgres -d uk_road_db -f config/sql/quality_validation.sql

```

#### 5. Connecting the BI Application Layer

* Launch **Power BI Desktop**.


* Choose **Get Data** $\rightarrow$ **PostgreSQL Database**.
* Link to your host `127.0.0.1` using the database configuration matching step 2.


* Confirm standard **Import Mode** to keep the performance capabilities of the optimized Star Schema model intact.



---

## 📁 Project Structure

```
uk-road-safety-analytics/
│
├── config/
│   ├── sql/
│   │   ├── schema_blueprint.sql     # Database structure DDL
│   │   ├── quality_validation.sql   # Data quality checks
│   │   └── analytical_queries.sql   # Core exploratory queries
│   └── database.env                 # Core engine environment variables
│
├── assets/
│   ├── dashboards/
│   │   └── UK_Road_Safety.pbix      # Finished Power BI application file
│   └── branding/
│       └── logo.png                 # Data Pioneers group icon
│
├── documents/
│   ├── FinalPresentation.pdf        # Stakeholder report file
│   └── Project_Briefing.md          # Comprehensive summary document
│
├── README.md                        # Documentation system (This file)
└── LICENSE                          # MIT open-source license statement

```

---

## 📈 Data Analysis & Core Discovery

### 1. Carriageway Infrastructure Impact

Analysis reveals that **Single Carriageway** layouts pose a significant safety risk across the UK infrastructure.

| Infrastructure Class | Fine Weather Count | Rainy Conditions | Aggregate Incidents |
| --- | --- | --- | --- |
| **Single Carriageway** | 1,229,680 

 | 175,893 

 | 1,528K 

 |
| **Dual Carriageway** | 238,876 

 | 38,575 

 | 303K 

 |
| **Roundabout** | 109,343 

 | 16,411 

 | 137K 

 |
| **One Way Street** | 35,670 

 | 4,468 

 | 43K 

 |
| **Slip Road** | 17,147 

 | 2,635 

 | 22K 

 |

> 💡 **Key Insight:** Single Carriageway roads drive the absolute majority of traffic collisions, generating **1.528 million incidents** across the infrastructure matrix.
> 
> 

### 2. Time-of-Day Risk Shifts

Accident frequencies strongly correlate with commercial transit peaks, identifying clear risk windows during standard daily commutes:

* 
**Morning Risk Window (8:00 AM - 9:00 AM):** A distinct morning spike emerges as corporate traffic hits the road grid.


* 
**Afternoon Peak Window (4:00 PM - 6:00 PM):** The highest daily accident accumulation occurs during the afternoon return rush hour.


* 
**Night Decline Window (11:00 PM - 5:00 AM):** Incidents drop significantly, tracking lower overall late-night traffic volumes.



### 3. Environmental Slicing Parameters

The data challenges the assumption that low visibility is the primary driver of traffic accidents.

* 
**Daylight Visibility:** 73.08% of all recorded incidents occur during clear daylight conditions.


* 
**Darkness (Artificial Illumination Active):** 19.74% of incidents happen under streetlights.


* 
**Darkness (No Street Lighting):** 5.50% of incidents occur on completely unlit dark roadways.


* 
**Surface Index:** 1.42 million crashes happen on completely **Dry Surfaces**, indicating that driver behavior and traffic volumes influence incident rates more than external environmental elements.


* 
**Precipitation Impact:** Rainy conditions present a significant risk factor, with "Raining no high winds" causing **237,982 accidents**.



---

## 📊 Key Performance Indicators

The core operational safety metrics across the historical scope are established as follows:

| Safety Target Variable | Current Platform Metric Evaluation | Operational Context |
| --- | --- | --- |
| **Total Tracked Accidents** | 2,000,000 (2M) 

 | Global absolute accident volume metric.

 |
| **Total Recorded Casualties** | 3,000,000 (3M) 

 | Aggregate injuries recorded across all data rows.

 |
| **Total System Fatalities** | 49,000 (49K) 

 | Absolute life loss tracking marker.

 |
| **Inherent Fatality Rate** | 2.40% 

 | Mean fatality calculation per recorded incident.

 |
| **Casualty Density Index** | 1.35 

 | Mean casualty density per transaction.

 |
| **Annual Crash Average** | 157.48K 

 | Long-term macro year tracking mean.

 |
| **Serious Scale Injuries** | 286,000 (286K) 

 | Total high-severity non-fatal incidents.

 |
| **Fixed Object Collisions** | 84,000 (84K) 

 | Incidents involving impacts with static infrastructure. |

---

## 🧪 Testing & Quality Assurance

### Missing Data Controls

The system requires checking for incomplete variables inside the source files before deploying analytical models. The validation framework runs this verification script:

```sql
SELECT 
    (SELECT COUNT(*) FROM public.fact_accidents WHERE accident_index IS NULL) AS null_indices,
    (SELECT COUNT(*) FROM public.fact_accidents WHERE weather_id IS NULL) AS null_weather,
    (SELECT COUNT(*) FROM public.fact_vehicles WHERE sex_of_driver IS NULL) AS null_driver_gender;

```

### Expected Test Validation Outcomes

| Diagnostic Query Target | Target Result | Quality Status |
| --- | --- | --- |
| `null_indices` Check | 0 | ✅ Passed (Uniqueness Preserved) 

 |
| `null_weather` Check | 0 | ✅ Passed (Dim Connections Valid) 

 |
| `null_driver_gender` Check | 0 | ✅ Passed (Demographics Polished) 

 |

---

## 📈 Future Roadmap

### Version 2.0 (Planned - Q3 2026)

* [ ] **Predictive Severity Modeling:** Build an automated Python machine learning pipeline (Random Forest/XGBoost) directly linked to PostgreSQL to estimate real-time incident severity based on local weather inputs.
* [ ] **Asset Risk Scoring Engine:** Implement geographic risk profiling to rank local council districts by serious injury probability.

### Version 3.0 (Long-Term - 2027)

* [ ] **Telemetry Stream Ingestion:** Integrate real-time API feeds tracking active traffic cameras and highway speed sensors.
* [ ] **Economic Loss Analytics:** Develop models calculating the financial impact of road closures, vehicle damage, and emergency response deployments to optimize local authority budgeting.

---

## 👥 Team Members

| Name | Role | Email | LinkedIn |
| --- | --- | --- | --- |
| **Abdallah Ahmed** | Data Engineer | abd10esa@gmail.com | [linkedin.com/in/abdallah-ahmed]() |
| **Yousef Elhwehy** | Lead Analyst | yousef566566@gmail.com | [linkedin.com/in/yousef-elhwehy]() |
| **Basel Mohamed** | Domain Expert   | gamerbasel81@gmail.com | [linkedin.com/in/basel-mohamed]() |
| **Moaz Elkersh** | BI Developer| aelkershmoaz@gmail.com | [linkedin.com/in/moaz-elkersh]() |

---

## 👥 Contributing Guidelines

### Pull Request & Enhancement Procedures

1. **Fork the main development repository.**
2. **Launch a localized feature branch tracking changes:**
```bash
git checkout -b feature/analytical-extension-optimization

```


3. **Commit code updates utilizing strict database design logic:**
```bash
git commit -m 'Implement optimized index mapping to fact_vehicles'

```


4. **Push the polished updates up to your origin repository branch:**
```bash
git push origin feature/analytical-extension-optimization

```


5. **Open a formal Pull Request for review.**

---

## 📊 Version History

| Platform Version | Release Date | Functional Updates & Engineering Core |
| --- | --- | --- |
| **v1.0.0** | 2026-06-15 | Database initialization; migration from raw CSV to normalized relational layouts. |
| **v1.1.0** | 2026-06-30 | Implemented the Power BI reporting tier, mapping custom visuals for environmental and infrastructure profiling. |
| **v1.2.0** | 2026-07-11 | Current Release — Polished database index constraints and resolved coordinate data-type mapping quirks. |

---

## 📞 Support & Contact

| Communication Channel | Reference Target |
| --- | --- |
| **GitHub Tracker Module** | Log architectural bugs or request structural query additions. |
| **System Security Inquiries** | Forward secure engineering feedback to `security-audit@datapioneers.org`. |

---

## 🙏 Acknowledgments

* 
**UK Department for Transport:** For providing the comprehensive historical data foundation (data.gov.uk).


* **Egypt Digital Pioneers Initiative (DEPI):** For providing project engineering frameworks and professional guidance.

---

**Built with ❤️ by the Data Pioneers Team.** *Empowering regional transportation authorities with clear, actionable insights since 2026.*

---

## ⭐ Star this Repository on GitHub

If this architectural schema or dashboard layout benefits your research, please give the repository a star on GitHub!

---

**© 2026 Data Pioneers. All rights reserved.**
