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

## 2.1. Coin Tossing a Single Bit

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

We now turn to the case that $P_1$ is corrupted. The simulator $\mathcal{S}$ works as follows:

1. $\mathcal{S}$ sends $\lambda$ externally to the trusted party computing $f_{ct}$ and receives back a bit $b$.
2. $\mathcal{S}$ invokes $\mathcal{A}$ and internally receives the message $c$ that $\mathcal{A}$ sends to $P_2$.
3. $\mathcal{S}$ internally hands $\mathcal{A}$ the bit $b_2=0$ as if coming from $P_2$, and receives back its reply. Then, $\mathcal{S}$ internally hands $\mathcal{A}$ the bit $b_2=1$ as if coming from $P_2$, and receives back its reply. We have the following case:
   1. If $\mathcal{A}$ replies with a valid decommitment $(b_1,r)$ such that $Com(b_1;r)=c$ in both iterations, then $\mathcal{S}$ externally sends $continue$ to the trusted party. In addition, $\mathcal{S}$ defines $b
      _2=b_1 \oplus b$, internally hands $\mathcal{A}$ the bit $b_2$, and outputs whatever $\mathcal{A}$ outputs.
   2. If $\mathcal{A}$ does not replay with a valid decommitment in either iteration, then $\mathcal{S}$ externally sends $abort_1$ to the trusted party. Then, $\mathcal{S}$ internally hands $\mathcal{A}$ a random bit $b_2$ and outputs whatever $\mathcal{A}$ outputs.
   3. If $\mathcal{A}$ replies with a valid decommitment $(b_1,r)$ such that $Com(b_1;r) = c$ only when given $b_2$ where $b_1 \oplus b_2 = b$, then $\mathcal{S}$ externally sends $continue$ to the trust party.  Then, $\mathcal{S}$ internally hands $\mathcal{A}$ the bit $b_2=b_1 \oplus b$ and outputs whatever $\mathcal{A}$ outputs.
   4. If $\mathcal{A}$ replies with a valid decommitment $(b_1,r)$ such that $Com(b_1;r) = c$ only when given $b_2$ where $b_1 \oplus b_2 \neq b$, then $\mathcal{S}$ externally sends $abort_1$ to the trust party.  Then, $\mathcal{S}$ internally hands $\mathcal{A}$ the bit $b_2=b_1 \oplus b \oplus 1$ and outputs whatever $\mathcal{A}$ outputs.

We prove that the output distribution is identical. 

- Case 3.1.  $\mathcal{A}$'s view in a real execution consists of a random bit $b_2$, and the honest $P_2$'s output equals $b = b_1 \oplus b_2$. Since $b_1$ is fully determined by the commitment $c$ before $b_2$ is chosen by $P_2$, it follows that $b$ is uniformly distributed. In contrast, in an ideal execution, the bit $b$ is uniformly chosen. Then, $\mathcal{A}$'s view consists of $b_2 = b_1 \oplus b$ and the honest $P_2$'s output equals $b$. Since $b_1$ is fully determined by the commitment $c$ before any information about $b$ is given to $\mathcal{A}$, it follows that $b_2 = b_1 \oplus b$ is uniformly distributed. In both cases, the bits $b$ and $b_2$ are uniformly distributed under the constraint that $b \oplus b_2 = b_1$. 
- Case 3.2. $\mathcal{A}$'s view consists of a uniformly distributed bit, exactly like in a real execution. In addition, the honest $P_2$ outputs $\perp$ in both the real and ideal executions.
- Case 3.3. and 3.4. $\mathcal{A}$ replies with a valid decommitment for exactly one value $\hat{b}_2 \in \{0,1\}$, let $b_1$ be the value committed in the commitment $c$ sent by $\mathcal{A}$. Then, in the real execution, if $P_2$ sends $\hat{b}_2$ then $\mathcal{A}$ replies with a valid decommiment and the honest $P_2$ outputs $b=b_1 \oplus \hat{b}_2$. In contrast, if $P_2$ sends $\hat{b}_2 \oplus 1$, then $\mathcal{A}$ does not reply with a valid decommitment and $P_2$ outputs $\perp$. Consider now the ideal execution. If $b \oplus b_1 = \hat{b}_2$, then $\mathcal{S}$ hands $\mathcal{A}$ the bit $\hat{b}_2$. In this case, $\mathcal{A}$ replies with a valid decommitment and the honest party $P_2$ outputs $b = b_1 \oplus \hat{b}_2$. In contrast, if $b \oplus b_1 = \hat{b}_2 \oplus 1$, then $\mathcal{S}$ hands $\mathcal{A}$ the bit $\hat{b} \oplus 1$. In this case, $\mathcal{A}$ does not replay with a valid decommitment and $P_2$ outputs $\perp$.

We see that the distribution over the view of $\mathcal{A}$ and the output of $P_2$ is identical in both cases.

## 2.2. Securely Tossing Many Coins and the Hybrid Model

**Multiple Coin Tossing**

- **Input:** Both parties have $1^n$ (where $l(n)$ is the number of coins to be tossed).

- **Security parameter: **Both parties have security parameter $1^n$.

- **Hybrid functionalities: **Let $L_1=\{c|\exists (x,r):c=Com(x;r)\}$ be the language of all valid commitment, and let $R_1$ be its  associated $\mathcal{NP}$-relation. Let $L_2=\{(c,x)|\exist r:c=Com(x;r)\}$ be the language of all pairs of commitments and committed values, and let $R_2$ be its associated $\mathcal{NP}$-relation.

  The parties have access to a trusted party that computes the zero-knowledge proof of knowledge functionalities $f_{zk}^{R_1}$ and $f_{zk}^{R_2}$ associated with relations $R_1$ and $R_2$, respectively.

- **The protocol (for tossing $l(n)$ coins):**

1. $P_1$ chooses a random $\rho_1 \in \{0,1\}^{l(n)}$ and a random $r \in \{0,1\}^{poly(n)}$ of length sufficient to commit to $l(n)$ bits, and sends $c=Com(\rho_1;r)$ to $P_2$.
2. $P_1$ sends $(c,(\rho_1,r))$ to $f_{zk}^{R_1}$.
3.  Upon receiving $c$, party $P_2$ sends $c$ to $f_{zk}^{R_1}$ and receives back a bit $b$. if $b=0$ then $P_2$ output $\perp$ and halts. Otherwise, it proceeds.
4. $P_2$ chooses a random $\rho_2 \in \{0,1\}^{l(n)}$ and sends $\rho_2$ to $P_1$.
5. Upon receiving $\rho_2$, party $P_1$ sends $\rho_1$ to $P_2$ and sends $((c, \rho_1),r)$ to $f_{zk}^{R_2}$. (If $P_2$ does not reply, or replies with an invalid value, then $P_1$ sets $\rho_2=0^{l(n)}$. ) 
6. Upon receiving $\rho_1$, party $P_2$ sends $(c, \rho_1)$ to $f_{zk}^{R_2}$ and receives back a bit $b$. If $b=0$ then $P_2$ outputs $\perp$ and halts. Otherwise, it outputs $\rho = \rho_1 \oplus \rho_2$.
7. $P_1$ outputs $\rho=\rho_1 \oplus \rho_2$.

**Theorem:** Assume that $Com$ is a perfectly-binding commitment scheme and let $l$ be a polynomial. Then, this protocol securely computes the functionality $f_{ct}^l(\lambda, \lambda) = (U_{l(n)}, U_{l(n)})$ in the $(f_{zk}^{R_1}, f_{zk}^{R_2})$-hybrid model.

**Proof:** We first consider the case that $P_1$ is corrupted, and then the case that $P_2$ is corrupted.

**$P_1$ corrupted:** Simulator $\mathcal{S}$ works as follows:

1. $\mathcal{S}$ invokes $\mathcal{A}$, and receives the message $c$ that $\mathcal{A}$ sends to $P_2$, and the message $(c',(\rho_1,r))$ that $\mathcal{A}$ sends to $f_{zk}^{R_1}$.

2. If $c' \neq c$ or $c \neq Com(\rho_1;r)$, then $\mathcal{S}$ sends $abort_1$ to the trusted party computing $f_{ct}^l$, simulates $P_2$ aborting, and outputs whatever $\mathcal{A}$ outputs. Otherwise, it proceeds to the next step.

3. $\mathcal{S}$ sends $1^n$ to the external trusted party computing $f_{ct}^l$ and receives back a string $\rho \in \{0,1\}^{l(n)}$.

4. $\mathcal{S}$ sets $\rho_2=\rho \oplus \rho_1$, and internally hands $\rho_2$ to $\mathcal{A}$.

5. $\mathcal{S}$ receives the message $\rho'_1$ that $\mathcal{A}$ sends to $P_2$, and the message $((c'',\rho''_1),r'')$ that $\mathcal{A}$ sends to $f_{zk}^{R_2}$. If $c'' \neq c$ or $\rho'_1 \neq \rho''_1$ or $c \neq Com(\rho''_1;r'')$ then $\mathcal{S}$ sends $abort_1$ to the trusted party computing $f_{ct}^l$, simulates $P_2$ aborting, and outputs whatever $\mathcal{A}$ outputs.

   Otherwise, $\mathcal{S}$ externally sends $continue$ to the trusted party, and outputs whatever $\mathcal{A}$ outputs.

We consider three phases of the execution: (1). $\mathcal{A}$, controlling $P_1$, sends $c$ to $P_2$ and $(c,(\rho_1,r))$ to $f_{zk}^{R_1}$; (2). $P_2$ sends $\rho_2$ to $P_1$; and (3). $\mathcal{A}$ sends $\rho_1$ to $P_2$ and $((c,\rho_1),r)$ to $f_{zk}^{R_2}$.

- Phase 1: Since $\mathcal{A}$ is deterministic and there is no rewinding, the distribution over the first phase is identical in the real and ideal executions. (If these messages cause $P_2$ to output $\perp$, then this is the entire distribution and so is identical.)
- Phase 2: Assume that the phase 1 messages do not result in $P_2$ outputting $\perp$. Then, we claim that for every triple $(c,\rho_1,r)$ making up the phase 1 messages, the distribution over $\rho_2$ received by $\mathcal{A}$ is identical in the real and ideal executions. In a real execution, the honest $P_2$ chooses $\rho_2 \in \{0,1\}^{l(n)}$ uniformly and independently of $(c, \rho_1,r)$. In contrast, in an ideal execution, $\rho \in \{0,1\}^{l(n)}$ is chosen uniformly and then $\rho_2$ is set to equal $\rho \oplus \rho_1$. Since $\rho$ is chosen independently of $\rho_1$, we have that $\rho_2=\rho_1 \oplus \rho$ is also uniformly distributed in $\{0,1\}^{l(n)}$ and independent of $(c,\rho_1,r)$.

- Phase 3: Assume again that the phase 1 messages do not result in $P_2$ outputting $\perp$. Then, we claim that for every $(c, \rho_1, r, \rho_2)$ making up the phase 1 and 2 messages, it holds that the honest $P_2$ outputs the exact same value in a real execution with $\mathcal{A}$ and in an ideal execution with $\mathcal{S}$. In order to see this, observe that this phase consists only of $\mathcal{A}$ sending $\rho'_1$ to $P_2$ and $((c'',\rho''_1),r'')$ to $f_{zk}^{R_2}$. there are two cases:
  - $c''=c$ and $\rho'_1=\rho''_1$ and $c=Com(\rho''_1;r'')$: In this case, in a real execution the trusted party computing $f_{zk}^{R_2}$ will send 1 to $P_2$ and in an ideal execution $\mathcal{S}$ will send $continue$ to the trusted party. This holds because both $\mathcal{A}$ and $P_2$ send the same public statement $(c,\rho'_1)$ to $f_{zk}^{R_2}$ and it holds that $c=Com(\rho'_1;r'')$. Now, in a real execution, $P_2$ outputs $\rho'_1 \oplus \rho_2$, whereas in an ideal execution $P_2$ outputs $\rho=\rho_1 \oplus \rho_2$. However, since $c$ is a perfectly-binding commitment scheme, we have that $\rho'_1=\rho_1$. This implies that in this case the honest $P_2$ outputs the same $\rho=\rho_1 \oplus \rho_2$ in  the real and ideal executions.
  - $c'' \neq c$ or $\rho'_1 \neq \rho''_1$ or $c \neq Com(\rho'';r'')$: In this case, in an ideal execution $\mathcal{S}$ will send $abort_1$ to the trusted party, and the honest $P_2$ will output $\perp$.  In a real execution, in this case, the trusted party computing $f_{zk}^{R_2}$ will send 0 to $P_2$. This is because either $\mathcal{A}$ and $P_2$ send different statements to the trusted party ($(c, \rho'_1)$ versus $(c'', \rho''_1)$) or the witness is incorrect and $c \neq Com(\rho'';r'')$. Thus, the honest $P_2$ in a real protocol execution will also output $\perp$. 

This proves that the overall joint distribution over $\mathcal{A}$'s view and $P_2$' output is identical in the real and ideal executions.

**$P_2$ is corrupted.** Simulator $\mathcal{S}$ works as follows:

1. $\mathcal{S}$ sends $1^n$ to the external trusted party computing $f_{ct}^l$ and receives back a string $\rho \in \{0,1\}^{l(n)}$. $\mathcal{S}$ externally sends $continue$ to the trusted party ($P_1$ always receives output).
2. $\mathcal{S}$ chooses a random $r \in \{0,1\}^{poly(n)}$ of sufficient length to commit to $l(n)$ bits, and computes $c=Com(0^{l(n)};r)$.
3. $\mathcal{S}$ internally invokes $\mathcal{A}$ and hands it $c$.
4. $\mathcal{S}$ receives back some $\rho_2$ from $\mathcal{A}$ (if $\mathcal{A}$ doesn't send a valid $\rho_2$ then $\mathcal{S}$ sets $\rho_2=0^{l(n)}$ as in the real protocol).
5. $\mathcal{S}$ sets $\rho_1=\rho_2 \oplus \rho$ and internally hands $\mathcal{A}$ the message $\rho_1$ as if coming from $P_1$.
6. $\mathcal{S}$ receives some pair $(c',\rho'_1)$ from $\mathcal{A}$ as it sends to $f_{zk}^{R_2}$ (as the verifier). If $(c',\rho'_1) \neq (c, \rho_1)$ then $\mathcal{S}$ internally simulates $f_{zk}^{R_2}$ sending 0 to $\mathcal{A}$. Otherwise, $\mathcal{S}$ internally simulates $f_{zk}^{R_2}$ sending 1 to $\mathcal{A}$.
7. $\mathcal{S}$ outputs whatever $\mathcal{A}$ outputs. 

Let $S'$ work in the same way as $\mathcal{S}$ except that instead of receiving $\rho$ externally from the trusted party, $S'$ chooses $\rho$ by itself (uniformly at random) after receiving $\rho_2$ from $\mathcal{A}$. In addition, the output of the honest party is set to be $\rho$. Stated differently, $S'$ outputs the pair $(\rho,output(\mathcal{A}))$.

Simulator $S'$ works as follows:

1. $\mathcal{S}'$ chooses a random $r \in \{0,1\}^{poly(n)}$ of sufficient length to commit to $l(n)$ bits, and computes $c=Com(0^{l(n)};r)$.
2. $\mathcal{S}'$ internally invokes $\mathcal{A}$ and hands it $c$.
3. $\mathcal{S}'$ receives back some $\rho_2$ from $\mathcal{A}$ (if $\mathcal{A}$ doesn't send a valid $\rho_2$ then $\mathcal{S}'$ sets $\rho_2=0^{l(n)}$ as in the real protocol).
4. $\mathcal{S}'$ chooses $\rho$ uniformly at random, sets $\rho_1=\rho_2 \oplus \rho$ and internally hands $\mathcal{A}$ the message $\rho_1$ as if coming from $P_1$.
5. $\mathcal{S}'$ receives some pair $(c',\rho'_1)$ from $\mathcal{A}$ as it sends to $f_{zk}^{R_2}$ (as the verifier). If $(c',\rho'_1) \neq (c, \rho_1)$ then $\mathcal{S}'$ internally simulates $f_{zk}^{R_2}$ sending 0 to $\mathcal{A}$. Otherwise, $\mathcal{S}'$ internally simulates $f_{zk}^{R_2}$ sending 1 to $\mathcal{A}$.
6. $\mathcal{S}'$ outputs $(\rho, output(\mathcal{A}))$. 

It is immediate that
$$
\left\{ S'(1^n) \right\}_{n \in \mathbb{N}} \equiv \left\{ IDEAL_{f_{ct}^l, \mathcal{S}} (1^n, 1^n, n) \right\}_{n \in \mathbb{N}}
$$
We present $S'$ as a way of proving the indistinguishability of two distribution, and not as a valid simulator.

Now we describe the distinguisher $D$.

$D$ is given a commitment $c$ and (random) string $R$ as input

1. $D$ internally invokes $\mathcal{A}$ and hands it $c$.
2. $D$ receives back some $\rho_2$ from $\mathcal{A}$ (if $\mathcal{A}$ doesn't send a valid $\rho_2$ then $D$ sets $\rho_2=0^{l(n)}$ as in the real protocol).
3. $D$ sets $\rho_1 = R$ and $\rho = \rho_1 \oplus \rho_2$, internally hands $\mathcal{A}$ the message $\rho_1$ as if coming from $P_1$.
4. $D$ receives some pair $(c',\rho'_1)$ from $\mathcal{A}$ as it sends to $f_{zk}^{R_2}$ (as the verifier). If $(c',\rho'_1) \neq (c, \rho_1)$ then $D$ internally simulates $f_{zk}^{R_2}$ sending 0 to $\mathcal{A}$. Otherwise, $D$ internally simulates $f_{zk}^{R_2}$ sending 1 to $\mathcal{A}$.
5. $D$ outputs $(\rho, output(\mathcal{A}))$. 

- If $D$ receives the commitment $c = Com(0^{l(n)})$ then its output is identical to the output of $S'$. Since $\rho_1$ is random and independent of everything else, $\rho$ is uniformly distributed, exactly as in an execution of $\mathcal{S}'$. The commitment $c$ is also exactly as generated by $\mathcal{S}'$. Thus, the output distribution is identical.
- If $\mathcal{D}$ receives the commitment $c=Com(\rho_1)$ then its output is identical to the joint output distribution from a real execution. This holds because the commitment from $P_1$ is a commitment to a random $\rho_1$, and the same $\rho_1$ is sent to $\mathcal{A}$ in the last message of the protocol. In addition, the output of the honest party is $\rho_1 \oplus \rho_2$ exactly like in a real execution. Thus, this is just a real execution between an honest $P_1$ and the adversary $\mathcal{A}$.

It follows from the hiding property of the commitment scheme that the output distribution generated by $\mathcal{D}$ are computationally indistinguishable. Thus
$$
\left\{ \mathcal{S}'(1^n) \right\}_{n \in \mathbb{N}} \overset{c}{\equiv} \left\{ REAL_{\pi,\mathcal{A}} (1^n,1^n,n)\right\}_{n \in \mathbb{N}}
$$
This complete the proof.

# 3. Extracting Inputs - Oblivious Transfer

The simulator must extractor the input from the advesary, send it to the trusted party and receive back the output.

We will study the oblivious transfer functionality defined by $f_{ot}((x_0,x_1), \sigma)=(\lambda, x_{\sigma})$.

**RAND Procedure:** Let $\mathbb{G}$ be a multiplicative group pf prime order $q$. Define
$$
RAND(g,x,y,z)=(u,v)=(g^s \cdot y^t, x^s \cdot z^t).
$$
where $s,t \leftarrow \mathbb{Z}_q$ are chosen uniformly at random.

**Claim:** Let $g$ be agenerator of $\mathbb{G}$ and let $x,y,z \in \mathbb{G}$. If $(g,x,y,z)$ do not form a Diffie-Hellman tuple (i.e., there does not exist $a \in \mathbb{Z}_q$ such that $y=g^a$ and $z=x^a$), then $RAND(g,x,y,z)$ is uniformly distributed in $\mathbb{G}^2$.

**Oblivious Transfer**

- **Input:** Party $P_1$'s input is a pair $(x_0,x_1)$ and party $P_2$'s input is a bit $\sigma$. We assume for simplicity that $x_0, x_1 \in \mathbb{G}$ where $\mathbb{G}$ is defined in the auxiliary input.
- **Auxiliary input:** Both parties hold a security parameter $1^n$ and $(\mathbb{G},q,g_0)$, where $\mathbb{G}$ is an efficient representation of a group of prime order $q$ with a generator $g_0$, and $q$ is of length $n$.
- **Hybrid functionality:** Let $L=\{(\mathbb{G},q,g_0,x,y,z)|\exists a \in \mathbb{Z}_q: y=(g_0)^a \land z=x^a\}$. be the language of all Diffie-Hellman tuple, and let $R_L$ be its associated $\mathcal{NP}$-relation. The parties have access to a trusted party that computes the zero-knowledge proof of knowledge functionlity $f_{zk}^{R_L}$ associated with relation $R_L$.
- **The protocol:**
  1. $P_2$ chooses random values $y, \alpha \in \mathbb{Z}_q$ and computes $g_1=(g_0)^y, h_0=(g_0)^\alpha$ and $h_1=(g_1)^{\alpha+1}$ and sends $(g_1,h_0,h_1)$ to $P_1$.
  2. $P_2$ sends statement $\left( \mathbb{G},q,g_0,g_1,h_0, \frac{h_1}{g_1} \right)$ and witness $\alpha$ to $f_{zk}^{R_L}$.
  3. $P_1$ sends statement $\left( \mathbb{G},q,g_0,g_1,h_0, \frac{h_1}{g_1} \right)$ to $f_{zk}^{R_L}$ and receives back a bit. If the bit equals 0, then it halts and output $\perp$. Otherwise, it proceeds to the next step.
  4. $P_2$ chooses a random value $r \in \mathbb{Z}_q$, computes $g=(g_\sigma)^r$ and $h=(h_{\sigma})^r$ and sends $(g,h)$ to $P_1$.
  5. $P_1$ computes $(u_0, v_0)=RAND(g_0,g,h_0,h)$, and $(u_1,v_1)=RAND(g_1,g,h_1,h)$. $P_1$ sends $P_2$ the values $(u_0,w_0)$ where $w_0=v_0 \cdot x_0$, and $(u_1, w_1)$ where $w_1=v_1 \cdot x_1$.
  6. $P_2$ computes $x_{\sigma}=w_{\sigma}/(u_{\sigma})^r$.
  7. $P_1$ outputs $\lambda$ and $P_2$ outputs $x_{\sigma}$.

------

**Theorem:** Assume that the DDH problem is hard in the auxilirary-input group $\mathbb{G}$. Then, the protocol securely computes $f_{ot}$ in the presence of malicious adversaries.

**Correctness: **$w_{\sigma}/(u_{\sigma})^r =(v_{\sigma} \cdot x_{\sigma})/(g_{\sigma}^s \cdot h_{\sigma}^t)^r = (g^s \cdot h^t \cdot x_{\sigma})/(g^s \cdot h^t) = x_{\sigma}$.

------

$P_1$ is corrupted. Simulator $\mathcal{S}$ works as follows:

1. $\mathcal{S}$ internally invokes $\mathcal{A}$ controlling $P_1$. (Assume that $\mathcal{A}$ is deterministic)
2. $\mathcal{S}$ chooses $y, \alpha \in \mathbb{Z}_q$ and computes $g_1=(g_0)^y$, $h_0=(g_0)^\alpha$, $h_1=(g_1)^\alpha$. (For real protocol, $h_1=(g_1)^{\alpha+1}$.)
3. $\mathcal{S}$ internally hands $(g_1,h_0,h_1)$ to $\mathcal{A}$.
4. When $\mathcal{A}$ sends a message intended for $f_{zk}^{R_L}$. If the message equals $(\mathbb{G},q,g_0,g_1,h_0,\frac{h_1}{g_1})$ then $\mathcal{S}$ internally hands $\mathcal{A}$ the bit 1 as if it came from $f_{zk}^{R_L}$. If the message equals anything else, then $\mathcal{S}$ simulates $\mathcal{A}$ receiving 0 from $f_{zk}^{R_L}$.
5. $\mathcal{S}$ chooses a random value $r \in \mathbb{Z}_q$, computes $g=(g_0)^r$ and $h=(h_0)^r$, and internally sends $(g,h)$ to $\mathcal{A}$. (This is exactly like an honest $P_2$ with input $\sigma=0$.)
6. When $\mathcal{A}$ sends messages $(u_0,w_0),(u_1,w_1)$ then simulator $\mathcal{S}$ computes $x_0=w_0/(u_0)^r$ and $x_1=w_1/(u_1)^{r \cdot y^{-1} \mod{q}}$. (If the message is not formed correctly, then $\mathcal{S}$ sends $abort_1$ to the trusted party and output whatever $\mathcal{A}$ outputs. Otherwise, it proceeds.)
7. $\mathcal{S}$ sends $(x_0,x_1)$ to the trusted party computing $f_{ot}$. (Formally, $\mathcal{S}$ receives back output $\lambda$ and then sends $continue$ to the trusted party.)
8. $\mathcal{S}$ outputs whatever $\mathcal{A}$ outputs, and halts.

Denote the OT protocol by $\pi$, and denote by $\pi'$ a protocol that is the same as $\pi$ except for the two following differences:

1. $P_2$ chooses $y,\alpha \in \mathbb{Z}_q$ and computes $g_1=(g_0)^y$, $h_0=(g_0)^\alpha$ and $h_1=(g_1)^\alpha$ (instead of computing $h_1=(g_1)^{\alpha+1}$.)
2. $f_{zk}^{R_L}$ is modified so that it sends 1 to $P_1$ if and only if $P_1$ and $P_2$ sends the same statement.

We claim for every non-uniform p.p.t. adversary $\mathcal{A}$ controlling $P_1$:
$$
\left\{REAL_{\pi,\mathcal{A}(z)}((x_0,x_1),\sigma,n) \right\}_{x_0,x_1,\sigma,z,n} \overset{c}{\equiv} \left\{ REAL_{\pi',\mathcal{A}(z)}((x_0,x_1),\sigma,n) \right\}_{x_0,x_1,\sigma,z,n}.
$$
We make no claim when $P_2$ is corrupted. It is not true when $P_2$ is corrupted.

- There is one difference between $\pi$ and $\pi'$ and this is how $g_1,h_0,h_1$ are chosen.

- The change to $f_{zk}^{R_L}$ is just to ensure that the output is always 1 unless $\mathcal{A}$ sends a different statement. Since $P_2$ is honest, this makes no difference.

We have to show both that the joint distribution over $\mathcal{A}$'s view and $P_2$'s output is indistinguishable in $\pi$ and $\pi'$.

We use the following variant DDH assumption in $\mathbb{G}$.

**Lemma:** For every non-uniform p.p.t. distinguisher $D$ there exists a negligible function $\mu$ s.t.
$$
|Pr[D(\mathbb{G},q,g_0,g_1,(g_0)^r,(g_1)^r)=1]-Pr[D(\mathbb{G},q,g_0,g_1,(g_0)^r,(g_1)^{r+1})=1]| \leq \mu(n),
$$
Where $|\mathbb{G}|=q$ is a prime, $g_0,g_1$ are random generators, and $r \in \mathbb{Z}_q$ is randomly chosen. 

**Proof of the lemma:** Consider the standard DDH assumption
$$
|Pr[D(\mathbb{G},q,g_0,g_1,(g_0)^r,(g_1)^r)=1]-Pr[D(\mathbb{G},q,g_0,g_1,(g_0)^r,(g_1)^{s})=1]| \leq \mu(n),
$$
where $r,s \in \mathbb{Z}_q$ are randomly chosen.

We will prove the lemma by proving
$$
|Pr[D(\mathbb{G},q,g_0,g_1,(g_0)^r,(g_1)^s)=1]-Pr[D(\mathbb{G},q,g_0,g_1,(g_0)^r,(g_1)^{r+1})=1]| \leq \mu(n),
$$
In order to see this, $D'$ receiving $(g_0,g_1,h_0,h_1)$ can run $D$ on $(g_0,g_1,h_0,h_1 \cdot g_1)$.

- If $D'$ received $(g_0,g_1,(g_0)^r,(g_1)^r)$ then it generates $(g_0,g_1,(g_0)^r,(g_1)^{r+1})$.
- If $D'$ received $(g_0,g_1,(g_0)^r,(g_1)^s)$ then it generates $(g_0,g_1,(g_0)^r,(g_1)^{s+1})$, which is identical to the distribution over $(g_0, g_1,(g_0)^r,(g_1)^s)$.

If $D$ can distinguish  $(g_0,g_1,(g_0)^r,(g_1)^{r+1})$ and $(g_0, g_1,(g_0)^r,(g_1)^s)$ with non-negligible probability, then $D'$ can use $D$ to solve the standard DDH problem.

So, the lemma holds.

Now, we assume, by contradiction, that there exists an adversary $\mathcal{A}$ controlling $P_1$, a distinguisher $D_{\pi}$, a polynomial $p(\cdot)$, and an infinite series of tuples $(\mathbb{G},q,g,x_0,x_1,\sigma,z,n)$ with $|q|=n$ such that
$$
\left| Pr[D_{\pi}(REAL_{\pi,\mathcal{A}(z)}((x_0,x_1),\sigma,n))=1]-Pr[D_{\pi}(REAL_{\pi',\mathcal{A}(z)}((x_0,x_1),\sigma,n))=1] \right| \geq \frac{1}{p(n)}.
$$
We construct a non-uniform probabilistic-polynomial time distinguisher $D$ who receives input $(\mathbb{G},q,g_0,g_1,h_0,h_1)$, and a tuple $(x_0,x_1,\sigma,z,n)$ on its advice tape, and works as follows:

1. $D$ invokes $\mathcal{A}$ and an honest $P_2$ with security parameter $1^n$, respective inputs $x_0,x_1$ and $\sigma$, and auxiliary input $z$ for $\mathcal{A}$.
2. $D$ runs the execution between $\mathcal{A}$ and $P_2$ following the instruction of $\pi'$ with one change. Instead of $P_2$ choosing $y,\alpha \in \mathbb{Z}_q$ and generating $g_1,h_0,h_1$, distinguisher $D$ takes these values from its input. Everything else is the same.
3. $D$ invokes $D_{\pi}$ on the joint output of $\mathcal{A}$ and the honest $P_2$  from this execution, and outputs whatever $D_{\pi}$ outputs.

The only difference between $\pi$ and $\pi'$ is how the values $g_1,h_0,h_1$ are chosen, we have
$$
Pr[D(\mathbb{G},q,g_0,g_1,(g_0)^r,(g_1)^r)=1] = Pr[D_{\pi}(REAL_{\pi',\mathcal{A}(z)}((x_0,x_1),\sigma,n))=1]
$$
and
$$
Pr[D(\mathbb{G},q,g_0,g_1,(g_0)^r,(g_1)^{r+1})=1] = Pr[D_{\pi}(REAL_{\pi,\mathcal{A}(z)}((x_0,x_1),\sigma,n))=1]
$$
Thus,
$$
|Pr[D(\mathbb{G},q,g_0,g_1,(g_0)^r,(g_1)^r)=1]-Pr[D(\mathbb{G},q,g_0,g_1,(g_0)^r,(g_1)^{r+1})=1]| \geq \frac{1}{p(n)}.
$$
in contradiction to the DDH assumption.

Next, we prove that for every $\mathcal{A}$ controlling $P_1$:
$$
\left\{ REAL_{\pi',\mathcal{A}}((x_0,x_1),\sigma,n) \right\}_{x_0,x_1,\sigma \in \{0,1\}^{\star};n \in \mathbb{N}} \equiv \left\{ IDEAL_{f_{ot},\mathcal{S}} ((x_0,x_1),\sigma,n) \right\}_{x_0,x_1,\sigma \in \{0,1\}^{\star};n \in \mathbb{N}}
$$
There are two differences between the description of $\pi'$ and an ideal execution with $\mathcal{S}$:

1. In an ideal execution, the pair $(g,h)$ in the view of $\mathcal{A}$ is generated by computing $(g_0)^r$ and $(h_0)^r$. In contrast, in $\pi'$, these values are generated by $P_2$ computing $(g_\sigma)^r$ and $(h_{\sigma})^r$.
2. In an ideal execution, the honest $P_2$'s outputs are determined by the trusted party, based on $(x_0,x_1)$ sent by $\mathcal{S}$ and its input $\sigma$ (unknown to $\mathcal{S}$).  In contrast, in $\pi$, the honest $P_2$'s output is determined by the protocol instructions.

For the first difference, when $\sigma=0$ then this is immediate. 

When $\sigma=1$, $g_1=(g_0)^y$, $h_1=(h_0)^y$. Thus, $(g_0)^r=(g_1)^{r \cdot y^{-1} \mod{q}}$ and $(h_0)^r=(h_1)^{r \cdot y^{-1} \mod{q}}$. The values $r$ and $r \cdot y^{-1} \mod{q}$ are both uniformly distributed. Therefore, $((g_{\sigma})^r,(h_{\sigma})^r)$ as generated in $\pi'$ is identically distributed to $((g_0)^r,(h_0)^r)$ as generated by $\mathcal{S}$ in an ideal execution.

For the second difference, it suffices to show that the values $(x_0,x_1)$ sent by $\mathcal{S}$ to the trust party computing $f_{ot}$ are the exact outputs that $P_2$ receives in $\pi'$ on that transcript. There are two cases:

- $P_2$ in $\pi'$ has input $\sigma=0$: in both $\pi'$ and the ideal execution with $\mathcal{S}$ we have that $g=(g_0)^r$ and $h=(h_0)^r$. 

  - In $\pi'$, party $P_2$ outputs $x_0=w_0/(u_0)^r$. 
  - In an ideal execution with $\mathcal{S}$, the value $x_0$ is defined by $\mathcal{S}$ to be $w_0/(u_0)^r$. 

  Thus, the value is identical.

- $P_2$ in $\pi'$ has input $\sigma=1$:

  -  In $\pi'$, $(g,h)=((g_1)^r,(h_1)^r)$ for a random $r$, and $P_2$'s output is obtained by computing $x_1=w_1/(u_1)^r$. 
  - In an ideal execution with $\mathcal{S}$, $(g,h)=((g_0)^r,(h_0)^r)$ for a random $r$. $P_2$'s output is defined by $\mathcal{S}$ to be $w_1/(u_1)^{r \cdot y^{-1} \mod{q}}$.

  Fix the messages $(g_1,h_0,h_1)$ and $(g,h)$ sent by $P_2$ to $\mathcal{A}$ in either $\pi'$ or in an ideal execution with $\mathcal{S}$. In both cases, there exists a unique $y$ such that $g_1=(g_0)^y$ and $h_1=(h_0)^y$. Let $k$ be the unique value such that $g=(g_1)^k$ and $h=(h_1)^k$.

  - In an execution of $\pi'$ the value $k$ is set to equal $r$ as chosen by $P_2$.
  - In an ideal execution with $\mathcal{S}$, the value $k$ is set to equal $r \cdot y^{-1} \mod{q}$ where $r$ is the value chosen by $\mathcal{S}$.

  In both a real execution of $\pi'$ and an ideal execution with $\mathcal{S}$, party $P_2$'s output is determined by $w_1/(u_1)^k$. Thus, the output is the same in both cases. 

------

$P_2$ is corrupted. The simulator $\mathcal{S}$ works as follows:

1. $\mathcal{S}$ internally invokes $\mathcal{A}$ controlling $P_2$.
2. $\mathcal{S}$ internally obtains $(g_1,h_0,h_1)$ from $\mathcal{A}$, as it intends to send to $P_1$.
3. $\mathcal{S}$ internally obtains an input tuple and $\alpha$ from $\mathcal{A}$, as it intends to send to $f_{zk}^{R_L}$.
4. $\mathcal{S}$ checks that the input tuple equals $(\mathbb{G},q,g_0,g_1,h_0,\frac{h_1}{g_1})$, that $h_0=(g_0)^{\alpha}$ and $\frac{h_1}{g_1}=(g_1)^{\alpha}$. If not, $\mathcal{S}$ externally sends $abort_2$ to the trusted party computing $f_{ot}$, outputs whatever $\mathcal{A}$ outputs, and halts. Else, it preceeds.
5. $\mathcal{S}$ internally obtains a pair $(g,h)$ from $P_2$. If $h=g^{\alpha}$ then $\mathcal{S}$ sets $\sigma=0$. Otherwise, it sets $\sigma=1$.
6. $\mathcal{S}$ externally sends $\sigma$ to the trusted party computing $f_{ot}$ and receives back $x_{\sigma}$.
7. $\mathcal{S}$ computes $(u_{\sigma},v_{\sigma})=RAND(g_{\sigma},g,h_{\sigma},h)$ and $w_{\sigma}=v_{\sigma} \cdot x_{\sigma}$. In addition, $\mathcal{S}$ sets $(u_{1-\sigma},w_{1-\sigma})$ to be independent uniformly distributed in $\mathbb{G}^2$.
8. $\mathcal{S}$ internally hands $(u_0,w_0),(u_1,w_1)$ to $\mathcal{A}$.
9. $\mathcal{S}$ outputs whatever $\mathcal{A}$ outputs and halts.

We construct an alternative simulator $\mathcal{S}'$ in an alternative ideal model with a trusted party who sends both of $P_1$'s inputs $x_0,x_1$ to $\mathcal{S}'$ upon receiving $\sigma$. $\mathcal{S}'$ computes $(u_{1-\sigma},w_{1-\sigma})$ by first computing $(u_{1-\sigma},v_{1-\sigma})=RAND(g_{1-\sigma},g,h_{1-\sigma},h)$ and $w_{1-\sigma}=v_{1-\sigma} \cdot x_{1-\sigma}$, instead of choosing them uniformly. 

First, we claim the output distribution of the adversary $\mathcal{S}'$ in the alternative ideal model is identical to the output of the adversary $\mathcal{A}$ in a real execution with an honest $P_1$. Because $\mathcal{S}'$ generates $(u_0,w_0)$ and $(u_1,w_1)$ exactly like an honest $P_1$, using the correct inputs $(x_0,x_1)$. In addition, $\mathcal{S}'$ verifies the validity of $(g_1,h_0,h_1)$ using witness $\alpha$, exactly like $f_{zk}^{R_L}$. Thus, the result is just a real execution of the protocol.

Now, we claim that the output distribution of the adversary $\mathcal{S}'$ in the alternative ideal model is identical to the output of the adversary $\mathcal{S}$ in an ideal execution with $f_{ot}$. We need to argue that the values $(u_{1-\sigma},w_{1-\sigma})$ generated by $\mathcal{S}'$ are independent uniformly distributed values in $\mathbb{G}$.

- $\sigma=0$, by step 5, we have $h=g^\alpha$, and by step $4$, we have $h_1=g_1^{\alpha+1}$. This implies that $(g_1,g,h_1,h)=(g_1,g,(g_1)^{\alpha+1},g^{\alpha})$ which is not a Diffie-Hellman tuple, it follows that $(u_1,v_1)=RAND(g_1,g,h_1,h)$ is uniformly distributed in $\mathbb{G}^2$, so $(u_1,w_1)=(u_1,v_1 \cdot x_1)$ is uniformly distributed. Thus, $(u_1,w_1)$ are identically distributed in the execution with $\mathcal{S}$ and $\mathcal{S}'$.
- $\sigma=1$, by step 5, we have $h \neq g^{\alpha}$, let $\alpha' \neq \alpha \mod{q}$ s.t. $h=g^{\alpha'}$. By step 4, we have $h_0 = (g_0)^\alpha$ and so $(g_0,g,h_0,h)=(g_0,g,(g_0)^{\alpha},g^{\alpha'})$ where $\alpha' \neq \alpha \mod{q}$. thus, it is not a Diffie-Hellman tuple.  As in the previous claim, $(u_0,w_0)$ is uniformly distributed and so has the same distribution of $\mathcal{S}$. 

This complete the proof.
