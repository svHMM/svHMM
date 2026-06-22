# svHMM <img src="https://img.shields.io/badge/R-v4.0+-blue.svg" alt="R version" align="right">

[![Development Version](https://img.shields.io/badge/developer-GitHub-orange.svg)](https://github.com/svHMM/svHMM)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**svHMM** is an R package for fast approximate Bayesian inference in **Stochastic Volatility in Mean (SVM) models with heavy-tailed distributions**, powered by **Hidden Markov Models (HMMs)**. 

By discretizing the continuous latent volatility process into a high-dimensional Markov chain, `svHMM` avoids computationally intensive MCMC algorithms, delivering extremely fast parameter estimation and volatility extraction.

The package implements the methodology proposed in:
> *Stochastic Volatility in Mean Models with Heavy Tails: A Fast Approximate Bayesian Inference Using Hidden Markov Models* (2026).

---

## Installation

You can install the development version of `svHMM` directly from GitHub using the `remotes` package:

```r
# Install remotes if you haven't already
if (!requireNamespace("remotes", quietly = TRUE)) {
  install.packages("remotes")
}

# Install svHMM (use username/repo format)
remotes::install_github("svHMM/svHMM")

```

## Toy Example

Here is a quick demonstration on how to simulate data from a Stochastic Volatility in Mean model with a Student-t distribution ($SVM\text{-}t$), initialize the parameters, fit the model using the HMM approximation, and extract the latent log-volatility.

```r
library(svHMM)

# 1. Simulate data from an SVM-t process
set.seed(123) # For reproducibility
log.ret <- svmt.sim(
  mu    = 1.0, 
  phi   = 0.98, 
  sigma = 0.2, 
  nu    = 10, 
  beta  = c(0.2, -0.01, -0.2),
  y0    = 0.2,
  g_dim = 1e3
)

# Plot simulated returns
plot(log.ret$y, type = 'l', main = "Simulated Log-Returns", ylab = "y_t", xlab = "Time")

# 2. Set initial parameter values
theta0 <- list(
  mu0    = log(var(log.ret$y)), 
  phi0   = 0.98, 
  sigma0 = 0.15, 
  nu0    = 8, 
  beta0  = c(mean(log.ret$y), -0.01, -0.2)
)

# 3. Fit the SVM-t model using the HMM approach
# m = 200 grid points for volatility discretization
fit <- svmtHMM(
  y          = log.ret$y, 
  m          = 200, 
  gmax       = 5, 
  theta_init = theta0, 
  y0         = 0.2
)

# 4. Display estimation summary
summary(fit)

# 5. Extract and plot Estimated vs. True Volatility
lv <- logvol(fit, plot = TRUE)
lines(exp(0.5 * log.ret$h), col = 'red', lwd = 2)
legend("topright", legend = c("Estimated Volatility", "True Volatility"), 
       col = c("black", "red"), lwd = c(1, 2), bne = "n")
