---
type: concept
topics:
  - "[[Machine Learning]]"
status: complete
updated: 2026-09-30
---

# Parametric Methods

> [!info] Supporting Material
> - [[Intro to ML - Ethem Alpaydin]]

> [!summary] Summary
> A parametric method assumes the sample was drawn from a known family of distributions with a small number of parameters. Those parameters are estimated, here by maximum likelihood, and the estimator is judged by its bias, variance, and mean square error.

> [!important] Key Points
> - The distribution family is assumed. Only a few parameters are learned.
> - [[Maximum Likelihood Estimation]] supplies the parameter estimates.
> - An estimator is unbiased when its bias is $0$ for every parameter value.
> - Variance is how far the estimator spreads around its own expected value. Bias is how far that expected value sits from the true parameter.
> - $\mathrm{MSE} = \mathrm{Var}(d) + \mathrm{bias}(d)^2$.

## Assumptions

- Assume the sample is drawn from a certain distribution.
- The model is defined by a small number of parameters.
- Use [[Maximum Likelihood Estimation]] to estimate the parameters.

## Evaluating an Estimator: Bias and Variance

- Use [[Mean Square Error]].
- The estimator is unbiased if its bias is $0$ for all parameter values.
- Variance: how much, on average, $d$ varies around its expected value.
- Bias: how far the expected value sits from the correct parameter $\theta$.
- $\mathrm{Error} = \mathrm{Var}(d) + \mathrm{bias}(d)^2$.

## Related Concepts

- [[Maximum Likelihood Estimation]] - estimates the parameters.
- [[Mean Square Error]] - the error used to judge the estimator.
- [[Burnoulli Density|Bernoulli density]] - a two-outcome parametric model.
- [[Gaussian(Normal) Density|Gaussian density]] - a continuous parametric model.
