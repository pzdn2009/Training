# ISLR

## ISLR

Data for An Introduction to Statistical Learning with Applications in R

## Wage 數據集

Wage and other data for a group of 3000 workers in the Mid-Atlantic region.

```r
install.packages("ISLR")
library(ISLR)
data(Wage)
str(Wage)
```

| Format | A data frame with 3000 observations on the following 12 variables. |
| :--- | :--- |
| year | Year that wage information was recorded |
| age | Age of worker |
| sex | Gender |
| maritl | A factor with levels 1. Never Married 2. Married 3. Widowed 4. Divorced and 5. Separated indicating marital status |
| race | A factor with levels 1. White 2. Black 3. Asian and 4. Other indicating race |
| education | A factor with levels 1. &lt; HS Grad 2. HS Grad 3. Some College 4. College Grad and 5. Advanced Degree indicating education level |
| region | Region of the country \(mid-atlantic only\) |
| jobclass | A factor with levels 1. Industrial and 2. Information indicating type of job |
| health | A factor with levels 1. &lt;=good&lt; code=""&gt; and 2. &gt;=Very Good indicating health level of worker |
| health\_ins | A factor with levels 1. Yes and 2. No indicating whether worker has health insurance |
| logwage | Log of workers wage |
| wage | Workers raw wage |

