## Fourier analysis on the Boolean Function

- We consider the boolean cube $\{-1,1\}^n$ with the uniform measure,
- and study (balanced) Boolean functions $f: \{-1,1\}^n \rightarrow \{-1,1\}$.

**Definition**

- The influence of coordinate $i \in [n]$ is defined as

$$
\textbf{I}_{i}[f] = \underset{x}{Pr}[f(x) \neq f(x \oplus e_i)]
$$

- The total influence of $f$ is $\textbf{I}[f]=\textbf{I}_{1}[f]+ \cdots +\textbf{I}_{n}[f]$.

**key question**

- Suppose $f:\{-1,1\}^n \rightarrow \{-1,1\}$ has a small total influence, $\textbf{I}[f] \leq K$ what can be said about its structure?
- Appears in TCS, learning theory, extremal combinatorics, sharp threshold...

## Theorem of KKL and Friedgut

**Examples**

- Dictatorship: $f(x)=x_1$.
- Juntas: $f(x)=g(x_1,...,x_k)$ for some $g:\{-1,1\}^K \rightarrow \{-1,1\}$.

**Theorem[Kahn-Kalai-Linial]**

- If $\textbf{I}[f] \leq K$, then there exists $i \in [n]$ such that $\textbf{I}_i[f] \geq e^{-O(K)}$. (i.e. $f$ resembles as a dictatorship, can it be strengthened?)

**Theorem[Friedgut]**

- If $\textbf{I}[f] \leq K$, then $f$ essentially depends on $e^{O(K)}$ coordinates: there exists $S \subseteq [n]$ of size $e^{O(K)}$, and $g: \{0,1\}^S \rightarrow \{-1,1\}$ such that $f(x)=g(x_S)$, for $1-\epsilon$ fraction of $x$'s.

Both theorems only meaningful for $K=o(\log n)$. "the logarithmic barrier"

## Basics of Fourier analysis

**Definition [Inner Product]**

- Let $f,g: \{-1,1\}^n \rightarrow \mathbb{R}$. Define $\left< f, g\right> = \mathbb{E}_{x}[f(x)g(x)]$.

**Definition [Characters]**

- For each $S \subseteq [n]$, define $\chi_{S}: \{-1,1\}^n \rightarrow \{-1,1\}$, $\chi_{S}(x) = \Pi_{i \in S} x_i$.

- Fact: $\{\chi_{S}\}_{S \subseteq [n]}$ is an orthonormal basis for the space of real-valued functions.

**Definition [Fourier Decomposition] **

- Write $f: \{-1,1\}^n \rightarrow \mathbb{R}$ according to the basis of characters: $f(x)= \sum_{S \subseteq[n]} \hat{f}(S) \chi_{S}(x)$.
- Coefficients are given by $\hat{f}(S) = \left< f, \chi_S \right>$.

**Fact [Parseval/Plancherel]**

- For $f,g: \{-1,1\}^n \rightarrow \mathbb{R}$ we have that

$$
\left< f, g\right> = \sum_{s \subseteq[n]} \hat{f}(S)\hat{g}(S).
$$

- Particular case of interest: $f=g$ is boolean (+1,-1 valued), getting we have that

$$
1= \left< f,f\right> = \sum_{S \subseteq [n]} \hat{f}(S)^2
$$

​	i.e. $(\hat{f}(S)^2)_{S \subseteq [n]}$ is a distribution.

## The Fourier Entropy Conjecture

Morally, If $\textbf{I}[f] \leq K$, then the Fourier spectrum of $f$ is concentrated on $e^{O(K)}$ coefficients

**Conjecture**

- There exists an absolute constant $C > 0$, such that $H_{shannon}[\hat{f}^2] \leq C \cdot \textbf{I}[f]$ for all $f:\{-1,1\}^n \rightarrow \{-1,1\}$, where $H_{shannon}[\hat{f}^2] = \sum_S \hat{f}(S)^2 \cdot \log \left(1/\hat{f}(S)^2 \right)$.

## The Fourier Min-Entropy Conjecture

- $H_{\infty}[\hat{f}^2] \leq C \cdot \textbf{I}[f]$.
- Clearly weaker than FEC; stronger than [KKL]
- Yet (just as) wide open!

## Fourier analytic formulas for influences

**Lemma**

- Let $f:\{-1,1\}^n \rightarrow \{-1,1\}$. Then $\textbf{I}_i[f]=\sum_{i \in S}\hat{f}(S)^2$.

**Corollary**

- Let $f:\{-1,1\}^n \rightarrow \{-1,1\}$. Then $\textbf{I}[f] = \sum_{S} |S|\hat{f}(S)^2
  $.

**Remark**

- Note: function $f:\{-1,1\}^n \rightarrow \{-1,1\}$ satisfying $\textbf{I}[f] \leq K$ are approximated by low degree polynomials.
- Indeed, by the corollary and markov's inequality $\sum_{|S| \geq K/\epsilon} \hat{f}(S)^2 \leq \epsilon$.
  - $K \geq \textbf{I}[f]=\sum_{S}|S|\hat{f}(S)^2 \geq \sum_{|S| \geq K/\epsilon}(K/\epsilon)\hat{f}(S)^2$,
  - $\sum_{|S| \geq K/\epsilon} \hat{f}(S)^2 \leq \epsilon$.

## Main result

**Theorem [KKLMS]**

- For all boolean, balanced functions $f:\{-1,1\}^n \rightarrow \{-1,1\}$ it holds that 

$$
\sum_S \hat{f}(S)^2 \cdot \log \left(1/\hat{f}(S)^2 \right) \leq C \cdot \sum_{S} |S| \log (|S|+1) \hat{f}(S)^2
$$

**Theorem [KKLMS]**

- For all boolean, balanced functions $f:\{-1,1\}^n \rightarrow \{-1,1\}$, $\forall T \in \mathbb{N}$,

$$
\sum_{|S| \leq T} \hat{f}(S)^2 \cdot \log \left(1/\hat{f}(S)^2 \right) \leq C \cdot \sum_{|S| \leq T} |S| \log (|S|+1) \hat{f}(S)^2 + C \cdot \textbf{I}[f].
$$

## Hypercontractivity

**Hypercontractivity [Bonami, Bcker, Gross]**

- If $p:\{-1,1\}^n \rightarrow \mathbb{R}$ multilinear of degree $d$, then $||p||_4 \leq \sqrt{3}^d \cdot ||p||_2$, where $||p||_q = E[|p(x)|^q]^{1/q}$.

**Fact**

- Suppose $p:\{-1,1\}^n \rightarrow \{-1,0,1\}$ satisfies $Pr[p(x) \neq 0] = \delta$. Then $deg(p) \geq \Omega(\log (1/\delta))$.

**Proof**

- $||p||_4 = E[|p(x)|^4]^{1/4}=\delta^{1/4}$.
- $||p||_2=E[|p(x)|^2]^{1/2}=\delta^{1/2}$.

- By hypercontractivity: $||p||_4 \leq \sqrt{3}^{deg(p)} ||p||_2$, this implies that $deg(p) \geq \Omega(\log (1/\delta))$.

**(Robust) Fact**

- Suppose $p:\{-1,1\}^n \rightarrow \{-1,0,1\}$ satisfies $Pr[p(x) \neq 0] = \delta$. Then $\sum_{|S| \leq 0.1 \log (1/\delta)} \hat{p}(S)^2 \leq \delta^{9/8} \ll \delta$.

**Proof**

- Define the low degree part $g(x) = \sum_{|S| \leq 0.1 \log (1/\delta)} \hat{p}(S) \chi_{S}(x)$. Then by Plancherel $\sum_{|S| \leq 0.1 \log (1/\delta)} \hat{p}(S)^2 = \left< g,p\right>$.

- Using Holder and Hypercontractivity $\left< g,p \right> \leq ||g||_4||p||_{4/3} \leq 3^{deg(g)/2} ||g||_2||p||_{4/3}$.

- By Parseval, $||g||_2 \leq ||p||_2$.

- $||p||_2=E[|p(x)|^2]^{1/2}=\delta^{1/2}$.

- $||p||_{4/3}=E[|p(x)|^{4/3}]^{3/4}=\delta^{3/4}$.

- $\left< g,p \right> \leq 3^{deg(g)/2} ||p||_2||p||_{4/3} \leq 3^{1/20 \cdot \log (1/\delta)} \delta^{5/4} \leq e^{(\log 3)/20 \cdot \log (1/\delta)} \delta^{5/4} $

  $= \delta^{-(\log 3)/20} \delta^{5/4} \leq \delta^{-1/10}\delta^{5/4} \leq \delta^{-1/8}\delta^{5/4} = \delta^{9/8}$.

**Theorem[Kahn-Kalai-Linial]**

- If $\textbf{I}[f] \leq K$, then there exists $i \in [n]$ such that $\textbf{I}_i[f] \geq e^{-O(K)}$.

**Proof of KKL theorem:**

- Assume for contradiction that $I_i[f] \leq e^{-100K}$ for all $i$.
- For each $i \in [n]$, define $\partial_{i} f: \{-1,1\}^n \rightarrow \{-1,0,1\}$ by $\partial_{i}f(x) = \frac{f(x)-f(x \oplus e^i)}{2}$; note that

$$
\partial_{i} f(x)=\sum_{i \in S} \hat{f}(S) \chi_{S}(x)
$$

- Note that $Pr[\partial_{i}f(x) \neq 0] = \textbf{I}_i[f] \leq e^{-100K}$. By the robust fact we get that

$$
\sum_{i \in S, |S| \leq 10K} \hat{f}(S)^2 = \sum_{|S| \leq 10K} \hat{\partial_{i}f}(S)^2 \leq \textbf{I}_{i}[f]^{9/8} \leq e^{-100K/8}\textbf{I}_{i}[f].
$$

- Summing over $i$ gives us that $\sum_{|S| \leq 10K} \hat{f}(S)^2 \leq \sum_{|S| \leq 10K} |S|\hat{f}(S)^2 \leq e^{-100K/8} \sum_{i} \textbf{I}_{i}[f] \leq e^{-100K/8}K \leq 0.1$.

- $\sum_{|S|>10K} \hat{f}(S)^2 \geq 0.9$.
- $\textbf{I}[f]= \sum_{S}|S|\hat{f}(S)^2 \geq \sum_{|S|>10K} |S|\hat{f}(S)^2 > \sum_{|S|>10K} 10K \cdot \hat{f}(S)^2 \geq 9K > K$. 

- This is a contradiction.

## On The Fourier Entropy Conjecture

It's all about correlation inequalities.

**Definition:** Suppose $g:\{-1,1\} \rightarrow \mathbb{R}$, $i \in [n]$, define $\textbf{I}_{i}[g] = E_{x}(\frac{g(x)-g(x \oplus e_i)}{2})^2$.

**Lemma:** Suppose $f:\{-1,1\}^n \rightarrow \{-1,1\}$ is a balanced boolean function, $d \in \mathbb{N}$ and $\textbf{I}_{i}[f^{\leq d}] \leq \delta$, then 
$$
\left< f, f^{\leq d}\right> \leq \delta^{1/8} \cdot e^{O(d)} \cdot \textbf{I}[f].
$$
The Dream: try to reduce the degree of $f^{\leq d}$ prior to applying hypercontractivity.

**Definition:** Suppose $f:\{-1,1\}^n \rightarrow \mathbb{R}$, $A \subseteq [n]$, $z \in \{-1,1\}^{\bar{A}}$, then $f_{\bar{A} \rightarrow z}: \{-1,1\}^{A} \rightarrow \mathbb{R}$ s.t. $f_{\bar{A} \rightarrow z}(y) = f(y,z)$.

- $f^{\leq d}(x) = \sum_{|S| \leq d} \hat{f}(S) \prod_{i \in S \cap A} x_i \cdot \prod_{i \in S \cap \bar{A}} x_i$.
- $f_{\bar{A} \rightarrow z}^{\leq d}(x) = \sum_{|S| \leq d} \hat{f}(S) \prod_{i \in S \cap \bar{A}} z_i\prod_{i \in S \cap A} x_i$. (the characters from $\chi_S$ to $\chi_{S \cap A}$).

Suppose we want to reduce the degree of $f^{\leq d}$ to $v \ll d$. Choose $A$ randomly by including each $i \in [n]$ w.p. $\frac{v}{d}$. Then $|S| \leq d$ implies that $|S \cap A| \lesssim v$.

**Assumption:** $deg(f_{\bar{A} \rightarrow z}) \sim v$.

**Lemma:** Suppose $f:\{-1,1\}^n \rightarrow \{-1,1\}$, $g:\{-1,1\}^n \rightarrow \mathbb{R}$ with $deg(g) \leq d$ and $\textbf{I}_{i}[f^{\leq d}] \leq \delta$. Then 
$$
\left< f,g\right> \leq \delta^{1/8} \cdot e^{O(d)} \cdot \textbf{I}[f,g],
$$
where $\textbf{I}[f,g] = \sum_{i \in [n]} \sqrt{\textbf{I}_i[f]\textbf{I}_i[g]} \leq \sqrt{\textbf{I}[f]\textbf{I}[g]}$.

**Lemma:** Suppose $f:\{-1,1\}^n \rightarrow \{-1,1\}$, $g:\{-1,1\}^n \rightarrow \mathbb{R}$ with $deg(g) \leq d$ and $\delta>0$, then
$$
\left< f, g \right> \leq (\frac{1}{\delta})^d \max_{S} |\hat{f}(S)| \cdot |\hat{g}(S)| + \delta^{1/8} \cdot e^{O(d)} \cdot \textbf{I}[f,g].
$$
