# Strong Convexity + Smoothness (β-smooth) — Cheat Sheet

Let \(f:\mathbb{R}^n\to\mathbb{R}\) be differentiable (unless stated otherwise).  
We write \(\|\cdot\|\) for the Euclidean norm.

---

## 0) The one-line picture (what these properties *mean*)

- **Strong convexity (\(\alpha\))**: \(f\) has **curvature at least \(\alpha\)** everywhere → it grows at least quadratically away from minimizers.
- **Smoothness (\(\beta\))**: \(f\) has **curvature at most \(\beta\)** everywhere → it is never “too curved”; gradients don’t change too fast.

If both hold, \(f\) is “sandwiched” between two quadratics.

---

## 1) Correct first-order inequalities (the ones I always use)

### (A) \(\alpha\)-strong convexity (quadratic *lower* bound)
\(f\) is \(\alpha\)-strongly convex iff for all \(x,y\),
$$
f(y)\ \ge\ f(x)+\langle \nabla f(x),y-x\rangle+\frac{\alpha}{2}\|y-x\|^2.
$$

### (B) \(\beta\)-smoothness (quadratic *upper* bound / descent lemma)
\(f\) is \(\beta\)-smooth iff for all \(x,y\),
$$
f(y)\ \le\ f(x)+\langle \nabla f(x),y-x\rangle+\frac{\beta}{2}\|y-x\|^2.
$$

**Common mistake I watch for:** the inequality directions are opposite.  
Strong convexity gives a **lower** quadratic support; smoothness gives an **upper** quadratic envelope.

---

## 2) The “sandwich” when both hold

If \(f\) is both \(\alpha\)-strongly convex and \(\beta\)-smooth, then for all \(x,y\),
$$
f(x)+\langle \nabla f(x),y-x\rangle+\frac{\alpha}{2}\|y-x\|^2
\ \le\ f(y)\ \le\
f(x)+\langle \nabla f(x),y-x\rangle+\frac{\beta}{2}\|y-x\|^2.
$$

So the same linearization sits in between two quadratics.

**No, it does NOT have to hold with equality.**  
Equality for *both* bounds for all \(x,y\) essentially forces \(f\) to be an exact quadratic (details in §8).

---

## 3) Equivalent gradient conditions (super useful)

### (A) \(\beta\)-smooth  ⇔  Lipschitz gradient
$$
\|\nabla f(x)-\nabla f(y)\|\ \le\ \beta\|x-y\|\quad\forall x,y.
$$

Also equivalent (Baillon–Haddad / cocoercivity for convex \(f\)):
$$
\langle \nabla f(x)-\nabla f(y),x-y\rangle\ \ge\ \frac{1}{\beta}\|\nabla f(x)-\nabla f(y)\|^2.
$$

### (B) \(\alpha\)-strongly convex  ⇔  strongly monotone gradient
$$
\langle \nabla f(x)-\nabla f(y),x-y\rangle\ \ge\ \alpha\|x-y\|^2\quad\forall x,y.
$$

### (C) If both hold
Combine them:
$$
\alpha\|x-y\|^2\ \le\ \langle \nabla f(x)-\nabla f(y),x-y\rangle\ \le\ \beta\|x-y\|^2.
$$

---

## 4) If \(f\) is twice differentiable: Hessian criteria (the cleanest test)

If \(f\in C^2\), then:

- \(\beta\)-smooth  ⇔  for all \(x\),
$$
\nabla^2 f(x)\ \preceq\ \beta I.
$$

- \(\alpha\)-strongly convex  ⇔  for all \(x\),
$$
\nabla^2 f(x)\ \succeq\ \alpha I.
$$

So both hold iff
$$
\alpha I\ \preceq\ \nabla^2 f(x)\ \preceq\ \beta I\quad \forall x.
$$

This is literally “eigenvalues of Hessian stay in \([\alpha,\beta]\).”

---

## 5) Bregman divergence viewpoint (my favorite clean bound)

Define the Bregman divergence:
$$
D_f(y,x)=f(y)-f(x)-\langle \nabla f(x),y-x\rangle.
$$

Then:
- \(\alpha\)-strong convexity ⇔ \(D_f(y,x)\ge \frac{\alpha}{2}\|y-x\|^2\).
- \(\beta\)-smoothness ⇔ \(D_f(y,x)\le \frac{\beta}{2}\|y-x\|^2\).

So if both hold:
$$
\frac{\alpha}{2}\|y-x\|^2\ \le\ D_f(y,x)\ \le\ \frac{\beta}{2}\|y-x\|^2.
$$

---

## 6) Consequences around the minimizer \(x^\star\)

Assume \(f\) is convex and differentiable and has minimizer \(x^\star\) with \(\nabla f(x^\star)=0\).

### (A) Quadratic growth (from strong convexity)
$$
f(x)-f(x^\star)\ \ge\ \frac{\alpha}{2}\|x-x^\star\|^2.
$$

### (B) Upper bound on suboptimality (from smoothness)
Using smoothness with \(y=x^\star\):
$$
f(x)-f(x^\star)\ \le\ \frac{\beta}{2}\|x-x^\star\|^2.
$$

### (C) Gradient norm bounds (when both hold)
From strong monotonicity + Lipschitz:
$$
\alpha\|x-x^\star\|\ \le\ \|\nabla f(x)\|\ \le\ \beta\|x-x^\star\|.
$$

Also a classic PL-type bound holds for \(\alpha\)-strongly convex + \(\beta\)-smooth:
$$
f(x)-f(x^\star)\ \le\ \frac{1}{2\alpha}\|\nabla f(x)\|^2
\quad\text{and}\quad
\|\nabla f(x)\|^2\ \ge\ 2\alpha\,(f(x)-f(x^\star)).
$$

(These are the inequalities that make GD linear.)

---

## 7) Gradient descent: step sizes + rate (why we care)

Gradient descent: \(x_{k+1}=x_k-\eta \nabla f(x_k)\).

If \(f\) is \(\beta\)-smooth and convex, choosing \(0<\eta\le 1/\beta\) gives descent.

If \(f\) is \(\alpha\)-strongly convex and \(\beta\)-smooth, then with \(\eta=1/\beta\),
$$
f(x_k)-f(x^\star)\ \le\ \left(1-\frac{\alpha}{\beta}\right)^k \big(f(x_0)-f(x^\star)\big).
$$

Condition number: \(\kappa=\beta/\alpha\).  
Smaller \(\kappa\) → faster.

---

## 8) Your question: “If both hold, does it have to be equality?”

**No.** The definition only requires the inequalities.  
In general they’re strict for many \(x\neq y\).

### When can equality happen?
- If equality holds in the **smoothness upper bound** for all \(x,y\), \(f\) must behave like a quadratic with curvature \(\beta\).
- If equality holds in the **strong convexity lower bound** for all \(x,y\), \(f\) must behave like a quadratic with curvature \(\alpha\).

For **both** to be equalities for all \(x,y\), you essentially need:
- \(\alpha=\beta\), and
- \(f\) is exactly a quadratic:
$$
f(x)=\frac{\alpha}{2}\|x\|^2+\langle b,x\rangle+c
$$
(up to the usual affine/constant terms).

So: **equality is the “pure quadratic” extreme case**, not the general case.

---

## 9) Quick examples (to sanity check)

### Example 1: Quadratic (tight / equality-type behavior)
Let \(f(x)=\frac{1}{2}x^\top Qx\) with \(Q\succeq 0\).  
If eigenvalues of \(Q\) lie in \([\alpha,\beta]\), then \(f\) is \(\alpha\)-strongly convex and \(\beta\)-smooth.  
If \(Q=\alpha I\), then \(\alpha=\beta\) and the bounds are maximally tight.

### Example 2: Strongly convex + smooth but not equality
\(f(x)=\frac{1}{2}\|x\|^2 + 0.01\sum_i \cos(x_i)\) on regions where Hessian stays bounded between two constants.  
The inequalities hold, but you don’t get equality except maybe at special points/directions.

---

## 10) Mini checklist (how I verify fast)

1) If I can compute Hessian: check \(\alpha I\preceq \nabla^2 f(x)\preceq \beta I\).  
2) If not: try gradient tests:
   - smooth: \(\|\nabla f(x)-\nabla f(y)\|\le \beta\|x-y\|\)
   - strong: \(\langle \nabla f(x)-\nabla f(y),x-y\rangle \ge \alpha\|x-y\|^2\)
3) Remember the sandwich: lower bound uses \(\alpha\), upper bound uses \(\beta\).

---

