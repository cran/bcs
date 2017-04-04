
<!-- README.md is generated from README.Rmd. Please edit that file -->
bcs - Bayesian compressive sensing for R
========================================

Implements the fast Laplace algorithm for Bayesian compressive sensing. This algorithm comes from the paper 'Bayesian Compressive Sensing using Laplace Priors' by Babacan, Molina, and Katsaggelos. The original code was written in Matlab by the same authors as the paper. See LICENSE for more details.

##### Example

``` r
library(bcs)

# size of the basis function expansion
N <- 64
# generate sparse coefficient vector
w <- rep(0,N)
w[sample(1:N,10)] <- runif(10,-1,1)
# create wavelet basis trasform matrix
wavelet.basis <- WaveletBasis(N)
# generate actual signal
signal <- wavelet.basis%*%w
# now we try and recover 'w' and 'signal' from samples
num_samps <- 25
# create random measurement matrix
measure.mat <- matrix(runif(num_samps*N),num_samps,N)
measure.mat <- measure.mat/matrix(rep(sqrt(apply(measure.mat^2,2,sum)),
                                      num_samps),num_samps,N,byrow=TRUE);
PHI <- measure.mat%*%wavelet.basis
# actual samples we see
y <- measure.mat%*%signal
# use fast Laplace algorithm
w_est <- FindSparse(PHI, y)

# estimate signal
signal_est <- wavelet.basis%*%w_est
# Root mean squared error of estimate
error <- sqrt(mean((signal - signal_est)^2))
```

##### Installation:

`install.packages("devtools")`

`devtools::install_github("gmatt18/bcs")`

##### Overview:

The main function in `bcs` is `FindSparse`. This function requires a sensing matrix and samples from the signal or function of interest in order to recover the sparse coefficients. `bcs` also includes functions that create transformation matrices for the wavelet, Fourier, and B-spline bases.
