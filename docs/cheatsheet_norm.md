# Norms + Dual Norms in Optimization — Cheat Sheet

Let $\|\cdot\|$ be a norm on $\mathbb{R}^n$ and $\|\cdot\|_*$ its dual norm.

---

## 1) Norm axioms + immediate consequences

**Axioms**
- Positive homogeneity: $\|\alpha x\| = |\alpha|\,\|x\|$
- Triangle inequality: $\|x+y\| \le \|x\|+\|y\|$
- Definiteness: $\|x\|=0 \iff x=0$

**Consequences**
- Reverse triangle: $\big|\|x\|-\|y\|\big| \le \|x-y\|$
- Convexity: for $\theta\in[0,1]$,

  $$
  \|\theta x+(1-\theta)y\| \le \theta\|x\|+(1-\theta)\|y\|
  $$

- Balls scale: $\{x:\|x\|\le r\} = r\,\{x:\|x\|\le 1\}$

---

## 2) Dual norm definitions + core inequalities

**Dual norm**

$$
\|y\|_* = \sup_{\|x\|\le 1} y^\top x \;=\; \sup_{\|x\|=1} y^\top x
$$

**General Hölder inequality**

$$
y^\top x \le \|x\|\,\|y\|_* \quad \forall x,y
$$

**Bipolar / dual-of-dual representation**

$$
\|x\| = \sup_{\|y\|_*\le 1} y^\top x
$$

**Max over a ball**

$$
\sup_{\|x\|\le r} y^\top x = r\,\|y\|_*
$$

---

## 3) Dual cones, balls, and support functions

**Primal unit ball**

$$
B = \{x:\|x\|\le 1\}
$$

**Dual unit ball**

$$
B_* = \{y:\|y\|_*\le 1\}
$$

**Support function view**

$$
\sigma_B(y) = \sup_{x\in B} y^\top x = \|y\|_*
$$

---

## 4) Conjugates (Fenchel) — the “norm − linear” pattern

For $f(x)=\|x\|$, its conjugate is

$$
f^*(y)=\sup_x \left(y^\top x-\|x\|\right)
=
\begin{cases}
0, & \|y\|_*\le 1,\\
+\infty, & \text{otherwise.}
\end{cases}
$$

Equivalently,

$$
\inf_x \left(\|x\|-y^\top x\right)
=
\begin{cases}
0, & \|y\|_*\le 1,\\
-\infty, & \text{otherwise.}
\end{cases}
$$

**Indicator connection.** Let $\delta_{B_*}(y)=0$ if $y\in B_*$ and $+\infty$ otherwise.  
Then $f^*(y)=\delta_{B_*}(y)$.

---

## 5) Subgradients of a norm (KKT workhorse)

**Characterization**

$$
g\in \partial\|x\|
\iff
\|g\|_*\le 1 \ \text{and}\ g^\top x = \|x\|
$$

**At the origin**

$$
\partial\|0\| = \{g:\|g\|_*\le 1\} = B_*
$$

---

## 6) Common dual pairs (memorize)

- $\|x\|_2 \leftrightarrow \|y\|_2$
- $\|x\|_1 \leftrightarrow \|y\|_\infty$
- $\|x\|_\infty \leftrightarrow \|y\|_1$

Weighted 2-norm: if $W\succ 0$ and

$$
\|x\|_W = \sqrt{x^\top W x},
$$

then

$$
\|y\|_{W,*} = \sqrt{y^\top W^{-1} y}.
$$

---

## 7) Modeling epigraphs (LP/SOCP forms)

**2-norm (SOC)**

$$
\|x\|_2 \le t \iff (t,x)\in \mathcal{Q}^{n+1}
$$

**1-norm (LP)**

$$
\|x\|_1 \le t \iff \exists u\ge 0:\ -u\le x\le u,\ \mathbf{1}^\top u \le t
$$

**Infinity-norm (LP)**

$$
\|x\|_\infty \le t \iff -t\mathbf{1}\le x\le t\mathbf{1}
$$

**Quadratic form (SOC via factorization)**  
If $P\succeq 0$ and $P=L^\top L$, then

$$
x^\top P x \le t^2 \iff \|Lx\|_2 \le t
$$

---

## 8) Distance to hyperplanes/halfspaces in a norm (uses dual norm)

Hyperplane $H=\{z:a^\top z=b\}$:

$$
\mathrm{dist}_{\|\cdot\|}(x,H) = \frac{|a^\top x-b|}{\|a\|_*}
$$

Halfspace $\{z:a^\top z\le b\}$:

$$
\mathrm{dist}_{\|\cdot\|}(x,\{a^\top z\le b\})
=
\frac{\max(0,a^\top x-b)}{\|a\|_*}
$$

---

## 9) Equality-constrained norm minimization dual (classic Boyd result)

Primal:

$$
\min_x \|x\|\quad \text{s.t.}\quad Ax=b
$$

Dual:

$$
\max_\nu b^\top \nu \quad \text{s.t.}\quad \|A^\top \nu\|_* \le 1
$$

Lower bound property:

$$
p^\star \ge b^\top \nu \quad \text{for any }\nu\text{ with }\|A^\top \nu\|_* \le 1
$$

---

## 10) Quick “translation rules” (mental toolbox)

- $\|y\|_* = \sup_{\|x\|\le 1} y^\top x$ (support function)
- $y^\top x \le \|x\|\,\|y\|_*$ (Hölder)
- Conjugate of a norm is an indicator of the dual ball
- Subgradient of a norm is a dual-ball vector that attains equality
