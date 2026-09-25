
<!-- README.md is generated from README.Rmd. Please edit that file -->

# growthstack

<!-- badges: start -->

[![R-CMD-check](https://github.com/williamkannis/growthstack/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/williamkannis/growthstack/actions/workflows/R-CMD-check.yaml)

<!-- badges: end -->

The goal of growthstack is to …

## Installation

You can install the development version of growthstack from
[GitHub](https://github.com/) with:

``` r
# install.packages("pak")
pak::pak("williamkannis/stacked_growth_modelling")
```

## Example

This is a basic example which shows you how to solve a common problem:

``` r
#library(growthstack)
## basic example code
```

What is special about using `README.Rmd` instead of just `README.md`?
You can include R chunks like so:

``` r
summary(cars)
#>      speed           dist       
#>  Min.   : 4.0   Min.   :  2.00  
#>  1st Qu.:12.0   1st Qu.: 26.00  
#>  Median :15.0   Median : 36.00  
#>  Mean   :15.4   Mean   : 42.98  
#>  3rd Qu.:19.0   3rd Qu.: 56.00  
#>  Max.   :25.0   Max.   :120.00
```

You’ll still need to render `README.Rmd` regularly, to keep `README.md`
up-to-date. `devtools::build_readme()` is handy for this.

You can also embed plots, for example:

<img src="man/figures/README-pressure-1.png" alt="" width="100%" />

In that case, don’t forget to commit and push the resulting figure
files, so they display on GitHub and CRAN.

## Usage

### Simulating data

### Fitting growth models

### Indivudal model predictions

### Stacked model predictions

## Growth models

Functions fit growth models using three model forms: von Bertalanffy,
Gompertz, and Logistic; and three effect structures: Random effect only,
Categorical second-level predictors, and continuous second-level
predictors. See below for more information of growth forms and model
type:

### Growth forms

For each growth form, the equation for length (L) at age (t), and the
differential equation for instantaneous growth (G) at length are given
below. Asymptotic terms (L<sub>∞</sub>) describe the maximum length for
the average fish, the scaling terms (g<sub>1-3</sub>) describe the slope
of the growth curve, and the inflection term (t<sub>0</sub>,
t<sub>inf</sub>) describes the age at which maximum growth rate occurs

**von Bertalanffy (vb)**

$$L(t) = L_{\infty} (1 - e^{-g_1 (t - t_0)})$$
$$G(L) = g_1 (L_{\infty} - L)$$

**Gompertz (gz)**

$$L(t) = L_{\infty} e^{-e^{-g_2 (t - t_{\text{inf}})}}$$
$$G(L) = g_2 \times L \times \ln\left(\frac{L_{\infty}}{L}\right)$$

**Logistic (lg)**

$$L(t) = \frac{L_{\infty}}{1 + e^{-g_3 (t - t_{\text{inf}})}}$$
$$G(L) = g_3 \times L \left(1 - \frac{L}{L_{\infty}}\right)$$

### Effect structure

**Random effect only model (random)**

$$L_{i,j} = \text{FUN}_x (\text{age}_{i,j} \mid L_{\infty j}, g_j, t_j) + \varepsilon_{i,j}$$

$$\varepsilon_{i,j} \sim \text{StudentT}(\nu, 0, \sigma^2)$$

$$\log \begin{pmatrix} L_{\infty j} \\ g_j \\ t_j + 10 \end{pmatrix} \sim \text{MVN}(\mu, \Sigma)$$

$$\mu = \log(\bar{L}_{\infty}, \bar{g}, \bar{t})$$

Where FUN is the length-at-age function for growth form x (Table 1),
*L<sub>i,j</sub>* and *age<sub>i,j</sub>* are length and age of fish *i*
at sampling event *j*, *L<sub>∞j</sub>*, *g<sub>j</sub>*, and
*t<sub>j</sub>* are respectively the asymptote, scaling
(g<sub>1-3,j</sub>), and inflection (t<sub>0,j</sub> or
t<sub>inf,j</sub>) parameters, and ε<sub>i,j</sub> are the Student’s t
distributed errors with ν degrees of freedom. μ contains the grand means
of the global parameters and Σ is a covariance-variance matrix. To aid
in model convergence, growth parameters were estimated on the natural
log scale, with the addition of 10 to the inflection parameter to allow
for negative values. For the von Bertalanffy model, the inflection
parameter t<sub>0</sub> was often close to -10 and biased by the
addition of 10, so we left t<sub>0</sub> on the non-transformed scale.

**Categorical predictors (categorical)**

Same structure as random effect model but:

$$\mu = \log \begin{pmatrix} \bar{L}_{\infty} + \gamma_{1,h} \times \text{hydr}_j \\ \bar{g} + \gamma_{2,h} \times \text{hydr}_j \\ \bar{t} + \gamma_{3,h} \times \text{hydr}_j \end{pmatrix}$$

Where γ<sub>1-3,h</sub> are the fixed effects coefficients for
hydroperiod classification *h* (1 = short, 2 = intermediate, 3 = long)
of sampling event *j* on the three growth parameters.

**Continuous predictors (continuous)**

Same structure as random effect model but:

$$\mu = \log \begin{pmatrix} \bar{L}_{\infty} + \gamma_{1,k} \times \text{PC}_{k,j} \\ \bar{g} + \gamma_{2,k} \times \text{PC}_{k,j} \\ \bar{t} + \gamma_{3,k} \times \text{PC}_{k,j} \end{pmatrix}$$

Where γ<sub>1-3,h</sub> are the fixed effects coefficients for
environmental PC *k* on the three growth parameters.

## License

The code in this repository is licensed under the [MIT
License](LICENSE).
