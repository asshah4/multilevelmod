
<!-- README.md is generated from README.Rmd. Please edit that file -->

# multilevelmod

<!-- badges: start -->

[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
[![CRAN
status](https://www.r-pkg.org/badges/version/multilevelmod)](https://CRAN.R-project.org/package=multilevelmod)
[![Codecov test
coverage](https://codecov.io/gh/tidymodels/multilevelmod/branch/master/graph/badge.svg)](https://codecov.io/gh/tidymodels/multilevelmod?branch=master)
[![R-CMD-check](https://github.com/tidymodels/multilevelmod/workflows/R-CMD-check/badge.svg)](https://github.com/tidymodels/multilevelmod/actions)
<!-- badges: end -->

multilevelmod enables the use of multi-level models (such as
mixed-effects models or Bayesian hierarchical models) with the parsnip
package.

## Installation

You can install the released version of multilevelmod from
[CRAN](https://CRAN.R-project.org) with:

``` r
# not yet
install.packages("multilevelmod")
```

For the development version:

``` r
devtools::install_github("tidymodels/multilevelmod")
```

## Example

Loading mixedlevelmod will trigger it to add a few modeling *engines* to
the parsnip model database. For Bayesian models, there are now
`stan-glmer` engines for `linear_reg()`, `logistic_reg()`, and
`poisson_reg()`.

To use these, the function `parsnip::fit()` function should be used
instead of `parsnip::fit_xy()` so that the model terms can be specified
using the `lme`/`lme4` syntax.

The `sleepstudy` data is used as an example:

``` r
library(multilevelmod)
#> Loading required package: parsnip
data(sleepstudy, package = "lme4")

mixed_model_spec <- linear_reg() %>% set_engine("lmer")

mixed_model_fit <- 
  mixed_model_spec %>% 
  fit(Reaction ~ Days + (Days | Subject), data = sleepstudy)

mixed_model_fit
#> parsnip model object
#> 
#> Fit time:  57ms 
#> Linear mixed model fit by REML ['lmerMod']
#> Formula: Reaction ~ Days + (Days | Subject)
#>    Data: data
#> REML criterion at convergence: 1743.628
#> Random effects:
#>  Groups   Name        Std.Dev. Corr
#>  Subject  (Intercept) 24.741       
#>           Days         5.922   0.07
#>  Residual             25.592       
#> Number of obs: 180, groups:  Subject, 18
#> Fixed Effects:
#> (Intercept)         Days  
#>      251.41        10.47
```

For a Bayesian model:

``` r
hier_model_spec <- linear_reg() %>% set_engine("stan-glmer")

hier_model_fit <- 
  hier_model_spec %>% 
  fit(Reaction ~ Days + (Days | Subject), data = sleepstudy)

hier_model_fit
#> parsnip model object
#> 
#> Fit time:  48.3s 
#> stan_glmer
#>  family:       gaussian [identity]
#>  formula:      Reaction ~ Days + (Days | Subject)
#>  observations: 180
#> ------
#>             Median MAD_SD
#> (Intercept) 251.6    6.6 
#> Days         10.4    1.7 
#> 
#> Auxiliary parameter(s):
#>       Median MAD_SD
#> sigma 25.9    1.5  
#> 
#> Error terms:
#>  Groups   Name        Std.Dev. Corr
#>  Subject  (Intercept) 24.3         
#>           Days         6.9     0.08
#>  Residual             26.0         
#> Num. levels: Subject 18 
#> 
#> ------
#> * For help interpreting the printed output see ?print.stanreg
#> * For info on the priors used see ?prior_summary.stanreg
```

## Contributing

This project is released with a [Contributor Code of
Conduct](https://contributor-covenant.org/version/2/0/CODE_OF_CONDUCT.html).
By contributing to this project, you agree to abide by its terms.

  - For questions and discussions about tidymodels packages, modeling,
    and machine learning, please [post on RStudio
    Community](https://rstd.io/tidymodels-community).

  - If you think you have encountered a bug, please [submit an
    issue](https://github.com/tidymodels/multilevelmod/issues).

  - Either way, learn how to create and share a
    [reprex](https://rstd.io/reprex) (a minimal, reproducible example),
    to clearly communicate about your code.

  - Check out further details on [contributing guidelines for tidymodels
    packages](https://www.tidymodels.org/contribute/) and [how to get
    help](https://www.tidymodels.org/help/).
