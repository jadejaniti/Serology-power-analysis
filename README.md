# Serology data QC, seroprevalence, and power analysis in R

An end-to-end analysis of a public serosurvey (parvovirus B19 IgG, Belgium, 3,080 sera), covering the work a laboratory data analyst does on serology data: data quality control, an independent check of the assay cutoff, seroprevalence estimation, and study design with power and sample-size calculations.

**Report:** open `analysis.html` in a browser for the full rendered analysis with code, tables, and figures.

## Key results

| Question | Method | Result |
|---|---|---|
| Is the dataset complete and internally consistent? | Missingness, range checks, concentration vs serostatus agreement | 7.3% of records lack a quantitative result; 2 reported negatives sit above the lowest positive and are flagged for source verification |
| Does the reported cutoff hold up? | Two-component Gaussian mixture on log10 concentration | Data-driven cutoff 20.2 IU/mL; 99.9% agreement with reported calls (Cohen's kappa 0.998) |
| How does seroprevalence change with age? | Wilson 95% CIs; logistic regression with a natural spline for age | Rises from 18% (ages 0–5) to 84% (61+); no clear sex effect after age adjustment (OR 0.86, 95% CI 0.73–1.02) |
| How many sera are needed for a precise estimate? | Precision-based sample size with design effect | e.g. about 380 sera for ±5 points at 55% prevalence |
| How many sera per group to detect a 10-point difference? | Cohen's h, `pwr::pwr.2p.test` | 374 per group (80% power), 501 per group (90% power) |
| What difference could this survey detect between sexes? | Detectable effect size, `pwr::pwr.2p2n.test` | About 4.7 percentage points at 80% power |
| What sample size does an age-adjusted design need? | Simulation from the fitted model (OR 1.5, 400 replicates per size) | About 1,200 sera in total for 80% power |

![Mixture model cutoff](figures/mixture_plot-1.png)
![Power curves](figures/power_curves-1.png)

## Repository contents

```
analysis.Rmd                    analysis source (R Markdown)
analysis.html                   rendered report
data/parvob19_be_serology.csv   line-listed serology data
figures/                        figures produced by the report
```

## How to reproduce

Requires R (4.3 or later) and these packages: `rmarkdown`, `knitr`, `ggplot2`, `scales`, `pwr`, `mixtools`.

```r
install.packages(c("rmarkdown", "knitr", "ggplot2", "scales", "pwr", "mixtools"))
rmarkdown::render("analysis.Rmd")
```

The simulation uses a fixed random seed, so results are reproducible.

## Data source and license

The data are the `parvob19_be_2001_2003` dataset from the R package [serosv](https://github.com/OUCRU-Modelling/serosv) (MIT license), exported here to CSV without modification. Original study:

> Mossong J, Hens N, Friederichs V, et al. Parvovirus B19 infection in five European countries: seroepidemiology, force of infection and maternal risk of infection. *Epidemiology and Infection* 2008; 136(8): 1059–68. doi:10.1017/S0950268807009661

## Skills demonstrated

Data QC and documentation · assay cutoff evaluation · mixture models · binomial confidence intervals · logistic regression with splines · power and sample-size calculation (`pwr`) · simulation-based power analysis · publication-quality figures with `ggplot2` · reproducible reporting with R Markdown

## Author

Niti B. Jadeja, Ph.D. · ORCID [0000-0002-2199-9605](https://orcid.org/0000-0002-2199-9605)
