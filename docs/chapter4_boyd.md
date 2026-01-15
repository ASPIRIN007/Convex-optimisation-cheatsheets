# Lagrangian Duality + KKT + Sensitivity — My Notes (Boyd Ch.5 vibe)

We start with the primal problem

$$
\begin{aligned}
\min_x\quad & f_0(x)\\
\text{s.t.}\quad & f_i(x)\le 0,\quad i=1,\dots,m,\\
& h_j(x)=0,\quad j=1,\dots,p.
\end{aligned}
$$

Throughout: $$\lambda\in\mathbb{R}^m,\ \lambda\ge 0,\qquad \nu\in\mathbb{R}^p\ \text{free}.$$

---

## 0) Quick picture in my head

- Primal: optimize over $$x$$ directly with constraints.
- Dual: optimize over "prices" $$\lambda,\nu$$ that penalize constraint violation.
- Dual is always a convex optimization problem in $$\lambda,\nu$$ (maximize a concave function over a convex set).
- Gap: $$d^\star \le p^\star$$ always (weak duality), equality needs conditions (strong duality).

---

## 1) Lagrangian: turning constraints into penalties

Define the Lagrangian

$$
L(x,\lambda,\nu)= f_0(x)+\sum_{i=1}^m \lambda_i f_i(x)+\sum_{j=1}^p \nu_j h_j(x).
$$

### Why $$\lambda_i\ge 0$$ for inequalities?
If $$x$$ is feasible, $$f_i(x)\le 0$$, so with $$\lambda_i\ge 0$$ we get $$\lambda_i f_i(x)\le 0$$ and hence

$$
L(x,\lambda,\nu)\le f_0(x)\qquad \text{for all feasible } x.
$$

So for any fixed $$\lambda\ge 0,\nu$$, minimizing $$L$$ over $$x$$ gives something that can’t exceed the true optimum.

---

## 2) Dual function = best lower bound from a fixed pricing

Dual function

$$
g(\lambda,\nu)=\inf_x L(x,\lambda,\nu).
$$

### Lower-bound (weak duality) in one line
For any feasible $$x$$ and any $$\lambda\ge 0$$:

$$
g(\lambda,\nu)\le L(x,\lambda,\nu)\le f_0(x).
$$

Taking inf over feasible $$x$$ gives

$$
g(\lambda,\nu)\le p^\star.
$$

So every dual-feasible point gives a certificate lower bound.

---

## 3) Dual problem

$$
\begin{aligned}
\max_{\lambda,\nu}\quad & g(\lambda,\nu)\\
\text{s.t.}\quad & \lambda\ge 0.
\end{aligned}
$$

Let dual optimal value be $$d^\star$$. Always:

$$
d^\star \le p^\star.
$$

---

## 4) Why is the dual always “convex” (even if primal is not)?

For fixed $$x$$, $$L(x,\lambda,\nu)$$ is affine in $$(\lambda,\nu)$$.
Then $$g(\lambda,\nu)=\inf_x L(x,\lambda,\nu)$$ is the pointwise infimum of affine functions,
hence **concave**.

Therefore maximizing $$g$$ over a convex set ($$\lambda\ge 0$$) is a convex optimization problem (in the dual variables).

Big caution: even if the dual is convex, evaluating $$g(\lambda,\nu)=\inf_x L$$ can be hard if the inner inf is nonconvex/NP-hard.

---

## 5) Indicator-function viewpoint (why Lagrangian is natural)

Define indicator functions

$$
I_-(u)=
\begin{cases}
0,& u\le 0\\
+\infty,& u>0
\end{cases}
\qquad
I_0(u)=
\begin{cases}
0,& u=0\\
+\infty,& u\neq 0
\end{cases}
$$

Then primal is equivalent to unconstrained extended-value minimization:

$$
\min_x\ \Big(
f_0(x)+\sum_{i=1}^m I_-(f_i(x))+\sum_{j=1}^p I_0(h_j(x))
\Big).
$$

Key envelope identities:

$$
I_-(u)=\sup_{\lambda\ge 0}\lambda u,
\qquad
I_0(u)=\sup_{\nu\in\mathbb{R}}\nu u.
$$

So duality is basically: replace hard indicators by their linear supports and swap inf/sup (when allowed).

---

## 6) Strong duality: when do we get $$p^\star=d^\star$$?

### In convex optimization (standard case)
Assume
- $$f_0,f_i$$ convex,
- $$h_j$$ affine,
- a constraint qualification holds (Slater is the common one).

**Slater condition (one clean version):**
There exists $$\tilde x$$ such that
$$
f_i(\tilde x)<0\ \forall i,\qquad h(\tilde x)=0.
$$

Then strong duality holds and dual optimum is attained:
$$
p^\star=d^\star,\qquad (\lambda^\*,\nu^\*)\ \text{exists}.
$$

---

## 7) KKT conditions (the checklist I always use)

Under convexity + CQ (e.g. Slater), $$x^\*$$ is primal optimal iff there exist $$\lambda^\*,\nu^\*$$ such that:

### (i) Primal feasibility
$$
f_i(x^\*)\le 0,\quad i=1,\dots,m,\qquad h_j(x^\*)=0,\quad j=1,\dots,p.
$$

### (ii) Dual feasibility
$$
\lambda^\*\ge 0.
$$

### (iii) Complementary slackness
$$
\lambda_i^\*\,f_i(x^\*)=0\quad \forall i.
$$

Meaning: if constraint is inactive ($$f_i(x^\*)<0$$), multiplier must be zero.

### (iv) Stationarity
If differentiable:

$$
\nabla f_0(x^\*)+\sum_{i=1}^m \lambda_i^\*\nabla f_i(x^\*)+\sum_{j=1}^p \nu_j^\*\nabla h_j(x^\*)=0.
$$

If nonsmooth, replace gradients by subgradients:

$$
0\in \partial f_0(x^\*)+\sum_{i=1}^m \lambda_i^\*\partial f_i(x^\*)+\sum_{j=1}^p \nu_j^\*\nabla h_j(x^\*).
$$

### Geometric meaning of stationarity
Objective descent direction is blocked by a conic combination of active constraint normals + equality normals.
It’s like: "net force = 0".

---

## 8) Saddle point view (useful for intuition)

If strong duality holds and KKT holds, then $$ (x^\*,\lambda^\*,\nu^\*) $$ is a saddle point:

$$
L(x^\*,\lambda,\nu)\le L(x^\*,\lambda^\*,\nu^\*)\le L(x,\lambda^\*,\nu^\*)
$$

for all $$x$$ and all $$\lambda\ge 0,\nu$$.

---

## 9) Sensitivity / perturbation (this is the part in your slide)

Perturbed primal:

$$
\begin{aligned}
p^\*(u,v)=\min_x\quad & f_0(x)\\
\text{s.t.}\quad & f_i(x)\le u_i,\quad i=1,\dots,m,\\
& h_j(x)=v_j,\quad j=1,\dots,p.
\end{aligned}
$$

Dual of perturbed problem becomes:

$$
\max_{\lambda\ge 0,\nu}\ \ g(\lambda,\nu)-u^T\lambda - v^T\nu.
$$

### Global sensitivity inequality (works without differentiability)
Assume strong duality holds at unperturbed problem and $$\lambda^\*,\nu^\*$$ are optimal for $$u=0,v=0$$.
Apply weak duality to the perturbed problem using the same $$\lambda^\*,\nu^\*$$:

$$
p^\*(u,v)\ge g(\lambda^\*,\nu^\*)-u^T\lambda^\*-v^T\nu^\*
$$

but $$g(\lambda^\*,\nu^\*)=p^\*(0,0)$$ (strong duality), so

$$
\boxed{
p^\*(u,v)\ge p^\*(0,0)-u^T\lambda^\*-v^T\nu^\*.
}
$$

Interpretation: $$(-\lambda^\*,-\nu^\*)$$ gives a supporting hyperplane (global affine lower bound) to the value function.

### Local sensitivity (when differentiable at the origin)
If $$p^\*(u,v)$$ is differentiable at $$(0,0)$$, then:

$$
\boxed{
\lambda_i^\*=-\frac{\partial p^\*(0,0)}{\partial u_i},
\qquad
\nu_j^\*=-\frac{\partial p^\*(0,0)}{\partial v_j}.
}
$$

So multipliers are literally slopes of optimal value vs. constraint right-hand sides.

Important nuance I keep in mind:
- The inequality gives a bound for all perturbations.
- It does NOT say the optimal dual variables stay the same for all perturbations.
- Dual can change when the active set / slope region changes (kinks in $$p^\*$$).

---

## 10) “Weak duality always holds” (convex or not)

Yes: weak duality uses only:
- $$\lambda\ge 0$$
- definition of $$g=\inf_x L$$
- feasibility implies $$L\le f_0$$

So for any problem (convex/nonconvex):

$$
d^\star\le p^\star.
$$

Strong duality is the one that needs conditions.

---

## 11) Conjugate-function lens (fast way to see duals)

If a primal has the form
$$
\min_x\ f(x)+g(Ax)
$$
then dual often pops out using Fenchel conjugates:

$$
f^\*(y)=\sup_x (y^T x - f(x)).
$$

Typical dual template:

$$
\max_y\ -f^\*(-A^T y)-g^\*(y).
$$

This is why “norm - linear” or “sup(y^T x - ||x||)” shows up: it’s exactly a conjugate computation.

Useful identity:
$$
(||\cdot||)^\*(y)=I_{\{||y||_\*\le 1\}}(y).
$$

---

## 12) Two quick examples I like to sanity-check with KKT

### Example 1: Simple inequality constraint (1D)
$$
\min_x\ x^2\quad \text{s.t.}\quad x\ge 1.
$$

Write as $$f(x)=x^2$$, constraint $$1-x\le 0$$.
Lagrangian:
$$
L=x^2+\lambda(1-x),\quad \lambda\ge 0.
$$
Stationarity:
$$
2x-\lambda=0 \Rightarrow \lambda=2x.
$$
Complementary slackness: constraint active at optimum, so $$x^\*=1$$, hence $$\lambda^\*=2$$.
Sensitivity: if constraint becomes $$x\ge 1-\epsilon$$ (i.e. RHS perturbed), optimal value changes slope = $$-\lambda^\*$$ in the right coordinate.

### Example 2: Equality constrained least squares
$$
\min_x\ \frac12||x||_2^2\quad \text{s.t.}\quad Ax=b.
$$
Lagrangian:
$$
L=\frac12||x||_2^2+\nu^T(Ax-b)
$$
Stationarity:
$$
x + A^T\nu=0\Rightarrow x^\*=-A^T\nu^\*.
$$
Plug in constraint:
$$
A(-A^T\nu^\*)=b \Rightarrow -(AA^T)\nu^\*=b.
$$
So $$\nu^\*=-(AA^T)^{-1}b$$ if invertible.

---

## 13) Mini translation table (stuff I use while solving)

- Dual variable for inequality: nonnegative
$$
f_i(x)\le 0\Rightarrow \lambda_i\ge 0
$$

- Dual variable for equality: free
$$
h(x)=0\Rightarrow \nu\in\mathbb{R}
$$

- Complementary slackness:
$$
\lambda_i>0\Rightarrow f_i(x^\*)=0,\qquad f_i(x^\*)<0\Rightarrow \lambda_i=0
$$

- Strong duality + KKT = saddle point and sensitivity slopes.

- Dual is convex optimization in $$(\lambda,\nu)$$, even if primal isn't.

---
