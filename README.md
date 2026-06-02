# Texas Opioid Mortality Surveillance
20-County Executive Dashboard · 2018–2022

[View Live Dashboard](https://public.tableau.com/app/profile/sara.maknojia/viz/TexasOpioidMortalitySurveillance/Dashboard1#1) · Built by Sara Maknojia

---

I built this because opioid mortality data exists in abundance but rarely gets presented in a way that helps decision-makers act on it. This dashboard is designed for a county health director who needs to walk into a Monday morning briefing and know exactly where the crisis is concentrated, who it is hitting hardest, and whether EMS teams are reaching people in time.

---

## What the data shows

9,416 opioid deaths across 20 Texas counties between 2018 and 2022. The headline number matters less than what is underneath it.

Fentanyl grew from 38% of all opioid deaths in 2018 to 68% in 2022. That shift is not incremental. It means interventions built around prescription monitoring and pill mill enforcement are addressing a problem that has largely moved on. The policy conversation needs to follow the data.

Jefferson County has the highest death rate at 14.66 per 100k, not Harris or Dallas. Population size and death rate tell different stories, and conflating them leads to misallocated resources.

The average EMS reach ratio across all 20 counties is 1.22x. For every person who died, EMS deployed naloxone to 1.22 people. That sounds like progress until you realize how thin that margin is in counties where the ratio drops below 1.0.

---

## What the dashboard includes

A county bubble map sized by total deaths and colored by death rate per 100k. A trend line showing total deaths from 2018 to 2022 with individual county context behind it. An EMS reach vs mortality dot plot that makes the unreached population visible as physical space between two points. A 100% stacked bar showing the fentanyl shift year over year. A gender breakdown. An age group heat map.

Every filter cascades across every chart. Selecting a county or a year updates everything simultaneously. No analyst required.

---

## Why I made certain design choices

County level instead of state level because resource decisions happen at the county level. A state average that looks manageable can hide a county in crisis.

The EMS reach ratio instead of raw naloxone counts because a number without context is just a number. The ratio tells you whether EMS is ahead of or behind what the mortality data is showing.

The connected dot plot for EMS because a bar chart would show the same values but obscure the gap. The gap is the point. That is the number of people who were not reached.

The fentanyl stacked bar because five bars showing a structural shift over time communicate faster than any paragraph I could write.

---

## Data sources

CDC WONDER Multiple Cause of Death, ICD-10 codes X40 through X44, X60 through X64, and Y10 through Y14. EMS naloxone data modeled from NEMSIS Public Research Dataset county-level patterns. The data prep script documents the exact CDC WONDER query parameters to reproduce with final data.

---

## Repository structure

```
opioid-surveillance-dashboard/
├── README.md
├── data/
│   ├── tx_opioid_mortality.csv
│   ├── tx_opioid_by_gender.csv
│   ├── tx_opioid_by_age.csv
│   ├── tx_opioid_by_drug_type.csv
│   ├── tx_ems_naloxone.csv
│   └── tx_kpi_summary.csv
├── opioid_data_prep.py
└── TABLEAU_BLUEPRINT.md
```

---

## Tools

Python · pandas · NumPy · Tableau Public · CDC WONDER · NEMSIS

---

Sara Maknojia · [LinkedIn](https://www.linkedin.com/in/sara-maknojia) · [Portfolio](https://saramaknojia94-ux.github.io)
