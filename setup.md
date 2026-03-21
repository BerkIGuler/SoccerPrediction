# Match score model

> **Preview:** Default Cursor/VS Code Markdown often shows raw `$…$` math. Use a math-enabled preview or GitHub for typesetting.

For one match, let $X_A$ and $X_B$ be goals for teams A and B (non‑negative integers). The **joint PMF** is

$$
p_{A,B}(i,j) = \mathbb{P}(X_A=i, X_B=j),
$$

with $p_{A,B}\ge 0$ and probabilities summing to **1**.

**Outcomes** (partition the score pairs):

- A wins: $\sum_{i>j} p_{A,B}(i,j)$
- Draw: $\sum_{i} p_{A,B}(i,i)$
- B wins: $\sum_{j>i} p_{A,B}(i,j)$

**Simplest construction:** assume **independence**,

$$
\mathbb{P}(X_A=i, X_B=j)=\mathbb{P}(X_A=i)\ \mathbb{P}(X_B=j),
$$

so the joint is the **outer product** of two marginal PMFs. You can pick those marginals by hand (as in the notebook) or use **Poisson** $(\lambda_A,\lambda_B)$ with the same independence—$\lambda$’s can encode attack/defence strength. More general joints need an explicit model (e.g. copula or hierarchical conditionals), not an informal product of “given A / given B” terms.

The notebook’s matrix is $p_{A,B}$ on a finite goal grid; the three outcome probabilities are the sums above.
