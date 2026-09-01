# Grid & Digital Operations — Executive Dashboard (Power BI)

A blended executive dashboard for a utility company, combining grid
reliability, financial performance, and safety KPIs with IT system health
and engineering delivery metrics — built for executive leadership and IT
department heads.

**Power BI Desktop is free** — no subscription or Pro license is needed to
build and demo this locally. You'd only need Pro/Premium to publish it live
to the Power BI cloud service, which isn't required for a portfolio piece.

All data is synthetic — no real utility company data is used.

## Files included

```
outages.csv       Outage events — source for SAIDI/SAIFI reliability measures
financials.csv    Monthly revenue, opex, capex by region
safety.csv        Monthly recordable incidents, near misses, lost-time incidents
it_metrics.csv    IT system uptime, incident count, MTTR (sampled monthly)
deployments.csv   Weekly engineering delivery metrics by team
dax_measures.txt  All DAX measures, ready to paste into Power BI Desktop
```

## Build steps

### 1. Import the data
Power BI Desktop > **Get Data** > **Text/CSV** > import all 5 CSV files.
Load each as its own table (`outages`, `financials`, `safety`,
`it_metrics`, `deployments`).

### 2. Fix data types in Power Query
Open **Transform Data**. For each table, confirm the date-type column
(`date`, `month`, or `week_start`) is set to **Date** type, not text.

### 3. Build a shared Date table
The five tables have different date grains (daily, weekly, monthly), so a
single shared Date dimension table is what lets one slicer filter all of
them together — this is the Power BI equivalent of the `listen:` blocks in
the Looker version.

In Power Query, add a **MonthKey** custom column to each table:
`= Date.StartOfMonth([date_column])`

Then create a new table via **Modeling > New Table**:
```
DateTable = CALENDAR(DATE(2024,1,1), DATE(2025,12,31))
```
Add a matching column:
```
MonthKey = DATE(YEAR(DateTable[Date]), MONTH(DateTable[Date]), 1)
```
Right-click `DateTable` > **Mark as Date Table**.

### 4. Create relationships
In the **Model** view, drag `DateTable[MonthKey]` to each fact table's
`MonthKey` column, creating five one-to-many relationships (DateTable is
the "1" side). This lets a single date slicer filter reliability,
financial, safety, IT, and delivery visuals simultaneously.

### 5. Add the DAX measures
Open `dax_measures.txt` and paste each measure into Power BI (right-click
the relevant table > **New Measure**, paste the formula). This gives you
SAIDI, SAIFI, revenue, operating margin, IT uptime, MTTR, and on-time
delivery as ready-to-use measures.

### 6. Build the report page — controller-first layout

This dashboard is built for a **controller-level financial audience**, with
IT/engineering metrics included as a small supporting footer rather than
equal billing. The zone order (top to bottom) matches how a controller
actually reads a report — variance first, everything else after:

1. **Slicers** at the top: `DateTable[Date]` (range) and `financials[region]`,
   set to filter the whole page.

2. **Status strip** (only if something needs attention): a text box or card
   using `Revenue Variance Status` / `Opex Variance Status` — only surface
   this if a measure is Unfavorable, so it doesn't clutter a clean month.

3. **Headline KPI row** (the zone a controller actually reads first):
   - Total Revenue, with `Revenue Variance %` as the card's built-in
     comparison/trend indicator
   - Total Opex, with `Opex Variance %`
   - Operating Margin %
   - SAIDI Minutes (reliability ties directly to opex/capex spend, so it
     belongs here, not in the IT section)

4. **Variance charts** (the core controller view):
   - **Actual vs. Budget Revenue by month** — clustered column or line,
     two series (`Total Revenue` vs `Total Budget Revenue`)
   - **Opex Variance % by region** — bar chart, conditional formatting
     (red for unfavorable, green for favorable) using `Opex Variance Status`

5. **Supporting operational detail**:
   - Bar chart: Total Outage Count by `outages[cause]`
   - Bar chart: Recordable Incidents by month (`safety`)

6. **IT/engineering footer** (afterthought section, visually smaller —
   e.g. a single row of 2-3 compact KPI cards, no full charts):
   - Avg IT Uptime %
   - Avg On-Time Delivery %
   - Total IT Incidents


---
*This is a personal portfolio project built with synthetic data to
demonstrate Power BI data modeling and executive dashboard design for the
utility industry.*
