# 🚦 UK Road Safety Analysis: Data-Driven Insights

## 📖 About the Project
Road traffic accidents are a major public safety concern. This project leverages historical **UK Road Safety Data** (sourced from the UK Department for Transport) to analyze accident patterns, causes, and severity.

Unlike standard exploratory projects, this analysis focuses on a **diagnostic deep dive** to understand *why* accidents happen. By merging and analyzing three massive datasets (Accidents, Vehicles, and Casualties), the project aims to provide actionable recommendations to authorities to reduce fatalities and improve road safety.

## ❓ The Business Problem
Government bodies and traffic departments often possess vast amounts of data but lack actionable insights. The core questions driving this analysis are:
* **When** do the most severe accidents occur? (Time & Seasonality analysis).
* **Where** are the country's "Black Spots"? (Geospatial analysis).
* **Who** is most at risk? (Demographic & Behavioral analysis).
* **Why** do certain conditions lead to fatal outcomes? (Correlation between weather, road surface, and speed limits).

## ⚙️ Methodology & Workflow

### 1. Data Engineering & Transformation
The raw data consisted of three separate tables with coded values. The preparation phase involved:
* **Data Merging:** Performed complex joins to combine `Accidents`, `Vehicles`, and `Casualties` tables into a single Master Dataset using `Accident_Index`.
* **Data Cleaning:** Handled missing values in critical columns (Age, Location, Time) to ensure accuracy.
* **Decoding:** Mapped numerical categorical codes (e.g., `1`, `2`) to meaningful labels (e.g., `Sunny`, `Rainy`) using lookup references.

### 2. Exploratory Data Analysis (EDA)
Conducted a descriptive analysis to identify general trends:
* Visualized accident frequency by **Hour of Day** and **Day of Week**.
* Analyzed monthly trends to detect seasonal patterns (e.g., Winter vs. Summer).

### 3. Diagnostic Deep Dive (The Core Analysis)
Went beyond the surface to find correlations and root causes:
* **Severity Analysis:** Investigated factors contributing to `Fatal` vs. `Slight` accidents.
* **Vulnerable Road Users:** Focused analysis on high-risk groups like **Motorcyclists** and **Pedestrians**.
* **Environmental Impact:** Assessed how `Road Surface Conditions` (Dry, Wet, Snow) correlate with accident rates.

### 4. Visualization & Dashboarding
Built an interactive dashboard (Tableau/Power BI) featuring:
* **Geospatial Maps:** Pinpointing high-risk accident zones.
* **Dynamic Filters:** allowing users to filter by Year, Weather, and Vehicle Type.
* **KPIs:** Tracking total casualties and severity ratios.

## 🛠️ Tools & Technologies
* **Python:** For Data Cleaning, Manipulation, and Statistical Analysis.
    * *Libraries:* Pandas, NumPy, Matplotlib, Seaborn.
* **SQL:** For querying and validating data structure.
* **Tableau / Power BI:** For data visualization and storytelling.
* **Jupyter Notebook:** For documenting the analytical process.

## 🚀 Key Findings
*(To be updated with your actual results)*
* **Example:** Fridays between 5 PM and 7 PM show the highest accident frequency due to rush hour traffic.
* **Example:** While wet roads cause many accidents, fatal accidents are surprisingly more common on dry roads due to higher driving speeds.
