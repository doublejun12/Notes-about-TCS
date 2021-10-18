# 1. Eigenvalues

**Theorem (Spectral Theorem):** Given a graph $G$, there exists orthogonal functions $\varphi_0, \varphi_1,...,\varphi_{n-1}$ with $\varphi_0 \equiv \mathbb{1}$ and real numbers $\lambda_0,\lambda_1,...,\lambda_{n-1}$ with $0=\lambda_0 \leq \lambda_1 \leq \cdots \leq \lambda_{n-1} \leq 2$ such that $L \varphi_i = \lambda_i \varphi_i$.

Remember that $L=I-K$, $K$ has same eigenvectors as $L$, and eigenvalues $k_i=1-\lambda_i$.
$$
-1 \leq k_{n-1} \leq \cdots \leq k_1 \leq k_0 = 1.
$$
Given $f:V \rightarrow \mathbb{R}$, we can uniquely express as a linear combination of all $\varphi_i$.
$$
f=\hat{f}(0)\varphi_0 + \hat{f}(1)\varphi_1+\cdots+\hat{f}(n-1)\varphi_{n-1},
$$
where $\hat{f}(i) \in \mathbb{R}$ for all $i$.

Multiply by $L$, we have 
$$
\begin{align}
Lf &= \lambda_0\hat{f}(0)\varphi_0 + \lambda_1\hat{f}(1)\varphi_1+\cdots+\lambda_{n-1}\hat{f}(n-1)\varphi_{n-1}\\
&= \lambda_1\hat{f}(1)\varphi_1+\cdots+\lambda_{n-1}\hat{f}(n-1)\varphi_{n-1}
\end{align}
$$
I.e. $\hat{Lf}(i) = \lambda_i \hat{f}(i)$.

If $g=\hat{g}(0)\varphi_0 + \hat{g}(1)\varphi_1+\cdots+\hat{g}(n-1)\varphi_{n-1}$, then $\left<f, g\right>= \underset{0 \leq i \leq n-1}{\sum} \hat{f}(i)\hat{g}(i)$.

**Corollary:** 

- $||f||_2^2 =  \left<f, f\right> = \underset{0 \leq i \leq n-1}{\sum} \hat{f}(i)^2$.
- $\underset{u \sim \pi}{\mathbb{E}}[f(u)] = \left<f, \mathbb{1} \right> = \left<f, \varphi_0\right> = \hat{f}(0)$.
- $Var[f(u)] = ||f||_2^2-\mathbb{E}[f(u)]^2 = \underset{0 < i \leq n-1}{\sum} \hat{f}(i)^2$.
- $\mathcal{E}[f]=\left<f, Lf \right> = \underset{0 < i \leq n-1}{\sum} \lambda_i\hat{f}(i)^2$.

If $Var[f]=1$,
$$
\mathcal{E}[f] = \underset{0 < i \leq n-1}{\sum} \lambda_i\hat{f}(i)^2 \geq \lambda_1 \underset{0 < i \leq n-1}{\sum} \hat{f}(i)^2 = \lambda_1,
$$
the equation holds when $f = \varphi_1$. Indeed, $\underset{f}{\min}\left\{\frac{\mathcal{E}[f]}{Var[f]} \right\} = \lambda_1$.

# 2. Conductance and Sparse-Cut

Recall: For $S \subseteq V$, the conductance is $\Phi(S) = \underset{u \sim v}{Pr}[v \notin S | u \in S] = \frac{\mathcal{E}[\mathbb{1}_S]}{Vol(S)} = \frac{\mathcal{E}[\mathbb{1}_S]}{\mathbb{E}[\mathbb{1}_S]}$.

The "Sparse-Cut" problem is to determine $\Phi_G = \underset{S}{\min} \left\{\Phi(S)  \right\}$, where $0< vol(S) \leq \frac{1}{2}$. 

Consider the following equation
$$
\frac{\mathcal{E}[\mathbb{1}_S]}{Var[\mathbb{1}_S]} = \frac{\mathcal{E}[\mathbb{1}_S]}{Var[\mathbb{1}_S]} = \frac{\mathcal{E}[\mathbb{1}_S]}{\mathbb{E}[\mathbb{1}_S^2]-\mathbb{E}[\mathbb{1}_S]^2} = \frac{\mathcal{E}[\mathbb{1}_S]}{\mathbb{E}[\mathbb{1}_S](1-\mathbb{E}[\mathbb{1}_S])} = \frac{\mathcal{E}[\mathbb{1}_S]}{vol(S) \cdot vol(\bar{S})}.
$$
$\frac{1}{2} \leq vol(\bar{S}) < 1$, so $\Phi(S) < \frac{\mathcal{E}[\mathbb{1}_S]}{vol(S) \cdot vol(\bar{S})} \leq 2 \cdot \Phi(S)$. Indeed $\Phi_G < \underset{S, \bar{S} \neq \phi}{\min}\left\{\frac{\mathcal{E}[\mathbb{1}_S]}{Var[\mathbb{1}_S]} \right\} \leq 2 \cdot \Phi_G$.

From last section, we have $\lambda_1 \leq \underset{S, \bar{S} \neq \phi}{\min}\left\{\frac{\mathcal{E}[\mathbb{1}_S]}{Var[\mathbb{1}_S]} \right\}$, put them all, we have the following corollary:

**Corollary:** $\Phi_G \geq \frac{1}{2} \lambda_1$.

**Cheeger's Inequality:** $\Phi_G \leq const \cdot \sqrt{\lambda_1}$.

# 3. Mixing Time

$\lambda_1$ "large" ($\lambda_1 \geq \epsilon$) $\Rightarrow$ "fast mixing of random walk".

Recall that $K$ has 

- eigenvector $\varphi_0, ..., \varphi_{n-1}$ with $\varphi \equiv \mathbb{1}$, and
- eigenvalues $1= \kappa_0 \geq \kappa_1 \geq \cdots \geq \kappa_{n-1} \geq -1$ and $\kappa_{n-1} = -1$ if and only if $G$ is bipartite.

$\kappa_1 = 1 - \lambda_1 \leq 1 - \epsilon$ $\Rightarrow$ Fast mixing. 

**Main Theorem:** Say $|\kappa_i| \leq 1 - \epsilon$ for all $i>0$. Then for any worst-case distribution $\rho_0$ on $V$, if $u_0 \sim \rho_0$, and $u_0 \rightarrow u_1 \rightarrow \cdots \rightarrow u_t$ is Standard Random Walk, $t \geq const \cdot \frac{\ln (n)}{\epsilon}$, then $\rho_t$ is "very close" to $\pi$.

"$\rho$ is close to $\pi$" if $f(u) = \frac{\rho[u]}{\pi[u]} \approx 1$ for all $u \in V$, notice $f:V \rightarrow \mathbb{R}^{\geq 0}$. 

Closeness: $d_{\chi^2}(\rho,\pi) := \mathbb{E}[(f(u)-1)^2]$.

**Fact:** For any $\rho$, $\mathbb{E}[f(u)]=1$. so, $\hat{f}(0)=1$.

**Proof of the fact:** $\mathbb{E}[f(u)] = \sum_u \pi[u] \cdot f(u) = \sum_u \pi[u] \cdot \frac{\rho[u]}{\pi[u]} = \sum_u \rho[u] = 1$.

**Lemma: **$d_{\chi^2}(\rho,\pi) := \mathbb{E}[(f(u)-1)^2] = \mathbb{E}[(f(u)-\mathbb{E}[f(u)])^2] = Var[f(u)] = \sum_{0 < i \leq n-1} \hat{f}(i)^2$.

**Proof idea of the main theorem:** 

Instead of taking $\rho_0, \rho_1, ..., \rho_t$ we take $f_0,f_1,...,f_t$.

Say 
$$
\begin{align}
f_0 &= \hat{f}(0)\varphi_0 + \hat{f}(1)\varphi_1 + \cdots + \hat{f}(n-1)\varphi_{n-1}\\
&= \mathbb{1} + \hat{f}(1)\varphi_1 + \cdots + \hat{f}(n-1)\varphi_{n-1}
\end{align}
$$
But what is $f_1$?

**Claim: **$f_1=Kf_0$.

**Proof of the Claim:** For any $u \in V$, 

- $f_1(u) = \frac{\rho_1[u]}{\pi[u]}= \frac{\sum_v \rho_0[v] \cdot K_{v,u}}{\pi[u]} = \sum_v \frac{2|E| \cdot \rho_0[v]}{deg(v) \cdot deg(u)}$.
- $Kf_0(u)=\sum_v K_{u,v}f_0(v) = \sum_v \frac{1}{deg(u)} \cdot \frac{\rho_0[v]}{\pi[v]} = \sum_v \frac{2|E| \cdot \rho_0[v]}{deg(u) \cdot deg(v)}$.

Now, we have 
$$
f_1=Kf_0=\mathbb{1}+\kappa_1\hat{f}(1)\varphi_1+ \cdots+\kappa_{n-1}\hat{f}(n-1)\varphi_{n-1}.
$$
Apply $K$ $t$ times, we have
$$
f_t=K^tf_0=\mathbb{1}+\kappa_1^t\hat{f}(1)\varphi_1+ \cdots+\kappa_{n-1}^t\hat{f}(n-1)\varphi_{n-1}.
$$
The closeness of $\rho_t$ and $\pi$ is
$$
\begin{align}
d_{\chi^2}(\rho_t, \pi) = Var[f_t] & = \underset{1 \leq i \leq n-1}{\sum} \kappa_i^{2t} \hat{f}(i)^2\\
& \leq \max\{\kappa_i^{2t}\} \cdot \underset{1 \leq i \leq n-1}{\sum} \hat{f}(i)^2\\
& = \max\{\kappa_i^{2t}\} \cdot d_{\chi^2}(\rho_0, \pi)\\
& \leq (1-\epsilon)^{2t} \cdot d_{\chi^2}(\rho_0, \pi) \leq exp(-2t\epsilon) \cdot d_{\chi^2}(\rho_0, \pi).
\end{align}
$$
The "worst" $\rho_0$ of form $\rho_0[u_0] = 1$ for one $u_0 \in V$, in this case $f_0(u)=\frac{1}{\pi[u_0]}$ if $u=u_0$ and $f_0(u)=0$ else.
$$
d_{\chi^2}(\rho_0, \pi) = Var[f(u)] \leq \mathbb{E}[f_0^2]=\pi[u_0] \cdot \frac{1}{\pi[u_0]^2} = \frac{1}{\pi[u_0]} \leq 2|E|.
$$
Say $G$ is regular, $\frac{1}{\pi[u_0]} = \frac{2|E|}{d} = n$.

So, if we set $t \geq \ln (n)/\epsilon$, $d_{\chi^2}(\rho_t, \pi) \leq exp(-2t\epsilon) \cdot n \leq n^{-2} \cdot n = 1/n$.