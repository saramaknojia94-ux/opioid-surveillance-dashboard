# Texas Opioid Surveillance Dashboard
## Tableau Build Blueprint — Senior Portfolio Project

---

## The story this dashboard tells
"In Texas's 20 largest counties, who is dying from opioid overdoses,
where, and are EMS teams reaching them in time?"

Audience: County health director, COO, state health commissioner.
Use case: Monday morning briefing. 90 seconds to understand the state of the crisis.

---

## Data sources (6 CSVs — all connect on county + year)

| File | Used for |
|---|---|
| tx_opioid_mortality.csv | Map, trend line, county ranking |
| tx_opioid_by_gender.csv | Gender KPI + bar chart |
| tx_opioid_by_age.csv | Age heat map |
| tx_opioid_by_drug_type.csv | Drug type stacked bar / trend |
| tx_ems_naloxone.csv | EMS reach vs mortality gap |
| tx_kpi_summary.csv | KPI tile header row |

Join strategy in Tableau: create a data model with tx_opioid_mortality
as the primary table, join others on [county] + [year].

---

## Color system (use these exactly — looks like a real health product)

Background:         #F4F2ED  (warm off-white)
Dark text:          #1C1B19
Primary navy:       #1A3A52
Alert red:          #C0392B  (high death rate, worsening trend)
Positive green:     #1A6B3C  (improving, EMS reached)
Amber warning:      #B7770D  (mid-range, watch)
Neutral gray:       #7A7875
Grid / borders:     #D5D2CA
KPI card bg:        #FFFFFF  with 1px #D5D2CA border

Diverging scale for heat map: #1A6B3C → #F4F2ED → #C0392B

---

## Dashboard specs

Fixed canvas: 1400 x 900px
Font: Tableau Book throughout. Never use bold except KPI numbers.

Layout zones:
- Top bar (80px):   Title + subtitle + data source note
- KPI row (130px):  5 KPI cards full width
- Main area (690px): Left panel 55% | Right panel 45%

---

## SECTION 1 — KPI Header Row
Source: tx_kpi_summary.csv, filtered to selected year

Five cards, evenly spaced, white background, 1px border:

Card 1: Total Opioid Deaths
  - Big number: [total_opioid_deaths] in 24pt navy bold
  - Below: YoY arrow + [yoy_change_pct]% in red if positive, green if negative
  - Label: "OPIOID DEATHS · [YEAR]" in 9pt gray uppercase

Card 2: Deaths per 100k (avg across 20 counties)
  - Number: [deaths_per_100k_avg]
  - Context line: "20-county average"

Card 3: Hardest-hit county
  - County name in 16pt navy
  - Rate below: "[highest_rate] per 100k"

Card 4: EMS Naloxone Deployments
  - Number: [total_naloxone_deployments]
  - Context: "estimated overdose reversals"

Card 5: EMS Reach Ratio
  - Number: [ems_reach_ratio_avg]x
  - Context: "naloxone deployments per death"
  - Color red if < 1.2, amber if 1.2-1.8, green if > 1.8

---

## SECTION 2 — Main Visualizations

### VIZ 1: County map (left panel, top half)
Source: tx_opioid_mortality.csv
Type: Filled map by county (use FIPS or lat/lng)
Color: Deaths per 100k — diverging scale, white = avg, red = high
Tooltip: "[County] · [Year] · [opioid_deaths] deaths · [crude_rate_100k] per 100k"
Interaction: Click county to filter all other charts

This is the first thing a COO sees. It answers "where is the problem worst."


### VIZ 2: Trend line — Deaths over time (left panel, bottom half)
Source: tx_opioid_mortality.csv + tx_opioid_by_drug_type.csv
Type: Dual-layer line chart
  Layer 1: Total opioid deaths by year — thick navy line
  Layer 2: Fentanyl deaths — dashed red line overlaid
X axis: Year (2018–2022)
Y axis: Deaths
Add annotation at 2020: "COVID + fentanyl surge"
Add annotation at 2022: "Fentanyl: [X]% of all deaths"

Why this matters: The divergence between total deaths and fentanyl specifically
tells the policy story — this is no longer a prescription opioid problem.


### VIZ 3: Gender + Age breakdown (right panel, top)
Type: Side-by-side bar chart
Source: tx_opioid_by_gender.csv
X: Deaths  |  Y: Gender  |  Color: Male = navy, Female = coral
Below it: Small heat map
Source: tx_opioid_by_age.csv
Rows: Age group  |  Columns: Year  |  Color: deaths (light to dark red)

This answers "who" in two views stacked vertically.
The heat map shows the 25-34 age group worsening over time at a glance.


### VIZ 4: EMS Reach vs Mortality (right panel, bottom)
Source: tx_ems_naloxone.csv
Type: Dot plot / connected dot chart
  Each row = one county
  Left dot: opioid_deaths (red)
  Right dot: naloxone_deployments (green)
  Line connecting them shows the gap
Sort: By gap size descending

This is your most executive-facing chart. The gap between the two dots
is the number of people EMS did not reach. That's a budget conversation.
No other chart in a junior portfolio looks like this.


### VIZ 5: Drug type shift (right panel, middle — small multiples)
Source: tx_opioid_by_drug_type.csv
Type: 100% stacked bar chart by year
Color: Fentanyl = red | Rx opioids = amber | Heroin = gray | Other = light gray
Annotation: Show fentanyl % label on each bar

This shows the structural shift from Rx to fentanyl in 5 bars.
A policy director needs to see this to understand why prior interventions
targeting prescription pads are no longer sufficient.

---

## Dynamic filters (make ALL charts respond to these)

1. Year slider (2018–2022) — affects everything
2. County multi-select — affects all except statewide KPIs
3. Urbanization (Large Metro / Fringe Metro / Medium / Small) — quick segmentation
4. Gender toggle (All / Male / Female) — affects gender chart + KPI tiles
5. Drug type filter — affects trend line and drug type chart

In Tableau: set each filter to "Apply to worksheets > All using related data sources"
This is what makes it look like a real product, not a school project.

---

## Tooltip standard (apply to every single mark)

Format: [County] | [Year]
[Metric name]: [Value]
Compared to 20-county avg: [Above / Below / At avg]

Example on map:
  Jefferson County | 2022
  Opioid deaths: 47
  Rate per 100k: 18.6
  20-county avg: 10.4 → Above average

Never leave default Tableau tooltips on. This one detail separates
senior dashboard builders from everyone else.

---

## Titles and labels

Dashboard title: Texas Opioid Mortality Surveillance
Subtitle:        20-County Overview · 2018–2022 · Source: CDC WONDER, NEMSIS
                 (9pt gray, left-aligned under title)

Sheet titles: Turn off sheet titles. Use floating text objects instead.
This gives you pixel-level control over typography.

Data note (bottom left, 8pt gray):
"Mortality data: CDC WONDER Multiple Cause of Death (ICD-10: X40–X44, X60–X64, Y10–Y14).
EMS data: NEMSIS Public Research Dataset. County suppression threshold: n<10 cells excluded."

---

## Publishing to Tableau Public

Name:        Texas Opioid Mortality Surveillance · 20-County View 2018–2022
Description: Executive operations dashboard tracking opioid mortality, 
             demographic breakdown, and EMS naloxone response across Texas's 
             20 largest counties. Built to support county-level resource 
             allocation decisions. Data: CDC WONDER + NEMSIS.
Tags:        opioid, texas, public health, EMS, surveillance, healthcare analytics

---

## GitHub Repo Structure

opioid-surveillance-dashboard/
├── README.md                    ← case study write-up (see below)
├── data/
│   ├── raw/                     ← original CDC WONDER exports (TSV)
│   └── processed/               ← your 6 cleaned CSVs
├── notebooks/
│   └── opioid_data_prep.py      ← this script
└── dashboard/
    └── tableau_link.md          ← link to live Tableau Public dashboard

---

## README case study structure

### The question
Not "I built a dashboard" — frame it as:
"County health leaders in Texas need to understand where opioid deaths
are concentrated, who is most affected, and whether EMS resources are
reaching people before they die. This dashboard is designed to answer
those questions without a data analyst in the room."

### Design decisions (this is what makes the README senior-level)

1. Why county-level, not state-level?
   State averages hide the gap between Jefferson County (18.6/100k) and
   Collin County (5.1/100k). Resource allocation decisions happen at the
   county level, so the data needs to be there too.

2. Why EMS reach ratio instead of just naloxone counts?
   Raw naloxone numbers without context are meaningless. The ratio of
   deployments to deaths tells you whether EMS is ahead of or behind
   the crisis. A ratio below 1.2 is a resource gap — that's actionable.

3. Why the drug type stacked bar?
   The shift from prescription opioids to fentanyl is the policy story
   of the decade. A single bar chart shows in 5 seconds that interventions
   targeting pill mills are no longer sufficient. That reframes the
   budget conversation.

4. Why connected dot plot for EMS?
   Side-by-side bars would show the same data but bury the gap. The
   connected dot chart makes the unreached population visible as a
   physical space between two points. Executives respond to that.

5. What self-serve means here:
   Every filter cascades across every chart. A county commissioner can
   select their county, their year, and their demographic of concern and
   get a complete picture without submitting a data request. That was
   the design constraint from the start.

