
# Convex Sets + Mappings + Cones + Perspective — Cheat Sheet

This sheet collects a few “convexity-preserving operations” that show up everywhere in convex optimization:
- projections
- affine images + preimages
- perspective / linear-fractional ideas
- proper cones + generalized inequalities
- minimum vs minimal elements
- distance-to-set regions (and why they can fail convexity)

---

## 1) Projection of a convex set is convex

Let \(S \subseteq \mathbb{R}^m \times \mathbb{R}^n\) be convex. Define the projection onto the first block:
\[
T=\{x_1\in\mathbb{R}^m\mid \exists x_2\in\mathbb{R}^n \text{ s.t. } (x_1,x_2)\in S\}.
\]

**Claim.** \(T\) is convex.

**Proof (one-liner style).**  
Take \(u_1,v_1\in T\). Then \(\exists u_2,v_2\) with \((u_1,u_2),(v_1,v_2)\in S\).  
For \(\theta\in[0,1]\),
\[
(\theta u_1+(1-\theta)v_1,\ \theta u_2+(1-\theta)v_2)
= \theta(u_1,u_2)+(1-\theta)(v_1,v_2)\in S
\]
(by convexity of \(S\)). Hence the first coordinate \(\theta u_1+(1-\theta)v_1\in T\). ✅

**Mental picture.** Projection “drops coordinates.” Convexity survives because you can convex-combine witnesses.

---

## 2) Affine maps: image + inverse image preserve convexity

### 2.1 Affine function
A function \(f:\mathbb{R}^n\to\mathbb{R}^m\) is affine if
\[
f(x)=Ax+b.
\]

### 2.2 Image of a convex set under affine map
Let \(S\subseteq\mathbb{R}^n\) be convex. Define
\[
f(S)=\{f(x)\mid x\in S\}.
\]

**Claim.** \(f(S)\) is convex.

**Reason.** If \(y_1=f(x_1)\), \(y_2=f(x_2)\), then for \(\theta\in[0,1]\),
\[
\theta y_1+(1-\theta)y_2
= \theta f(x_1)+(1-\theta)f(x_2)
= f(\theta x_1+(1-\theta)x_2)\in f(S)
\]
since \(\theta x_1+(1-\theta)x_2\in S\).

### 2.3 Inverse image (preimage) of a convex set under affine map
Let \(C\subseteq\mathbb{R}^m\) be convex. Define
\[
f^{-1}(C)=\{x\in\mathbb{R}^n\mid f(x)\in C\}.
\]

**Claim.** \(f^{-1}(C)\) is convex.

**Reason.** If \(x_1,x_2\in f^{-1}(C)\) then \(f(x_1),f(x_2)\in C\). For \(\theta\in[0,1]\),
\[
f(\theta x_1+(1-\theta)x_2)=\theta f(x_1)+(1-\theta)f(x_2)\in C,
\]
so \(\theta x_1+(1-\theta)x_2\in f^{-1}(C)\).

**Big takeaway.**
- affine image preserves convexity
- affine preimage preserves convexity
- projection is a special case of affine image (drop coordinates)

---

## 3) Inverse function vs inverse image (preimage) — don’t mix

Let \(f:X\to Y\).

**Inverse function** \(f^{-1}:Y\to X\) exists only if \(f\) is bijective (on the sets considered):
\[
f^{-1}(y)=x \iff f(x)=y.
\]

**Inverse image / preimage** works for any \(f\):
\[
f^{-1}(B)=\{x\in X \mid f(x)\in B\},\quad B\subseteq Y.
\]

Same symbol \(f^{-1}\), different meaning:
- \(f^{-1}(y)\) (point input) = inverse function (if it exists)
- \(f^{-1}(B)\) (set input) = preimage (always exists)

---

## 4) Perspective function (and why it preserves convexity)

Define \(P:\mathbb{R}^{n+1}\to\mathbb{R}^n\) by
\[
P(z,t)=\frac{z}{t},\qquad \text{dom }P=\mathbb{R}^n\times \mathbb{R}_{++}.
\]

### 4.1 Line segments map to line segments (reparameterized)
Take \(x=(\tilde x,x_{n+1})\), \(y=(\tilde y,y_{n+1})\) with \(x_{n+1},y_{n+1}>0\). For \(\theta\in[0,1]\),
\[
P(\theta x+(1-\theta)y)
=\frac{\theta \tilde x+(1-\theta)\tilde y}{\theta x_{n+1}+(1-\theta)y_{n+1}}.
\]

This can be rewritten as
\[
P(\theta x+(1-\theta)y)=\mu(\theta)\,P(x)+(1-\mu(\theta))\,P(y),
\]
where
\[
\mu(\theta)=\frac{\theta x_{n+1}}{\theta x_{n+1}+(1-\theta)y_{n+1}}\in[0,1].
\]

**What matters.** As \(\theta\) sweeps \(0\to1\), \(\mu(\theta)\) sweeps \(0\to1\) (monotone + continuous), so:
\[
P([x,y])=[P(x),P(y)].
\]

### 4.2 Convexity of \(P(C)\)
If \(C\subseteq \text{dom }P\) is convex, then for any \(x,y\in C\),
\([x,y]\subseteq C\). Apply the segment mapping:
\[
[P(x),P(y)] = P([x,y]) \subseteq P(C).
\]
So \(P(C)\) contains the whole segment between any two of its points ⇒ \(P(C)\) is convex.

### 4.3 Convexity of the preimage under perspective
If \(D\subseteq \mathbb{R}^n\) is convex, then
\[
P^{-1}(D)=\{(z,t)\in \mathbb{R}^{n+1}\mid z/t\in D,\ t>0\}
\]
is convex.

**Quick check.** If \((z_1,t_1),(z_2,t_2)\in P^{-1}(D)\), then \(z_1/t_1, z_2/t_2\in D\). For \(\theta\in[0,1]\),
\[
\frac{\theta z_1+(1-\theta)z_2}{\theta t_1+(1-\theta)t_2}
= \mu\frac{z_1}{t_1}+(1-\mu)\frac{z_2}{t_2}\in D
\]
for a suitable \(\mu\in[0,1]\) (same trick). Also \(\theta t_1+(1-\theta)t_2>0\). Hence the convex combo stays in \(P^{-1}(D)\).

**Pin-hole camera intuition.** Divide by depth \(t\) = “project to image plane.” Convex objects stay convex after projection.

---

## 5) Proper cones + generalized inequalities

A cone \(K\subseteq\mathbb{R}^n\) is **proper** if:
- \(K\) is convex
- \(K\) is closed
- \(K\) is solid: \(\text{int }K\neq \emptyset\)
- \(K\) is pointed: \(K\cap(-K)=\{0\}\) (contains no line)

### 5.1 Generalized inequality induced by \(K\)
Define
\[
x \preceq_K y \iff y-x\in K,
\qquad
x \prec_K y \iff y-x\in \text{int }K.
\]

**What \(\text{int }K\) means.** Interior of \(K\):
\[
\text{int }K=\{u\in K\mid \exists \epsilon>0: B(u,\epsilon)\subseteq K\}.
\]
So strict inequality means “difference is strictly inside the cone.”

### 5.2 Standard examples
- \(K=\mathbb{R}^n_+\): \(x\preceq_K y \iff x_i\le y_i\ \forall i\).  
  \(\text{int }K=\{u: u_i>0\ \forall i\}\) gives strict componentwise inequality.
- \(K=S^n_+\) (PSD cone): \(X\preceq_K Y \iff Y-X\succeq 0\).  
  \(\text{int }K\) = PD matrices.

---

## 6) Minimum vs minimal element (w.r.t. \(\preceq_K\))

Let \(S\subseteq\mathbb{R}^n\).

### 6.1 Minimum element
\(x\in S\) is a **minimum element** if
\[
\forall y\in S,\quad x\preceq_K y.
\]
If a minimum exists, it is **unique**.

**Set form.** \(x\) is minimum iff
\[
S \subseteq x+K.
\]
Because \(y\in x+K \iff y-x\in K \iff x\preceq_K y\).

### 6.2 Minimal element
\(x\in S\) is **minimal** if there is no strictly smaller different point:
\[
y\in S,\ y\preceq_K x \ \Rightarrow\ y=x.
\]
A set can have many minimal elements.

**Set form.**
\[
(x-K)\cap S=\{x\}.
\]
Because \(y\in x-K \iff x-y\in K \iff y\preceq_K x\).

**How to remember.**
- minimum = “beats everyone”
- minimal = “nobody beats it (except itself)”
- minimum ⇒ minimal, but not conversely

---

## 7) Distance to a set + “closer to S than T” region

Distance-to-set:
\[
\text{dist}(x,S)=\inf_{z\in S}\|x-z\|_2.
\]
(Inf is used because a closest point may not exist if \(S\) is not closed.)

Region:
\[
\Omega=\{x\mid \text{dist}(x,S)\le \text{dist}(x,T)\}.
\]

### 7.1 Important fact: \(\Omega\) is NOT convex in general
Even if \(S\) is convex and \(T\) is a point, \(\Omega\) can be nonconvex.

**Why the “halfspace intersection” trick fails.**
- For a fixed point \(s\) and \(t\), the set \(\{x:\|x-s\|\le \|x-t\|\}\) is a halfspace (convex).
- But \(\text{dist}(x,S)\le \|x-t\|\) means:
  \[
  \inf_{s\in S}\|x-s\|\le \|x-t\|
  \]
  which behaves like “there exists an \(s\in S\) that is close enough” ⇒ **union**, not intersection.

If \(S=\{s_1,\dots,s_k\}\) finite:
\[
\min_i \|x-s_i\|\le \|x-t\|
\iff
\bigcup_{i=1}^k \{x:\|x-s_i\|\le \|x-t\|\}.
\]
Union of convex sets need not be convex.

### 7.2 Special case when it IS convex
If \(S\) is a **single point** \(S=\{s\}\), then:
\[
\|x-s\|\le \text{dist}(x,T)
\iff
\forall t\in T:\ \|x-s\|\le \|x-t\|,
\]
which is an intersection of halfspaces ⇒ convex.

**Useful algebra (point vs point).**
\[
\|x-s\|^2\le \|x-t\|^2
\iff
2(t-s)^\top x \le \|t\|^2-\|s\|^2.
\]
So it’s literally a halfspace boundary (a perpendicular bisector hyperplane).

---

## 8) Quick “convexity-preserving” checklist

- Projection of convex set → convex ✅
- Affine image of convex set → convex ✅
- Affine preimage of convex set → convex ✅
- Perspective image of convex set (within dom) → convex ✅
- Cone-induced order: use \(K\) + \(\text{int }K\) for strict ✅
- Minimum vs minimal: minimum unique, minimal possibly many ✅
- “Closer to set \(S\) than set \(T\)” region → not convex in general ❌ (unless special structure like singleton)

---

