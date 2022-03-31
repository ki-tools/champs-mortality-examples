# champs-mortality examples

This repository stores some examples of web reports created using the [champs-mortality](https://github.com/ki-tools/champs-mortality) R package.

Note that the package is still under active development and a lot of validation needs to occur before the reported statistics can be trusted.

Also note that the web reports are still under active development and will eventually contain a more information about the methodology and all the necessary footnotes where special computations are made.

The examples below create the reports in this repository.

The outputs of these can be viewed here:

- https://ki-tools.github.io/champs-mortality-examples/lri-cc
- https://ki-tools.github.io/champs-mortality-examples/lri-uc
- https://ki-tools.github.io/champs-mortality-examples/maln-cc
- https://ki-tools.github.io/champs-mortality-examples/maln-uc
- https://ki-tools.github.io/champs-mortality-examples/hiv-cc
- https://ki-tools.github.io/champs-mortality-examples/hiv-uc
- https://ki-tools.github.io/champs-mortality-examples/cbd-cc
- https://ki-tools.github.io/champs-mortality-examples/ntd-cc
- https://ki-tools.github.io/champs-mortality-examples/ntd-cc-age

<!-- 
```r
dd_raw <- read_and_validate_data("_ignore/datasets55c")
```
-->

```r
library(champsmortality)

# read in the data (needs to be configured already)
dd_raw <- read_and_validate_data("__data_directory__")
dd <- process_data(cd_raw, start_year = 2017, end_year = 2020)

# LRI in causal chain
lri_cc <- get_rates_and_fractions(dd,
  condition = "Lower respiratory infections",
  cond_name_short = "LRI"
)
make_outputs(lri_cc, path = "lri-cc")

# LRI in underlying cause
lri_uc <- get_rates_and_fractions(dd,
  condition = "Lower respiratory infections",
  cond_name_short = "LRI",
  causal_chain = FALSE
)
make_outputs(lri_uc, path = "lri-uc")

# Malnutrition in causal chain restricted to just infants and children
age_levels <- list("Infant" = "Infant", "Child" = "Child")
maln_cc <- get_rates_and_fractions(dd,
  condition = "Malnutrition",
  cond_name_short = "Maln",
  factor_groups = list(age = age_levels)
)
make_outputs(maln_cc, path = "maln-cc")

# Malnutrition in underlying cause
maln_uc <- get_rates_and_fractions(dd,
  condition = "Malnutrition",
  cond_name_short = "Maln",
  causal_chain = FALSE,
  factor_groups = list(age = age_levels)
)
make_outputs(maln_uc, path = "maln-uc")

# HIV in causal chain
hiv_cc <- get_rates_and_fractions(dd,
  condition = "HIV",
)
make_outputs(hiv_cc, path = "hiv-cc")

# HIV in underlying cause
hiv_uc <- get_rates_and_fractions(dd,
  condition = "HIV",
  causal_chain = FALSE
)
make_outputs(hiv_uc, path = "hiv-uc")

# CBD in causal chain with custom age groups
age_levels <- list(
  "Stillbirth" = "Stillbirth",
  "Neonate" = "Neonate",
  "Infants and Children" = c("Infant", "Child")
)
cbd_cc <- get_rates_and_fractions(dd,
  condition = "Congenital birth defects",
  cond_name_short = "CBD",
  factor_groups = list(age = age_levels)
)
make_outputs(cbd_cc, path = "cbd-cc")

# Neural tube defects in causal chain with custom age groups
ntd_cc <- get_rates_and_fractions(dd,
  icd10_regex = "^Q00|^Q01|^Q05",
  cond_name = "Neural tube defects",
  cond_name_short = "NTD",
  factor_groups = list(age = age_levels)
)
make_outputs(ntd_cc, path = "ntd-cc")

# Neural tube defects in causal chain with custom age groups
# and forcing to adjust by age
ntd_cc_age <- get_rates_and_fractions(dd,
  icd10_regex = "^Q00|^Q01|^Q05",
  cond_name = "Neural tube defects",
  cond_name_short = "NTD",
  adjust_vars_override = "age",
  factor_groups = list(age = age_levels)
)
make_outputs(ntd_cc_age, path = "ntd-cc-age")
```
