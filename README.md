# Weather Impact on Flight Operations — Power BI Dashboard

**Author:** Suraj Dhondiyal
**Contact:** Surajdhondiyal1@gmail.com · [LinkedIn](https://www.linkedin.com/in/suraj-dhondiyal-a390532a6/)



## Project Overview

I built an interactive Power BI dashboard that analyzes how the weather affects commercial flight operations. The dashboard quantifies the scale of weather impact (delays, cancellations, no-shows) and measures its effect on punctuality and passenger satisfaction. The goal is to make operational trends and passenger behavior visible so stakeholders can prioritize mitigation and communications.

**Dataset:** \~10,000 flight records (CSV) with columns such as `Flight_ID`, `Departure_Time`, `Flight_Status`, `Delay_Minutes`, `Weather_Impact_Label`, `Flight_Satisfaction_Score`, `Travel_Purpose`, and `Seat_Class`.


## What I did (step‑by‑step — from my side)

1. **Data ingestion & validation**

   * Imported the `Flights_Dataset.csv` into Power BI Desktop.
   * Validated column formats and checked for missing/erroneous values.

2. **Data cleaning & type conversion**

   * Converted `Departure_Time` from `Text` to `Date/Time` to enable time-based aggregations.
   * Standardized categorical fields (`Flight_Status`, `Weather_Impact_Label`, `Seat_Class`, `Travel_Purpose`).
   * Created a binary `No_Show_Flag` and numeric `Delay_Minutes` where necessary.

3. **Dimensional modelling**

   * Created a dedicated **Date** table (calendar) with Year, Month Number, Month Name, Quarter and Month-Year columns.
   * Built relationships: `Dates[Date]` → `Flights_Dataset[Departure_Time]` (1:\*).
   * Ensured `Month Name` is sorted by `Month Number` for correct chronological ordering.

4. **Feature engineering**

   * Added calculated columns: `Year`, `Month Number`, `Month Name`, `Quarter`, and `Day` from `Departure_Time` for flexible slicing.

5. **DAX measures (key metrics)**

   * Implemented reusable measures for KPIs and ratios (see `docs/metrics_dax.md` for full code). Examples:

 DAX
Total Flights = COUNTROWS('Flights_Dataset')

Weather Impact Flights =
CALCULATE([Total Flights], 'Flights_Dataset'[Weather_Impact_Label] = "Weather Impact")

Average Delay (Weather) =
CALCULATE( AVERAGE('Flights_Dataset'[Delay_Minutes]), 'Flights_Dataset'[Weather_Impact_Label] = "Weather Impact")

Weather Impact On-Time Rate =
DIVIDE(CALCULATE([Total Flights], 'Flights_Dataset'[Flight_Status] = "On-time", 'Flights_Dataset'[Weather_Impact_Label] = "Weather Impact"), [Weather Impact Flights])



6. **Visual design & layout**

   * Built a clean, single-page report with a large background visual (airplane image) and grouped KPI cards on the left.
   * KPIs include Total Flights, Weather Impact count, Weather Delay (avg), Weather Impact On-Time Rate, Weather-related Cancellations, Weather Impact No-Show Rate.
   * Used a muted theme with accent colors to highlight weather-affected vs unaffected series.

7. **Key visuals created**

   * **Flight Delay Over Time (Area/Line chart):** Compares no-weather vs weather-affected average delay across months to reveal seasonal trends.
   * **Average Satisfaction by Weather Condition (Clustered Column):** Shows average passenger satisfaction for weather-affected vs unaffected flights.
   * **Cancellations in Bad Weather (Donut Chart):** Share of flights cancelled due to weather.
   * **No-Show Count by Weather & Travel Purpose (Clustered Column):** Breaks down no-shows by travel purpose (Business, Family, Emergency, Leisure) and weather label.
   * **Passenger Class Slicer (Seat_Class):** Dynamic slicer allowing users to filter the entire dashboard by Economy, Business, First, and Premium classes.

8. **Interactivity & usability**

   * Set up cross-filtering between visuals and synchronize the passenger class slicer across all visuals.
   * Added informative tooltips for deeper context on hover.

9. **Export & documentation**

   * Documented all DAX, measures, and the Date table in `docs/metrics_dax.md`.
   * Created a data dictionary in `docs/data_dictionary.md` explaining all columns used.



## Key Findings (examples from analysis)

* **Weather increases average delay** (weather-affected flights show higher mean delay vs unaffected flights).
* **On-time performance drops under bad weather** — On‑Time Rate for weather-impacted flights is substantially lower.
* **Passenger satisfaction is slightly lower** on weather-affected flights compared to no-impact flights.
* **Cancellations and no-shows are higher** for certain travel purposes (e.g., leisure/family) during bad weather windows.

These insights can help operations and customer service teams plan staffing, optimize communications, and prioritize schedule buffers during high-risk periods.


## How to open and use

1. Clone the repository or download the files.
2. Open `Flight_Weather_Dashboard.pbix` in **Power BI Desktop**.
3. If needed, update the CSV data source path to `data/Flights_Dataset.csv` (Transform Data → Data source settings).
4. Refresh the report to recalculate measures and visuals.
5. Use the **Passenger Class** slicer to filter the dashboard view.


## Files included in this repo

* `Flight_Weather_Dashboard.pbix` — Power BI report (add your PBIX here)
* `data/Flights_Dataset.csv` — dataset used for the analysis
* `images/` — screenshots used in README
* `docs/metrics_dax.md` — detailed DAX and Date table code
* `docs/data_dictionary.md` — column definitions and data types
* `README.md` — this file
* `LICENSE` — MIT license

## Next steps (suggested improvements)

* Add a **Weather Severity** score (light/moderate/severe) to quantify impact more precisely.
* Integrate external weather APIs for richer context (wind, visibility, storms).
* Build a predictive model to estimate delay probability given weather forecast and route characteristics.
* Publish to Power BI Service and enable scheduled refresh to keep data up to date.

## A note from me

This dashboard is my end-to-end Power BI implementation: data prep, modeling, DAX, visual build, and storytelling. Replace the author/contact fields above with your details, and feel free to edit any visuals or DAX snippets to match your dataset column names.


**Made with Power BI** ✈️
