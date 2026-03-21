# Match score model

> **Preview:** Cursor/VS Code’s default Markdown preview may show raw `$…$`; use a math-enabled preview or GitHub for typesetting.

For one match, let $X_A$ and $X_B$ be goals for A and B.

## Rates from attack vs opponent defense

Each team has an **attack** strength and a **defense** strength (from data or priors). Expected goals for the match are:

- **$\lambda_A$** — driven by **A’s attack** against **B’s defense** (how well A scores when facing B’s defensive quality).
- **$\lambda_B$** — driven by **B’s attack** against **A’s defense**.

You choose a map from those four ingredients into two non‑negative numbers, e.g. $\lambda_A = f(\text{atk}_A, \text{def}_B)$ and $\lambda_B = f(\text{atk}_B, \text{def}_A)$. The exact form of $f$ (linear, log‑linear, tabulated, etc.) is up to you.

## Poisson + independence

Assume

$$
X_A \sim \mathrm{Poisson}(\lambda_A), \qquad X_B \sim \mathrm{Poisson}(\lambda_B),
\qquad X_A \perp X_B \ \text{(given the match rates).}
$$

Then the **joint PMF** is

$$
\mathbb{P}(X_A=i, X_B=j)
  = e^{-(\lambda_A+\lambda_B)} \frac{\lambda_A^i}{i!} \frac{\lambda_B^j}{j!}.
$$

For computation you can truncate to a finite goal grid (e.g. $0,\ldots,9$) and renormalize if needed.

## Match outcomes

- A wins: $\mathbb{P}(X_A > X_B)$
- Draw: $\mathbb{P}(X_A = X_B)$
- B wins: $\mathbb{P}(X_B > X_A)$

These three probabilities sum to **1**.

**Summary:** Pick $(\lambda_A, \lambda_B)$ from **A’s attack vs B’s defense** and **B’s attack vs A’s defense**, then use independent Poisson goals to get the full score distribution and outcome probabilities.
