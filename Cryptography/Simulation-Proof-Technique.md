# 1. Define Security for Malicious Adversaries

## 1.1. The definition

**Execution in the ideal model.** 

Let $i \in \{1,2\}$ be the index of the corrupted party, controlled by an adversary $\mathcal{A}$. An ideal execution for a function $f:\{0,1\}^* \times \{0,1\}^* \rightarrow \{0,1\}^* \times \{0,1\}^*$ proceeds as follows:

**Inputs:** $x$ is the input of $P_1$, $y$ is the input of $P_2$, $z$ is the auxiliary input of $\mathcal{A}$, $1^n$ is the security parameter for all party.

**Send inputs to trusted party:** Honest party $P_j$ sends its prescribed input to the trusted party. The  corrupted party $P_i$ controlled by $\mathcal{A}$ may either abort (the input message is $abort_i$), send its prescribe input, or send some other input of the same length to the trusted party. This decision is made by $\mathcal{A}$ and may depend on the input value of $P_i$ and the auxiliary input $z$. Denote the pair of inputs sent to the trusted party by $(x',y')$.

**Early abort option:** If the trusted party receives an input of the form $abort_i$, it sends $abort_i$ to the honest party $P_j$ and the ideal execution terminates. Otherwise, the execution proceeds to the next step.

**Trusted party sends output to adversary:** The trusted party computes $f_1(x',y')$ and $f_2(x',y')$ and sends $f_i(x',y')$ to the party $P_i$.

**Adversary instructs trusted party to continue or halt:** $\mathcal{A}$ sends either $continue$ or $abort_i$ to the trusted party. If it sends $continue$, the trusted party sends $f_j(x',y')$ to the honest party $P_j$. Otherwise, if $\mathcal{A}$ sends $abort_i$, the trusted party sends $abort_i$ to party $P_j$.

**Output:** The honest party always outputs the output value it obtained from the trusted party. The corrupted party outputs nothing. The adversary $\mathcal{A}$ outputs any arbitrary function of the prescribed input of the corrupted party,  the auxiliary input $z$, and the value $f_i(x',y')$ obtained from the trusted party.

$IDEAL_{f,\mathcal{A}(z),i}(x,y,z):$ defined as the output pair of the honest party and the adversary $\mathcal{A}$ from the above ideal execution. 

$REAL_{\pi,\mathcal{A}(z),i}(x,y,z):$ defined as the output pair of the honest party and the adversary $\mathcal{A}$ from the real execution of $\pi$. 

**Definition** Let $f$ be a two-party functionality and let $\pi$ be a two-party protocol that computes $f$. Protocol $\pi$ is said to securely compute $f$ with abort in the presence of static malicious adversaries if for every non-uniform p.p.t. adversary $\mathcal{A}$ for the real model, there exists a non-uniform p.p.t. adversary $\mathcal{S}$ for the ideal model, such that for every $i \in \{1,2\}$,

$$\left\{IDEAL_{f,\mathcal{S}(z),i}(x,y,z)\right\}_{x,y,z,n} \equiv \left\{REAL_{\pi,\mathcal{A}(z),i}(x,y,z)\right\}_{x,y,z,n}$$,

where $|x| = |y|$.

**Deterministic versus probabilistic adversaries:** The simulator $\mathcal{S}$ who is given input $x$ and auxiliary input $z$ for the corrupted party, can begin by choosing a random string $r$ and defining the residual adversary $\mathcal{A}'(\cdot) := \mathcal{A}(x,z,r; \cdot )$, with security parameter $1^n$.

# 2. Determining Output - Coin Tossing

**Blum's Coin Tossing of a Single Bit**

- **Security parameter:**  Both parties have security parameter $1^n$
- **The protocol:**
  1.  $P_1$ chooses a random $b_1 \in \{0,1\}$ and a random $r \in \{0,1\}^n$ and sends $c = Com(b_1;r)$ to $P_2$.
  2. Upon receiving $c$, party $P_2$ chooses a random $b_2 \in \{0,1\}$ and sends $b_2$ to $P_1$.
  3. Upon receiving $b_2$, party $P_1$ sends $(b_1,r)$ to $P_2$ and outputs $b = b_1 \oplus b_2$. (If $P_2$ does not reply. or replies with an invalid value, then $P_2$ sets $b_2 = 0$.)
  4. Upon receiving $(b_1, r)$ from $P_1$, party $P_2$ checks that $c = Com(b_1; r)$. If yes, it outputs $b = b_1 \oplus b_2$; else it outputs $\perp$.

**Theorem:** Assume that $Com$ is a perfectly-binding commitment scheme. Then the protocol securely computes the bit coin-tossing functionality defined by $f_{ct}(\lambda, \lambda) = (U_1, U_1)$.

**Proof:** Let $\mathcal{A}$ be a non-uniform p.p.t. adversary, we may consider deterministic $\mathcal{A}$. we first consider that $P_2$ is corrupted. We describe the simulator $\mathcal{S}$:

1. $\mathcal{S}$ sends $\lambda$ externally to the trusted party computing $f_{ct}$ and receives back a bit $b$.
2. $\mathcal{S}$ initializes a counter $i = 1$.
3. $\mathcal{S}$ invokes $\mathcal{A}$, chooses a random $b_1 \in \{0,1\}$ and $r \in \{0,1\}^n$ and internally hands $\mathcal{A}$ the value $c = Com(b_1; r)$ as if it was sent by $P_1$.
4. If $\mathcal{A}$ replies with $b_2 = b \oplus b_1$, then $\mathcal{S}$ internally hands $\mathcal{A}$ the pair $(b_1, r)$ and outputs whatever $\mathcal{A}$ outputs. (As in the protocol, if $\mathcal{A}$ does not replay or replies with an invalid value, then this is interpreted as $b_2 = 0$.)
5. If $\mathcal{A}$ replies with $b_2 \neq b \oplus b_1$ and $i < n$, then $\mathcal{S}$ sets $i = i+1$ and return back to Step 3.
6. If $i = n$, then $\mathcal{S}$ outputs $fail$.

We first prove that $\mathcal{S}$ outputs $fail$ with negligible probability. Observe that an iteration succeeds if and only if $b_1 \oplus b_2 = b$, where $b_2$ is $\mathcal{A}$'s response to $Com(b_1, r)$. We have:
$$
\begin{align}
	Pr[\mathcal{A}(Com(b_1))=b_1 \oplus b] & = \frac{1}{2} \cdot Pr[\mathcal{A}		(Com(0))=b]+\frac{1}{2} \cdot Pr[\mathcal{A}(Com(1))=1 \oplus b]\\
	& = \frac{1}{2} \cdot Pr[\mathcal{A}(Com(0))=b]+\frac{1}{2} \cdot (1-Pr[\mathcal{A}(Com(1))=b])\\
	& = \frac{1}{2} + \frac{1}{2} \cdot (Pr[\mathcal{A}(Com(0))=b] - Pr[\mathcal{A}(Com(1))=b])
\end{align}
$$
Since $Com$ is a perfectly-biding commitment scheme, thus is computationally hiding, so there exists a negligible function $\mu$ such that for every $b \in \{0,1\}$,
$$
\left|Pr[\mathcal{A}(Com(0))=b]-Pr[\mathcal{A}(Com(1))=b]\right| \leq \mu(n),
$$
 and so
$$
\frac{1}{2} \cdot (1-\mu(n)) \leq Pr[\mathcal{A}(Com(b_1)) = b_1 \oplus b] \leq \frac{1}{2} \cdot (1+\mu(n)).
$$
$\mathcal{S}$ outputs $fail$ if and only if $\mathcal{A}(Com(b_1)) \neq b_1 \oplus b$ in all $n$ iterations, we have that $\mathcal{S}$ outputs $fail$ with probability at most
$$
\left( \frac{1}{2} \cdot \left(1+\mu(n)\right)\right)^{n}<\left( \frac{2}{3}\right)^n.
$$
Next, we show that conditioned on the fact that $\mathcal{S}$ does not output $fail$, the output distributions $IDEAL$ and $REAL$ are statistically close. We can write $b_2 = \mathcal{A}(Com(b_1;r))$. The $REAL$ and $IDEAL$ distribution are of the form $(b,\mathcal{A}(Com(b_1;r),b_1,r))$, where $b = b_1 \oplus \mathcal{A}(Com(b_1;r))$. The difference between the distributions is as follows:

- **Real:** In a real execution, $b_1$ and $r$ are uniformly distributed.
- **Ideal:** In an ideal execution, a random $b$ is chosen, and then random $b_1$ and $r$ are chosen under the constraint that $b_1 \oplus \mathcal{A}(Com(b_1;r)) = b$.

Next, we show that these distributions are statistically close, we calculate the probability that every $(b_1,r)$ is chosen according to the distribution. Fix $\hat{b}_1,\hat{r}$. in the real execution it is immediate that $(\hat{b}_1, \hat{r})$ appears with probability exactly $2^{-(n+1)}$.

Regarding the ideal execution, denote by $S_b = \{(b_1,r)|b_1 \oplus \mathcal{A}(Com(b_1;r)) = b\}$. We claim that the fixed $(\hat{b}_1,\hat{r})$ appears in the ideal execution with probability 
$$
\frac{1}{2} \cdot \frac{1}{|S_b|}.
$$
For every $b$, 
$$
Pr[(b_1,r)\in S_b] = Pr[\mathcal{A}(Com(b_1;r))=b_1 \oplus b]
$$
This implies that 
$$
\frac{1}{2} \cdot (1-\mu(n)) \leq \frac{|S_b|}{2^{n+1}} \leq \frac{1}{2} \cdot (1+\mu(n)),
$$
and so
$$
2^n \cdot (1-\mu(n)) \leq |S_b| \leq 2^n \cdot (1+\mu(n)).
$$
We conclude that the pair $(\hat{b}_1,\hat{r})$ appears in the ideal execution with probability between $2^{-(n+1)} \cdot (1-\mu(n)) $ and $2^{-(n+1)} \cdot (1+\mu(n))$. This is statistically close to the probability that $(\hat{b}_1,\hat{r})$ appears in a real execution.

