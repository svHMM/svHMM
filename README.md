# svHMM <img src="https://img.shields.io/badge/R-v4.0+-blue.svg" alt="R version" align="right">

[![Development Version](https://img.shields.io/badge/developer-GitHub-orange.svg)](https://github.com/svHMM/svHMM)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**svHMM** is an R package for fast approximate Bayesian inference in **Stochastic Volatility in Mean (SVM) models with heavy-tailed distributions**, powered by **Hidden Markov Models (HMMs)**. 

By discretizing the continuous latent volatility process into a high-dimensional Markov chain, `svHMM` avoids computationally intensive MCMC algorithms, delivering extremely fast parameter estimation and volatility extraction.

The package implements the methodology proposed in:
> *Stochastic Volatility in Mean Models with Heavy Tails: A Fast Approximate Bayesian Inference Using Hidden Markov Models* (2026).

---

## 🚀 Installation

You can install the development version of `svHMM` directly from GitHub using the `remotes` package:

```r
# Install remotes if you haven't already
if (!requireNamespace("remotes", quietly = TRUE)) {
  install.packages("remotes")
}

# Install svHMM (use username/repo format)
remotes::install_github("svHMM/svHMM")
