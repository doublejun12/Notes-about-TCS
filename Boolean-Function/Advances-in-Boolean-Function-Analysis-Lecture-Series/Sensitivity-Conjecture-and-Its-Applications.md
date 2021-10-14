## Sensitivity and block sensitivity

**Definition:** Given a boolean function $f:\{0,1\}^n \rightarrow \{0,1\}$. The local sensitivity $s(f,x)$ on the input   $x$ is defined as the number of indices $i$, such that $f(x) \neq f(x^{\{i\}})$, where $x^{\{i\}}$ is obtained by flipping the $i$-th bit of $x$. The sensitivity $s(f):= \max_{x\in\{0,1\}^n}s(f,x)$.

**Example:** the $AND$ function $f(x_1,...,x_n) = x_1 \land \cdots \land x_n$. $s(f, \vec{0}) = 0, s(f, \vec{1}) = n, s(f)=n$.

**Definition:** The local block sensitivity $bs(f,x)$ on the input $x$ is the maximum number of disjoint blocks $B_1,...,B_k$ of $[n]$, such that for each $B_i$, $f(x) \neq f(x^{B_i})$. Here $x^{B_i}$ is the $n$-bit string obtained from $x$ by flipping its coordinates in $B_i$. The block sensitivity $bs(f):=\max_{x \in \{0,1\}^n} bs(f,x)$.

**Example:** the $AND$ function $f(x_1,...,x_n) = x_1 \land \cdots \land x_n$. $bs(f, \vec{0}) = 1, bs(f, \vec{1}) = n, s(f)=n$.

Obviously, $bs(f,x) \geq s(f,x)$ for every $x$ and thus $bs(f) \geq s(f)$. But does there exist a function $f$, such that $bs(f) > s(f)$?

## The Rubinstein function

Define $f: \{0,1\}^{n^2} \rightarrow \{0,1\}$ as
$$
f(x_{11}, \cdots, x_{nn}) = \bigvee_{i=1}^{n} g(x_{i1}, \cdots, x_{in}),
$$
where $g(x_1, \cdots, x_n) = 1$ if and only if $x_j = x_{j+1} = 1$ for some $1 \leq j \leq n-1$, and all other $x_k=0$.

- $bs(f) \geq bs(f, \vec{0}) = \Omega(n^2)$.

- $s(f) = O(n)$.

  - **Case 1:** $f(x)=0$

    Every row must output 0, there are at most two sensitive coordinates on each row, say when the row is
    $$
    0 \quad \cdots \quad 0 \quad 1 \quad 0 \quad \cdots \quad 0.
    $$
    So, $s(f,x) \leq 2n$.

  - **Case 2:** $f(x)=1$

    If two rows output 1, $s(f,x)=0$.

    If only one row outputs 1, $s(f,x) \leq n$.

A quick summary: $s(f) \leq bs(f)$, and Rubinstein's example shows that $bs(f)$ could be quadratic in $s(f)$. 

**Sensitivity Conjecture[Nisan Szegedy 1992]**

For every boolean function $f$, $bs(f) \leq poly(s(f))$.

Two complexity measures $s_1$ and $s_2$ of boolean functions are polynomially related if $\exists C_1, C_2 > 0$, such that for every boolean $f$:
$$
s_2(f)^{C_1} \leq s_1(f) \leq s_2(f)^{C_2}.
$$
The following measures are polynomially related:

- Block sensitivity $bs(f)$.
- Decision tree complexity $D(f)$.
- Certificate complexity $C(f)$.
- Degree (as real polynomial) $deg(f)$.
- Approximate degree $\tilde{deg}(f)$.
- Randomized query complexity $R(f)$.
- Quantum query complexity $Q(f)$.

In some sense, sensitivity measures how "smooth" a boolean function is, with respect to the Hamming distance. Low sensitivity means more smooth. The Sensitivity Conjecture asserts that

- Computationally, "smooth" (low-sensitivity) functions are easy to compute in some of the simplest models like the deterministic decision tree model.
- Algebraically, such functions have low degree.

**Bounds proven: (Exponential)**

- $bs(f) = O(s(f)4^{s(f)})$. (Simon 1983)
- $bs(f) \leq (e/\sqrt{2\pi})e^{s(f)} \sqrt{s(f)}$. (Kenyon, Kutin 2004)
- $bs(f) \leq 2^{s(f)-1} s(f)$ (Ambainis, Bavarian, Gao, Mao, Sun, Zuo 2013)

**Separations constructed: (Quadratic)**

- $bs(f) = \frac{1}{2} s(f)^2$. (Rubinstein 1995)
- $bs(f) = \frac{1}{2} s(f)^2 + s(f)$. (Virza 2011)
- $bs(f)=\frac{2}{3}s(f)^2-\frac{1}{2}s(f)$. (Ambainis, Sun 2011)

## The Gotsman-Linial equivalence

**Theorem (Gotsman, Linial 1992)**

The following are equivalent for any monotone function $h:\mathbb{N} \rightarrow \mathbb{R}$.

- For any induced subgraph $H$ of $Q^n$ with $|V(H)| \neq 2^{n-1}$, we have 

$$
\max \{\Delta(H), \Delta(Q^n-H)\} \geq h(n).
$$

​		where $\Delta$ is the maximum degree of a graph.

- For any boolean function $f$, we have $s(f) \geq h(deg(f))$.

Showing (i) for $h(n) = n^c$ if and only if Sensitivity conjecture.

The Gostman-Linial correspondence was established via an intermediate statement:

- For any boolean function $g$ of full degree $n$, $s(g) \geq h(n)$.

The direction we care about is $(i) \Rightarrow (iii) \Rightarrow (ii)$.

**Proof of $(i) \Rightarrow (iii)$:**

- Suppose there exists $g:\{0,1\}^n \rightarrow \{-1,1\}$, with $s(g)<h(n)$, $deg(g)=n$. Let $p(x)=(-1)^{x_1+ \cdots+ x_n}$.

- Consider the induced subgraph $H$ with vertex set
  $$
  V(H)=\{x:g(x) \cdot p(x) = 1\}.
  $$

- Obviously $s(g)=\max \{\Delta(H), \Delta(Q^n-H)\}$, and 

$$
(|V(H)|-|V(Q^n-H)|)/2^n=E[g(x)p(x)]=\hat{gp}(\phi)=\hat{g}([n]) \neq 0.
$$

- The last inequality follows from $deg(g)=n$.

**Proof of** $(iii) \Rightarrow (ii)$:

## Upper and lower bounds

**Theorem (Chung, Furedi, Graham, Seymour 1988)**:

1. $Q^n$ has a $(2^{n-1}+1)$-vertex induced subgraph of maximum degree $\lceil \sqrt{n} \rceil$. (quadratic separation)
2. Every $(2^{n-1}+1)$-vertex induced subgraph of $Q^n$ has maximum degree at least $(1/2-o(1)) \cdot \log_2 n $. (exponential upper bound)  

## The main theorem

**Theorem (H. 2019+):** 

- Every $(2^{n-1}+1)$-vertex induced subgraph of $Q^n$ contains a vertex of degree at least $\sqrt{n}$.

**Corollary:**

- For every boolean function $f$, $s(f) \geq \sqrt{deg(f)}$ 

The **sensitivity conjecture** is true.

- $bs(f) \leq 2 deg(f)^2$ (Nisan, Szegedy 1992)

  $bs(f) \leq deg(f)^2$ (Tal 2013)

  $bs(f) \leq \sqrt{2/3}deg(f)^2+1$ (Wellens 2020)

- These result imply $bs(f)=O(s(f)^4)$.

## Proof of the main theorem

**Principal Submatrix:** Given a $n \times n$ matrix $A$, a principal submatrix of $A$ is obtained by deleting the same set of rows and columns from $A$.

**Lemma 1 (Cauchy's Interlace Theorem):** Let $A$ be a symmetric $n \times n$ matrix, and $B$ be a $m \times m$ principle submatrix of $A$,  for some $m<n$.   If the eigenvalues of $A$ are $\lambda_1 \geq \lambda_2 \geq \cdots \geq \lambda_n$, and the eigenvalues of $B$ are $\mu_1 \geq \mu_2 \geq \cdots \geq \mu_m$, then for all $1 \leq i \leq m$,
$$
\lambda_i \geq \mu_i \geq \lambda_{i+n-m}.
$$
[You can find the proof here](https://arxiv.org/pdf/math/0502408.pdf)

------

**Lemma 2:** We define a sequence of symmetric square matrices iteratively as follows,
$$
A_1 = \left[\begin{array}{ll}
0 & 1 \\
1 & 0
\end{array}\right], A_n= \left[\begin{array}{ll}
A_{n-1} & I \\
I & -A_{n-1}
\end{array}\right].
$$
Then $A_n$ is a $2^n \times 2^n$ matrix whose eigenvalues are $\sqrt{n}$ of multiplicity $2^{n-1}$, and $-\sqrt{n}$ of multiplicity $2^{n-1}$.

**Proof of Lemma 2:** 

- We prove by induction that $A_n^2=nI$.

  - For $n=1$, $A_1^2=I$.

  - Suppose the statement holds for $n-1$, that is $A_{n-1}^2=(n-1)I$, then
    $$
    A_n^2= \left[\begin{array}{ll}
    A_{n-1}^2+I & 0 \\
    0 & A_{n-1}^2+I
    \end{array}\right] = nI.
    $$
    

- Therefore, the eigenvalues of $A_n$ are either $\sqrt{n}$ or $-\sqrt{n}$.

- Since $trace[A_n] = 0$, we know that $A_n$ has exactly half of the eigenvalues being $\sqrt{n}$ and the rest being $-\sqrt{n}$.

------

**Lemma 3:** Suppose $H$ is an $m$-vertex undirected graph, and $A$ is a symmetric matrix whose entries are in $\{-1,0,1\}$ and whose rows and columns are indexed by $V(H)$, and whenever $u$ and $v$ are non-adjacent in $H$, $A_{u,v}=0$. Then
$$
\Delta(H) \geq \lambda_1 := \lambda_1(A).
$$
**Proof of Lemma 3:** 

- Suppose $\vec{v}$ is the eigenvector corresponding to $\lambda_1$, then $\lambda_1 \vec{v} = A \vec{v}$.
- WLOG, assume $v_1$ is the coordinate of $\vec{v}$ that has the largest absolute value. Then

$$
|\lambda_1 v_1|=|(A \vec{v})_1| = \left| \sum_{j=1}^{m}A_{1,j}v_j \right| = \left| \sum_{j \sim 1} A_{1,j}v_j\right| \leq \sum_{j \sim 1} |A_{1,j}||v_1| \leq \Delta(H)|v_1|.
$$

- Therefore $|\lambda_1| \leq \Delta (H)$.

------

**Proof of the main theorem:**

- Let $A_n$ be the sequence of matrices defined in lemma 2. When we changing every $(-1)$ entry of $A_n$ to 1, we get exactly the adjacency matrix of $Q^n$, and thus $A_n$ and $Q_n$ satisfy the conditions in lemma 3.

- A $(2^{n-1}+1)$-vertex induced subgraph $H$ of $Q^n$ and the principal submatrix $A_H$ of $A_n$ naturally induced by $H$ alse satisfy the conditions of lemma 3. So,
  $$
  \Delta(H) \geq \lambda_1 (A_H).
  $$

- From lemma 2, the eigenvalues of $A_n$ are known to be 

$$
\sqrt{n}, \cdots, \sqrt{n},-\sqrt{n},\cdots,-\sqrt{n}.
$$

- Note that $A_H$ is a $(2^{n-1}+1) \times (2^{n-1}+1)$ submatrix of the $2^n \times 2^n$ matrix $A_n$. By Cauchy's Interlace Theorem, 

$$
\lambda_1 (A_H) \geq \lambda_{1+2^n-2^{n-1}-1}(A_n) = \lambda_{2^{n-1}}(A_n) = \sqrt{n}.
$$

- So, $\Delta(H) \geq \sqrt{n}$. complete our proof.
