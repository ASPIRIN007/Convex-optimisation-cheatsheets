# Lagrangian Duality + KKT + Geometric Intuition — Cheat Sheet

Let the primal problem be

$$
\min_x \ f_0(x)
\quad \text{s.t.}\quad
f_i(x)\le 0,\ i=1,\dots,m,\qquad
h_j(x)=0,\ j=1,\dots,p.
$$

---

## 1) Why Lagrange multipliers exist (intuition)

**“Prices for constraint violation.”** Introduce multipliers $\lambda_i\ge 0$ for inequalities and $\nu_j\in\mathbb{R}$ for equalities, and combine constraints into the objective:

$$
L(x,\lambda,\nu)=f_0(x)+\sum_{i=1}^m \lambda_i f_i(x)+\sum_{j=1}^p \nu_j h_j(x).
$$

- If $x$ is feasible, then $f_i(x)\le 0$ and with $\lambda_i\ge 0$ we get $\lambda_i f_i(x)\le 0$, so

  $$
  L(x,\lambda,\nu)\le f_0(x).
  $$

- If $x$ violates a constraint ($f_i(x)>0$), the term $\lambda_i f_i(x)$ grows with $\lambda_i$ → violation becomes expensive.

**Geometric picture.** At optimum, active constraints “push back” against the objective’s descent direction; multipliers are the weights of these pushes.

---

## 2) Indicator-function reformulation (extended-value view)

Define indicators:

$$
I_-(u)=
\begin{cases}
0, & u\le 0,\\
+\infty, & u>0,
\end{cases}
\qquad
I_0(u)=
\begin{cases}
0, & u=0,\\
+\infty, & u\ne 0.
\end{cases}
$$

Then the constrained problem is equivalent to:

$$
\min_x\ \Big(
f_0(x)+\sum_{i=1}^m I_-(f_i(x))+\sum_{j=1}^p I_0(h_j(x))
\Big).
$$

**Convexity note.**
- If $f_0,f_i$ are convex and $h_j$ affine (standard convex form), this extended-value objective is convex.
- If the original feasibility set is nonconvex (e.g., nonlinear equalities or nonconvex $f_i$), this form is generally nonconvex too (it encodes the same set).

---

## 3) Why replace indicators by linear underestimators?

For any $\lambda\ge 0$ and any $u\in\mathbb{R}$,

$$
\lambda u \le I_-(u).
$$

For any $\nu\in\mathbb{R}$ and any $u\in\mathbb{R}$,

$$
\nu u \le I_0(u).
$$

So replacing indicator terms with $\lambda_i f_i(x)$ and $\nu_j h_j(x)$ produces a global lower bound on the extended-value objective.

**Key identity (supporting-hyperplane envelope).**

$$
I_-(u)=\sup_{\lambda\ge 0}\lambda u,
\qquad
I_0(u)=\sup_{\nu\in\mathbb{R}}\nu u.
$$

This is the deeper reason the “linear estimator” is canonical: the indicator is literally the pointwise supremum of these linear functionals.

---

## 4) Dual function = best bound from Lagrangian relaxation

**Dual function**

$$
g(\lambda,\nu)=\inf_x L(x,\lambda,\nu).
$$

**Lower bound property (weak duality).** For any feasible $x$ and any $\lambda\ge 0$,

$$
g(\lambda,\nu)\le L(x,\lambda,\nu)\le f_0(x)
\quad \Rightarrow \quad
g(\lambda,\nu)\le p^\star.
$$

**Dual problem**

$$
\max_{\lambda\ge 0,\ \nu\in\mathbb{R}^p}\ g(\lambda,\nu).
$$

Interpretation: choose multipliers to make the lower bound as large (tight) as possible.

---

## 5) Concavity of the dual function

For each fixed $x$, the function $(\lambda,\nu)\mapsto L(x,\lambda,\nu)$ is affine.
Therefore $g(\lambda,\nu)$ is the pointwise infimum of affine functions, hence concave:

$$
g(\theta z_1+(1-\theta)z_2)\ge
\theta g(z_1)+(1-\theta)g(z_2),
\qquad
\theta\in[0,1].
$$

**Important distinction.**
- Pointwise supremum of convex functions is convex.
- Pointwise infimum of affine functions is concave.
- Pointwise infimum of general convex functions is not guaranteed convex/concave.

---

## 6) “Dual is convex even if primal is nonconvex” — what is true?

The dual feasible set is typically just

$$
\lambda\ge 0,\qquad \nu\ \text{free}.
$$

That set is convex regardless of primal convexity, and $g$ is concave regardless of primal convexity, so the dual is a convex optimization problem **in the dual variables**.

**Two caveats.**
- Even though the dual is convex, evaluating $g(\lambda,\nu)=\inf_x L(x,\lambda,\nu)$ can be computationally hard.
- Nonconvex primals can have a duality gap: $d^\star < p^\star$.

---

## 7) KKT conditions (convex case + regularity)

Assume convexity (convex $f_0,f_i$, affine $h_j$) and a constraint qualification (e.g., Slater).
Then $x^\star,\lambda^\star,\nu^\star$ are optimal iff:

**Primal feasibility**

$$
f_i(x^\star)\le 0,\qquad h_j(x^\star)=0.
$$

**Dual feasibility**

$$
\lambda^\star\ge 0.
$$

**Stationarity**

$$
\nabla f_0(x^\star)+\sum_{i=1}^m \lambda_i^\star \nabla f_i(x^\star)+\sum_{j=1}^p \nu_j^\star \nabla h_j(x^\star)=0.
$$

**Complementary slackness**

$$
\lambda_i^\star f_i(x^\star)=0\quad \forall i.
$$

Geometric meaning: the objective gradient is balanced by a nonnegative combination of active constraint normals; inactive constraints get zero multiplier.

---

## 8) When computing $g(\lambda,\nu)$ is NP-hard

Computing $g$ requires solving:

$$
g(\lambda,\nu)=\inf_x \Big(f_0(x)+\sum_i \lambda_i f_i(x)+\sum_j \nu_j h_j(x)\Big).
$$

This becomes NP-hard when the inner minimization is a hard global optimization problem, e.g.:

- Discrete/integer variables (MILP/MIQP structure).
- Nonconvex continuous minimization (indefinite QP over a polytope/box, general polynomial optimization).
- Nonlinear equalities producing nonconvex feasible geometry.
- Nonconvex $f_0$ (already makes $g(0,0)=\inf_x f_0(x)$ hard in general).

---

## 9) When duality is most useful (practical rule)

Duality is especially effective when:
- $g(\lambda,\nu)$ is efficiently computable (inner problem is convex or decomposes nicely), and
- the duality gap is small (often zero in convex problems under Slater; sometimes small/zero due to special structure in nonconvex problems).

Even with a nonconvex primal, a tractable dual can still provide strong lower bounds useful for:
- certificates and benchmarking,
- branch-and-bound pruning,
- relaxation-based algorithms.

---

## 10) Numerical examples (primal + dual variables)

### Example A: 1D quadratic with box constraints

Primal:

$$
\min_x\ 3x^2-2x-1
\quad \text{s.t.}\quad
0\le x\le 4.
$$

Unconstrained minimizer:

$$
\frac{d}{dx}(3x^2-2x-1)=6x-2=0
\Rightarrow x^\star=\frac{1}{3}.
$$

Since $x^\star\in(0,4)$, both constraints are inactive, so multipliers are zero:

$$
\lambda_{x\le 4}^\star=0,\qquad \lambda_{x\ge 0}^\star=0.
$$

Optimal value:

$$
f(x^\star)=3\cdot\frac{1}{9}-2\cdot\frac{1}{3}-1=-\frac{4}{3}.
$$

---

### Example B: 2D convex quadratic with multiple active constraints

Primal:

$$
\min_{x,y}\ (x-5)^2+(y-3)^2
$$

s.t.

$$
x+y\le 3,\qquad x\le 1.5,\qquad x\ge 0,\ y\ge 0.
$$

Candidate optimum on active constraints $x=1.5$ and $x+y=3$:

$$
x^\star=1.5,\qquad y^\star=1.5.
$$

KKT multipliers:
- active: $x+y\le 3$ with $\lambda_1^\star>0$, $x\le 1.5$ with $\lambda_2^\star>0$
- inactive: $x\ge 0$ and $y\ge 0$ so their multipliers are zero

Stationarity at $(1.5,1.5)$:

$$
\nabla f(x,y)=
\begin{bmatrix}
2(x-5)\\
2(y-3)
\end{bmatrix}
\Rightarrow
\nabla f(1.5,1.5)=
\begin{bmatrix}
-7\\
-3
\end{bmatrix}.
$$

Constraint normals:
- for $x+y\le 3$: $(1,1)$
- for $x\le 1.5$: $(1,0)$

So

$$
\begin{bmatrix}
-7\\
-3
\end{bmatrix}
+\lambda_1
\begin{bmatrix}
1\\
1
\end{bmatrix}
+\lambda_2
\begin{bmatrix}
1\\
0
\end{bmatrix}
=
\begin{bmatrix}
0\\
0
\end{bmatrix}.
$$

From the $y$-component: $-3+\lambda_1=0\Rightarrow \lambda_1^\star=3$.

From the $x$-component: $-7+\lambda_1+\lambda_2=0\Rightarrow \lambda_2^\star=4$.

Thus

$$
(x^\star,y^\star)=(1.5,1.5),
\qquad
\lambda_1^\star=3,\quad \lambda_2^\star=4,
$$

and nonnegativity multipliers are $0$.

Objective value:

$$
f(x^\star,y^\star)=(-3.5)^2+(-1.5)^2=12.25+2.25=14.5.
$$

---

## 11) Quick mental “translation rules”

- Lagrangian = objective + priced constraints:

  $$
  L=f_0+\lambda^\top f+\nu^\top h.
  $$

- Dual function is a bound:

  $$
  g(\lambda,\nu)=\inf_x L(x,\lambda,\nu)\le p^\star.
  $$

- Concavity comes from “inf of affine”:

  $$
  g \ \text{concave in }(\lambda,\nu).
  $$

- Dual feasible set is convex even if primal is not:

  $$
  \lambda\ge 0,\ \nu\ \text{free}.
  $$

- Duality is most computationally useful when the inner infimum is tractable and the gap is small.
