---
title: "Prioriactions in Action"
author: "**José Salgado-Rojas / Matías Moreno-Faguett**"
date: "December 16, 2022"
output:
  ioslides_presentation:
    incremental: false
    ccs: styles.css
    keep_md: yes
    logo: Figures/Logo.png
    widescreen: yes
    df_print: paged
    smaller: yes
  beamer_presentation: default
subtitle: Tutorial for ``R`` Users <br>![CRAN/METACRAN](https://www.r-pkg.org/badges/version/prioriactions)
bibliography: bibtex.bib
editor_options:
  markdown:
    wrap: 72
---

```{=html}
<!---
- ioslides manual: 
   https://bookdown.org/yihui/rmarkdown/ioslides-presentation.html
- Compile from command-line
Rscript -e "rmarkdown::render('R_for_HPC.Rmd'); knitr::knit('R_for_HPC.Rmd', tangle=TRUE)"
-->
```
```{=html}
<!---
  Note: following css chunks are required for scrolling support beyond slide boundaries
-->
```
```{=html}
<style>
slides > slide {
  overflow-x: auto !important;
  overflow-y: auto !important;
}
slides > slide.title-slide p {
  color: black;
}
slides > slide.title-slide hgroup h1{
  color: #4C7843;
}

</style>
```
<style type="text/css">
pre {
  max-height: 300px;
  overflow-y: auto;
}
pre[class] {
  max-height: 300px;
}


</style>

<style type="text/css">
.scroll-300 {
  max-height: 300px;
  overflow-y: auto;
  background-color: inherit;
}
</style>

## 

### <https://github.com/prioriactions/teaching>

# Outline

-   ::: white
    **Features**
    :::

-   Overview

-   Toy example

-   Actual case study: The Mitchell River, Australia

-   Shiny Application

## Why ![](Figures/r_logo.png){#id .class width="8%" height="8%"}?

Reproducibility, Academic use, ...

<center>![Source: https://medium.com](Figures/whyr.webp){.class
width="60%" height="60%"}</center>

## Features

::: {style="float: left; width: 50%;"}
<br>

-   [Powered by **Rcpp and RcppArmadillo**]{style=""}
    <img src="Figures/rcpp.png" style="vertical-align:middle" width="10%" height="10%"/>

-   Find **optimal** solutions

-   Solve a specific new problem using mathematical programming

-   Ideally, fast 🙂 <br> <br>
:::

::: {style="float: left; width: 50%;"}
<center>![](Figures/Logo.png){.class width="80%" height="80%"}</center>
:::

## Overview

[**Three**]{style="color: #B93600;"} questions: **Planning objective**,
**Optimization objective**, and **Intensity of threats**.

<center>![](Figures/Diagram1.jpg){.class width="70%"
height="70%"}</center>

## Workflow

<center>![](Figures/Diagram2.png){.class width="80%"
height="80%"}</center>

## [**Q1.**]{style="color: #B93600;"} Planning objective?

#### Recovery vs Conservation

<center>![](Figures/PlanningObjectives1.png){.class width="60%"
height="60%"}</center>

## Workflow

<center>![](Figures/Diagram2.png){.class width="80%"
height="80%"}</center>

## [**Q2.**]{style="color: #B93600;"} Threat intensities?

#### Binary vs Continuous

<br>

<center>![Bolch, E.A. et al. (2020). Remote Detection of Invasive Alien
Species](Figures/invasive.webp){.class width="80%"
height="80%"}</center>

## [**Q2.**]{style="color: #B93600;"} Threat intensities?

#### Binary vs Continuous

<br>

<center>![Source:
https://ib.bioninja.com.au](Figures/threats.jpg){.class width="70%"
height="70%"}</center>

## Workflow

<center>![](Figures/Diagram2.png){.class width="80%"
height="80%"}</center>

# I) Input data validation

## Inputs (`inputData()`)

Data that defines the spatial prioritization problem (**planning units
data**, **feature data**, **threats data**, and their **spatial
distributions**).

<div>

<center>[**5**]{style=""}
<img src="Figures/table.png" style="vertical-align:middle" width="10%" height="10%"/>
[**+**]{style=""} [**2**]{style=""}
<img src="Figures/table_optional.png" style="vertical-align:middle" width="10%" height="10%"/>
[(optional)]{style=""}</center>

</div>


```r
inputData(
  pu,
  features,
  dist_features,
  threats,
  dist_threats,
  sensitivity = NULL,
  boundary = NULL
)
```

## Inputs

**What if I have my data in another format?**<br>
[**A:**]{style="color: red;"} There are functions that allow you to
import and convert data to `data.frame` format

Examples:


```r
##from .txt to data.frame
df <- read.table('myfile.txt',sep='\t')
```


```r
##from .csv to data.frame
df <- read.csv("myfile.csv")
```

<center>![Source:
https://libguides.umn.edu](Figures/data-frame.png){.class width="80%"
height="80%"}</center>

<br>

## #1 Planning units data (`pu`)

#### `data.frame`

<br> Specifies the **planning units (PU)** of the corresponding instance
and their corresponding **monitoring cost** and **status**. Each row
corresponds to a different planning unit. This file is inherited from
the *pu.dat* in Marxan. <br> <br>

::: {style="float: left; width: 50%;"}
-   [**id**]{style="color: #4385CD;"}<br> `integer` unique identifier
    for each planning unit.

-   [**monitoring_cost**]{style="color: #4385CD;"}<br> `numeric` cost of
    including each planning unit in the reserve system.

-   [**status** (optional)]{style="color: #4385CD;"}<br> `integer` value
    that indicate if each planning unit should be available to be
    selected (`0`), locked-in (`2`) as part of the solution, or
    locked-out (`3`) and excluded from the solution.
:::

::: {style="float: right; width: 40%;"}

```r
## id monitoring_cost status
## 1                2      0
## 2                2      0
## 3                2      0
## 4                2      0
## 5                2      0
## 6                2      0
```
:::

## #1 Planning units data (`pu`)

### [**id**]{style="color: #4385CD;"}<br>

<br>

<center>![Game, E. T. y H. S. Grantham. (2008). Manual del Usuario de
Marxan: Para la versión Marxan 1.8.10](Figures/pu.png){.class
width="70%" height="70%"}</center>

## #1 Planning units data (`pu`)

### [**monitoring_cost**]{style="color: #4385CD;"}<br>

<center>![Source: https://marxansolutions.org](Figures/costs.png){.class
width="50%" height="50%"}</center>

## #1 Planning units data (`pu`)

### [**status** (optional)]{style="color: #4385CD;"}<br>

-   free (`0`)
-   locked-in (`2`)
-   locked-out (`3`)

<center>![Source:
https://marxansolutions.org](Figures/status.png){.class width="40%"
height="40%"}</center>

## #2 Features data (`features`)

#### `data.frame`

<br> Specifies the **conservation features** to consider in the
optimization problem. Each row corresponds to a different feature. This
file is inherited from the *spec.dat* in Marxan. <br> <br>

::: {style="float: left; width: 50%;"}
-   [**id**]{style="color: #4385CD;"}<br> `integer` unique identifier
    for each conservation feature.

-   [**target_recovery**]{style="color: #4385CD;"}<br> `numeric` amount
    of recovery target to achieve for each conservation feature.

-   [**target_conservation** (optional)]{style="color: #4385CD;"}<br>
    `numeric` amount of conservation target to achieve for each
    conservation feature.

-   [**name** (optional)]{style="color: #4385CD;"}<br> `character` name
    for each conservation feature.
:::

::: {style="float: right; width: 40%;"}

```r
##id target_recovery     name
## 1              11 feature1
## 2              16 feature2
## 3               8 feature3
## 4               9 feature4
```
:::

## #2 Features data (`features`)

### [**id**]{style="color: #4385CD;"} (Species) <br>

<center>![Source:
https://gefespeciesamenazadas.mma.gob.cl](Figures/species.jpg){.class
width="45%" height="45%"}</center>

## #2 Features data (`features`)

### [**id**]{style="color: #4385CD;"} (Ecosystem Services)<br>

<center>![Source:
https://ecology.fnal.gov](Figures/servicios.jpg){.class width="45%"
height="45%"}</center>

## #2 Features data (`features`)

### [**id**]{style="color: #4385CD;"} (Habitats) <br>

<br>

<center>![Source:
https://www.asemafor.cl/areas-protegidas-en-chile](Figures/habitats.jpg){.class
width="70%" height="70%"}</center>

## #2 Features data (`features`)

### [**recovery_target**]{style="color: #4385CD;"}<br>

<br>

<center>![](Figures/t.png){.class width="50%"
height="50%"}</center>

## #2 Features data (`features`)

### [**conservation_target (optional)**]{style="color: #4385CD;"}<br>

::: {style="float: left; width: 50%;"}
<br>

<center>![](Figures/target1.png){.class width="100%"
height="100%"}</center>
:::

::: {style="float: left; width: 50%;"}
<center>![](Figures/target2.png){.class width="100%"
height="100%"}</center>
:::

<center>Source: <https://marxansolutions.org></center>

## #3 Distribution Features data (`dist_features`)

#### `data.frame`

<br> Specifies the **spatial distribution of conservation features**
across planning units. Each row corresponds to a combination of
`planning unit` and `feature`. <br> <br>

::: {style="float: left; width: 50%;"}
-   [**pu**]{style="color: #4385CD;"}<br> `integer` *id* of a planning
    unit where the conservation feature listed on the same row occurs.

-   [**feature**]{style="color: #4385CD;"}<br> `integer` *id* of each
    conservation feature.

-   [**amount**]{style="color: #4385CD;"}<br> `numeric` amount of the
    feature in the planning unit. Set to `1` to work with
    presence/absence.
:::

::: {style="float: right; width: 30%;"}

```r
#pu feature amount
#1        3      1
#2        3      1
#3        3      1
#4        3      1
#5        3      1
#6        3      1
```
:::

## #3 Distribution Features data (`dist_features`)

### [**amount (binaries)**]{style="color: #4385CD;"}<br>

<center>![Source: https://calumma.typepad.com](Figures/sd1.png){.class
width="50%" height="50%"}</center>

## #3 Distribution Features data (`dist_features`)

### [**amount (continuous)**]{style="color: #4385CD;"}<br>

<center>![Source: https://ib.bioninja.com.au](Figures/sd2.jpeg){.class
width="70%" height="70%"}</center>

## #4 Threats data (`threats`)

#### `data.frame`

<br> Specifies the **threats** to consider in the optimization exercise.
Each row corresponds to a different `threats`. <br> <br>

::: {style="float: left; width: 50%;"}
-   [**id**]{style="color: #4385CD;"}<br> `integer` unique identifier
    for each threat.

-   [**blm_actions** (optional)]{style="color: #4385CD;"}<br> `numeric`
    penalty of connectivity between threats. Default is `0`.

-   [**name** (optional)]{style="color: #4385CD;"}<br> `character`
    \`name for each conservation feature.
:::

::: {style="float: right; width: 35%;"}

```r
#>id    name blm_actions
#> 1 threat1           0
#> 2 threat2           0
```
:::

## #4 Threats data (`threats`)

### [**blm_actions**]{style="color: #4385CD;"}

<center>![Source:
https://docs.marxanweb.org](Figures/connectivity2.png){.class
width="50%" height="50%"}</center>

## #5 Distribution Threats data (`dist_threats`)

#### `data.frame`

<br> specifies the **spatial distribution of threats** across planning
units. Each row corresponds to a combination of `planning unit` and
`threat`. <br> <br>

::: {style="float: left; width: 60%;"}
-   [**pu**]{style="color: #4385CD;"}<br> `integer` *id* of a planning
    unit where the threat listed on the same row occurs.

-   [**threat**]{style="color: #4385CD;"}<br> `integer` *id* of each
    conservation feature.

-   [**amount**]{style="color: #4385CD;"}<br> `numeric` amount of the
    threat in the planning unit. Set to `1` to work with
    presence/absence. Continuous amount values require that feature
    `sensitivities` to threats be established.
:::

::: {style="float: right; width: 30%;"}

```r
#>pu threat amount action_cost status
#> 8      2      1           2      0
#> 9      2      1           2      0
#>10      2      1           2      0
#>11      1      1           3      0
#>11      2      1           4      0
#>12      1      1           3      0
```
:::

## #5 Distribution Threats data (`dist_threats`)

#### `data.frame`

<br> specifies the **spatial distribution of threats** across planning
units. Each row corresponds to a combination of `planning unit` and
`threat`. <br> <br>

-   [**action_cost**]{style="color: #4385CD;"}<br> `numeric` cost of an
    action to abate the threat in each planning unit.

-   [**status** (optional)]{style="color: #4385CD;"}<br> `integer` value
    that indicate if each planning unit should be available to be
    selected (`0`), locked-in (`2`) as part of the solution, or
    locked-out (`3`) and excluded from the solution. :::

## #5 Distribution Threats data (`dist_threats`)

### [**amount (continuous)**]{style="color: #4385CD;"}<br>

<center>![Source:
https://ib.bioninja.com.au](Figures/threats.jpg){.class width="70%"
height="70%"}</center>

## #6 Sensitivity data (`sensitivity`)

### Optional

#### `data.frame`

<br> specifies the **sensitivity** of each `feature` to each `threat`.
Each row corresponds to a combination of `feature` and `threat`. <br>
<br>

::: {style="float: left; width: 70%;"}
-   [**feature**]{style="color: #4385CD;"}<br> `integer` *id* of each
    conservation feature.

-   [**threat**]{style="color: #4385CD;"}<br> `integer` *id* of each
    threat.

-   [**a** (optional)]{style="color: #4385CD;"}<br> `numeric` the
    minimum intensity of the threat at which the features probability of
    persistence starts to decline.

-   [**b** (optional)]{style="color: #4385CD;"}<br> `numeric` the value
    of intensity of the threat over which the feature has a probability
    of persistence of `0`.
:::

::: {style="float: right; width: 30%;"}

```r
#>feature threat
#>      1      1
#>      2      1
#>      3      1
#>      4      1
#>      1      2
#>      2      2
```
:::

## #6 Sensitivity data (`sensitivity`)

### Optional

#### `data.frame`

<br> specifies the **sensitivity** of each `feature` to each `threat`.
Each row corresponds to a combination of `feature` and `threat`. <br>
<br>

-   [**c** (optional)]{style="color: #4385CD;"}<br> `numeric` minimum
    probability of persistence of a features when a threat reaches its
    maximum intensity value.

-   [**d** (optional)]{style="color: #4385CD;"}<br> `numeric` maximum
    probability of persistence of a features in absence threats. :::

## #6 Sensitivity data (`sensitivity`)

### [**a, b, c and d**]{style="color: #4385CD;"}<br>

<center>![](Figures/sensitivity1.png){.class width="70%"
height="70%"}</center>

## #6 Sensitivity data (`sensitivity`)

### [**a, b, c and d**]{style="color: #4385CD;"}<br>

<center>![](Figures/sensitivities2.jpg){.class width="60%"
height="60%"}</center>

## #7 Boundary data (`boundary`)

### Optional

#### `data.frame`

<br> Specifies the **spatial relationship** between pair of
`planning units`. Each row corresponds to a combination of
`planning unit`. <br> <br>

::: {style="float: left; width: 60%;"}
-   [**id1**]{style="color: #4385CD;"}<br> `integer` *id* of each
    planning unit.

-   [**id2**]{style="color: #4385CD;"}<br> `integer` *id* of each
    planning unit.

-   [**boundary**]{style="color: #4385CD;"}<br> `numeric` penalty
    applied in the objective function when only one of the planning
    units is present in the solution.
:::

::: {style="float: right; width: 30%;"}

```r
#>id1 id2 boundary
#>  1   1        0
#>  2   1        1
#>  3   1        2
#>  4   1        3
#>  5   1        4
#>  6   1        5
```
:::

## Workflow

<center>![](Figures/Diagram2.png){.class width="80%"
height="80%"}</center>

## [**Q3.**]{style="color: #B93600;"} Optimization objective?

#### Minimize Costs (**`minimizeCosts`**) vs Maximize Benefits (**`maximizeBenefits`**)

<br>

<center>![](Figures/mincost.png){.class width="80%"
height="80%"}</center>

$x_{ik}$ is the decisions variable that specifies whether an action to
abate threat $k$ in planning unit $i$ has been selected $(1)$ or not
$(0)$.

$c_{ik}$ is the cost of the action to abate the threat $k$ in the
planning unit $i$.

$b_{is}(x)$ is the benefit of the feature $s$ in the planning unit $i$
(ranging between $0$ and $1$).

$t_s$ is the recovery target for feature $s$.

$f(x)$ is the function of connectivity penalty.

## [**Q3.**]{style="color: #B93600;"} Optimization objective?

#### Minimize Costs (**`minimizeCosts`**) vs Maximize Benefits (**`maximizeBenefits`**)

<br>

<center>![](Figures/maxben.png){.class width="80%"
height="80%"}</center>

$x_{ik}$ is the decisions variable that specifies whether an action to
abate threat $k$ in planning unit $i$ has been selected $(1)$ or not
$(0)$.

$c_{ik}$ is the cost of the action to abate the threat $k$ in the
planning unit $i$.

$b_{is}(x)$ is the benefit of the feature $s$ in the planning unit $i$
(ranging between $0$ and $1$).

$B$ is the budget available.

$f(x)$ is the function of connectivity penalty.

## Workflow

<center>![](Figures/Diagram2.png){.class width="80%"
height="80%"}</center>

# II) Create mathematical model

## Create the model (`problem()`)

Create an **optimization model** for the multi-action conservation
planning problem, following the mathematical formulations used in
Salgado-Rojas et al. (2020).

<br>


```r
problem(
  x,
  model_type = "minimizeCosts",
  budget = 0,
  blm = 0
)
```

## Workflow

<center>![](Figures/Diagram2.png){.class width="80%"
height="80%"}</center>

# III) Solve model

## Solve model (`solve()`)

Solves the **optimization model** associated with the multi-action
conservation planning problem. This function is used to solve the
mathematical model created by the `problem()` function.

<br>


```r
solve(
  a,
  solver = "",
  gap_limit = 0,
  time_limit = .Machine$integer.max,
  solution_limit = FALSE,
  cores = 2,
  verbose = TRUE,
  name_output_file = "output",
  output_file = TRUE
)
```

## Solve model (`solve()`)

### [**solver**]{style="color: #4385CD;"} (`gurobi, cplex, symphony`)<br>

<br>

<div>

<center>

**Rsymphony** <br>

<center>

<img src="Figures/cplex-gurobi.webp" style="vertical-align:middle" width="50%" height="50%"/>

</div>

## Solve model (`solve()`)

### [**gap limit**]{style="color: #4385CD;"} (percentage) <br>

<center>![Source: https://www.math.uwaterloo.ca](Figures/gap.jpg){.class
width="60%" height="60%"}</center>

## Solve model (`solve()`)

### [**time limit**]{style="color: #4385CD;"} (seconds) <br>

### [**solution_limit**]{style="color: #4385CD;"} (First solution?) <br>

### [**verbose**]{style="color: #4385CD;"} (Information on screen?) <br>

## Overview

<center>![](Figures/Diagram2.png){.class width="80%"
height="80%"}</center>

# Getting information of solutions

## `getActions()`

Returns the spatial deployment of the **actions for each planning unit**
of the corresponding solution.

<br>


```r
getActions(x, format = "wide")
```

Output example,


```r
##   solution_name pu 1 2 conservation connectivity
## 1           sol  1 0 0            0            0
## 2           sol  2 0 0            0            0
## 3           sol  3 0 0            0            0
## 4           sol  4 0 0            0            0
## 5           sol  5 0 0            0            0
## 6           sol  6 0 0            0            0
```

## `getSolutionBenefit()`

Returns the **total benefit** induced by the corresponding solution. The
total benefit is computed as the sum of the benefits obtained, for all
features, across all the units in the planning area.

<br>


```r
getSolutionBenefit(x, type = "total")
```

Output example,


```r
##   solution_name feature benefit.conservation benefit.recovery benefit.total
## 1           sol       1                    0               11            11
## 2           sol       2                    0               16            16
## 3           sol       3                    0               10            10
## 4           sol       4                    0                9             9
```

## `getCost()`

Provides the sum of **costs** to actions and monitoring applied in a
solution.

<br>


```r
getCost(x)
```

Output example,


```r
##   solution_name monitoring threat_1 threat_2
## 1           sol         61       20       65
```

# Outline

-   Features

-   ::: white
    **Toy example**
    :::

-   Overview

-   Actual case study: The Mitchell River, Australia

-   Shiny Application

## Package installation and documentation

<!-- badges: start -->

![CRAN/METACRAN](https://www.r-pkg.org/badges/version/prioriactions)
[![Lifecycle:
stable](https://img.shields.io/badge/lifecycle-stable-brightgreen.svg)](https://lifecycle.r-lib.org/articles/stages.html#stable)
[![R-CMD-check](https://github.com/prioriactions/prioriactions/workflows/R-CMD-check/badge.svg)](https://github.com/prioriactions/prioriactions/actions)
[![](https://cranlogs.r-pkg.org/badges/grand-total/prioriactions)](https://cran.rstudio.com/web/packages/prioriactions/index.html)

<!-- badges: end -->

Package `prioriactions` can be found at
[CRAN](https://cloud.r-project.org/web/packages/prioriactions/index.html),
so it can be installed using:


```r
install.packages("prioriactions")
```

Latest stable versions can be downloaded and installed from GitHub as
follows (package `remotes` should be installed first):


```r
if (!require(remotes)) install.packages("remotes")
remotes::install_github("prioriactions/prioriactions")
```

So, we load the `prioriactions` package.


```r
# load package
library(prioriactions)
```

## Toy Example

##### (Available in the [Get Started](https://prioriactions.github.io/prioriactions/articles/prioriactions.html) vignette on the `prioriactions` website)

<br />

-   This example contains **100 planning units**, **4 features** and **2
    threats**.

-   The distribution of features and threats can be plotted on a grid of
    10 x 10 units.

-   `prioriactions` contains this simulated example inside the setup
    files. You can extract it by the `data()` function.

## #1 Planning units data

The monitoring cost values ranging from 1 to 10 and all status of 0 (not
locked).


```r
# load planning unit data from prioriactions
data(sim_pu_data) #To load simulated data

#head(sim_pu_data)
```


```{=html}
<div id="htmlwidget-d148742d43cc73cc97e4" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-d148742d43cc73cc97e4">{"x":{"filter":"none","vertical":false,"data":[[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100],[2,2,2,2,2,2,2,2,1,1,3,3,3,3,3,3,2,2,2,2,4,4,4,4,4,3,3,3,2,2,4,4,5,4,4,4,4,3,3,3,5,5,5,5,5,5,4,4,3,3,6,6,6,6,6,5,5,4,4,4,7,7,7,7,6,6,5,5,4,4,8,8,8,8,7,6,6,5,5,4,9,9,9,8,8,7,6,6,5,5,10,10,10,9,9,8,7,6,5,5],[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th>id<\/th>\n      <th>monitoring_cost<\/th>\n      <th>status<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"pageLength":5,"columnDefs":[{"className":"dt-right","targets":[0,1,2]}],"order":[],"autoWidth":false,"orderClasses":false,"lengthMenu":[5,10,25,50,100]}},"evals":[],"jsHooks":[]}</script>
```

## #1 Planning units data

A `RasterLayer` object can be used to present this spatial information.
The pixel values correspond to the **monitoring costs** of each planning
unit.


```r
library(raster) #To plot rasters

r <- raster(ncol=10, nrow=10, xmn=0, xmx=10, ymn=0, ymx=10)
values(r) <- sim_pu_data$monitoring_cost
plot(r)
```

<img src="Workshop_prioriactions_IEB_2022_files/figure-html/toyExample6-1.png" width="60%" />

## #2 Features data

Contains information about the **features** such as its *id* and
*targets* (mandatory when `minimizing costs`).


```r
# load features data from prioriactions
data(sim_features_data)

#head(sim_features_data)
```


```{=html}
<div id="htmlwidget-1694d99c1a41d42764c5" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-1694d99c1a41d42764c5">{"x":{"filter":"none","vertical":false,"data":[[1,2,3,4],[40,20,50,30],["feature1","feature2","feature3","feature4"]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th>id<\/th>\n      <th>target_recovery<\/th>\n      <th>name<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"pageLength":5,"columnDefs":[{"className":"dt-right","targets":[0,1]}],"order":[],"autoWidth":false,"orderClasses":false,"lengthMenu":[5,10,25,50,100]}},"evals":[],"jsHooks":[]}</script>
```

## #3 Features distribution data

Contains information on the **spatial distribution of these features**
across planning units.


```r
# load features data from prioriactions
data(sim_dist_features_data)

#head(sim_features_data)
```


```{=html}
<div id="htmlwidget-65ccbb4e0f90d9992da8" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-65ccbb4e0f90d9992da8">{"x":{"filter":"none","vertical":false,"data":[[1,2,3,4,5,6,7,7,8,8,9,9,10,10,11,11,12,13,14,15,16,17,17,18,18,19,19,20,20,21,21,22,22,23,24,25,26,27,27,28,28,29,29,30,30,31,31,32,32,33,34,35,36,37,38,38,39,39,40,40,41,41,42,42,43,43,44,45,46,47,48,48,49,49,50,50,51,51,52,52,53,53,54,54,55,56,56,57,57,58,58,58,59,59,59,60,60,60,60,61,61,62,62,63,63,64,64,65,65,66,66,67,68,68,69,69,69,70,70,70,71,72,73,74,74,75,75,76,77,78,78,79,79,79,80,80,80,81,82,83,84,84,85,85,86,86,87,87,88,88,89,89,89,90,90,90,91,92,93,93,94,94,95,95,96,96,97,97,98,98,99,99,99,100,100,100],[3,3,3,3,3,3,2,3,2,3,2,3,2,3,1,3,3,3,3,3,3,2,3,2,3,2,3,2,3,1,3,1,3,3,3,3,3,2,3,2,3,2,3,2,3,1,3,1,3,3,3,3,3,3,2,3,2,3,2,3,1,3,1,3,1,3,3,3,3,3,2,3,2,3,2,3,1,3,1,3,1,3,1,3,3,3,4,3,4,2,3,4,2,3,4,1,2,3,4,1,3,1,3,1,3,1,3,3,4,3,4,4,2,4,1,2,4,1,2,4,1,1,1,1,4,1,4,4,4,1,4,1,2,4,1,2,4,1,1,1,1,4,1,4,1,4,1,4,1,4,1,2,4,1,2,4,1,1,1,4,1,4,1,4,1,4,1,4,1,4,1,2,4,1,2,4],[1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th>pu<\/th>\n      <th>feature<\/th>\n      <th>amount<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"pageLength":5,"columnDefs":[{"className":"dt-right","targets":[0,1,2]}],"order":[],"autoWidth":false,"orderClasses":false,"lengthMenu":[5,10,25,50,100]}},"evals":[],"jsHooks":[]}</script>
```

## #3 Features distribution data

To plot the spatial distribution of the first feature,


```r
# load amount of features data
features <- reshape2::dcast(sim_dist_features_data, 
                            pu~feature,
                            value.var = "amount", 
                            fill = 0)

values(r) <- features$`1`
plot(r)
```

![](Workshop_prioriactions_IEB_2022_files/figure-html/toyExample10_1-1.png)<!-- -->

## #3 Features distribution data

<center>![](Figures/toyexample_1.png){.class width="80%"
height="80%"}</center>

## #4 Threats data

Provides information about the threats such as their `id` and `name`.


```r
# load threats data from prioriactions
data(sim_threats_data)
```


```{=html}
<div id="htmlwidget-8e9d563e864a99104c17" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-8e9d563e864a99104c17">{"x":{"filter":"none","vertical":false,"data":[[1,2],["threat1","threat2"],[0,0]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th>id<\/th>\n      <th>name<\/th>\n      <th>blm_actions<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"pageLength":5,"columnDefs":[{"className":"dt-right","targets":[0,2]}],"order":[],"autoWidth":false,"orderClasses":false,"lengthMenu":[5,10,25,50,100]}},"evals":[],"jsHooks":[]}</script>
```

## #5 Threats distribution data

Provides information on the **spatial distribution** of these threats


```r
# load threats data from prioriactions
data(sim_dist_threats_data)
```


```{=html}
<div id="htmlwidget-71bc76bb700b15c70369" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-71bc76bb700b15c70369">{"x":{"filter":"none","vertical":false,"data":[[8,9,10,11,11,12,12,13,13,14,14,16,17,19,20,21,22,23,23,24,24,25,25,26,27,28,29,30,31,31,32,33,33,34,35,35,37,38,39,40,41,41,42,42,43,43,44,44,45,45,46,46,47,48,49,50,51,51,52,52,53,53,54,54,55,55,56,56,57,57,58,59,60,61,61,62,62,63,63,64,64,65,65,66,66,67,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100],[2,2,2,1,2,1,2,1,2,1,2,2,2,2,2,2,1,1,2,1,2,1,2,2,2,2,2,2,1,2,2,1,2,2,1,2,2,2,2,2,1,2,1,2,1,2,1,2,1,2,1,2,2,2,2,2,1,2,1,2,1,2,1,2,1,2,1,2,1,2,2,2,2,1,2,1,2,1,2,1,2,1,2,1,2,1,2,2,2,2,1,1,1,1,1,1,1,1,2,2,1,1,1,1,1,1,1,1,1,2,1,1,1,1,1,1,1,1,1,2],[1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1],[2,2,2,3,4,3,3,4,2,4,2,1,1,2,2,4,2,3,3,4,2,5,2,2,2,2,2,2,1,5,4,2,3,3,4,2,2,2,2,2,1,5,2,4,2,3,3,3,4,3,5,3,3,3,3,3,1,5,2,4,2,4,3,3,4,3,5,3,7,4,4,4,4,1,4,2,4,3,3,4,3,5,3,6,4,7,4,5,5,6,2,3,4,4,5,6,7,8,7,7,3,4,5,6,6,7,8,8,9,9,5,5,6,7,8,8,9,9,9,10],[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th>pu<\/th>\n      <th>threat<\/th>\n      <th>amount<\/th>\n      <th>action_cost<\/th>\n      <th>status<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"pageLength":5,"columnDefs":[{"className":"dt-right","targets":[0,1,2,3,4]}],"order":[],"autoWidth":false,"orderClasses":false,"lengthMenu":[5,10,25,50,100]}},"evals":[],"jsHooks":[]}</script>
```

## #5 Threats distribution data

<center>![](Figures/toyexample_2.png){.class width="80%"
height="80%"}</center>

## #6 Sensitivity data

Indicates which features is sensitive to what threat.


```r
# load threats data from prioriactions
data(sim_sensitivity_data)
```


```{=html}
<div id="htmlwidget-dd2acb29edeb6d28ff0a" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-dd2acb29edeb6d28ff0a">{"x":{"filter":"none","vertical":false,"data":[[1,2,3,4,1,2,3,4],[1,1,1,1,2,2,2,2]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th>feature<\/th>\n      <th>threat<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"pageLength":5,"columnDefs":[{"className":"dt-right","targets":[0,1]}],"order":[],"autoWidth":false,"orderClasses":false,"lengthMenu":[5,10,25,50,100]}},"evals":[],"jsHooks":[]}</script>
```

## #7 Boundary data

Provides information on the **spatial relationship** between PU and they
are presented in long format.


```r
# load boundary data from prioriactions
data(sim_boundary_data)
```


```{=html}
<div id="htmlwidget-2f123918eeb264df8edc" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-2f123918eeb264df8edc">{"x":{"filter":"none","vertical":false,"data":[[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100],[1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,6,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,7,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,9,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,11,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,12,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,13,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,14,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,15,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,16,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,18,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,19,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,20,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,21,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,22,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,23,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,24,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,25,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,26,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,27,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,28,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,29,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,30,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,31,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,32,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,33,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,34,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,35,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,36,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,37,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,38,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,39,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,40,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,41,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,42,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,43,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,44,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,45,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,46,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,47,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,48,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,49,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,50,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,51,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,52,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,53,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,54,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,55,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,56,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,57,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,58,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,59,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,60,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,61,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,62,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,63,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,64,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,65,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,66,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,67,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,68,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,69,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,70,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,71,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,72,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,73,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,74,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,75,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,76,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,77,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,78,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,79,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,80,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,81,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,82,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,83,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,84,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,85,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,86,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,87,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,88,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,89,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,90,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,91,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,92,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,93,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,94,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,95,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,96,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,97,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,98,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,99,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100,100],[0,1,2,3,4,5,6,7,8,9,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,9.05538513813742,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,9.21954445729289,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,9.48683298050514,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,9.8488578017961,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,10.295630140987,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,10,10.816653826392,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,9.89949493661167,10.6301458127346,11.4017542509914,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,10,10.6301458127346,11.3137084989848,12.0415945787923,9,9.05538513813742,9.21954445729289,9.48683298050514,9.8488578017961,10.295630140987,10.816653826392,11.4017542509914,12.0415945787923,12.7279220613579,1,0,1,2,3,4,5,6,7,8,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,10,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,9.89949493661167,10.6301458127346,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,10,10.6301458127346,11.3137084989848,9.05538513813742,9,9.05538513813742,9.21954445729289,9.48683298050514,9.8488578017961,10.295630140987,10.816653826392,11.4017542509914,12.0415945787923,2,1,0,1,2,3,4,5,6,7,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,9.89949493661167,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,10,10.6301458127346,9.21954445729289,9.05538513813742,9,9.05538513813742,9.21954445729289,9.48683298050514,9.8488578017961,10.295630140987,10.816653826392,11.4017542509914,3,2,1,0,1,2,3,4,5,6,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,10,9.48683298050514,9.21954445729289,9.05538513813742,9,9.05538513813742,9.21954445729289,9.48683298050514,9.8488578017961,10.295630140987,10.816653826392,4,3,2,1,0,1,2,3,4,5,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,9.8488578017961,9.48683298050514,9.21954445729289,9.05538513813742,9,9.05538513813742,9.21954445729289,9.48683298050514,9.8488578017961,10.295630140987,5,4,3,2,1,0,1,2,3,4,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,10.295630140987,9.8488578017961,9.48683298050514,9.21954445729289,9.05538513813742,9,9.05538513813742,9.21954445729289,9.48683298050514,9.8488578017961,6,5,4,3,2,1,0,1,2,3,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,10,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,10.816653826392,10.295630140987,9.8488578017961,9.48683298050514,9.21954445729289,9.05538513813742,9,9.05538513813742,9.21954445729289,9.48683298050514,7,6,5,4,3,2,1,0,1,2,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,9.89949493661167,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,10.6301458127346,10,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,11.4017542509914,10.816653826392,10.295630140987,9.8488578017961,9.48683298050514,9.21954445729289,9.05538513813742,9,9.05538513813742,9.21954445729289,8,7,6,5,4,3,2,1,0,1,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,10,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,10.6301458127346,9.89949493661167,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,11.3137084989848,10.6301458127346,10,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,12.0415945787923,11.4017542509914,10.816653826392,10.295630140987,9.8488578017961,9.48683298050514,9.21954445729289,9.05538513813742,9,9.05538513813742,9,8,7,6,5,4,3,2,1,0,9.05538513813742,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,9.21954445729289,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,9.48683298050514,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,9.8488578017961,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,10.295630140987,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,10.816653826392,10,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,11.4017542509914,10.6301458127346,9.89949493661167,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,12.0415945787923,11.3137084989848,10.6301458127346,10,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,12.7279220613579,12.0415945787923,11.4017542509914,10.816653826392,10.295630140987,9.8488578017961,9.48683298050514,9.21954445729289,9.05538513813742,9,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,9.05538513813742,0,1,2,3,4,5,6,7,8,9,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,9.05538513813742,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,9.21954445729289,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,9.48683298050514,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,9.8488578017961,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,10.295630140987,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,10,10.816653826392,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,9.89949493661167,10.6301458127346,11.4017542509914,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,10,10.6301458127346,11.3137084989848,12.0415945787923,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,1,0,1,2,3,4,5,6,7,8,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,10,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,9.89949493661167,10.6301458127346,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,10,10.6301458127346,11.3137084989848,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,2,1,0,1,2,3,4,5,6,7,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,9.89949493661167,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,10,10.6301458127346,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,3,2,1,0,1,2,3,4,5,6,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,10,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,4,3,2,1,0,1,2,3,4,5,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5,4,3,2,1,0,1,2,3,4,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,6,5,4,3,2,1,0,1,2,3,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,10,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,7,6,5,4,3,2,1,0,1,2,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,9.89949493661167,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,10.6301458127346,10,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,8,7,6,5,4,3,2,1,0,1,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,10,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,10.6301458127346,9.89949493661167,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,11.3137084989848,10.6301458127346,10,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,9.05538513813742,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,9,8,7,6,5,4,3,2,1,0,9.05538513813742,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,9.21954445729289,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,9.48683298050514,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,9.8488578017961,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,10.295630140987,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,10.816653826392,10,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,11.4017542509914,10.6301458127346,9.89949493661167,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,12.0415945787923,11.3137084989848,10.6301458127346,10,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,9.21954445729289,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,9.05538513813742,0,1,2,3,4,5,6,7,8,9,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,9.05538513813742,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,9.21954445729289,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,9.48683298050514,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,9.8488578017961,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,10.295630140987,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,10,10.816653826392,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,9.89949493661167,10.6301458127346,11.4017542509914,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,1,0,1,2,3,4,5,6,7,8,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,10,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,9.89949493661167,10.6301458127346,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,2,1,0,1,2,3,4,5,6,7,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,9.89949493661167,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,3,2,1,0,1,2,3,4,5,6,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,4,3,2,1,0,1,2,3,4,5,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5,4,3,2,1,0,1,2,3,4,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,6,5,4,3,2,1,0,1,2,3,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,7,6,5,4,3,2,1,0,1,2,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,9.89949493661167,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,8,7,6,5,4,3,2,1,0,1,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,10,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,10.6301458127346,9.89949493661167,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,9.21954445729289,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,9.05538513813742,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,9,8,7,6,5,4,3,2,1,0,9.05538513813742,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,9.21954445729289,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,9.48683298050514,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,9.8488578017961,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,10.295630140987,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,10.816653826392,10,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,11.4017542509914,10.6301458127346,9.89949493661167,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,9.48683298050514,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,9.21954445729289,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,9.05538513813742,0,1,2,3,4,5,6,7,8,9,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,9.05538513813742,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,9.21954445729289,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,9.48683298050514,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,9.8488578017961,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,10.295630140987,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,10,10.816653826392,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,1,0,1,2,3,4,5,6,7,8,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,10,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,2,1,0,1,2,3,4,5,6,7,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,3,2,1,0,1,2,3,4,5,6,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,4,3,2,1,0,1,2,3,4,5,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5,4,3,2,1,0,1,2,3,4,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,6,5,4,3,2,1,0,1,2,3,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,7,6,5,4,3,2,1,0,1,2,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,8,7,6,5,4,3,2,1,0,1,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,10,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,9.48683298050514,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,9.21954445729289,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,9.05538513813742,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,9,8,7,6,5,4,3,2,1,0,9.05538513813742,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,9.21954445729289,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,9.48683298050514,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,9.8488578017961,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,10.295630140987,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,10.816653826392,10,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,9.8488578017961,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,9.48683298050514,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,9.21954445729289,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,9.05538513813742,0,1,2,3,4,5,6,7,8,9,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,9.05538513813742,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,9.21954445729289,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,9.48683298050514,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,9.8488578017961,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,10.295630140987,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,1,0,1,2,3,4,5,6,7,8,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,2,1,0,1,2,3,4,5,6,7,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,3,2,1,0,1,2,3,4,5,6,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,4,3,2,1,0,1,2,3,4,5,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5,4,3,2,1,0,1,2,3,4,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,6,5,4,3,2,1,0,1,2,3,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,7,6,5,4,3,2,1,0,1,2,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,8,7,6,5,4,3,2,1,0,1,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,9.8488578017961,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,9.48683298050514,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,9.21954445729289,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,9.05538513813742,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,9,8,7,6,5,4,3,2,1,0,9.05538513813742,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,9.21954445729289,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,9.48683298050514,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,9.8488578017961,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,10.295630140987,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,10.295630140987,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,9.8488578017961,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,9.48683298050514,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,9.21954445729289,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,9.05538513813742,0,1,2,3,4,5,6,7,8,9,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,9.05538513813742,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,9.21954445729289,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,9.48683298050514,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,9.8488578017961,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,1,0,1,2,3,4,5,6,7,8,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,2,1,0,1,2,3,4,5,6,7,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,3,2,1,0,1,2,3,4,5,6,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,4,3,2,1,0,1,2,3,4,5,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5,4,3,2,1,0,1,2,3,4,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,6,5,4,3,2,1,0,1,2,3,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,7,6,5,4,3,2,1,0,1,2,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,8,7,6,5,4,3,2,1,0,1,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,10.295630140987,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,9.8488578017961,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,9.48683298050514,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,9.21954445729289,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,9.05538513813742,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,9,8,7,6,5,4,3,2,1,0,9.05538513813742,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,9.21954445729289,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,9.48683298050514,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,9.8488578017961,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,10,10.816653826392,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,10.295630140987,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,9.8488578017961,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,9.48683298050514,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,9.21954445729289,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,9.05538513813742,0,1,2,3,4,5,6,7,8,9,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,9.05538513813742,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,9.21954445729289,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,9.48683298050514,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,10,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,1,0,1,2,3,4,5,6,7,8,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,2,1,0,1,2,3,4,5,6,7,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,3,2,1,0,1,2,3,4,5,6,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,4,3,2,1,0,1,2,3,4,5,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5,4,3,2,1,0,1,2,3,4,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,6,5,4,3,2,1,0,1,2,3,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,7,6,5,4,3,2,1,0,1,2,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,10,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,8,7,6,5,4,3,2,1,0,1,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,10.816653826392,10,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,10.295630140987,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,9.8488578017961,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,9.48683298050514,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,9.21954445729289,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,9.05538513813742,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,9,8,7,6,5,4,3,2,1,0,9.05538513813742,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,9.21954445729289,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,9.48683298050514,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,9.89949493661167,10.6301458127346,11.4017542509914,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,10,10.816653826392,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,10.295630140987,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,9.8488578017961,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,9.48683298050514,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,9.21954445729289,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,9.05538513813742,0,1,2,3,4,5,6,7,8,9,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,9.05538513813742,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,9.21954445729289,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,9.89949493661167,10.6301458127346,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,10,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,1,0,1,2,3,4,5,6,7,8,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,9.89949493661167,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,2,1,0,1,2,3,4,5,6,7,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,3,2,1,0,1,2,3,4,5,6,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,4,3,2,1,0,1,2,3,4,5,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5,4,3,2,1,0,1,2,3,4,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,6,5,4,3,2,1,0,1,2,3,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,9.89949493661167,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,7,6,5,4,3,2,1,0,1,2,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,10.6301458127346,9.89949493661167,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,10,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,8,7,6,5,4,3,2,1,0,1,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,11.4017542509914,10.6301458127346,9.89949493661167,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,10.816653826392,10,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,10.295630140987,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,9.8488578017961,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,9.48683298050514,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,9.21954445729289,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,9.05538513813742,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,9,8,7,6,5,4,3,2,1,0,9.05538513813742,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,9.21954445729289,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,10,10.6301458127346,11.3137084989848,12.0415945787923,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,9.89949493661167,10.6301458127346,11.4017542509914,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,10,10.816653826392,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,10.295630140987,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,9.8488578017961,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,9.48683298050514,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,9.21954445729289,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,9.05538513813742,0,1,2,3,4,5,6,7,8,9,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,9.05538513813742,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,10,10.6301458127346,11.3137084989848,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,9.89949493661167,10.6301458127346,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,10,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,1,0,1,2,3,4,5,6,7,8,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,10,10.6301458127346,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,9.89949493661167,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,2,1,0,1,2,3,4,5,6,7,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,10,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,3,2,1,0,1,2,3,4,5,6,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,4,3,2,1,0,1,2,3,4,5,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5,4,3,2,1,0,1,2,3,4,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,10,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,6,5,4,3,2,1,0,1,2,3,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,10.6301458127346,10,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,9.89949493661167,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,7,6,5,4,3,2,1,0,1,2,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,11.3137084989848,10.6301458127346,10,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,10.6301458127346,9.89949493661167,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,10,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,8,7,6,5,4,3,2,1,0,1,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,12.0415945787923,11.3137084989848,10.6301458127346,10,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,11.4017542509914,10.6301458127346,9.89949493661167,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,10.816653826392,10,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,10.295630140987,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,9.8488578017961,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,9.48683298050514,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,9.21954445729289,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,9.05538513813742,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,9,8,7,6,5,4,3,2,1,0,9.05538513813742,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,9,9.05538513813742,9.21954445729289,9.48683298050514,9.8488578017961,10.295630140987,10.816653826392,11.4017542509914,12.0415945787923,12.7279220613579,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,10,10.6301458127346,11.3137084989848,12.0415945787923,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,9.89949493661167,10.6301458127346,11.4017542509914,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,10,10.816653826392,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,10.295630140987,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,9.8488578017961,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,9.48683298050514,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,9.21954445729289,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,9.05538513813742,0,1,2,3,4,5,6,7,8,9,9.05538513813742,9,9.05538513813742,9.21954445729289,9.48683298050514,9.8488578017961,10.295630140987,10.816653826392,11.4017542509914,12.0415945787923,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,10,10.6301458127346,11.3137084989848,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,9.89949493661167,10.6301458127346,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,10,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,9.4339811320566,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,8.94427190999916,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,8.54400374531753,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,8.24621125123532,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,8.06225774829855,1,0,1,2,3,4,5,6,7,8,9.21954445729289,9.05538513813742,9,9.05538513813742,9.21954445729289,9.48683298050514,9.8488578017961,10.295630140987,10.816653826392,11.4017542509914,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,10,10.6301458127346,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,9.89949493661167,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,9.21954445729289,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,8.60232526704263,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,8.06225774829855,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,7.61577310586391,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,7.28010988928052,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,7.07106781186548,2,1,0,1,2,3,4,5,6,7,9.48683298050514,9.21954445729289,9.05538513813742,9,9.05538513813742,9.21954445729289,9.48683298050514,9.8488578017961,10.295630140987,10.816653826392,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,10,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,9.21954445729289,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,8.48528137423857,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,7.81024967590665,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,7.21110255092798,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,6.70820393249937,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,6.32455532033676,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,6.08276253029822,3,2,1,0,1,2,3,4,5,6,9.8488578017961,9.48683298050514,9.21954445729289,9.05538513813742,9,9.05538513813742,9.21954445729289,9.48683298050514,9.8488578017961,10.295630140987,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,9.4339811320566,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,8.60232526704263,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.81024967590665,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,7.07106781186548,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,6.40312423743285,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.8309518948453,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.3851648071345,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5.09901951359278,4,3,2,1,0,1,2,3,4,5,10.295630140987,9.8488578017961,9.48683298050514,9.21954445729289,9.05538513813742,9,9.05538513813742,9.21954445729289,9.48683298050514,9.8488578017961,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,8.94427190999916,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.06225774829855,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.21110255092798,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,6.40312423743285,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,5.65685424949238,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,5,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,4.47213595499958,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,4.12310562561766,5,4,3,2,1,0,1,2,3,4,10.816653826392,10.295630140987,9.8488578017961,9.48683298050514,9.21954445729289,9.05538513813742,9,9.05538513813742,9.21954445729289,9.48683298050514,10,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,8.54400374531753,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,7.61577310586391,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,6.70820393249937,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,5.8309518948453,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,5,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,4.24264068711928,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,3.60555127546399,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,3.16227766016838,6,5,4,3,2,1,0,1,2,3,11.4017542509914,10.816653826392,10.295630140987,9.8488578017961,9.48683298050514,9.21954445729289,9.05538513813742,9,9.05538513813742,9.21954445729289,10.6301458127346,10,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,8.24621125123532,9.89949493661167,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,7.28010988928052,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,6.32455532033676,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,5.3851648071345,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,4.47213595499958,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,3.60555127546399,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,2.82842712474619,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,2.23606797749979,7,6,5,4,3,2,1,0,1,2,12.0415945787923,11.4017542509914,10.816653826392,10.295630140987,9.8488578017961,9.48683298050514,9.21954445729289,9.05538513813742,9,9.05538513813742,11.3137084989848,10.6301458127346,10,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,8.06225774829855,10.6301458127346,9.89949493661167,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,7.07106781186548,10,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,6.08276253029822,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,5.09901951359278,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,4.12310562561766,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,3.16227766016838,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,2.23606797749979,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,1.4142135623731,8,7,6,5,4,3,2,1,0,1,12.7279220613579,12.0415945787923,11.4017542509914,10.816653826392,10.295630140987,9.8488578017961,9.48683298050514,9.21954445729289,9.05538513813742,9,12.0415945787923,11.3137084989848,10.6301458127346,10,9.4339811320566,8.94427190999916,8.54400374531753,8.24621125123532,8.06225774829855,8,11.4017542509914,10.6301458127346,9.89949493661167,9.21954445729289,8.60232526704263,8.06225774829855,7.61577310586391,7.28010988928052,7.07106781186548,7,10.816653826392,10,9.21954445729289,8.48528137423857,7.81024967590665,7.21110255092798,6.70820393249937,6.32455532033676,6.08276253029822,6,10.295630140987,9.4339811320566,8.60232526704263,7.81024967590665,7.07106781186548,6.40312423743285,5.8309518948453,5.3851648071345,5.09901951359278,5,9.8488578017961,8.94427190999916,8.06225774829855,7.21110255092798,6.40312423743285,5.65685424949238,5,4.47213595499958,4.12310562561766,4,9.48683298050514,8.54400374531753,7.61577310586391,6.70820393249937,5.8309518948453,5,4.24264068711928,3.60555127546399,3.16227766016838,3,9.21954445729289,8.24621125123532,7.28010988928052,6.32455532033676,5.3851648071345,4.47213595499958,3.60555127546399,2.82842712474619,2.23606797749979,2,9.05538513813742,8.06225774829855,7.07106781186548,6.08276253029822,5.09901951359278,4.12310562561766,3.16227766016838,2.23606797749979,1.4142135623731,1,9,8,7,6,5,4,3,2,1,0]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th>id1<\/th>\n      <th>id2<\/th>\n      <th>boundary<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"pageLength":5,"columnDefs":[{"className":"dt-right","targets":[0,1,2]}],"order":[],"autoWidth":false,"orderClasses":false,"lengthMenu":[5,10,25,50,100]}},"evals":[],"jsHooks":[]}</script>
```

## Step 1: Initialize the problem

After having loaded our data, we will now create the data object through
the `inputData()` function.


```r
# create conservation problem
b <- inputData(pu = sim_pu_data, 
               features = sim_features_data, 
               dist_features = sim_dist_features_data, 
               threats = sim_threats_data, 
               dist_threats = sim_dist_threats_data, 
               sensitivity = sim_sensitivity_data, 
               boundary = sim_boundary_data)

# print problem
print(b)
```


```
## Data
##   planning units: data.frame (100 units)
##   monitoring costs:     min: 1, max: 10
##   features:       feature1, feature2, feature3, feature4 (4 features)
##   threats:        threat1, threat2 (2 threats)
##   action costs:   min: 1, max: 10
```

## Step 1: Initialize the problem

It is advisable to determine the maximum benefit achievable for each
conservation feature through the `getPotentialBenefit()` function


```r
# get benefit information
getPotentialBenefit(b)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["feature"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["dist"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["dist_threatened"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["maximum.conservation.benefit"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["maximum.recovery.benefit"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["maximum.benefit"],"name":[6],"type":["dbl"],"align":["right"]}],"data":[{"1":"1","2":"47","3":"47","4":"0","5":"47","6":"47"},{"1":"2","2":"30","3":"28","4":"2","5":"28","6":"30"},{"1":"3","2":"66","3":"56","4":"10","5":"56","6":"66"},{"1":"4","2":"33","3":"33","4":"0","5":"33","6":"33"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

## Step 2: Create the mathematical model


```r
# create optimization problem
c <- problem(b, model_type = "minimizeCosts")
```

```
## Warning: The blm argument was set to 0, so the boundary data has no effect
```

```
## Warning: Some blm_actions argument were set to 0, so the boundary data has no
## effect for these cases
```

```r
# print model
print(c)
```

```
## Optimization Problem
##   model sense: minimization
##   dimensions:  284, 396, 12.656 kB (nrow, ncol, size)
##   variables:   396
```

## Step 3: Solve the model

To solve our model, we need an **optimizer**. In this case, we will use
the `Rsymphony` library.


```r
# solve optimization problem
d <- solve(c, solver = "symphony", verbose = TRUE, output_file = FALSE, cores = 2)

# print solution
print(d)
```

```
## Solution overview
##   name: sol
##   objective value: 738
##   gap:  0%
##   status:  Optimal solution (according to gap tolerance: 0)
##   runtime: 0.017 sec
```

## Getting information

#### `getActions()`


```r
# get actions from solution
actions <- getActions(d, format = "wide")

# print first six rows of data
# head(actions)
```


```{=html}
<div id="htmlwidget-a0aa64159aab4f6d948b" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-a0aa64159aab4f6d948b">{"x":{"filter":"none","vertical":false,"data":[["sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol","sol"],[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100],[0,0,0,0,0,0,0,0,0,0,1,1,1,1,0,0,0,0,0,0,0,1,0,1,0,0,0,0,0,0,1,0,1,0,1,0,0,0,0,0,1,1,1,0,0,0,0,0,0,0,1,1,1,1,0,1,1,0,0,0,1,1,1,1,1,1,0,0,0,0,1,0,0,1,1,0,0,1,0,0,0,0,0,1,1,1,1,1,1,0,0,0,1,1,1,1,1,1,1,0],[0,0,0,0,0,0,0,1,1,1,1,1,1,1,0,1,1,0,1,1,1,0,0,1,0,1,1,1,1,1,1,1,1,1,1,0,1,1,1,1,1,1,1,0,0,0,1,1,1,1,1,1,1,1,0,1,1,1,1,1,1,1,1,1,1,1,0,1,1,1,0,0,0,0,0,0,0,0,1,1,0,0,0,0,0,0,0,0,0,1,0,0,0,0,0,0,0,0,0,1],[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th>solution_name<\/th>\n      <th>pu<\/th>\n      <th>1<\/th>\n      <th>2<\/th>\n      <th>conservation<\/th>\n      <th>connectivity<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"pageLength":5,"columnDefs":[{"className":"dt-right","targets":[1,2,3,4,5]}],"order":[],"autoWidth":false,"orderClasses":false,"lengthMenu":[5,10,25,50,100]}},"evals":[],"jsHooks":[]}</script>
```

## Getting information

#### `getActions()`


```r
values(r) <- actions$`1`
plot(r)
```

![](Workshop_prioriactions_IEB_2022_files/figure-html/toyExample25-1.png)<!-- -->

## More connectivity?

#### We go back to step 2 and repeat.

<br>


```r
# create optimization problem
c2 <- problem(b, model_type = "minimizeCosts", blm = 10)
```

```
## Warning: Some blm_actions argument were set to 0, so the boundary data has no
## effect for these cases
```

```r
# print model
print(c2)
```

```
## Optimization Problem
##   model sense: minimization
##   dimensions:  29984, 10296, 883.856 kB (nrow, ncol, size)
##   variables:   10296
```

## More connectivity?


```r
# solve optimization problem
d2 <- solve(c2, solver = "symphony", verbose = TRUE, output_file = FALSE, cores = 2)

# print solution
print(d2)
```

```
## Solution overview
##   name: sol
##   objective value: 863
##   gap:  0%
##   status:  Optimal solution (according to gap tolerance: 0)
##   runtime: 22.847 sec
```


```r
# get actions from solution
actions2 <- getActions(d2, format = "wide")

values(r) <- actions2$`1`
plot(r)
```

![](Workshop_prioriactions_IEB_2022_files/figure-html/toyExample28-1.png)<!-- -->

# Outline

-   Features

-   Overview

-   Toy example

-   ::: white
    **Actual case study: The Mitchell River, Australia**
    :::

-   Shiny Application

## The Mitchell River, Australia

- We divided the whole catchment (71,630 km2) into **2316 sites** (i.e., sub-catchments)

- We sourced the distribution of **45 fish species** in the Mitchell river catchment as our conservation features. 

- We considered **four major threats** to freshwater fish species in the catchment: `water buffalo` (Bubalis bubalis), `cane toad` (Bufo marinus), `river flow alteration` (caused by infrastructure for water extractions and levee banks) and `grazing land use`.

<center>![](Figures/buffalo.jpg){.class
width="40%" height="40%"}</center>


## The Mitchell River, Australia

<center>![](Figures/mitchell1.png){.class
width="60%" height="60%"}</center>

# Outline

-   Features

-   Overview

-   Toy example

-   Actual case study: The Mitchell River, Australia

-   ::: white
    **Shiny Application**
    :::

## Shiny Application

<center>![Source: https://shiny.rstudio.com](Figures/shiny.png){.class
width="80%" height="80%"}</center>

## Shiny Application

### https://prioriactions.shinyapps.io/prioriactions/

<center>![](Figures/shiny_app.png){.class
width="80%" height="80%"}</center>

# Thank you!

## Questions?

<br>

<center>![](Figures/github1.png){.class
width="25%" height="25%"}</center>


