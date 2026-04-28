# Map of Mathematics

**Keywords**: map of math, map of mathematics, mathematical map, math map, semiconductor mathematics, mathematical fields, algebra, analysis, geometry, topology

---

**Map of Mathematics**

A comprehensive overview of mathematical fields, their connections, and foundational structures.



**1. Foundations of Mathematics**

At the deepest level, mathematics rests on questions about its own nature and structure.

**1.1 Logic**

- **Propositional Logic**: Studies logical connectives $\land$ (and), $\lor$ (or), $
eg$ (not), $\rightarrow$ (implies)
- **Predicate Logic**: Introduces quantifiers $\forall$ (for all) and $\exists$ (there exists)
- **Key Result**: Gödel's Incompleteness Theorems
  - First: Any consistent formal system $F$ capable of expressing arithmetic contains statements that are true but unprovable in $F$
  - Second: Such a system cannot prove its own consistency

**1.2 Set Theory**

- **Zermelo-Fraenkel Axioms with Choice (ZFC)**: The standard foundation
- **Key Concepts**:
  - Empty set: $\emptyset$
  - Union: $A \cup B = \{x : x \in A \text{ or } x \in B\}$
  - Intersection: $A \cap B = \{x : x \in A \text{ and } x \in B\}$
  - Power set: $\mathcal{P}(A) = \{B : B \subseteq A\}$
  - Cardinality: $|A|$, with $|\mathbb{N}| = \aleph_0$ (countable infinity)
- **Continuum Hypothesis**: Is there a set with cardinality strictly between $|\mathbb{N}|$ and $|\mathbb{R}|$?

**1.3 Category Theory**

- **Objects and Morphisms**: Abstract structures and structure-preserving maps
- **Key Concepts**:
  - Functors: $F: \mathcal{C} \to \mathcal{D}$ (maps between categories)
  - Natural transformations: $\eta: F \Rightarrow G$
  - Universal properties and limits
- **Philosophy**: "It's all about the arrows" — relationships matter more than objects

**1.4 Type Theory**

- **Dependent Types**: Types that depend on values
- **Curry-Howard Correspondence**: 
  $$\text{Propositions} \cong \text{Types}, \quad \text{Proofs} \cong \text{Programs}$$
- **Applications**: Proof assistants (Coq, Lean, Agda)



**2. Algebra**

The study of structure, operations, and their properties.

**2.1 Linear Algebra**

- **Vector Spaces**: A set $V$ over field $F$ with addition and scalar multiplication
- **Key Structures**:
  - Linear transformation: $T: V \to W$ where $T(\alpha u + \beta v) = \alpha T(u) + \beta T(v)$
  - Matrix representation: $[T]_{\mathcal{B}}$
  - Eigenvalue equation: $Av = \lambda v$
- **Fundamental Theorem**: Every matrix $A$ has a Jordan normal form
- **Singular Value Decomposition**:
  $$A = U \Sigma V^*$$

**2.2 Group Theory**

- **Definition**: A group $(G, \cdot)$ satisfies:
  - Closure: $a, b \in G \Rightarrow a \cdot b \in G$
  - Associativity: $(a \cdot b) \cdot c = a \cdot (b \cdot c)$
  - Identity: $\exists e \in G$ such that $e \cdot a = a \cdot e = a$
  - Inverses: $\forall a \in G, \exists a^{-1}$ such that $a \cdot a^{-1} = e$
- **Key Examples**:
  - Symmetric group $S_n$ (all permutations of $n$ elements)
  - Cyclic group $\mathbb{Z}/n\mathbb{Z}$
  - General linear group $GL_n(\mathbb{R})$ (invertible $n \times n$ matrices)
- **Lagrange's Theorem**: If $H \leq G$, then $|H|$ divides $|G|$
- **Classification of Finite Simple Groups**: Completed in 2004 (~10,000 pages)

**2.3 Ring Theory**

- **Definition**: A ring $(R, +, \cdot)$ has:
  - $(R, +)$ is an abelian group
  - Multiplication is associative
  - Distributivity: $a(b + c) = ab + ac$
- **Key Examples**:
  - Integers $\mathbb{Z}$
  - Polynomials $R[x]$
  - Matrices $M_n(R)$
- **Ideals**: $I \subseteq R$ is an ideal if $RI \subseteq I$ and $IR \subseteq I$
- **Quotient Rings**: $R/I$

**2.4 Field Theory**

- **Definition**: A field is a commutative ring where every nonzero element has a multiplicative inverse
- **Examples**: $\mathbb{Q}$, $\mathbb{R}$, $\mathbb{C}$, $\mathbb{F}_p$ (finite fields)
- **Field Extensions**: $L/K$ where $K \subseteq L$
- **Galois Theory**: Studies field extensions via their automorphism groups
  - **Fundamental Theorem**: There is a correspondence between intermediate fields of $L/K$ and subgroups of $\text{Gal}(L/K)$

**2.5 Representation Theory**

- **Definition**: A representation of group $G$ is a homomorphism $\rho: G \to GL(V)$
- **Characters**: $\chi_\rho(g) = \text{Tr}(\rho(g))$
- **Key Result**: Characters of irreducible representations form an orthonormal basis
  $$\langle \chi_\rho, \chi_\sigma \rangle = \frac{1}{|G|} \sum_{g \in G} \chi_\rho(g) \overline{\chi_\sigma(g)} = \delta_{\rho\sigma}$$



**3. Analysis**

The rigorous study of continuous change, limits, and infinity.

**3.1 Real Analysis**

- **Limits**: $\lim_{x \to a} f(x) = L$ iff $\forall \varepsilon > 0, \exists \delta > 0$ such that $0 < |x - a| < \delta \Rightarrow |f(x) - L| < \varepsilon$
- **Continuity**: $f$ is continuous at $a$ if $\lim_{x \to a} f(x) = f(a)$
- **Differentiation**:
  $$f'(x) = \lim_{h \to 0} \frac{f(x+h) - f(x)}{h}$$
- **Integration** (Riemann):
  $$\int_a^b f(x) \, dx = \lim_{n \to \infty} \sum_{i=1}^n f(x_i^*) \Delta x_i$$
- **Fundamental Theorem of Calculus**:
  $$\frac{d}{dx} \int_a^x f(t) \, dt = f(x)$$

**3.2 Measure Theory**

- **$\sigma$-Algebra**: Collection of sets closed under complements and countable unions
- **Measure**: $\mu: \Sigma \to [0, \infty]$ with:
  - $\mu(\emptyset) = 0$
  - Countable additivity: $\mu\left(\bigcup_{i=1}^\infty A_i\right) = \sum_{i=1}^\infty \mu(A_i)$ for disjoint $A_i$
- **Lebesgue Integral**:
  $$\int f \, d\mu = \sup \left\{ \int \phi \, d\mu : \phi \leq f, \phi \text{ simple} \right\}$$

**3.3 Complex Analysis**

- **Holomorphic Functions**: $f: \mathbb{C} \to \mathbb{C}$ is holomorphic if $f'(z)$ exists
- **Cauchy-Riemann Equations**: If $f = u + iv$, then
  $$\frac{\partial u}{\partial x} = \frac{\partial v}{\partial y}, \quad \frac{\partial u}{\partial y} = -\frac{\partial v}{\partial x}$$
- **Cauchy's Integral Formula**:
  $$f(z_0) = \frac{1}{2\pi i} \oint_\gamma \frac{f(z)}{z - z_0} \, dz$$
- **Residue Theorem**:
  $$\oint_\gamma f(z) \, dz = 2\pi i \sum_{k} \text{Res}(f, z_k)$$

**3.4 Functional Analysis**

- **Banach Spaces**: Complete normed vector spaces
- **Hilbert Spaces**: Complete inner product spaces
  - Inner product: $\langle \cdot, \cdot \rangle: V \times V \to \mathbb{C}$
  - Norm: $\|v\| = \sqrt{\langle v, v \rangle}$
- **Key Theorems**:
  - Hahn-Banach (extension of linear functionals)
  - Open Mapping Theorem
  - Closed Graph Theorem
  - Spectral Theorem: Normal operators on Hilbert spaces have spectral decompositions

**3.5 Differential Equations**

- **Ordinary Differential Equations (ODEs)**:
  - First order: $\frac{dy}{dx} = f(x, y)$
  - Linear: $y^{(n)} + a_{n-1}y^{(n-1)} + \cdots + a_0 y = g(x)$
- **Partial Differential Equations (PDEs)**:
  - Heat equation: $\frac{\partial u}{\partial t} = \alpha 
abla^2 u$
  - Wave equation: $\frac{\partial^2 u}{\partial t^2} = c^2 
abla^2 u$
  - Laplace equation: $
abla^2 u = 0$
  - Schrödinger equation: $i\hbar \frac{\partial \psi}{\partial t} = \hat{H}\psi$



**4. Geometry and Topology**

The study of space, shape, and structure.

**4.1 Euclidean Geometry**

- **Euclid's Postulates**: Five axioms defining flat space
- **Key Results**:
  - Pythagorean theorem: $a^2 + b^2 = c^2$
  - Sum of angles in triangle: $180°$
  - Parallel postulate: Given a line and a point not on it, exactly one parallel exists

**4.2 Non-Euclidean Geometries**

- **Hyperbolic Geometry** (negative curvature):
  - Multiple parallels through a point
  - Sum of angles in triangle: $< 180°$
  - Model: Poincaré disk with metric $ds^2 = \frac{4(dx^2 + dy^2)}{(1 - x^2 - y^2)^2}$
- **Elliptic/Spherical Geometry** (positive curvature):
  - No parallels
  - Sum of angles in triangle: $> 180°$

**4.3 Differential Geometry**

- **Manifolds**: Spaces locally homeomorphic to $\mathbb{R}^n$
- **Tangent Spaces**: $T_p M$ at each point $p$
- **Riemannian Metric**: $g_{ij}$ defining distances and angles
  $$ds^2 = g_{ij} \, dx^i \, dx^j$$
- **Curvature**:
  - Gaussian curvature: $K = \kappa_1 \kappa_2$ (product of principal curvatures)
  - Riemann curvature tensor: $R^i_{\ jkl}$
  - Ricci curvature: $R_{ij} = R^k_{\ ikj}$
  - Scalar curvature: $R = g^{ij} R_{ij}$
- **Gauss-Bonnet Theorem**:
  $$\int_M K \, dA = 2\pi \chi(M)$$
  where $\chi(M)$ is the Euler characteristic

**4.4 Topology**

- **Topological Space**: $(X, \tau)$ where $\tau$ is a collection of "open sets"
- **Homeomorphism**: Continuous bijection with continuous inverse
- **Key Invariants**:
  - Connectedness
  - Compactness
  - Euler characteristic: $\chi = V - E + F$

**4.5 Algebraic Topology**

- **Fundamental Group**: $\pi_1(X, x_0)$ — loops up to homotopy
  - $\pi_1(S^1) = \mathbb{Z}$
  - $\pi_1(\mathbb{R}^n) = 0$
- **Higher Homotopy Groups**: $\pi_n(X)$
- **Homology Groups**: $H_n(X)$ — "holes" in dimension $n$
  - $H_0$ counts connected components
  - $H_1$ counts 1-dimensional holes (loops)
  - $H_2$ counts 2-dimensional holes (voids)
- **Cohomology**: Dual theory with cup product structure

**4.6 Algebraic Geometry**

- **Affine Variety**: Zero set of polynomials
  $$V(f_1, \ldots, f_k) = \{x \in k^n : f_i(x) = 0 \text{ for all } i\}$$
- **Projective Variety**: Variety in projective space $\mathbb{P}^n$
- **Schemes**: Generalization using commutative algebra
- **Sheaves**: Local-to-global data structures
- **Key Results**:
  - Bézout's Theorem: Degree $m$ and $n$ curves intersect in $mn$ points (counting multiplicities)
  - Riemann-Roch Theorem (for curves):
    $$\ell(D) - \ell(K - D) = \deg(D) - g + 1$$



**5. Number Theory**

The study of integers and their generalizations.

**5.1 Elementary Number Theory**

- **Divisibility**: $a | b$ iff $\exists k$ such that $b = ka$
- **Prime Numbers**: $p > 1$ with only divisors $1$ and $p$
- **Fundamental Theorem of Arithmetic**: Every integer $> 1$ factors uniquely into primes
  $$n = p_1^{a_1} p_2^{a_2} \cdots p_k^{a_k}$$
- **Modular Arithmetic**: $a \equiv b \pmod{n}$ iff $n | (a - b)$
- **Euler's Theorem**: If $\gcd(a, n) = 1$, then $a^{\phi(n)} \equiv 1 \pmod{n}$
- **Fermat's Little Theorem**: If $p$ is prime and $p 
mid a$, then $a^{p-1} \equiv 1 \pmod{p}$

**5.2 Analytic Number Theory**

- **Prime Number Theorem**:
  $$\pi(x) \sim \frac{x}{\ln x}$$
  where $\pi(x)$ counts primes $\leq x$
- **Riemann Zeta Function**:
  $$\zeta(s) = \sum_{n=1}^{\infty} \frac{1}{n^s} = \prod_p \frac{1}{1 - p^{-s}}$$
- **Riemann Hypothesis**: All non-trivial zeros of $\zeta(s)$ have real part $\frac{1}{2}$
- **Dirichlet L-Functions**: Generalization for arithmetic progressions

**5.3 Algebraic Number Theory**

- **Number Fields**: Finite extensions of $\mathbb{Q}$
- **Ring of Integers**: $\mathcal{O}_K$ — algebraic integers in $K$
- **Unique Factorization Failure**: $\mathcal{O}_K$ may not be a UFD
  - Example: In $\mathbb{Z}[\sqrt{-5}]$: $6 = 2 \cdot 3 = (1 + \sqrt{-5})(1 - \sqrt{-5})$
- **Ideal Class Group**: Measures failure of unique factorization
- **Class Number Formula**:
  $$h_K = \frac{w_K \sqrt{|d_K|}}{2^{r_1}(2\pi)^{r_2} R_K} \cdot \lim_{s \to 1} (s-1) \zeta_K(s)$$

**5.4 Famous Conjectures and Theorems**

- **Fermat's Last Theorem** (proved by Wiles, 1995):
  $$x^n + y^n = z^n \text{ has no positive integer solutions for } n > 2$$
- **Goldbach's Conjecture** (open): Every even integer $> 2$ is the sum of two primes
- **Twin Prime Conjecture** (open): Infinitely many primes $p$ where $p + 2$ is also prime
- **ABC Conjecture**: For coprime $a + b = c$, $\text{rad}(abc)^{1+\varepsilon} > c$ for almost all triples



**6. Combinatorics**

The study of discrete structures and counting.

**6.1 Enumerative Combinatorics**

- **Counting Principles**:
  - Permutations: $P(n, k) = \frac{n!}{(n-k)!}$
  - Combinations: $\binom{n}{k} = \frac{n!}{k!(n-k)!}$
- **Binomial Theorem**:
  $$(x + y)^n = \sum_{k=0}^{n} \binom{n}{k} x^{n-k} y^k$$
- **Generating Functions**:
  - Ordinary: $F(x) = \sum_{n=0}^{\infty} a_n x^n$
  - Exponential: $F(x) = \sum_{n=0}^{\infty} a_n \frac{x^n}{n!}$

**6.2 Graph Theory**

- **Definitions**:
  - Graph $G = (V, E)$: vertices and edges
  - Degree: $\deg(v) = |\{e \in E : v \in e\}|$
- **Handshaking Lemma**: $\sum_{v \in V} \deg(v) = 2|E|$
- **Euler's Formula** (planar graphs): $V - E + F = 2$
- **Key Problems**:
  - Graph coloring: $\chi(G)$ = chromatic number
  - Four Color Theorem: Every planar graph is 4-colorable
  - Hamiltonian cycles

**6.3 Ramsey Theory**

- **Principle**: "Complete disorder is impossible"
- **Ramsey Numbers**: $R(m, n)$ = minimum $N$ such that any 2-coloring of $K_N$ contains monochromatic $K_m$ or $K_n$
  - $R(3, 3) = 6$
  - $R(4, 4) = 18$
  - $43 \leq R(5, 5) \leq 48$ (exact value unknown)



**7. Probability and Statistics**

**7.1 Probability Theory**

- **Kolmogorov Axioms**:
  1. $P(A) \geq 0$
  2. $P(\Omega) = 1$
  3. Countable additivity: $P\left(\bigcup_{i} A_i\right) = \sum_{i} P(A_i)$ for disjoint $A_i$
- **Conditional Probability**: $P(A|B) = \frac{P(A \cap B)}{P(B)}$
- **Bayes' Theorem**:
  $$P(A|B) = \frac{P(B|A) P(A)}{P(B)}$$
- **Expectation**: $E[X] = \int x \, dF(x)$
- **Variance**: $\text{Var}(X) = E[(X - E[X])^2] = E[X^2] - (E[X])^2$

**7.2 Key Distributions**

| Distribution | PMF/PDF | Mean | Variance |
|-------------|---------|------|----------|
| Binomial | $\binom{n}{k} p^k (1-p)^{n-k}$ | $np$ | $np(1-p)$ |
| Poisson | $\frac{\lambda^k e^{-\lambda}}{k!}$ | $\lambda$ | $\lambda$ |
| Normal | $\frac{1}{\sigma\sqrt{2\pi}} e^{-\frac{(x-\mu)^2}{2\sigma^2}}$ | $\mu$ | $\sigma^2$ |
| Exponential | $\lambda e^{-\lambda x}$ | $\frac{1}{\lambda}$ | $\frac{1}{\lambda^2}$ |

**7.3 Limit Theorems**

- **Law of Large Numbers**:
  $$\bar{X}_n = \frac{1}{n} \sum_{i=1}^n X_i \xrightarrow{p} \mu$$
- **Central Limit Theorem**:
  $$\frac{\bar{X}_n - \mu}{\sigma / \sqrt{n}} \xrightarrow{d} N(0, 1)$$



**8. Applied Mathematics**

**8.1 Numerical Analysis**

- **Root Finding**: Newton's method: $x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}$
- **Interpolation**: Lagrange, splines
- **Numerical Integration**: Simpson's rule, Gaussian quadrature
- **Linear Systems**: LU decomposition, iterative methods

**8.2 Optimization**

- **Unconstrained**: Find $\min_x f(x)$
  - Gradient descent: $x_{k+1} = x_k - \alpha 
abla f(x_k)$
- **Constrained**: Lagrange multipliers
  $$
abla f = \lambda 
abla g \quad \text{at optimum}$$
- **Linear Programming**: Simplex method, interior point methods
- **Convex Optimization**: Global optimum = local optimum

**8.3 Mathematical Physics**

- **Classical Mechanics**: Lagrangian $L = T - V$, Euler-Lagrange equations
  $$\frac{d}{dt} \frac{\partial L}{\partial \dot{q}} - \frac{\partial L}{\partial q} = 0$$
- **Electromagnetism**: Maxwell's equations
- **General Relativity**: Einstein field equations
  $$R_{\mu
u} - \frac{1}{2} R g_{\mu
u} + \Lambda g_{\mu
u} = \frac{8\pi G}{c^4} T_{\mu
u}$$
- **Quantum Mechanics**: Schrödinger equation, Hilbert space formalism



**9. The Grand Connections**

**9.1 Langlands Program**

A web of conjectures connecting:

- Number theory (Galois representations)
- Representation theory (automorphic forms)
- Algebraic geometry
- Harmonic analysis

**Central idea**: $L$-functions from different sources are the same:
$$L(s, \rho) = L(s, \pi)$$
where $\rho$ is a Galois representation and $\pi$ is an automorphic representation.

**9.2 Mirror Symmetry**

- **Physics Origin**: String theory on Calabi-Yau manifolds
- **Mathematical Content**: Pairs $(X, \check{X})$ where:
  - Complex geometry of $X$ $\leftrightarrow$ Symplectic geometry of $\check{X}$
  - $h^{1,1}(X) = h^{2,1}(\check{X})$

**9.3 Topological Quantum Field Theory**

- **Axioms** (Atiyah): Functor from cobordism category to vector spaces
- **Examples**: Chern-Simons theory, topological string theory
- **Connections**: Knot invariants, 3-manifold invariants, quantum groups



**10. Summary Diagram**

**Interactive Visual Map of Mathematics**

An interactive diagram showing the hierarchical relationships between mathematical fields is available at:





The ASCII diagram below is retained for reference:

```
-
                    ┌─────────────────────────────────────────┐
                    │           FOUNDATIONS                   │
                    │   Logic ─ Set Theory ─ Category Theory  │
                    └─────────────────┬───────────────────────┘
                                      │
         ┌────────────────────────────┼────────────────────────────┐
         │                            │                            │
         ▼                            ▼                            ▼
    ┌─────────┐                 ┌──────────┐                 ┌──────────┐
    │ ALGEBRA │◄───────────────►│ ANALYSIS │◄───────────────►│ GEOMETRY │
    │         │                 │          │                 │ TOPOLOGY │
    └────┬────┘                 └────┬─────┘                 └────┬─────┘
         │                           │                            │
         │         ┌─────────────────┼─────────────────┐          │
         │         │                 │                 │          │
         ▼         ▼                 ▼                 ▼          ▼
    ┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
    │  NUMBER THEORY  │    │  COMBINATORICS   │    │   PROBABILITY   │
    │                 │    │  & GRAPH THEORY  │    │   & STATISTICS  │
    └────────┬────────┘    └────────┬─────────┘    └────────┬────────┘
             │                      │                       │
             └──────────────────────┼───────────────────────┘
                                    │
                                    ▼
                    ┌───────────────────────────────┐
                    │     APPLIED MATHEMATICS       │
                    │  Physics ─ Computing ─ Data   │
                    └───────────────────────────────┘
```

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/map-of-math) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
