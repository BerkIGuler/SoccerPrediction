# Match score model (probability setup)

> **Math in preview:** Cursor / VS Code’s built-in Markdown preview does **not** render LaTeX by default, so formulas may look like raw `$…$` or `\(...\)`. That’s expected. To get rendered math, use a preview that supports KaTeX/MathJax (e.g. the **“Markdown+Math”** extension in VS Code/Cursor), or view the file on **GitHub** (which renders `$…$` / `$$…$$`). Below, math uses `$` / `$$` for best compatibility with those tools.

## Random variables

Fix a single match between team **A** (home) and team **B** (away). Let

- $X_A \in \{0,1,2,\ldots\}$ = goals scored by A in that match  
- $X_B \in \{0,1,2,\ldots\}$ = goals scored by B in that match  

We work with their **joint distribution** on $\mathbb{Z}_{\ge 0}^2$.

## Joint PMF

The joint probability mass function (PMF) is

$$
p_{A,B}(i,j) := \mathbb{P}(X_A = i,\; X_B = j), \quad i,j \in \mathbb{Z}_{\ge 0}.
$$

**Validity:** $p_{A,B}(i,j) \ge 0$ and $\sum_{i=0}^{\infty}\sum_{j=0}^{\infty} p_{A,B}(i,j) = 1$.

For computation, it is common to **truncate** to a finite grid (e.g. $i,j \in \{0,\ldots,9\}$) and **renormalize** if the tail mass is negligible, so the displayed matrix is still a PMF on that support.

## Match outcomes from the joint

Once $p_{A,B}$ is fixed:

- **A wins:** $\mathbb{P}(X_A > X_B) = \sum_{i>j} p_{A,B}(i,j)$
- **Draw:** $\mathbb{P}(X_A = X_B) = \sum_{i} p_{A,B}(i,i)$
- **B wins:** $\mathbb{P}(X_B > X_A) = \sum_{j>i} p_{A,B}(i,j)$

These three probabilities are disjoint and sum to **1**.

## Modeling choice: full joint vs. factored marginals

You can specify $p_{A,B}$ **directly** (a full bivariate model) or build it from **marginals** plus an **independence** (or copula) assumption.

### Independent team scores (simplest)

Assume $X_A$ and $X_B$ are **independent** for this match (conditionally on whatever parameters you use):

$$
\mathbb{P}(X_A = i,\; X_B = j) = \mathbb{P}(X_A = i)\,\mathbb{P}(X_B = j).
$$

Then the joint is the **outer product** of the two marginal PMFs. This is **not** the same as multiplying arbitrary “$X_A$ given B” and “$X_B$ given A” terms unless those conditionals are constructed so that they imply this factorization (see below).

### Poisson marginals (optional)

A standard parsimonious choice is

$$
X_A \sim \mathrm{Poisson}(\lambda_A), \qquad X_B \sim \mathrm{Poisson}(\lambda_B),
$$

with **independence** $X_A \perp\!\!\!\perp X_B$, so

$$
\mathbb{P}(X_A = i,\; X_B = j)
  = e^{-(\lambda_A+\lambda_B)}\,\frac{\lambda_A^{\,i}}{i!}\,\frac{\lambda_B^{\,j}}{j!}.
$$

Here $\lambda_A$ and $\lambda_B$ are **match-level intensities** (expected goals). They may depend on **A’s attack**, **B’s defense**, etc. (and vice versa for $\lambda_B$). That is a **modeling** step on the parameters, not a substitute for the joint unless independence is assumed.

**Caveat:** Treating goals as Poisson is an approximation (e.g. correlation over time within a match, variance vs. mean, injury time). It can still be useful as a working model.

### What the product of conditionals is *not* (unless you assume more)

In general,

$$
\mathbb{P}(X_A = i,\; X_B = j) \neq \mathbb{P}(X_A = i \mid B)\,\mathbb{P}(X_B = j \mid A)
$$

when “$\mid B$” / “$\mid A$” are only informal shorthand. A valid construction is:

- Specify **conditional laws** that imply a joint (e.g. through a hierarchical model), **or**
- Specify **marginals + dependence** (copula, bivariate count model), **or**
- Assume **independence** and use marginal PMFs (Poisson or otherwise).

Use the factorization that matches the **assumptions** you are willing to defend.

## Summary

| Object | Role |
|--------|------|
| Joint $p_{A,B}(i,j)$ | Full joint PMF for the score pair; must sum to 1 (on the chosen support). |
| Marginals + independence | Build $p_{A,B}$ from $\mathbb{P}(X_A=i)$ and $\mathbb{P}(X_B=j)$. |
| Poisson rates $\lambda_A,\lambda_B$ | Optional parametric marginals; rates tie attack/defense/strength priors to goal distributions. |

The notebook’s score matrix is this joint PMF on a finite goal grid; outcome probabilities are the sums over $i>j$, $i=j$, and $j>i$ as above.
