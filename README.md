# Mann-Kendall and Cubic Regression Trend Analysis

Statistical trend analysis of historical **temperature and precipitation** records for the **Hunza River Basin, Karakoram**, combining the non-parametric **Mann-Kendall trend test** with **linear** and **3rd-order (cubic) polynomial regression** to characterize both the direction and the shape of long-term climatic change.

## Contents (`Mann Kendall and CUBIC REGRESSION trend.ipynb`)

| Section | Purpose |
|---|---|
| **Temperature** | Basin-wide Tmax/Tmin/Tmean trend analysis — annual-mean time series, linear trend, and cubic regression |
| **Precipitation** | Annual total precipitation trend analysis — Mann-Kendall test, linear + cubic regression, and per-station annual bar charts |

## Data

- **Temperature:** daily Tmax/Tmin from two stations, **Gilgit** and **Khunjerab**, averaged into a basin-wide daily series, then aggregated to annual means
- **Precipitation:** daily totals for the basin (1995–2023), plus per-station series from **Nalter**, **Khujreeb**, and **Ziarat**, aggregated to annual totals

## Methods

**Basin-wide averaging**
Station Tmax/Tmin are averaged (Gilgit + Khunjerab) to produce a basin-representative daily temperature series; Tmean is the mean of basin Tmax and Tmin. Annual means are computed by grouping on year.

**Mann-Kendall trend test** (implemented from scratch, not a library call)
- Computes the Mann-Kendall **S statistic** from all pairwise sign comparisons
- Corrects variance for tied values
- Derives the standardized **Z statistic** and a two-sided **p-value** from the normal distribution
- Estimates trend magnitude via the **Theil-Sen slope** (median of all pairwise slopes)
- Classifies the trend as Increasing / Decreasing / No trend at the 0.05 significance level

**Linear regression**
Ordinary least-squares fit (`scipy.stats.linregress`) of the annual series against year, reporting slope, intercept, R², and p-value — a Mann-Kendall-style linear-trend visualization.

**Cubic regression**
3rd-order polynomial fit (`numpy.polyfit`, mean-centered year for numerical stability) capturing non-monotonic trend shapes that a straight line misses. Reports:
- Polynomial coefficients and R² / adjusted R²
- An F-test p-value for overall model significance
- **Turning points** (roots of the derivative) within the data range, to flag inflection years where the trend direction changes
- Net change in temperature/precipitation from the start to the end of the record

## Outputs

- Basin-wide annual Tmax / Tmin / Tmean trend plots (linear and cubic, combined 3×2 figure with regression equations, R², and p-values annotated)
- Precipitation annual-total trend plots (Mann-Kendall + linear + cubic)
- Per-station (Nalter, Khujreeb, Ziarat) grouped annual precipitation bar charts
- Printed summary statistics (annual ranges, means, std. dev., trend direction/significance) for each variable

## Dependencies

`pandas` · `numpy` · `scipy` · `matplotlib` · `openpyxl` (for reading `.xlsx` input)

## Data requirements

Excel workbooks with a `Date` column (`dd/mm/yyyy`) and station columns:
- `Temperature.xlsx` — `Tmax (Gilgit)`, `Tmin (Gilgit)`, `Tmax (Khujreeb)`, `Tmin (Khunjreeb)`
- `Historical.xlsx` (precipitation) — `Precp`
- `Precipitation.xlsx` (per-station) — `Nalter`, `Khujreeb`, `Ziarat`

---

*Climate-trend component of a broader Hunza Basin glacier–hydrology research workflow, supporting the meteorological forcing used in the paired OGGM/GDM glacier dynamics modelling.*
