# Adult Income Intelligence (Power BI)

An interactive Power BI report on the Adult Census Income dataset: who earns above $50K, and how that share differs across demographic and employment groups.

This is a Power BI version of an earlier Python and Streamlit dashboard. Both versions use the same data and the same cleaning rules, and every number here was checked against the original Pandas analysis.

## Key findings

- **About 1 in 4 people earn above $50K.** 7,841 of 32,561 people (24.1%).
- **Education shows the largest differences.** A bachelor's degree or higher: 48.5% earn above $50K, compared with 13.0% for high school or less. Doctorate holders lead at 74.1%.
- **Occupation.** Executive and managerial roles have the highest rate (48.4%), followed by professional specialty roles (44.9%).
- **Age peaks in mid-career.** People aged 45 to 54 have the highest rate (40.1%). Under 25s are almost never above $50K (1.1%).
- **More hours, higher rate, up to a point.** The rate climbs from 20.7% at 35 to 40 hours to 44.6% at 50 to 59 hours, then dips slightly at 60+.
- **An observed income gap by sex.** Men: 30.6%. Women: 10.9%. The gap appears within each education level, for example 50.4% vs 20.9% for bachelor's degree holders.

These are observed differences in historical data. No other factors were controlled for, so they are not causes.

## What it means

Education and occupation show the largest observed differences in this data, followed by age and hours worked. The gap by sex stands out because it appears even when comparing people with the same education level.

**Next step:** build a model that accounts for several factors at once, to estimate how much each one is associated with income on its own.

## Report pages

1. **Income Overview.** Headline numbers, the income split, age and education breakdowns, and six executive insights.
2. **Income Patterns.** The above $50K rate by education level, occupation, marital status, age and weekly hours.
3. **Education, Sex & Work.** All 16 education levels, an education by sex matrix, and the rate by sex and workclass.
4. **Conclusions & Methodology.** The conclusions, data quality checks (row counts, duplicates, missing values), how the data was cleaned, and the limitations.

All pages have filters for age group, education, occupation, sex and income. The filters are synced across pages.

## How it was built

- **Power Query** loads the CSV, trims stray spaces, labels missing values as "unknown" (1,836 workclass, 1,843 occupation, 583 native country), adds a 1 or 0 flag for earning above $50K, and groups age, weekly hours and education into readable bands (the 16 original education levels become 7, and the originals are kept).
- **DAX measures** handle the counts, the above $50K rate, median hours and data quality checks.
- **Executive insights** are written in DAX. They find the strongest group for the current filters and turn it into a sentence. No AI service is used. Groups smaller than 20 people (or 0.5% of the selection) are skipped so tiny groups don't top the list by chance.
- **Rates, not counts,** are used for comparisons, so a group of 400 people and a group of 10,000 can be compared fairly.

## Validation against the Python version

This confirms the Power BI and Python versions agree. It does not test the assumptions behind the analysis.

| Check | Pandas | Power BI |
|---|---|---|
| People | 32,561 | 32,561 |
| Above $50K | 7,841 | 7,841 |
| Above $50K rate | 24.1% | 24.1% |
| Median weekly hours | 40 | 40 |
| Age range | 17 to 90 | 17 to 90 |
| Exact duplicate rows | 24 | 24 |
| Unknown workclass / occupation | 1,836 / 1,843 | 1,836 / 1,843 |
| Top education group | Doctorate, 74.1% | Doctorate, 74.1% |
| Top occupation | Exec-managerial, 48.4% | Exec-managerial, 48.4% |
| Top age group | 45 to 54, 40.1% | 45 to 54, 40.1% |

Two differences from the original app, both intentional:
- **Unknown native country:** the CSV has 583 blank values. The Power BI version labels all of them as unknown. Depending on the Pandas version, the Streamlit app could report 0 here.
- **Hours insight:** the Streamlit app compared every exact hour value (its top result was exactly 55 hours). This version compares hour bands, which is easier to read and less sensitive to small groups.

## Limitations

- The data comes from the 1994 U.S. Census and does not describe today's workforce.
- Income is a category (above or below $50K), not an exact salary.
- `fnlwgt` is a census sampling weight and is not used, so results describe the sample rather than the U.S. population.
- 24 exact duplicate rows are kept, since identical records can belong to different people.

## Opening the report

1. Open `AdultIncomeIntelligence.pbip` in Power BI Desktop. The dataset is included in the `data` folder.
2. If the data doesn't load, go to **Home → Transform data → Edit parameters** and set **CsvPath** to the full location of `data\adult_all.csv` on your computer, then click **Apply changes**.
3. A PDF export of the finished report is included for anyone without Power BI Desktop.

## Data

Adult (Census Income) dataset, originally from the UCI Machine Learning Repository.
