---
type: concept
topics:
  - "[[Machine Learning]]"
status: complete
updated: 2026-09-30
Class: cse-585
---

# Class-prior log odds

> [!summary] Summary
> Class-prior log odds measure how much more likely one class is than the other before any features are considered. For two classes, take the class proportions, form their ratio, and take the natural log. The sign says which class is more common. The features never enter the calculation.

> [!important] Key Points
> - The class prior is the share of observations in that class: $P(C_1) = N_1 / N$.
> - Prior odds are $P(C_1) / P(C_0)$.
> - Prior log odds are $\ln(P(C_1) / P(C_0))$.
> - Positive means class 1 is more common, zero means the classes are equal, and negative means class 0 is more common.
> - With $P(C_1) = 0.7$ and $P(C_0) = 0.3$, the log odds are about $0.847$.

Class-prior log odds describe the relative likelihood of the two classes **before considering any features**.

For binary classes $C_0$ and $C_1$:

## 1. Class Prior

The class prior is the proportion of observations belonging to each class:

$$
P(C_1) = \frac{N_1}{N}
$$

$$
P(C_0) = \frac{N_0}{N}
$$

## 2. Prior Odds

The odds of Class 1 relative to Class 0 are:

$$
\text{Odds} = \frac{P(C_1)}{P(C_0)}
$$

## 3. Prior Log Odds

Take the natural logarithm of the odds:

$$
\text{Log Odds} = \ln\left(\frac{P(C_1)}{P(C_0)}\right)
$$

### Interpretation

- $\text{Log Odds} > 0$: Class 1 is more common
- $\text{Log Odds} = 0$: Classes are equally common
- $\text{Log Odds} < 0$: Class 0 is more common

## Example

Suppose:

- $P(C_1) = 0.7$
- $P(C_0) = 0.3$

Then:

$$
\text{Odds} = \frac{0.7}{0.3} \approx 2.33
$$

$$
\text{Log Odds} = \ln(2.33) \approx 0.847
$$

The positive log odds indicate that **Class 1 is more likely before considering any features**.

> [!NOTE]
> Class-prior log odds depend only on the distribution of the classes. The numerical features are not used in this calculation.

## Related Concepts

- [[Parametric Methods]] - also uses class information when a distribution is assumed.
- [[Statistic]] - the class counts $N_0$ and $N_1$ are computed from the sample.
