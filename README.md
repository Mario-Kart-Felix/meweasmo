
<!-- README.md is generated from README.Rmd. Please edit that file -->

# meweasmo R-package: MEta-WEb ASsembly MOdels

[![DOI](https://zenodo.org/badge/130283899.svg)](https://zenodo.org/badge/latestdoi/130283899)

This package simulates the assembly of ecological networks from a
regional metaweb. It uses a generalized Lotka-Volterra stochastic model.
Species can migrate from the meta-web and establish the ecological
interactions specified in the metaweb.

License: MIT

## Installation

Using the ‘devtools’ package:

``` r

install.packages("devtools")
library(devtools)
install_github('lsaravia/meweasmo')
library(meweasmo)
```

## Get Started

### Simulate a 6 species network without migration

1)  Create a metaweb with a Lotka-Volterra interaction matrix structure,
    I added one last column with intrinsic growth rate.

<!-- end list -->

``` r

library(tibble)

parms <- tibble::tribble(
  ~a1,   ~a2,  ~a3,    ~a4,   ~a5,  ~a6,    ~r,
    0,  4e-6, 3e-5,   2e-5,     0,    0,  -0.01,
-1e-5, -1e-4,    0,   1e-4,  6e-5,    0, -0.001,
-1e-4,     0,    0,      0,  6e-5, 5e-5,  -0.01,
-1e-4,  1e-4,    0,  -5e-4, -1e-5,    0,   0.08,
    0, -1e-4,-1e-4,      0, -1e-6,    0,   0.04,
    0,     0,-1e-4,   2e-4,     0,    0,   0.00
)
```

2)  Set the initial conditions and simulate the model with no migration
    during 1000 time steps

<!-- end list -->

``` r

library(meweasmo)

yini  <- c(N1 = 100, N2 = 100, N3 = 100, N4 = 100, N5 = 100, N6 = 100)
A <- as.matrix(parms[,1:6])
r<- as.numeric(parms$r)
m <- c(0,0,0,0,0,0)
A1 <- metaWebNetAssemblyGLV(A,m,r,yini,1000,0.1)
```

3)  Plot the simulation

<!-- end list -->

``` r

df1 <- as.data.frame(t(A1$STime))
df1$time <- 1:1000
names(df1) <- gsub("V", "sp", names(df1))
 
library(tidyr)
df1 <- gather(df1,key="Species",value="N", sp1:sp6)

library(ggplot2)
ggplot(df1, aes(time,N,colour=Species)) + geom_point(size=0.1) +  theme_bw() + scale_color_viridis_d()
```

<img src="man/figures/README-unnamed-chunk-4-1.png" width="60%" />

## References

1.  Ecological Network assembly: how the regional metaweb influences
    local food webs Leonardo A. Saravia, Tomás I. Marina, Marleen De
    Troch, Fernando R. Momo bioRxiv 340430; doi:
    <https://doi.org/10.1101/340430>
