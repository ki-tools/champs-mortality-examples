# champs-mortality examples

This repository stores some examples of web reports created using the [champs-mortality](https://github.com/ki-tools/champs-mortality) R package.

Note that the package is still under active development and a lot of validation needs to occur before the reported statistics can be trusted.

Also note that the web reports are still under active development and will eventually contain a more information about the methodology and all the necessary footnotes where special computations are made.

The examples below create the reports in this repository. Note that currently all of the adjustment factors are hard-coded. This is done for now while we determine whether the automatic selection should happen at the site-level or not.

The outputs of these can be viewed here:

- https://ki-tools.github.io/champs-mortality-examples/cbd-cc/
- https://ki-tools.github.io/champs-mortality-examples/lri-cc/
- https://ki-tools.github.io/champs-mortality-examples/lri-uc/
- https://ki-tools.github.io/champs-mortality-examples/malnutrition-cc/

```r
library(champsmortality)

dd_raw <- read_and_validate_data("__data_directory__")
dd <- process_data(cd_raw, start_year = 2017, end_year = 2020)

age_levels <- list(
  "Stillbirth" = "Stillbirth",
  "Neonate" = "Neonate",
  "Infants and Children" = c("Infant", "Child")
)

# CBD in causal chain forcing age adjustment and custom age groups
cbd_cc <- get_rates_and_fractions(dd,
  condition = "Congenital birth defects",
  cond_name = "CBD",
  adjust_vars_override = "age",
  factor_groups = list(age = age_levels)
)
make_site(cbd_cc, path = "cbd-cc")

# LRI in caucal chain forcing age and location adjustment
lri_cc <- get_rates_and_fractions(cd,
  condition = "Lower respiratory infections",
  causal_chain = TRUE,
  cond_name = "LRI",
  adjust_vars_override = c("age", "location")
)
make_site(lri_cc, path = "lri-cc")

# LRI in underlying cause forcing age and location adjustment
lri_uc <- get_rates_and_fractions(cd,
  condition = "Lower respiratory infections",
  causal_chain = FALSE,
  cond_name = "LRI",
  adjust_vars_override = c("age", "location")
)
make_site(lri_uc, path = "lri-uc")

# Malnutrition in caucal chain cause forcing age and location adjustment
mal_cc <- get_rates_and_fractions(cd,
  condition = "Malnutrition",
  adjust_vars_override = c("age", "location")
)
make_site(mal_cc, path = "malnutrition-cc")
```