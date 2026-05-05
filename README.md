# Human Development Index Analysis (2013–2022)

A statistical deep-dive into global human development trends across 175 economies over a decade. Using stratified random sampling, the project builds a representative 150-country dataset and applies a full inferential statistics toolkit — confidence intervals, one-sample, paired, and two-sample t-tests — to quantify HDI growth, persistent regional inequality, and the relationship between income and human development.

---

## What This Project Does

- Constructs two new country-level variables by cross-referencing external datasets (IMF classification, ISO 3166 continent codes) using `XLOOKUP`
- Draws a stratified random sample (n = 150) from 175 economies with a hard constraint on Advanced economy representation, solved via convolution of hypergeometric distributions
- Tracks mean and median HDI across 2013–2022, identifying the COVID-19 dip in 2020 and the absence of convergence between countries
- Tests whether the IMF's cited 20-year average HDI benchmark (0.695) still holds — it did in 2013, but is significantly outdated by 2022
- Quantifies the Europe–Africa HDI gap (~0.32) and the Advanced–Emerging gap (~0.24), both stationary across the full decade
- Fits a log-linear regression of HDI on ln(GDP per capita) for 2022, achieving R² = 0.896

---

## Workbook Structure

| Sheet | Contents |
|-------|----------|
| DataSheet | Full dataset with `continent` and `economic_status` variables |
| Sheet 1 | Stratified sample (n = 150), quota table, hypergeometric constraint calculation |
| Sheet 2 | Boxplots of HDI per year (2013–2022), summary statistics table |
| Sheet 3 | 95% confidence intervals for mean HDI per year |
| Sheet 4 | One-sample t-test vs. IMF reference value 0.695 (years 2013 and 2022) |
| Sheet 5 | Paired t-test: within-country HDI change 2013 → 2022 |
| Sheet 6 | Two-sample t-tests: Europe vs. Africa; Advanced vs. Emerging economies |
| Sheet 7 | Type I/II error analysis and power discussion |
| Sheet 8 | Scatter plot and log-linear regression: HDI ~ ln(GDP per capita), 2022 |

---

## Sampling Design

Proportional stratified random sampling with continents as strata. Random order generated via:

```excel
=SORTBY(SEQUENCE(175), RANDARRAY(175))
```

Pasted immediately as static values. The assignment required ≥ 30 Advanced economies in the final sample. With only 32 Advanced economies in the population (24 concentrated in Europe), the probability of satisfying this in a single draw was **~6.96%** — computed via convolution of per-stratum hypergeometric distributions. Sampling was repeated until the constraint was satisfied.

| Continent | Population | Sample | Share |
|-----------|-----------|--------|-------|
| Africa | 48 | 41 | 27.4% |
| Asia | 44 | 38 | 25.1% |
| Europe | 40 | 34 | 22.9% |
| North America | 21 | 18 | 12.0% |
| South America | 11 | 9 | 6.3% |
| Oceania | 10 | 9 | 5.7% |
| Australia | 1 | 1 | 0.6% |

---

## Results at a Glance

**HDI grew globally, but inequality between regions did not shrink.**

| Finding | Value |
|---------|-------|
| Mean HDI 2013 | 0.712 |
| Mean HDI 2022 | 0.734 |
| Paired t-test p-value (2022 vs. 2013) | ≈ 3.8 × 10⁻²² |
| IMF benchmark (0.695) valid in 2013? | Yes (p = 0.178) |
| IMF benchmark (0.695) valid in 2022? | No (p = 0.003) |
| Europe–Africa HDI gap (stable 2013–2022) | ~0.32 |
| Advanced–Emerging HDI gap (stable 2013–2022) | ~0.24 |
| R² of HDI ~ ln(GDP per capita), 2022 | 0.896 |

---

## Key Methodological Choices

- **Chebyshev vs. empirical rule:** Chebyshev used throughout — HDI distribution is negatively skewed, not normal
- **Paired vs. two-sample t-test:** Paired design in P6 because the same 150 countries appear in both 2013 and 2022, eliminating between-country variance from the test
- **Log transformation of GDP:** The HDI–income relationship is concave; ln(GDP) linearizes it and substantially improves model fit
- **Static value pasting:** All randomly generated orders were immediately fixed to ensure reproducibility across sessions

---

## Data Sources

- UNDP Human Development Reports — HDI values per country, 2013–2022
- IMF World Economic Outlook — Advanced vs. Emerging economy classification
- World Bank Open Data — GDP per capita (NY.GDP.PCAP.CD), 2022
- [lukes/ISO-3166-Countries-with-Regional-Codes](https://github.com/lukes/ISO-3166-Countries-with-Regional-Codes) — country-to-continent mapping

---

## Notes

- Cuba and Yemen excluded from the  regression due to missing World Bank GDP data (n = 148)
- The ≥ 30 Advanced economy constraint introduces mild upward bias in Advanced country representation
- North America and South America were split manually; Australia treated as a separate single-country stratum
