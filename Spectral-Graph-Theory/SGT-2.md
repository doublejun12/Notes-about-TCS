# 1. Markov Transition Operator

Recall that
$$
\begin{align}
\mathcal{E}[f] = \frac{1}{2} \underset{\textbf{u} \sim \textbf{v}}{\mathbb{E}} \left[ (f(\textbf{u}) - f(\textbf{v}))^2 \right] &= \frac{1}{2} \underset{\textbf{u} \sim \textbf{v}}{\mathbb{E}} \left[ (f(\textbf{u}) )^2 \right] + \frac{1}{2} \underset{\textbf{u} \sim \textbf{v}}{\mathbb{E}} \left[ (f(\textbf{v}) )^2 \right] - \underset{\textbf{u} \sim \textbf{v}}{\mathbb{E}} \left[ f(\textbf{u})f(\textbf{v}) \right]\\
&= \underset{\textbf{u} \sim \pi}{\mathbb{E}} \left[ (f(\textbf{u}) )^2 \right] - \underset{\textbf{u} \sim \textbf{v}}{\mathbb{E}} \left[ f(\textbf{u})f(\textbf{v}) \right]\\
&= ||f||_2 - \underset{\textbf{u} \sim \textbf{v}}{\mathbb{E}} \left[ f(\textbf{u})f(\textbf{v}) \right].
\end{align}
$$
We can write
$$
\underset{\textbf{u} \sim \textbf{v}}{\mathbb{E}} \left[ f(\textbf{u})f(\textbf{v}) \right] = \underset{\textbf{u} \sim \pi}{\mathbb{E}}\underset{\textbf{v} \sim \textbf{u}}{\mathbb{E}} \left[ f(\textbf{u})f(\textbf{v}) \right] = \underset{\textbf{u} \sim \pi}{\mathbb{E}} \left[ f(\textbf{u}) \underset{\textbf{v} \sim \textbf{u}}{\mathbb{E}} \left[ f(\textbf{v}) \right] \right]
$$
Now we can define a new function $Kf: V \rightarrow \mathbb{R}$, for $u \in V$, $Kf(u) := \underset{\textbf{v} \sim u}{\mathbb{E}} \left[ f(\textbf{v}) \right]$.

**Fact:** $K(f+g) = Kf+Kg$.

**Proof:** For all $u \in V$, 
$$
\begin{align}
	K(f+g)(u) &= \underset{\textbf{v} \sim u}{\mathbb{E}} \left[ (f+g)(\textbf{v}) \right]\\
	&= \underset{\textbf{v} \sim u}{\mathbb{E}} \left[ (f(\textbf{v})+g(\textbf{v}) \right]\\
	&= \underset{\textbf{v} \sim u}{\mathbb{E}} \left[ f(\textbf{v})\right]+ \underset{\textbf{v} \sim u}{\mathbb{E}} \left[g(\textbf{v}) \right]\\
	&= Kf(u)+Kg(u).
\end{align}
$$

------

**Definition: **The operator $K$ is the Markov Transition Operator(Matrix) for $G$.

 $K_{u,v}=\frac{1}{deg(u)}$ if $(u,v) \in E$; $K_{u,v}=0$ else. So, $K$ is the adjacency matrix $A$, normalized so that row-sums are $1$.

If $G$ is a $d$-regular graph then $K=\frac{1}{d}A$, and $K$ is symmetric $K^{\mathrm{T}}=K$.

**Fact:** For $f,g:V \rightarrow \mathbb{R}$, $\left< f,Kg \right>=\underset{\textbf{u} \sim \textbf{v}}{\mathbb{E}} \left[ f(\textbf{u})g(\textbf{v}) \right]$. 

**Proof:** 
$$
\begin{align}
\left< f,Kg \right> &= \underset{\textbf{u} \sim \pi}{\mathbb{E}} \left[ f(\textbf{u}) \cdot Kg(\textbf{u}) \right]\\
&= \underset{\textbf{u} \sim \pi}{\mathbb{E}} \left[ f(\textbf{u}) \cdot \underset{\textbf{v} \sim \textbf{u}}{\mathbb{E}} \left[ g(\textbf{v}) \right] \right]\\
&= \underset{\textbf{u} \sim \pi}{\mathbb{E}} \underset{\textbf{v} \sim \textbf{u}}{\mathbb{E}} \left[ f(\textbf{u}) \cdot g(\textbf{v}) \right]\\
&= \underset{\textbf{v} \sim \textbf{u}}{\mathbb{E}} \left[ f(\textbf{u}) \cdot g(\textbf{v}) \right]\\
\end{align}
$$

------

**Corollary:** $\left<f, Kg \right> = \left< Kf, g \right>$, which implies $K$ is self-adjoint.

Suppose $S \subseteq V, T \subseteq V$, and $f=1_S, g=1_T$, then $\left< f,Kg \right> = \underset{\textbf{u} \sim \textbf{v}}{\mathbb{E}} \left[ 1_S(\textbf{u})1_T(\textbf{v}) \right] = \underset{\textbf{u} \sim \textbf{v}}{Pr}[\textbf{u} \in S \land \textbf{v} \in T]$.

Let $\rho$ be any distribution on vertices, we do:

1. $u \sim \rho$.
2. 1 random step $u \rightarrow v$.

If $\rho'$ is the distribution of v then $\rho K=\rho'$.

**Corollary:** $\pi K=\pi$.

**Claim:** $K^2=K \circ K$ operator as $K^2f(u) = \underset{u \rightarrow w \text{ 2 step}}{\mathbb{E}}[f(w)]$

**Proof:** Given $f$, let $g=Kf$, then $K^2f=K(Kf)=Kg$.

For all $u \in V$:
$$
\begin{align}
(K^2f)(u) &= Kg(u) =\underset{v \sim u}{\mathbb{E}}[g(v)] =\underset{v \sim u}{\mathbb{E}}[Kf(v)] = \underset{v \sim u}{\mathbb{E}}[\underset{w \sim v}{\mathbb{E}}[f(w)]] =\underset{u \rightarrow w \text{ 2 step}}{\mathbb{E}}[f(w)].
\end{align}
$$

------

**Corollary:** $\forall t \in \mathbb{N}$, $(K^t f)(u) = \underset{u \rightarrow w \text{ t step}}{\mathbb{E}}[f(w)]$, even when $t=0$.

# 2. The Laplacian

Now, we consider the quadratic form $\mathcal{E}[f]$.
$$
\begin{align}
\mathcal{E}[f] = \frac{1}{2}\underset{u \sim v}{\mathbb{E}} \left[ (f(u)-f(v))^2 \right] &= \left< f,f \right> - \underset{u \sim v}{\mathbb{E}} \left[ f(u)f(v)\right]\\
&= \left< f,f \right> - \underset{u \sim \pi}{\mathbb{E}} \underset{v \sim u}{\mathbb{E}} \left[ f(u)f(v)\right]\\
&= \left< f,f \right> - \underset{u \sim \pi}{\mathbb{E}} \left[ f(u) \underset{v \sim u}{\mathbb{E}} f(v)\right]\\
&= \left< f,f \right> - \underset{u \sim \pi}{\mathbb{E}} \left[ f(u) \cdot Kf(u)\right]\\
&= \left< f,f \right> - \left< f, Kf\right>\\
&= \left< f, (I-K)f\right>
\end{align}
$$
**Definition:** $L=I-K$ is the (normalized) laplacian operator for $G$.

For a $d$-regular graph $G$, $L=I-\frac{1}{d}A=\frac{1}{d}(dI-A)$.

I.e. $Lf:V \rightarrow \mathbb{R}$, $Lf(u) = f(u)-\underset{v \sim u}{\mathbb{E}}[f(v)]$.

Let $S \subseteq V$, $f=1_S$, then

- $\left< f,Lf \right> = \mathcal{E}[f] = \underset{u \sim v}{Pr}[u \in S, v \notin S]$.
- $\left< f, f\right> = \underset{u \sim \pi}{\mathbb{E}}[f(u)^2] = \underset{u \sim \pi}{Pr}[u \in S]$.

$\frac{\left< f,Lf\right>}{\left< f,f \right>} = \underset{u \sim v}{Pr}[v \notin S|u \in S] = Pr[\text{Pick a random }u \in S\text{ do 1 step, that you get out of }S] \in [0,1]$.

**Definition:** $\frac{\left< f,Lf\right>}{\left< f,f \right>}$ is the conductance $\Phi(S)$.

We want to max $\mathcal{E}[f] = \left< f,Lf\right> = \frac{1}{2}\underset{u \sim v}{\mathbb{E}} \left[ (f(u)-f(v))^2 \right]$ subject to $||f||_2^2= \left< f, f\right>= \underset{u \sim \pi}{\mathbb{E}}[f(u)^2]=1$. 

A maximizer exists call it $\varphi:V \rightarrow \mathbb{R}$.

**Claim:** $L \varphi = \lambda\varphi$ for some $\lambda$.

**Proof: **Assume for the sake of contradiction $\varphi$ and $L\varphi$ are not parallel. Suppose $\psi$ in a unit vector and orthogonal to $\varphi$, and let $\epsilon \neq 0$ be small real number. Let $f=\frac{1}{\sqrt{1+\epsilon^2}}(\varphi+\epsilon \cdot \psi)$, we have$||f||_2^2 = \frac{1}{1+\epsilon^2}||\varphi+\epsilon \cdot \psi||_2^2 = \frac{1}{1+\epsilon^2}||\varphi||_2^2+\epsilon^2 \cdot ||\psi||_2^2 = 1$ by Pythagorus.
$$
\begin{align}
	\left< f,Lf \right> &= \frac{1}{1+\epsilon^2}\left< \varphi+\epsilon \cdot \psi, L\varphi+\epsilon \cdot L\psi \right>\\
	&= \frac{1}{1+\epsilon^2} \left(\left< \varphi, L\varphi \right> + 2 \epsilon \left<\psi, L \varphi \right> +O(\epsilon^2) \right)\\
	& > \left< \varphi, L\varphi \right>.
\end{align}
$$
If we choose $|\epsilon|$ small enough. 

------

**Corollary:** $\mathcal{E}[\varphi] = \left< \varphi, L \varphi\right> = \left< \varphi, \lambda \varphi\right> = \lambda \left< \varphi, \varphi\right> = \lambda \in [0,2]$.

**Fact:** $\underset{u \sim \pi}{\mathbb{E}}[\varphi(u)] = 0$ and $\left< \varphi, \mathbb{1} \right>=0$.

**Proof of the Fact: ** Assume $\mathbb{E}[\varphi(u)] \neq 0$, define $f=\varphi-\mathbb{E}[\varphi(u)]$. Notice that $\mathcal{E}[f] = \mathcal{E}[\varphi]$ does not change but $||f||_2^2 = \left<\varphi-\mathbb{E}[\varphi(u)], \varphi-\mathbb{E}[\varphi(u)] \right> = ||\varphi||_2^2-\mathbb{E}[\varphi(u)]^2 < ||\varphi||_2^2 = 1$. Let $f' = \frac{f}{||f||_2}$, $||f'||_2^2=1$ and $\mathcal{E}[f'] = \frac{1}{||f||_2^2} \mathcal{E}[f] > \mathcal{E}[f] = \mathcal{E}[\varphi]$, which is a contradiction since $\varphi$ is the maximizer.

Now, we consider Max $\left< f,Lf \right>$ subject to $||f||_2^2=1$ and $f \perp \varphi$. using the same argument above, the maximizing vector $\varphi'$ having $L \varphi'=\lambda'\varphi'$ for some $\lambda' \leq \lambda$ and $\underset{u \sim \pi}{\mathbb{E}}[\varphi'(u)] = 0$ $(\left< \varphi', \mathbb{1} \right>=0)$. Notice that $L\mathbb{1}=0$.

**Theorem (Spectral Theorem):** Given a graph $G$, there exists orthogonal functions $\varphi_0, \varphi_1,...,\varphi_{n-1}$ with $\varphi_0 \equiv \mathbb{1}$ and real numbers $\lambda_0,\lambda_1,...,\lambda_{n-1}$ with $0=\lambda_0 \leq \lambda_1 \leq \cdots \leq \lambda_{n-1} \leq 2$ such that $L \varphi_i = \lambda_i \varphi_i$.
