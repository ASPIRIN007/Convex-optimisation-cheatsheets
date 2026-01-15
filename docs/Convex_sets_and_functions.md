## 0) Quick symbols I keep using

- Convex combination:  \(\theta x + (1-\theta)y\), \(\theta\in[0,1]\)
- Affine combination: \(\theta x + (1-\theta)y\), \(\theta\in\mathbb{R}\)
- Epigraph:  \(\mathrm{epi}\,f=\{(x,t)\mid f(x)\le t\}\)
- Hypograph: \(\mathrm{hypo}\,f=\{(x,t)\mid f(x)\ge t\}\)
- Sublevel set: \(S_\alpha=\{x\mid f(x)\le \alpha\}\)
- Superlevel set: \(U_\alpha=\{x\mid f(x)\ge \alpha\}\)

---

# PART A — Convex sets (what you prove 100 times)

## 1) “k-point convexity” from the 2-point definition (exercise 2.1 idea)

**Given:** \(C\subseteq\mathbb{R}^n\) convex in the usual sense (2-point).
Show: for \(x_1,\dots,x_k\in C\), \(\theta_i\ge0\), \(\sum_{i=1}^k\theta_i=1\),
\[
\sum_{i=1}^k\theta_i x_i\in C.
\]

**Induction template**
- Base \(k=2\): exactly convexity definition.
- Step \(k\to k+1\): let \(\bar\theta=\sum_{i=1}^k\theta_i = 1-\theta_{k+1}\).
  - If \(\bar\theta=0\): then \(\theta_{k+1}=1\) and the point is \(x_{k+1}\in C\).
  - Else define normalized weights \(\tilde\theta_i=\theta_i/\bar\theta\) for \(i\le k\),
    so \(\sum_{i=1}^k\tilde\theta_i=1\).
  - By induction, \(z=\sum_{i=1}^k\tilde\theta_i x_i \in C\).
  - Then
    \[
    \sum_{i=1}^{k+1}\theta_i x_i
    = \bar\theta\, z + \theta_{k+1}x_{k+1},
    \]
    which is a **2-point convex combination** of \(z\in C\) and \(x_{k+1}\in C\),
    hence is in \(C\).

This proof is the one I reuse whenever I need “finite convex combinations”.

---

## 2) Voronoi halfspace (exercise 2.7)

Set:
\[
\{x\mid \|x-a\|_2\le \|x-b\|_2\}.
\]

Square both sides:
\[
\|x-a\|_2^2 \le \|x-b\|_2^2
\]
\[
(x-a)^T(x-a)\le (x-b)^T(x-b).
\]

Expand and cancel \(x^Tx\):
\[
-2a^Tx + a^Ta \le -2b^Tx + b^Tb
\]
\[
2(b-a)^T x \le b^Tb - a^Ta.
\]

So it’s the (closed) halfspace
\[
(b-a)^T x \le \frac{\|b\|_2^2-\|a\|_2^2}{2}.
\]

Geometric: boundary is the perpendicular bisector hyperplane.

---

## 3) Polyhedra check style (exercise 2.8 vibe)

A set is a **polyhedron** if it can be written as
\[
S=\{x\mid Ax\le b,\ Fx=g\}.
\]

### (a) \(S=\{y_1a_1+y_2a_2\mid -1\le y_1\le 1,\ -1\le y_2\le 1\}\)
This is the linear image of a box:
\[
y\in[-1,1]^2,\quad x=Ay,\ A=[a_1\ a_2].
\]
Since \([-1,1]^2\) is a polyhedron and linear maps preserve polyhedrality, \(S\) is a polyhedron.

A direct explicit form is possible but not necessary; the “linear image of a polyhedron” argument is already enough.

### (b) \(x\ge0,\ \mathbf{1}^Tx=1,\ \sum x_i a_i=b_1,\ \sum x_i a_i^2=b_2\)
All are linear equalities/inequalities in \(x\). So it is a polyhedron:
\[
\underbrace{x\ge0}_{Ax\le b},\qquad
\underbrace{\begin{bmatrix}\mathbf{1}^T\\ a^T\\ (a^{\circ 2})^T\end{bmatrix}x=\begin{bmatrix}1\\ b_1\\ b_2\end{bmatrix}}_{Fx=g}.
\]

### (c) \(x\ge0,\ x^Ty\le 1\ \forall y:\|y\|_2=1\)
Use dual norm:
\[
\sup_{\|y\|_2=1} x^Ty = \|x\|_2.
\]
So condition “for all unit \(y\)” is equivalent to \(\|x\|_2\le 1\) plus \(x\ge0\).
That set is convex but **not** polyhedral (Euclidean ball boundary is curved).

### (d) \(x\ge0,\ x^Ty\le 1\ \forall y:\sum |y_i|=1\)
Here the constraint is
\[
\sup_{\|y\|_1=1} x^Ty = \|x\|_\infty \le 1.
\]
So it becomes
\[
x\ge0,\quad x_i\le 1\ \forall i,
\]
which **is** polyhedral.

---

## 4) Classic “product ≥ 1” convex set (exercise 2.11)

Set \(S=\{(x_1,x_2)\in\mathbb{R}^2_{++}\mid x_1x_2\ge 1\}\).

Take two points \(u=(u_1,u_2)\), \(v=(v_1,v_2)\) in \(S\).
We need \((\theta u + (1-\theta)v)\in S\), i.e.
\[
(\theta u_1 + (1-\theta)v_1)(\theta u_2 + (1-\theta)v_2)\ge 1.
\]

Useful path (the hint is AM–GM):
- Since \(u_1u_2\ge1\) and \(v_1v_2\ge1\), then
  \[
  (u_1u_2)^\theta (v_1v_2)^{1-\theta}\ge 1.
  \]
- Rewrite:
  \[
  (u_1^\theta v_1^{1-\theta})(u_2^\theta v_2^{1-\theta})\ge 1.
  \]
- Apply weighted AM–GM on each coordinate:
  \[
  u_1^\theta v_1^{1-\theta}\le \theta u_1+(1-\theta)v_1,
  \]
  \[
  u_2^\theta v_2^{1-\theta}\le \theta u_2+(1-\theta)v_2.
  \]
- Multiply inequalities:
  \[
  (u_1^\theta v_1^{1-\theta})(u_2^\theta v_2^{1-\theta})
  \le (\theta u_1+(1-\theta)v_1)(\theta u_2+(1-\theta)v_2).
  \]
Combine with the earlier \(\ge 1\) to get the desired product \(\ge1\).
So \(S\) is convex.

---

# PART B — Convex functions (how you *recognize* convexity)

## 5) Epigraph facts you keep using

- \(f\) is convex  \(\iff\)  \(\mathrm{epi}(f)\) is convex.
- \(f\) is concave \(\iff\) \(\mathrm{hypo}(f)\) is convex.
- \(f\) is quasiconvex \(\iff\) all sublevel sets \(S_\alpha\) are convex.
- \(f\) is quasiconcave \(\iff\) all superlevel sets \(U_\alpha\) are convex.

**Halfspace / cone / polyhedron epigraph test (exercise 3.6 idea)**
- \(\mathrm{epi}(f)\) is a halfspace  
  \(\iff f\) is affine (since a halfspace boundary is a hyperplane \(t=a^Tx+b\)).
- \(\mathrm{epi}(f)\) is a polyhedron  
  \(\iff f\) is polyhedral convex, i.e. \(f(x)=\max_i(a_i^Tx+b_i)\).
- \(\mathrm{epi}(f)\) is a convex cone  
  \(\iff f\) is convex + positively homogeneous: \(f(\lambda x)=\lambda f(x)\) for \(\lambda\ge0\),
  and \(f(0)=0\). (Then epi is closed under conic scaling.)

---

## 6) Linear-fractional example (why “open” and “closed” halfspaces show up)

For
\[
f(x)=\frac{a^Tx+b}{c^Tx+d},\qquad \mathrm{dom}f=\{x\mid c^Tx+d>0\}.
\]

\(\alpha\)-sublevel set:
\[
S_\alpha=\left\{x\mid c^Tx+d>0,\ \frac{a^Tx+b}{c^Tx+d}\le \alpha\right\}.
\]

Because \(c^Tx+d>0\), multiply without flipping sign:
\[
a^Tx+b \le \alpha(c^Tx+d)
\iff (a-\alpha c)^Tx \le \alpha d - b.
\]

So
\[
S_\alpha = \underbrace{\{x\mid c^Tx+d>0\}}_{\text{open halfspace}}
\ \cap\ 
\underbrace{\{x\mid (a-\alpha c)^Tx \le \alpha d-b\}}_{\text{closed halfspace}}.
\]

Intersection is convex \(\Rightarrow\) \(f\) is quasiconvex.
(Same logic for superlevel sets gives quasiconcave too → quasilinear.)

---

## 7) “Check on lines” principle (quasiconvex)

\(f\) is quasiconvex iff its restriction to any line is quasiconvex.

Reason: sublevel sets convex in \(\mathbb{R}^n\) iff their intersection with any line is convex in \(\mathbb{R}\).
So to test quasiconvexity you can reduce to 1D along \(x(t)=x_0+tv\).

---

## 8) Differentiable quasiconvex condition (why it’s NOT automatic)

Claim (Boyd 3.4.3):
\[
f(y)\le f(x)\ \Rightarrow\ \nabla f(x)^T(y-x)\le 0.
\]

Why it’s meaningful:
- In 1D, this says: if moving from \(x\) to \(y\) decreases \(f\), then the derivative at \(x\) must not “point uphill” toward \(y\).
- In higher-D, \(\nabla f(x)\) gives the direction of **steepest increase**. So if \(y\) has lower value, the vector to \(y\) must make an obtuse angle with \(\nabla f(x)\).

Not true for arbitrary differentiable functions.
It encodes: “sublevel sets are convex so gradients support them.”

Also: gradient is **not** \(\frac{f(y)-f(x)}{y-x}\) in \(\mathbb{R}^n\).
That’s a *finite difference slope along a direction*. Gradient is the vector of partial derivatives and satisfies:
\[
f(x+tv)=f(x)+t\nabla f(x)^Tv+o(t).
\]

---

# PART C — Schur complement (where it comes from)

## 9) Quadratic in \((x,y)\) and minimizing out \(y\)

Let
\[
f(x,y)=x^TAx + 2x^TBy + y^TCy,
\quad
\begin{bmatrix}A & B\\ B^T & C\end{bmatrix}\succeq 0,\ C\succ0.
\]

Fix \(x\). Minimize over \(y\):
- This is a convex quadratic in \(y\) since \(C\succ0\).
- First-order condition:
  \[
  \nabla_y f = 2B^Tx + 2Cy = 0
  \Rightarrow y^\star = -C^{-1}B^Tx.
  \]
Plug back:
\[
g(x)=\inf_y f(x,y)
= x^TAx - 2x^TB C^{-1}B^T x + x^T B C^{-1} C C^{-1}B^T x
= x^T(A-BC^{-1}B^T)x.
\]

Now the key logic:
- If the block matrix is PSD and \(C\succ0\), then for every \(x\),
  \[
  g(x)=\inf_y f(x,y)\ge 0.
  \]
- But \(g(x)=x^T(A-BC^{-1}B^T)x\ge 0\ \forall x\) holds **iff**
  \[
  A-BC^{-1}B^T\succeq 0.
  \]
That matrix \(A-BC^{-1}B^T\) is the Schur complement of \(C\).

---

# PART D — Conjugates (the “support function” viewpoint)

## 10) Definition + what “dom \(f^\*\)” actually means

\[
f^\*(y)=\sup_{x\in\mathrm{dom}f}(y^Tx-f(x)).
\]

- For each fixed \(x\), the map \(y\mapsto y^Tx-f(x)\) is affine in \(y\).
- Sup of affine functions is convex \(\Rightarrow f^\*\) always convex (even if \(f\) isn’t).

**Domain**
\[
\mathrm{dom}f^\*=\{y\mid \sup_x(y^Tx-f(x))<\infty\}.
\]
If for some \(y\) the expression can blow up to \(+\infty\), then \(f^\*(y)=+\infty\) and that \(y\notin\mathrm{dom}f^\*\).

That’s why some examples just report “\(\mathrm{dom}f^\*=\mathbb{R}_+\)” etc:
they’re saying “outside this set the value is \(+\infty\)”.

---

## 11) Conjugate of max function (exercise 3.36a)

Let \(f(x)=\max_{i=1,\dots,n} x_i\) on \(\mathbb{R}^n\).

Compute:
\[
f^\*(y)=\sup_x\left(y^Tx-\max_i x_i\right).
\]

Key trick: use the representation
\[
\max_i x_i = \sup_{\lambda\in\Delta}\ \lambda^T x,
\qquad
\Delta=\{\lambda\ge0,\ \mathbf{1}^T\lambda=1\}.
\]
So
\[
y^Tx-\max_i x_i
= y^Tx-\sup_{\lambda\in\Delta}\lambda^Tx
= \inf_{\lambda\in\Delta}(y-\lambda)^Tx.
\]

Now:
- If \(y\notin\Delta\), we can pick a direction in \(x\) that makes \((y-\lambda)^Tx\) arbitrarily large → supremum is \(+\infty\).
- If \(y\in\Delta\), choose \(\lambda=y\), then \((y-\lambda)^Tx=0\) for all \(x\), so the supremum is \(0\).

Therefore
\[
f^\*(y)=
\begin{cases}
0, & y\in\Delta\ (\text{i.e. } y\ge0,\ \mathbf{1}^Ty=1),\\
+\infty, & \text{otherwise}.
\end{cases}
\]
So it’s the indicator of the simplex.

---

## 12) Power function conjugate (exercise 3.36d)

### Case 1: \(f(x)=x^p\) on \(\mathbb{R}_{++}\), \(p>1\)

\[
f^\*(y)=\sup_{x>0}(yx-x^p).
\]

If \(y\le 0\): the best is to push \(x\to0^+\), giving supremum \(0\).
If \(y>0\): optimize derivative:
\[
\frac{d}{dx}(yx-x^p)=y-px^{p-1}=0
\Rightarrow x^\*=\left(\frac{y}{p}\right)^{\frac{1}{p-1}}.
\]
Plug in:
\[
f^\*(y)=y\left(\frac{y}{p}\right)^{\frac{1}{p-1}}
-\left(\frac{y}{p}\right)^{\frac{p}{p-1}}
= \left(\frac{p-1}{p^{\frac{p}{p-1}}}\right)y^{\frac{p}{p-1}},
\quad y>0.
\]

So
\[
f^\*(y)=
\begin{cases}
0,& y\le 0,\\[4pt]
\displaystyle \frac{p-1}{p^{p/(p-1)}}\,y^{p/(p-1)},& y>0.
\end{cases}
\]

### Case 2: \(p<0\) on \(\mathbb{R}_{++}\)

Then \(x^p\to +\infty\) as \(x\to0^+\) and \(x^p\to0\) as \(x\to\infty\).
You handle it the same way:
- For \(y>0\): \(yx-x^p\to+\infty\) as \(x\to\infty\) ⇒ \(f^\*(y)=+\infty\).
- For \(y<0\): interior maximizer exists; solve \(y-px^{p-1}=0\) again.
You get a finite expression on \(y<0\) and \(+\infty\) otherwise.

(When I do this fast, I just check the tails first to know where it’s finite.)

---

# PART E — Duality + KKT (the part you actually solve problems with)

## 13) Lagrangian + dual function

Primal:
\[
\min_x\ f_0(x)\quad\text{s.t.}\quad f_i(x)\le0,\ h_j(x)=0.
\]

Lagrangian:
\[
L(x,\lambda,\nu)=f_0(x)+\sum_{i=1}^m\lambda_i f_i(x)+\sum_{j=1}^p\nu_j h_j(x),
\quad \lambda\ge0.
\]

Dual function:
\[
g(\lambda,\nu)=\inf_x L(x,\lambda,\nu).
\]

Weak duality:
\[
g(\lambda,\nu)\le p^\star\quad\forall\lambda\ge0,\nu.
\]

Dual problem:
\[
\max_{\lambda\ge0,\nu} g(\lambda,\nu).
\]

**Why dual is always “convex in dual vars”**
- For fixed \(x\), \(L\) is affine in \((\lambda,\nu)\).
- Infimum of affine functions is concave → \(g\) concave.
- Maximizing concave over convex set (\(\lambda\ge0\)) = convex optimization in \((\lambda,\nu)\).

---

## 14) KKT (convex case + Slater)

Assume:
- \(f_0,f_i\) convex
- \(h_j\) affine
- Slater holds

Then \(x^\*,\lambda^\*,\nu^\*\) optimal iff:

**Primal feasibility:** \(f_i(x^\*)\le0,\ h(x^\*)=0\)

**Dual feasibility:** \(\lambda^\*\ge0\)

**Stationarity:**
\[
\nabla f_0(x^\*)+\sum_i\lambda_i^\*\nabla f_i(x^\*)+\sum_j\nu_j^\*\nabla h_j(x^\*)=0
\]

**Complementary slackness:**
\[
\lambda_i^\* f_i(x^\*)=0\ \ \forall i.
\]

Mental picture:
- Only active constraints can have nonzero multipliers.
- Objective gradient is balanced by active constraint normals.

---

# PART F — Probability simplex sets (convexity checklist)

Let \(p\in\mathbb{R}^n\), \(p\ge0\), \(\mathbf{1}^Tp=1\).
Random variable \(x\) takes values \(a_i\) with \(\Pr(x=a_i)=p_i\).

Common move:
- Any expectation \(\mathbb{E}[\phi(x)]=\sum_i p_i\phi(a_i)\) is **affine in \(p\)**.

So constraints like \(\alpha\le \mathbb{E}[\phi(x)]\le\beta\) are just halfspaces in \(p\).

Variance:
\[
\mathrm{var}(x)=\mathbb{E}[x^2]-(\mathbb{E}[x])^2.
\]
Affine minus convex quadratic → concave in \(p\).
So:
- \(\mathrm{var}(x)\le \alpha\): sublevel of concave → generally **not convex**.
- \(\mathrm{var}(x)\ge \alpha\): superlevel of concave → **convex**.

Quantile:
\[
\mathrm{quartile}(x)=\inf\{\beta\mid \Pr(x\le \beta)\ge 0.25\}.
\]
For discrete \(a_i\), \(\Pr(x\le \beta)=\sum_{i:a_i\le \beta} p_i\) is affine in \(p\),
so conditions of the form “quartile \(\le\) / \(\ge\)” become intersections of halfspaces.

---

# PART G — Approximation width (exercise 3.42 flavor)

Given fixed \(\epsilon>0\),
\[
W(x)=\sup\{T\mid |x_1f_1(t)+\cdots+x_nf_n(t)-f_0(t)|\le\epsilon,\ 0\le t\le T\}.
\]

For a fixed \(t\), define \(a(t)=(f_1(t),\dots,f_n(t))\).
Then
\[
|a(t)^Tx-f_0(t)|\le \epsilon
\iff
\begin{cases}
a(t)^Tx \le f_0(t)+\epsilon\\
-a(t)^Tx \le \epsilon-f_0(t)
\end{cases}
\]
(two halfspaces in \(x\)).

Superlevel set of \(W\):
\[
\{x\mid W(x)\ge T\}
=
\bigcap_{t\in[0,T]}
\{x\mid |a(t)^Tx-f_0(t)|\le\epsilon\}.
\]
Intersection of convex sets → convex.
So \(W\) is **quasiconcave**.

(Intuition: “having width at least \(T\)” is a robust feasibility condition over all \(t\le T\).)

---
