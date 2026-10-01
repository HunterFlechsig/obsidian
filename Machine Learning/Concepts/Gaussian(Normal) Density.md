---
type: concept
topics:
  - "[[Parametric Methods]]"
status: draft
updated: 2026-09-30
aliases:
  - Gaussian Density
  - Normal Density
---

# Gaussian (Normal) Density

> [!info] Supporting Material
> - [[Intro to ML - Ethem Alpaydin]]

> [!summary] Summary
> A Gaussian, or normal, density is the parametric model for a continuous feature assumed to follow a normal distribution. The sample mean estimates the mean $\mu$. How the variance is averaged decides whether that variance estimate is biased.

> [!important] Key Points
> - The sample mean is an unbiased estimator of $\mu$.
> - Unbiased means the expected value of the estimator equals the true parameter at any sample size.
> - The maximum-likelihood variance divides by $n$ and is biased. The unbiased sample variance divides by $n-1$.

## Notes

- Used when the feature is assumed to follow a normal distribution.
- The sample mean is an unbiased estimator of $\mu$. Its expected value equals $\mu$ for any sample size. The estimate becoming exact as the sample grows is consistency, a separate property.
- The maximum-likelihood variance, often written $s^2$ with division by $n$, is a biased estimator of $\sigma^2$.

## Related Concepts

- [[Parametric Methods]] - the family this density belongs to.
- [[Maximum Likelihood Estimation]] - produces the mean and the biased variance estimate.
- [[Mean Square Error]] - combines estimator variance and bias.
