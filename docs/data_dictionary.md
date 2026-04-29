## Dataset Summary

| Item | Details |
|---|---|
| Dataset name | Flight Delay and Cancellation Dataset |
| Source | Kaggle |
| Raw file name | flights_sample_3m.csv |
| Last updated | April 2026 |
| Granularity | One row per flight |

## Column Definitions

| Column Name | Data Type | Description | Example Value | Used In | Cleaning Notes |
|---|---|---|---|---|---|
| FL_DATE | date | Flight Date | 2019-01-09 | EDA / KPI / Tableau | Parse to Date type |
| AIRLINE | string | Airline Name | United Air Lines Inc. | EDA / KPI / Tableau | |
| AIRLINE_DOT | string | Airline DOT Name | United Air Lines Inc.: UA | EDA / Tableau | |
| AIRLINE_CODE | string | Airline IATA Code | UA | EDA / KPI / Tableau | |
| DOT_CODE | int | DOT Identification Code | 19977 | EDA | |
| FL_NUMBER | int/string | Flight Number | 1562 | EDA / Tableau | |
| ORIGIN | string | Origin Airport IATA Code | FLL | EDA / KPI / Tableau | |
| ORIGIN_CITY | string | Origin City Name | Fort Lauderdale, FL | EDA / Tableau | Split state/city if needed |
| DEST | string | Destination Airport IATA Code | EWR | EDA / KPI / Tableau | |
| DEST_CITY | string | Destination City Name | Newark, NJ | EDA / Tableau | Split state/city if needed |
| CRS_DEP_TIME | int | Scheduled Departure Time (local) | 1155 | EDA | Converted from HHMM int → `HH:MM` string |
| DEP_TIME | float | Actual Departure Time (local) | 1151.0 | EDA | Converted from HHMM int → `HH:MM` string; NULL for cancelled flights |
| DEP_DELAY | float | Departure Delay in Minutes | -4.0 | EDA / KPI / Tableau | Filled with 0 for non-cancelled flights only; NULL retained for cancelled |
| TAXI_OUT | float | Taxi Out Time in Minutes | 19.0 | EDA / Tableau | Nullable |
| WHEELS_OFF | float | Wheels Off Time (local) | 1210.0 | EDA | Nullable |
| WHEELS_ON | float | Wheels On Time (local) | 1443.0 | EDA | Nullable |
| TAXI_IN | float | Taxi In Time in Minutes | 4.0 | EDA / Tableau | Nullable |
| CRS_ARR_TIME | int | Scheduled Arrival Time (local) | 1501 | EDA | Converted from HHMM int → `HH:MM` string |
| ARR_TIME | float | Actual Arrival Time (local) | 1447.0 | EDA | Converted from HHMM int → `HH:MM` string; NULL for cancelled flights |
| ARR_DELAY | float | Arrival Delay in Minutes | -14.0 | EDA / KPI / Tableau | Filled with 0 for non-cancelled flights only; NULL retained for cancelled |
| CANCELLED | float | Cancelled Flight Indicator (1=Yes) | 0.0 | EDA / KPI / Tableau | Cast to int (0/1); NaN filled with 0 |
| CANCELLATION_CODE | string | Reason for Cancellation (A/B/C/D) |  | EDA / Tableau | NaN filled with `'Not Cancelled'` for operational flights |
| DIVERTED | float | Diverted Flight Indicator (1=Yes) | 0.0 | EDA / KPI / Tableau | Cast to boolean/int |
| CRS_ELAPSED_TIME | float | Scheduled Elapsed Time (mins) | 186.0 | EDA | |
| ELAPSED_TIME | float | Actual Elapsed Time (mins) | 176.0 | EDA / Tableau | Nullable |
| AIR_TIME | float | Airborne Time (mins) | 153.0 | EDA | Nullable |
| DISTANCE | float | Flight Distance (miles) | 1065.0 | EDA / KPI / Tableau | |
| DELAY_DUE_CARRIER | float | Carrier Delay in Minutes | 0.0 | EDA / KPI / Tableau | Nullable (only present if delayed >= 15m) |
| DELAY_DUE_WEATHER | float | Weather Delay in Minutes | 0.0 | EDA / KPI / Tableau | Nullable |
| DELAY_DUE_NAS | float | NAS Delay in Minutes | 24.0 | EDA / KPI / Tableau | Nullable |
| DELAY_DUE_SECURITY | float | Security Delay in Minutes | 0.0 | EDA / KPI / Tableau | Nullable |
| DELAY_DUE_LATE_AIRCRAFT | float | Late Aircraft Delay in Minutes | 0.0 | EDA / KPI / Tableau | Nullable |

## Derived Columns

All derived columns are engineered in `02_cleaning.ipynb` (STEP 9) and are present in `data/processed/cleaned_dataset.csv`.

| Derived Column | Data Type | Logic | Business Meaning |
|---|---|---|---|
| `is_delayed` | bool | `arr_delay > 15` | Binary delay flag per FAA definition (15-min threshold); excludes early/on-time flights from delay analysis |
| `delay_bucket` | string | `arr_delay ≤ 15` → `"On Time"` · `15 < arr_delay ≤ 60` → `"15-60 min"` · `arr_delay > 60` → `"60+ min"` | Three-tier categorical severity grouping for dashboarding and distribution analysis |
| `route` | string | `origin + " → " + dest` | Origin-destination pair string; enables route-level aggregation and ranking |
| `year` | int | `fl_date.dt.year` | Year extracted from flight date; enables year-over-year trend analysis |
| `month` | int | `fl_date.dt.month` | Month (1–12) extracted from flight date; captures seasonality in delays |
| `day_of_week` | string | `fl_date.dt.day_name()` | Full weekday name (e.g., `Monday`); identifies day-of-week delay patterns |
| `total_delay_cause` | float | Sum of `delay_due_carrier + delay_due_weather + delay_due_nas + delay_due_security + delay_due_late_aircraft` | Total attributed delay minutes; used to validate against `arr_delay` and identify dominant delay drivers |

## Data Quality Notes

- `DEP_TIME`, `ARR_TIME`, `CRS_DEP_TIME`, `CRS_ARR_TIME` were stored as raw HHMM integers (e.g., `1447` = 14:47) — **converted to `HH:MM` strings** during cleaning.
- `DEP_DELAY` and `ARR_DELAY` nulls are filled with `0` **only for non-cancelled flights**. Cancelled flights intentionally retain `NaN` since no departure/arrival occurred.
- `CANCELLATION_CODE` nulls are filled with `'Not Cancelled'` for operational flights.
- `DELAY_DUE_*` fields are filled with `0` — nulls here represent flights with delays under the 15-minute BTS reporting threshold (not missing data).
- **Outlier removal threshold:** `arr_delay` and `dep_delay` values ≥ 500 minutes are removed. Verified by percentile analysis — these represent < 0.01% of operational flights and are likely data entry errors or multi-day diversion edge cases.
- Duplicate rows are identified and removed as part of the cleaning pipeline.
