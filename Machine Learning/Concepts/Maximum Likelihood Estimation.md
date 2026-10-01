---
type: concept
topics:
  - "[[Parametric Methods]]"
status: draft
updated: 2026-09-30
aliases:
  - MLE
---

# Maximum Likelihood Estimation

> [!info] Supporting Material
> - [[Intro to ML - Ethem Alpaydin]]

> [!summary] Summary
> Maximum likelihood estimation chooses the parameter value that makes the observed sample most probable under the assumed distribution. The likelihood of the sample is the product of the individual likelihoods. The log likelihood turns that product into a sum, which is easier to maximize.

> [!important] Key Points
> - The goal is the parameter $p$ that makes $p(x \mid p)$ assign the sample the highest probability.
> - The likelihood of the sample is the product of the per-observation likelihoods.
> - Maximizing the log likelihood is the same search, written as a sum.
> - Bernoulli, multinomial, and Gaussian densities are the common cases in these notes.

## Notes

- Estimate the parameters of a distribution.
- Find the parameters $p$ that make sampling from $p(x \mid p)$ most likely.
- The likelihood of $X$ is the product of the individual likelihoods.
- For MLE, find the $p$ that makes the observed $X$ most likely to have been drawn.
- The log likelihood converts the product to a sum.

## Common Distributions

- [[Burnoulli Density|Bernoulli density]] - two outcomes; the event occurs or it does not.
- [[Multinomial Density]] - many mutually exclusive outcomes.
- [[Gaussian(Normal) Density|Gaussian density]] - a normal distribution.

## Related Concepts

- [[Parametric Methods]] - the setting that needs these parameter estimates.
- [[Mean Square Error]] - one way to judge the resulting estimator.
