---
title: "Data Acquisition Project"
author: "Katie Youngberg"
date: "2025-11-19"
format:
  html:
    code-fold: true
    toc: true
---
## Introduction

Pregnancy is still a risk for women around the world: hundreds of women die every day from largely preventable complications of pregnancy and childbirth.

At the same time, countries differ dramatically in both economic resources (GDP per capita) and access to family-planning services (contraceptive prevalence). International agencies like WHO, UNICEF, the UN, and the World Bank routinely publish indicators on these topics.

## Motivating question 

How are maternal mortality rates across countries related to both economic development (GDP per capita) and access to contraception, and how have these patterns changed over time?

More specific questions:

- Do richer countries consistently have lower maternal mortality?

- Is contraceptive prevalence more strongly associated with maternal mortality than GDP alone?

- Have high-mortality countries improved since 1985, or are there places where progress has stalled?

- Among “first-world” (high-income) countries, who is doing especially well or surprisingly poorly?

## Ethics

All of the information used here is country-level, aggregate data with no individual-level or personally identifiable information. The sources themselves are major international organizations that publish data explicitly for public use in research and education, including:

- World Bank GDP per capita indicators

- UNICEF / WHO / World Bank / UNFPA maternal mortality indicators (MMR, SDG 3.1.1)

- UN Population Division & World Bank / UN contraceptive prevalence datasets

Because the data is already public and intended for reuse, there are no additional human-subjects or privacy concerns beyond properly citing the source and respecting any stated terms of use.

## Steps Summary 

High-level steps (leaving out code details):

- Pick indicators & sources

  - GDP per capita (current US$) → World Bank indicator NY.GDP.PCAP.CD.

  - Maternal mortality ratio (deaths per 100,000 live births) at multiple years (1985, 2000, 2010, 2020) → UN/WHO/World Bank joint estimates.

  - Contraceptive prevalence (% of women 15-49 using any method) → UN “World Contraceptive Use” / World Bank fertility indicators.


- Download raw data

  - For World Bank indicators, use the “Download CSV” button or API link on the indicator page.

  - For UN Data tables, use read_html or manually download the CSV from the “Download” link.

  - Save each dataset as a separate CSV (for example: maternal_mortality.csv, gdp_per_capita.csv, contraceptive_prevalence.csv).


- Clean and standardize country names

  - In Python, load each CSV with pandas.read_csv.

  - Keep only the columns you need: country name, year, and value.

  - Pivot the maternal mortality data so that years become columns (1985, 2000, 2010, 2020).

  - Standardize country names (fix spelling differences like “United States” vs “United States of America”, “Côte d'Ivoire” vs “Cote dIvoire”) so joins work cleanly.


- Merge datasets

  - Use pd.merge on the country column to create a combined dataframe with:

    - maternal mortality for several years

    - 2020 contraceptive prevalence

    - GDP per capita (latest or 2020)


- Export for statistical summary and visualization

  - Save the merged dataframe as womens_health_econ.csv (143 rows × 8 columns).

  - Perform effective analysis

## EDA highlights

- Mean MMR in 1985: ~450 deaths per 100,000 live births

- Mean MMR in 2020: ~172 deaths per 100,000

On average, maternal mortality has dropped by about 60% over 35 years in this set (450 → 172).

Range in 2020:

- Min: 1.1 deaths per 100k (Belarus)

- 25th percentile: ~22.5 deaths per 100,000

- Median: ~77.1 deaths per 100,000

- 75th percentile: ~225.1 deaths per 100,000

- Max: 1222.5 (South Sudan) deaths per 100,000

You can see this decline in the “Maternal Mortality Rates by Country” small-multiples panel: most bars shrink substantially from 1985 to 2020, but a handful of countries remain very high.


Contraceptive prevalence

Mean contraceptive prevalence: ~48%

Range: 4–88% of women using some form of contraception.

The “Contraceptive Prevalence” table in the dashboard shows this as a sortable list; e.g.:

Low usage: South Sudan (4%), Chad (5%), Angola (6%)

High usage: Norway (88%), Belgium/other European countries in the 70–80% range.


GDP per capita

Mean GDP per capita: about $11,100

Range: ~$154 to ~$107,000

The Overall GDP choropleth map shows clear regional patterns: North America, Western Europe, and a few high-income Asian/Oceanic countries are darkest, while much of Sub-Saharan Africa is lightest.

Associations

Correlation between 2020 maternal mortality and GDP per capita: r ≈ –0.36

Rough moderate negative relationship: richer countries tend to have lower maternal mortality, but there are exceptions.

Correlation between 2020 maternal mortality and contraceptive prevalence: r ≈ –0.67

Stronger negative association: countries with higher contraceptive use tend to have much lower maternal mortality.

Extremes:

Highest 2020 MMR: South Sudan, Chad, Nigeria, Central African Republic, Guinea-Bissau (725–1222 deaths per 100k). These countries have low GDP and very low contraceptive prevalence (4–15%).

Lowest 2020 MMR: Belarus, Norway, Australia, Spain, Japan (1.1–4.3 deaths per 100k), all with relatively high GDP and moderate-to-high contraceptive prevalence.

The “Mortality Rates Among First World Countries” bubble map emphasizes that even among high-income nations, there are differences: some have almost negligible maternal mortality, while others (like the US, if included) remain noticeably higher than peers, consistent with external statistics.

## Most Interesting Findings

# Global progress but persistent inequity

Average maternal mortality fell from ~450 to ~172 deaths per 100k between 1985 and 2020, but the worst-off countries are still above 700–1200 deaths per 100k, mirroring global estimates showing a ~40–50% decline but large gaps between low- and high-income regions.

# Contraceptive prevalence is strongly linked to better outcomes.

The strongest statistical relationship in my dataset is between higher contraceptive prevalence and lower maternal mortality (r ≈ –0.67), fitting with the idea that access to family planning helps prevent high-risk pregnancies and allows health systems to focus resources on fewer, safer births.


# Economic development helps, but isn’t everything

GDP per capita is negatively correlated with maternal mortality, but less strongly (r ≈ –0.36).

Some countries with moderate GDP do very well (low mortality) and others with relatively high GDP still underperform, suggesting that how health resources are organized and who they reach matters as much as overall wealth.

# First-world variation is real

Among high-income countries on your “first-world” map, some have maternal mortality ~1–3 deaths per 100k, while others are higher.

External data show the U.S., for example, has much higher maternal mortality than many peers despite high spending, opening the door to questions about health-care access, racial inequities, and policy differences even among rich nations.

# Long-term improvement is not guaranteed

The time-series panel suggests that, although most countries improve over time, some show plateaus or even upticks in more recent years, reflecting concerns in recent reports that progress on maternal mortality has slowed or stalled.

## Further Information

World Bank – GDP per capita (current US$)

World Bank – Maternal mortality ratio – modeled estimates per 100,000 live births.

WHO / UN Maternal mortality fact sheets & SDG 3.1 – background on definitions, trends, and targets.

UN Population Division – World Contraceptive Use – contraceptive prevalence datasets and methodology.

World Bank – Contraceptive prevalence, any method (% of married women 15–49) – alternative source for contraception indicators.

## Link to the code

https://github.com/kyoungb/DataAcquisitionBuild 