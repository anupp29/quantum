# Advanced Quantum Computing — Complete Viva Q&A
### K.K. Wagh Institute | Dr. Uday Wad | All Units + All Practicals

> **How to use this:** Every single question your examiner can ask is here. Formulas are inline. Answers are short and direct. Read the concept, then revise the Q&A. You've got this.

---

# TABLE OF CONTENTS
1. [Unit I — Grover's Search Algorithm](#unit-i--grovers-search-algorithm)
2. [Unit I — Shor's Factoring Algorithm](#unit-i--shors-factoring-algorithm)
3. [Unit I — Quantum Fourier Transform (QFT)](#unit-i--quantum-fourier-transform-qft)
4. [Unit II — Quantum Teleportation](#unit-ii--quantum-teleportation)
5. [Unit II — Superdense Coding](#unit-ii--superdense-coding)
6. [Unit II — QKD / BB84](#unit-ii--quantum-key-distribution-qkd--bb84)
7. [Unit III — BQP & Complexity Classes](#unit-iii--bqp--complexity-theory)
8. [Unit IV — Quantum Hardware](#unit-iv--quantum-hardware)
9. [Unit IV — Noise & Error Correction](#unit-iv--noise--error-correction)
10. [Unit V — VQE](#unit-v--variational-quantum-eigensolver-vqe)
11. [Unit V — QAOA](#unit-v--quantum-approximate-optimization-algorithm-qaoa)
12. [Practical 1 — Grover's (Qiskit)](#practical-1--grovers-algorithm-qiskit)
13. [Practical 2 — Shor's (Qiskit)](#practical-2--shors-algorithm-qiskit)
14. [Practical 3 — Teleportation & QKD (Qiskit)](#practical-3--teleportation--qkd-qiskit)
15. [Practical 4 — VQE (Qiskit)](#practical-4--vqe-qiskit)
16. [Practical 5 — Noise & Error Mitigation (Qiskit)](#practical-5--noise--error-mitigation-qiskit)
17. [Practical 6 — QAOA (Qiskit)](#practical-6--qaoa-qiskit)
18. [Common Traps — Don't Get Fooled](#common-traps--dont-get-fooled)
19. [Master Cheat Sheet](#master-cheat-sheet)

---

---

# UNIT I — GROVER'S SEARCH ALGORITHM

## 🔵 The Big Idea

You have **N unsorted items**. Find the one target item.
- Classical: check one by one → $O(N)$ steps
- Grover's: use quantum superposition + amplitude amplification → $O(\sqrt{N})$ steps
- That's a **quadratic speedup** (NOT exponential — a very common mistake!)

**Example:** $N = 1{,}000{,}000$ items → Classical needs ~500,000 tries → Grover's needs ~1,000 tries

---

## 📋 The Algorithm Step by Step

**Step 1 — Initialize:** All $n$ qubits start as $|0\rangle$

**Step 2 — Superposition:** Apply Hadamard to all qubits:
$$H^{\otimes n}|0\rangle^n = \frac{1}{\sqrt{N}}\sum_{x=0}^{N-1}|x\rangle$$
Every state now has equal amplitude $\frac{1}{\sqrt{N}}$

**Step 3 — Oracle (mark the target):**
$$U_f|x\rangle = \begin{cases} -|x\rangle & \text{if } x = x^* \\ +|x\rangle & \text{otherwise} \end{cases}$$
The target's amplitude becomes negative. You can't see this yet — measurement can't detect sign directly.

**Step 4 — Diffusion (amplify the target):**
$$D = 2|\psi\rangle\langle\psi| - I$$
Flips all amplitudes about their average. The negative amplitude shoots upward; others shrink.

**Step 5 — Repeat** Oracle + Diffusion for $R$ iterations:
$$R \approx \frac{\pi}{4}\sqrt{\frac{N}{M}}$$
where $M$ = number of target states.

**Step 6 — Measure:** Get the correct answer with near-100% probability.

---

## 📊 Numerical Example ($N=4$, target $= |11\rangle$)

| Step | $|00\rangle$ | $|01\rangle$ | $|10\rangle$ | $|11\rangle$ |
|------|----------|----------|----------|----------|
| Initial | +0.5 | +0.5 | +0.5 | +0.5 |
| After Oracle | +0.5 | +0.5 | +0.5 | **-0.5** |
| Average = 0.25 | | | | |
| After Diffusion | 0.0 | 0.0 | 0.0 | **+1.0** |

Probability of measuring $|11\rangle$ = $1.0^2 = 100\%$ ✅ (just 1 iteration!)

---

## ❓ VIVA QUESTIONS — GROVER'S

**Q: What is Grover's algorithm and what problem does it solve?**
> It is a quantum search algorithm that finds a target item in an unsorted list of $N$ items in $O(\sqrt{N})$ steps instead of the classical $O(N)$ steps. It gives a **quadratic speedup**.

---

**Q: What is the Oracle in Grover's algorithm?**
> The oracle is a quantum gate that marks the target state by flipping its phase (amplitude sign). It implements the condition $f(x) = 1$ if $x = x^*$, and 0 otherwise. Mathematically: $U_f|x\rangle = (-1)^{f(x)}|x\rangle$

---

**Q: What is the Diffusion operator?**
> It is the operator $D = 2|\psi\rangle\langle\psi| - I$. It reflects all amplitudes about their mean value. It converts the phase flip (done by oracle) into an actual increase in probability for the target state.

---

**Q: How many iterations does Grover's need?**
> $$R \approx \frac{\pi}{4}\sqrt{\frac{N}{M}}$$
> For $N=4, M=1$: **1 iteration**. For $N=16$: **2 iterations**. For $N=100$: ~8 iterations. Running too many makes probability decrease again.

---

**Q: What happens if you run MORE iterations than optimal?**
> The probability of the correct answer **starts decreasing again**. The amplitude overshoots. The algorithm needs exactly $\approx \frac{\pi}{4}\sqrt{N}$ iterations to peak.

---

**Q: Does Grover's give exponential speedup?**
> **NO.** Grover's gives **quadratic** speedup: $\sqrt{N}$ vs $N$. Shor's algorithm gives exponential speedup over classical factoring. This distinction is very important.

---

**Q: What is amplitude amplification?**
> The process of repeatedly increasing the probability amplitude of the correct (target) state while decreasing all others. Grover's achieves this by alternating Oracle (marks target) and Diffusion (inverts about mean).

---

**Q: What is phase kickback?**
> When the oracle flips the sign of the target state's amplitude making it negative: $-|x^*\rangle$. Measurement cannot detect phase directly, so diffusion is needed to convert this into measurable probability.

---

**Q: What is "invert about the mean"?**
> The diffusion step. New amplitude of each state = $2\bar{a} - a_i$ where $\bar{a}$ is the average amplitude. If one amplitude is below average (negative), this makes it jump above average (large positive).

---

**Q: What is the Hadamard gate's role in Grover's?**
> $H^{\otimes n}$ creates **uniform superposition** — all $N$ states have equal probability $\frac{1}{N}$. This is the starting point. Also used in the diffusion operator: $D = H^{\otimes n}(2|0\rangle\langle 0| - I)H^{\otimes n}$

---

**Q: What is a "marked state"?**
> The target state $|x^*\rangle$ that the oracle identifies as the answer. It is marked by phase flip. In a database, it is the item satisfying the search condition.

---

**Q: Can Grover's algorithm solve NP-complete problems efficiently?**
> Not really. NP-complete problems need exponential speedup to become polynomial. Grover's only gives quadratic speedup, which reduces exponential to a smaller exponential — still infeasible for large inputs.

---

**Q: What is $M$ in the iteration formula?**
> $M$ is the number of marked (target) states. With more targets, fewer iterations are needed: $R \approx \frac{\pi}{4}\sqrt{N/M}$

---

**Q: Write the formula for the initial equal superposition state.**
> $$|\psi\rangle = H^{\otimes n}|0\rangle^n = \frac{1}{\sqrt{N}}\sum_{x=0}^{N-1}|x\rangle$$

---

**Q: Why can't we just measure the marked state directly?**
> Because before any amplification, the marked state has the same probability $\frac{1}{N}$ as all other states. Measuring randomly gives the wrong answer $\frac{N-1}{N}$ of the time.

---

**Q: What is the probability of success after $R$ optimal iterations?**
> Close to 1 (100%). Exactly: $\sin^2\left((2R+1)\arcsin\frac{1}{\sqrt{N}}\right) \approx 1$ for optimal $R$.

---

**Q: Grover's speedup for $N = 1{,}000{,}000$?**
> Classical: up to 1,000,000 steps. Grover's: $\sqrt{1{,}000{,}000} = 1000$ steps. Speedup factor: **1000×**.

---
---

# UNIT I — SHOR'S FACTORING ALGORITHM

## 🔵 The Big Idea

RSA encryption (used to secure the entire internet) relies on the fact that **multiplying two large primes is easy, but factoring their product is hard** for classical computers.

Shor's algorithm factors a number $N$ in **polynomial time** $O(n^3)$ using a quantum computer (where $n = \log_2 N$ is the number of bits). This would break RSA if run on a large enough quantum computer.

---

## 📋 The Algorithm — 5 Steps

**Step 1 (Classical):** Check trivial cases — is $N$ even? Is $N$ a prime power? If yes, trivially factor it.

**Step 2 (Classical):** Pick random $a$ where $1 < a < N$. Compute $\gcd(a, N)$:
- If $\gcd(a,N) \neq 1$ → lucky! You found a factor. Done.
- If $\gcd(a,N) = 1$ → continue to quantum step.

**Step 3 (QUANTUM — the hard part):** Find period $r$ of $f(x) = a^x \bmod N$

The period $r$ is the smallest positive integer where:
$$a^r \equiv 1 \pmod{N}$$
The quantum computer does this using superposition + controlled modular exponentiation + QFT.

**Step 4 (Classical):** Check if $r$ is even. If odd → go back to Step 2.
$$\text{Factor 1} = \gcd(a^{r/2} - 1,\ N)$$
$$\text{Factor 2} = \gcd(a^{r/2} + 1,\ N)$$

**Step 5 (Classical):** Verify Factor 1 $\times$ Factor 2 $= N$.

---

## 🔢 Full Example: Factor $N = 15$ with $a = 7$

**Period finding:**
| $x$ | $7^x \bmod 15$ |
|-----|----------------|
| 1 | 7 |
| 2 | 4 |
| 3 | 13 |
| 4 | **1** ← period found! |

**Period $r = 4$** (even ✅)

$$a^{r/2} = 7^2 \bmod 15 = 49 \bmod 15 = 4$$

$$\text{Factor 1} = \gcd(4-1,\ 15) = \gcd(3, 15) = \mathbf{3}$$

$$\text{Factor 2} = \gcd(4+1,\ 15) = \gcd(5, 15) = \mathbf{5}$$

**Check:** $3 \times 5 = 15$ ✅

---

## 📊 Complexity Comparison

| Method | Time Complexity | Practical? |
|--------|----------------|------------|
| Classical brute force | $O(\sqrt{N})$ trial division | No for large N |
| Classical best (GNFS) | $O(e^{n^{1/3} (\log n)^{2/3}})$ | No for 2048-bit keys |
| Shor's quantum | $O(n^3)$ | Yes (polynomial!) |

---

## ❓ VIVA QUESTIONS — SHOR'S

**Q: What does Shor's algorithm do?**
> Factors a large composite number $N = p \times q$ in polynomial time $O(n^3)$ using a quantum computer, where $n = \log_2 N$ is the number of bits.

---

**Q: Why is Shor's algorithm important?**
> It can break RSA encryption. RSA security relies on factoring being computationally hard classically. Shor's does it in polynomial time, making RSA vulnerable to quantum attacks.

---

**Q: What is period finding?**
> Finding the smallest $r$ such that $a^r \equiv 1 \pmod{N}$. The function $f(x) = a^x \bmod N$ is periodic with period $r$. The quantum computer finds this period efficiently.

---

**Q: What is the role of QFT in Shor's?**
> After controlled modular exponentiation, the counting register has phase patterns encoding the period. The **inverse QFT** converts these phase patterns into peaks at multiples of $N/r$, allowing $r$ to be extracted.

---

**Q: Factor 15 using Shor's with $a = 7$.**
> Period: $7^4 \equiv 1 \pmod{15}$, so $r=4$. Then $7^2 \bmod 15 = 4$. $\gcd(3,15) = 3$ and $\gcd(5,15) = 5$. **Factors: 3 and 5.**

---

**Q: Why must $r$ be even?**
> Because we compute $a^{r/2}$, which requires $r/2$ to be an integer. If $r$ is odd, the formula doesn't work and we pick a different $a$ and retry.

---

**Q: What is modular exponentiation?**
> Computing $a^x \bmod N$ — raise $a$ to power $x$, take remainder divided by $N$. This function is periodic with period $r$. The quantum computer evaluates it for ALL $x$ simultaneously using superposition.

---

**Q: What is continued fractions and why is it used?**
> After measuring the quantum circuit, we get a decimal number close to $s/r$ for some integer $s$. Continued fractions finds the exact rational approximation, giving us $r$ precisely.

---

**Q: What are the quantum and classical parts of Shor's?**
> **Quantum:** Period finding using superposition + controlled modular exponentiation + inverse QFT.
> **Classical:** GCD computations, choosing random $a$, post-processing factors.

---

**Q: What is the speedup of Shor's over classical?**
> Classical best: sub-exponential $O(e^{n^{1/3}})$. Shor's: polynomial $O(n^3)$. This is an **exponential speedup**.

---

**Q: Why is classical factoring hard?**
> No polynomial-time classical algorithm is known. The best classical algorithm (Number Field Sieve) still runs in sub-exponential time — completely infeasible for 2048-bit RSA keys.

---

**Q: What is $\gcd$ and how is it used?**
> Greatest Common Divisor. $\gcd(a,b)$ = largest number dividing both $a$ and $b$. Used to extract factors: $p = \gcd(a^{r/2}-1, N)$. Computed efficiently using Euclidean algorithm in classical post-processing.

---

**Q: What two registers does Shor's circuit use?**
> 1. **Counting register** ($n$ qubits): superposition over all values of $x$. 2. **Work register**: stores $a^x \bmod N$ for each $x$ in superposition. After QFT on counting register, peaks encode $r$.

---

**Q: What happens in the quantum circuit of Shor's step by step?**
> 1. Apply $H^{\otimes n}$ to counting register (superposition).
> 2. Apply controlled modular exponentiation: $|x\rangle|0\rangle \to |x\rangle|a^x \bmod N\rangle$.
> 3. Apply inverse QFT to counting register.
> 4. Measure counting register → peaks at multiples of $N/r$.
> 5. Use continued fractions to extract $r$.

---
---

# UNIT I — QUANTUM FOURIER TRANSFORM (QFT)

## 🔵 The Big Idea

Classical Fourier Transform: converts a time-domain signal into frequency domain. QFT does the **same thing on quantum amplitudes**. It is a change of basis from computational basis ($|0\rangle$, $|1\rangle$) to Fourier basis ($|+\rangle$, $|-\rangle$, etc.).

Used **inside Shor's algorithm** to extract the period from the quantum state.

---

## 📐 The Formula

**Classical DFT:**
$$X_k = \sum_{j=0}^{N-1} x_j \cdot e^{2\pi i jk/N}$$

**QFT:**
$$\text{QFT}|x\rangle = \frac{1}{\sqrt{N}}\sum_{y=0}^{N-1} e^{2\pi i xy/N}|y\rangle$$

Same formula, but acts on quantum state amplitudes instead of classical signal values.

**Inverse QFT (used in Shor's):**
$$\text{QFT}^\dagger|y\rangle = \frac{1}{\sqrt{N}}\sum_{x=0}^{N-1} e^{-2\pi i xy/N}|x\rangle$$

---

## ⚙️ Product Form (Why it's Efficient)

QFT factors into independent single-qubit operations:
$$\text{QFT}|x\rangle = \frac{1}{\sqrt{N}}\Big(|0\rangle + e^{2\pi ix/2}|1\rangle\Big) \otimes \Big(|0\rangle + e^{2\pi ix/4}|1\rangle\Big) \otimes \cdots \otimes \Big(|0\rangle + e^{2\pi ix/2^n}|1\rangle\Big)$$

This factorization → only $O(n^2)$ gates needed!

---

## 🔧 The $U_{ROT_k}$ Gate

$$U_{ROT_k} = \begin{pmatrix}1 & 0 \\ 0 & e^{2\pi i/2^k}\end{pmatrix}$$

- $k=1$: Z gate (rotation by $\pi$)
- $k=2$: S gate (rotation by $\pi/2$)
- $k=3$: T gate (rotation by $\pi/4$)

---

## 📊 Complexity

| Algorithm | Complexity | In bits ($n = \log_2 N$) |
|-----------|------------|--------------------------|
| Classical DFT | $O(N^2)$ | $O(2^{2n})$ |
| Classical FFT | $O(N \log N)$ | $O(n \cdot 2^n)$ |
| **QFT** | $O(n^2)$ | **Polynomial!** |

---

## ❓ VIVA QUESTIONS — QFT

**Q: What is QFT in simple words?**
> QFT applies the Fourier Transform to quantum amplitudes. It changes the basis from computational ($|0\rangle/|1\rangle$) to Fourier basis. It reveals the frequency/phase content of quantum states.

---

**Q: Write the QFT formula.**
> $$\text{QFT}|x\rangle = \frac{1}{\sqrt{N}}\sum_{y=0}^{N-1} e^{2\pi i xy/N}|y\rangle$$

---

**Q: What is the gate complexity of QFT?**
> $O(n^2)$ gates for $n$ qubits. Classical FFT needs $O(N \log N) = O(n \cdot 2^n)$. QFT is **exponentially faster**.

---

**Q: How is QFT used in Shor's algorithm?**
> After controlled modular exponentiation, the counting register has periodic phase patterns. **Inverse QFT** converts these phases into measurable peaks at multiples of $N/r$, allowing extraction of the period $r$.

---

**Q: What is the $U_{ROT_k}$ gate?**
> A phase rotation gate: $\begin{pmatrix}1 & 0 \\ 0 & e^{2\pi i/2^k}\end{pmatrix}$. Rotates $|1\rangle$ by phase $e^{2\pi i/2^k}$, leaves $|0\rangle$ unchanged. Used in QFT circuit for controlled phase rotations.

---

**Q: What is the Fourier basis?**
> States produced by QFT. For one qubit: $|+\rangle = \frac{1}{\sqrt{2}}(|0\rangle + |1\rangle)$ and $|-\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$ (equatorial states on Bloch sphere). These encode phase information.

---

**Q: Why are SWAP gates needed at the end of QFT?**
> The QFT circuit naturally produces output in reversed bit order. SWAP gates reverse the qubit ordering back to standard convention.

---

**Q: What is the inverse QFT?**
> QFT$^\dagger$ — same circuit but in reverse order with negative phase rotations. Formula: $\text{QFT}^\dagger|y\rangle = \frac{1}{\sqrt{N}}\sum_{x=0}^{N-1}e^{-2\pi ixy/N}|x\rangle$

---

**Q: Can we read QFT output directly?**
> No. QFT transforms amplitudes, but measurement collapses to one outcome per run. Information is extracted statistically across many measurements.

---

**Q: What is the product form of QFT and why does it matter?**
> QFT factors into tensor products of single-qubit states with phase rotations. This means QFT can be built from $n$ Hadamard gates and $\frac{n(n-1)}{2}$ controlled rotation gates — only $O(n^2)$ gates total, making it efficient.

---
---

# UNIT II — QUANTUM TELEPORTATION

## 🔵 The Big Idea

Alice has a qubit $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$. She doesn't know $\alpha$ and $\beta$. She wants Bob (far away) to have this exact state.

**She cannot:**
- Measure it → measurement destroys the state
- Copy it → **no-cloning theorem** forbids copying unknown quantum states
- Send the physical qubit → too fragile

**Solution:** Use 1 pre-shared entangled Bell pair + send 2 classical bits. The qubit state "teleports" — Alice's is destroyed, Bob's is created.

---

## 🧲 Two Essential Theorems

**No-Cloning Theorem:**
> There is no unitary $U$ such that $U|\psi\rangle|0\rangle = |\psi\rangle|\psi\rangle$ for all unknown $|\psi\rangle$. You cannot silently copy a quantum state.

**No-Teleportation Theorem:**
> You cannot convert a quantum state to classical bits and reconstruct it perfectly. Quantum entanglement is a necessary resource.

---

## 🔔 The Four Bell States

$$|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$$

$$|\Phi^-\rangle = \frac{1}{\sqrt{2}}(|00\rangle - |11\rangle)$$

$$|\Psi^+\rangle = \frac{1}{\sqrt{2}}(|01\rangle + |10\rangle)$$

$$|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle)$$

Teleportation uses $|\Phi^+\rangle$. Created by: $H$ on qubit 1, then CNOT (qubit 1 → qubit 2).

---

## 📋 The 4-Step Protocol (CHEN)

**C — Create Bell pair:** Telamon creates $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$. Gives qubit 1 to Alice, qubit 2 to Bob.

**H — Hadamard + CNOT by Alice:**
- Alice applies CNOT: her $\psi$-qubit (control) → her Bell qubit (target)
- Alice applies $H$ to her $\psi$-qubit
- System is now in entangled 3-qubit state

**E — Extract (measure) + send 2 bits:**
- Alice measures her 2 qubits → gets $|00\rangle$, $|01\rangle$, $|10\rangle$, or $|11\rangle$ (each with probability $\frac{1}{4}$)
- Sends 2 classical bits to Bob

**N — Now Bob corrects:**

| Alice sends | Bob's state | Bob applies |
|-------------|-------------|-------------|
| 00 | $\alpha|0\rangle + \beta|1\rangle$ | Nothing (already $|\psi\rangle$!) |
| 01 | $\alpha|1\rangle + \beta|0\rangle$ | $X$ gate (bit flip) |
| 10 | $\alpha|0\rangle - \beta|1\rangle$ | $Z$ gate (phase flip) |
| 11 | $\alpha|1\rangle - \beta|0\rangle$ | $ZX$ gate (both) |

After correction, Bob holds exactly $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$ ✅

---

## 🔢 Resource count

$$1 \text{ Bell pair} + 2 \text{ classical bits} \longrightarrow \text{Transfer 1 qubit state}$$

---

## ❓ VIVA QUESTIONS — TELEPORTATION

**Q: What is quantum teleportation?**
> Transfer of an unknown quantum state from Alice to Bob using a pre-shared entangled Bell pair and 2 classical bits, without physically moving the qubit.

---

**Q: What resources are needed?**
> One pre-shared Bell pair (entangled) + 2 classical bits sent via any classical channel (phone, internet, etc.).

---

**Q: Does teleportation copy the qubit?**
> **NO.** Alice's qubit is destroyed by measurement. Only one copy ever exists at any time. No-cloning is not violated.

---

**Q: Does teleportation allow faster-than-light communication?**
> **NO.** Bob's qubit is in a useless random state until he receives Alice's 2 classical bits. Those travel at most at the speed of light.

---

**Q: What is the no-cloning theorem?**
> There is no unitary operation $U$ such that $U|\psi\rangle|0\rangle = |\psi\rangle|\psi\rangle$ for all $|\psi\rangle$. Copying an unknown quantum state is physically impossible.

---

**Q: What gate does Bob apply for each 2-bit message?**
> - $00$ → $I$ (do nothing, already $|\psi\rangle$)
> - $01$ → $X$ gate (bit flip)
> - $10$ → $Z$ gate (phase flip)
> - $11$ → $ZX$ gate (apply $X$ first, then $Z$)

---

**Q: Who is Telamon?**
> A trusted third party who creates the Bell pair $|\Phi^+\rangle$ and distributes one qubit to Alice and one to Bob **before** communication starts.

---

**Q: What does CNOT do in Alice's step?**
> It entangles Alice's $\psi$-qubit with her Bell qubit. This creates the quantum correlations that allow Bob's qubit to be steered into the right state after Alice's measurement.

---

**Q: Why apply Hadamard after CNOT?**
> The Hadamard rotates Alice's $\psi$-qubit so that her measurement result (00/01/10/11) uniquely determines what correction Bob needs. Without it, the 4 outcomes don't properly encode the correction.

---

**Q: What are the 4 Bell states?**
> $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle+|11\rangle)$, $|\Phi^-\rangle = \frac{1}{\sqrt{2}}(|00\rangle-|11\rangle)$, $|\Psi^+\rangle = \frac{1}{\sqrt{2}}(|01\rangle+|10\rangle)$, $|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle-|10\rangle)$

---

**Q: What is Bob's state after Alice measures 10?**
> $\alpha|0\rangle - \beta|1\rangle$ (phase flipped). Bob applies $Z$ gate to get $\alpha|0\rangle + \beta|1\rangle = |\psi\rangle$.

---

**Q: Can teleportation work without entanglement?**
> **No.** Entanglement is the essential quantum resource that links Alice and Bob's qubits. Without it, no classical communication alone can transfer a quantum state.

---

**Q: What is the no-teleportation theorem?**
> You cannot convert a quantum state into classical bits, send them classically, and reconstruct the original quantum state. Quantum entanglement is required as an additional non-classical resource.

---

**Q: What is the initial 3-qubit state in teleportation?**
> $$|\psi\rangle \otimes |\Phi^+\rangle = (\alpha|0\rangle + \beta|1\rangle) \otimes \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$$
> $= \frac{1}{\sqrt{2}}(\alpha|000\rangle + \alpha|011\rangle + \beta|100\rangle + \beta|111\rangle)$

---

**Q: How is teleportation used practically?**
> In quantum networks and quantum internet for transferring quantum information between distant quantum computers, enabling distributed quantum computing.

---
---

# UNIT II — SUPERDENSE CODING

## 🔵 The Big Idea

Alice wants to send Bob **2 classical bits** but physically sends only **1 qubit**. They pre-share an entangled pair. Alice applies **one gate** to her qubit to encode 2 bits. Bob performs Bell measurement to decode.

**Teleportation vs Superdense Coding — they are EXACT DUALS:**

| | Teleportation | Superdense Coding |
|-|---------------|-------------------|
| **Sends** | 1 qubit state | 2 classical bits |
| **Uses** | 1 Bell pair + 2 classical bits | 1 Bell pair + 1 qubit |

Same resources, swapped roles!

---

## 📋 The Protocol

Pre-shared: $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$. Alice holds qubit 1.

**Alice's encoding:**

| Bits to send | Gate Alice applies | Bell state produced |
|---|---|---|
| 00 | $I$ (do nothing) | $|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle+|11\rangle)$ |
| 01 | $X$ (bit flip) | $|\Psi^+\rangle = \frac{1}{\sqrt{2}}(|01\rangle+|10\rangle)$ |
| 10 | $Z$ (phase flip) | $|\Phi^-\rangle = \frac{1}{\sqrt{2}}(|00\rangle-|11\rangle)$ |
| 11 | $ZX$ (X then Z) | $|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle-|10\rangle)$ |

Alice sends her qubit to Bob.

**Bob's decoding:** Bell measurement:
1. CNOT (Alice's qubit as control, his qubit as target)
2. Hadamard on first qubit
3. Measure → gets 2 bits directly

---

## ❓ VIVA QUESTIONS — SUPERDENSE CODING

**Q: What is superdense coding?**
> A protocol where Alice sends **2 classical bits** to Bob by physically sending only **1 qubit**, using a pre-shared entangled pair.

---

**Q: What gate sends "10"?**
> $Z$ gate (phase flip). The Bell pair becomes $|\Phi^-\rangle = \frac{1}{\sqrt{2}}(|00\rangle - |11\rangle)$.

---

**Q: What gate sends "11"?**
> $ZX$ gate — apply $X$ first (bit flip), then $Z$ (phase flip). The Bell pair becomes $|\Psi^-\rangle$.

---

**Q: What gate sends "01"?**
> $X$ gate (bit flip). The Bell pair becomes $|\Psi^+\rangle = \frac{1}{\sqrt{2}}(|01\rangle + |10\rangle)$.

---

**Q: How does Bob decode?**
> Bell measurement: CNOT with Alice's qubit as control, then $H$ on first qubit, then measure in computational basis. The 4 Bell states give 4 distinct 2-bit outcomes.

---

**Q: How is superdense coding dual to teleportation?**
> Teleportation uses **2 classical bits** to transfer **1 qubit state**. Superdense coding uses **1 qubit** to send **2 classical bits**. Same resources, exactly swapped.

---

**Q: Why can 1 qubit carry 2 bits?**
> Because of pre-shared entanglement. The 4 Bell states are perfectly orthogonal and distinguishable. Alice's one local gate selects which Bell state the pair is in, encoding 2 bits of information.

---

**Q: What if they don't have the entangled pair?**
> Superdense coding is impossible. Without entanglement, 1 qubit can only carry 1 classical bit of information (Holevo bound).

---

**Q: Memory trick for Alice's gates?**
> - 00 → I (Identity — do nothing)
> - 01 → X (eXchange the bits)
> - 10 → Z (Zero phase)
> - 11 → ZX (both)

---

**Q: What is Bell measurement?**
> A measurement that identifies which of the 4 Bell states a 2-qubit system is in. Performed by: CNOT → Hadamard → measure in computational basis.

---
---

# UNIT II — QUANTUM KEY DISTRIBUTION (QKD) / BB84

## 🔵 The Big Idea

Alice and Bob want a **shared secret key** to encrypt messages. Classical encryption (RSA) is secure because math is hard. QKD is secure because of **laws of physics** — even with unlimited computing power, Eve cannot intercept without being detected.

**Named after inventors:** Charles **B**ennett and Gilles **B**rassard, **1984** → BB84.

---

## ⚛️ 3 Physics Principles Making QKD Work

1. **Superposition:** Photons can be polarized in multiple bases. Alice can encode bits using different polarization bases.

2. **No-Cloning Theorem:** Eve cannot silently copy Alice's photons. She must measure them, which destroys the original state.

3. **Measurement Disturbance:** Any measurement of a quantum system disturbs it. Eve's interception changes the photons → Alice and Bob detect this.

---

## 📐 BB84 Encoding Scheme

| Basis | Symbol | Bit 0 | Bit 1 |
|-------|--------|-------|-------|
| Rectilinear | $+$ | $\|$ horizontal ($0°$) | $-$ vertical ($90°$) |
| Diagonal | $\times$ | ↗ diagonal ($45°$) | ↘ diagonal ($135°$) |

**Rule:** If Bob measures in **same basis** as Alice → correct bit **100%**. If **different basis** → random result (50% correct, 50% wrong).

---

## 📋 The 5 Phases of BB84

**Phase 1 — Alice sends:** Generates random bits with random bases. Encodes as polarized photons.

**Phase 2 — Bob measures:** Randomly picks a basis for each photon and measures.

**Phase 3 — Basis reconciliation:** Alice and Bob publicly compare **which basis** (not the bits) they used. Keep only bits where both used the **same basis**. Discard rest.
$$\text{Sifted key} \approx 50\% \text{ of original bits}$$

**Phase 4 — QBER check:**
$$\text{QBER} = \frac{\text{number of errors}}{\text{sifted key length}}$$
- QBER $\approx 0\%$ → no Eve → proceed
- QBER $> 11\%$ → Eve detected → abort and restart

**Why Eve causes 25% QBER:**
$$P(\text{error from Eve}) = P(\text{Eve wrong basis}) \times P(\text{Bob wrong}) = \frac{1}{2} \times \frac{1}{2} = 25\%$$

**Phase 5 — Key distillation:**
- **Error Correction:** Fix small errors from channel noise using classical codes
- **Privacy Amplification:** Hash the key to destroy any partial info Eve may have

---

## ❓ VIVA QUESTIONS — QKD / BB84

**Q: What is QKD?**
> Quantum Key Distribution — using quantum mechanics to share a secret key such that any eavesdropping is physically detectable regardless of Eve's computing power.

---

**Q: What makes BB84 secure?**
> Three quantum principles: superposition (multiple encoding bases), no-cloning (Eve can't silently copy photons), and measurement disturbance (Eve's measurement changes photon states, introducing detectable errors).

---

**Q: What is QBER?**
> Quantum Bit Error Rate = fraction of sifted key bits that differ between Alice and Bob. QBER $> 11\%$ implies an eavesdropper is present. Normal channel noise gives QBER $< 5\%$.

---

**Q: How much error does full Eve interception cause?**
> **25% QBER.**
> Eve guesses wrong basis $50\%$ of the time. When wrong, she collapses the photon to a random state. Bob then gets the wrong bit $50\%$ of those times.
> $$P(\text{error}) = \frac{1}{2} \times \frac{1}{2} = 25\%$$

---

**Q: What is the sifted key?**
> The subset of bits where Alice and Bob both used the **same basis**. On average **50%** of the original bits survive to become the sifted key.

---

**Q: What is privacy amplification?**
> Applying a hash function to the sifted key to produce a shorter final key. Even if Eve learned partial information during error correction, the hashed key is information-theoretically secret.

---

**Q: Who invented BB84 and when?**
> Charles Bennett and Gilles Brassard in **1984**. BB84 = Bennett-Brassard 1984.

---

**Q: How is QKD different from RSA?**
> RSA: secure because factoring is computationally hard (Shor's can break it). QKD: secure because of physical laws — impossible to intercept silently regardless of computing power (even quantum).

---

**Q: What are the two bases in BB84?**
> Rectilinear ($+$): $0° =$ bit 0, $90° =$ bit 1. Diagonal ($\times$): $45° =$ bit 0, $135° =$ bit 1.

---

**Q: Why discard mismatched-basis bits?**
> When Bob measures in a different basis from Alice, his result is random and carries no reliable information. Keeping those bits would add errors to the key.

---

**Q: What is the error threshold in BB84?**
> Approximately **11%**. Below this, error correction + privacy amplification can produce a provably secure key. Above 11%, abort.

---

**Q: Does BB84 use entanglement?**
> **NO.** BB84 does NOT need entanglement. It uses superposition and measurement disturbance. (The E91 QKD protocol uses entanglement — that's different.)

---

**Q: What is error correction in QKD?**
> Classical error-correcting codes (like Hamming codes) used to fix small discrepancies in Alice and Bob's sifted keys caused by channel noise (not Eve). Some key bits are revealed, which is why privacy amplification follows.

---

**Q: What is the Holevo bound?**
> The theoretical maximum classical information extractable from a quantum state. For $n$ qubits: at most $n$ classical bits. Eve cannot extract more than this even with unlimited quantum computers.

---
---

# UNIT III — BQP & COMPLEXITY THEORY

## 🔵 The Big Idea

Complexity theory classifies problems by how hard they are to solve. Quantum computers open a new class: **BQP**. The key question: does quantum computing solve ALL hard problems? Answer: not all — but some important ones.

---

## 📊 Classes Explained Simply

| Class | Meaning | Example |
|-------|---------|---------|
| **P** | Easy to SOLVE classically (polynomial time) | Sorting, shortest path |
| **NP** | Easy to VERIFY classically (may be hard to solve) | SAT, Sudoku, TSP |
| **BPP** | Easy to solve classically with coin flips (probabilistic) | Primality testing (old) |
| **BQP** | Easy to solve on quantum computer ($\geq \frac{2}{3}$ success, poly time) | Factoring (Shor's) |
| **QMA** | Quantum analogue of NP (verifiable with quantum proof) | Quantum SAT |

---

## 🔗 Relationships

$$P \subseteq BPP \subseteq BQP \subseteq PSPACE$$

- $P \subseteq BQP$: Quantum computers can do everything classical computers can
- Factoring ∈ BQP (Shor's) — but believed NOT ∈ P
- BQP and NP: **incomparable** — quantum doesn't solve all NP problems
- NP-complete problems: Grover's gives only quadratic speedup, not exponential

---

## 📊 Classical vs Quantum Comparison

| Problem | Classical | Quantum |
|---------|-----------|---------|
| Factoring | Sub-exp (not in P) | Poly (Shor's — in BQP) |
| Unstructured search | $O(N)$ | $O(\sqrt{N})$ Grover's |
| Quantum simulation | Exponential | Polynomial |
| NP-complete (SAT) | Exponential | No known poly solution |
| Sorting | $O(n \log n)$ | Same |

---

## ❓ VIVA QUESTIONS — COMPLEXITY

**Q: What is BQP?**
> Bounded-error Quantum Polynomial time. The set of decision problems solvable by a quantum computer in polynomial time with success probability $\geq \frac{2}{3}$.

---

**Q: What does "bounded error" mean?**
> The algorithm can fail, but with probability $\leq \frac{1}{3}$. By running it many times and taking majority vote, error can be made exponentially small.

---

**Q: How does BQP relate to P and NP?**
> $P \subseteq BQP$. BQP and NP are believed incomparable — quantum computers are stronger than classical for some problems (factoring) but not known to solve all NP problems efficiently.

---

**Q: Is factoring in NP?**
> Yes — verifying a factor takes polynomial time. It is also in BQP (Shor's). It is believed NOT to be in P classically.

---

**Q: Can quantum computers solve NP-complete problems?**
> Not known and generally believed unlikely. Grover's gives only quadratic speedup for NP problems — not enough to make them polynomial time.

---

**Q: What is QMA?**
> Quantum Merlin Arthur. Quantum analogue of NP. Problems whose solutions can be verified efficiently using a quantum verifier and a quantum "witness" (proof).

---

**Q: Give an example each of P, NP, BQP.**
> P: sorting. NP: Boolean satisfiability (SAT). BQP: integer factoring (via Shor's algorithm).

---
---

# UNIT IV — QUANTUM HARDWARE

## 🔵 The Big Idea

A qubit needs a physical 2-level quantum system. Two main implementations: **Superconducting qubits** (IBM, Google) and **Ion Traps** (IonQ, Honeywell). Each has trade-offs.

---

## ⚙️ Superconducting Qubits

- **What it is:** Josephson junction circuit cooled to ~15 mK. Energy levels = $|0\rangle$ and $|1\rangle$
- **Gate speed:** Very fast (~10–100 ns per gate)
- **Coherence time:** Short (~50–500 µs)
- **Connectivity:** Only nearest neighbors (2D grid)
- **Scalability:** Easier — use semiconductor chip manufacturing
- **Qubit count:** 1000+ (IBM, Google)
- **Used by:** IBM Quantum, Google Sycamore, Rigetti

## ⚙️ Ion Trap Qubits

- **What it is:** Individual charged atoms (ions) trapped by electromagnetic fields. Ion energy levels = $|0\rangle$ and $|1\rangle$
- **Gate speed:** Slow (~1–100 µs per gate)
- **Coherence time:** Very long (seconds to minutes!)
- **Connectivity:** All-to-all (any ion can interact with any other)
- **Scalability:** Hard to scale beyond ~50 qubits in one trap
- **Used by:** IonQ, Honeywell/Quantinuum

## 📊 Comparison Table

| Property | Superconducting | Ion Trap |
|----------|-----------------|---------|
| Gate speed | Fast (ns) | Slow (µs) |
| Coherence time | Short (ms) | Long (s–min) |
| Connectivity | Nearest-neighbor | All-to-all |
| Scalability | Easier | Harder |
| Temperature | 15 mK | Room temp (trap) |
| Current max qubits | 1000+ | ~20–50 |

---

## ❓ VIVA QUESTIONS — HARDWARE

**Q: What is a qubit physically?**
> A two-level quantum system. In superconducting: a Josephson junction circuit at 15 mK. In ion trap: energy levels of a trapped charged atom.

---

**Q: What is a Josephson junction?**
> A thin insulating layer between two superconductors. When cooled to near absolute zero, current can tunnel quantum mechanically, creating discrete energy levels used as qubit states.

---

**Q: Why do superconducting qubits need 15 mK?**
> Thermal energy at room temperature ($k_BT \approx 25$ meV) would swamp the tiny energy gap between $|0\rangle$ and $|1\rangle$ states (~5–10 GHz, or ~0.02 meV). At 15 mK, thermal energy is negligible.

---

**Q: What is the advantage of ion traps over superconducting?**
> Much longer coherence times (seconds vs milliseconds) and all-to-all connectivity. Disadvantage: much slower gate speed and harder to scale.

---

**Q: What is the advantage of superconducting over ion traps?**
> Much faster gate speed (nanoseconds vs microseconds) and easier to manufacture at scale. Already at 1000+ qubits.

---

**Q: What is all-to-all connectivity?**
> Any qubit can directly interact with any other qubit. Ion traps have this. Superconducting qubits have only nearest-neighbor connectivity — a qubit can only directly interact with its adjacent neighbors on the chip.

---
---

# UNIT IV — NOISE & ERROR CORRECTION

## 🔵 Types of Errors

- **Bit-flip:** $|0\rangle \leftrightarrow |1\rangle$ (like a classical bit error)
- **Phase-flip:** $|+\rangle \leftrightarrow |-\rangle$ (amplitude sign flips)
- **Combined:** both bit-flip and phase-flip
- **Decoherence:** Qubit loses quantum state due to environment interaction
- **Gate error:** Imperfect pulse → wrong gate applied
- **Readout error:** Measure $|0\rangle$ but record "1" (or vice versa)

---

## ⏱️ T1 and T2

- **$T_1$:** Energy relaxation time — time for qubit to decay from $|1\rangle$ to $|0\rangle$ naturally
- **$T_2$:** Dephasing time — time for phase of superposition to become random
- Always: $T_2 \leq 2T_1$

---

## 🛡️ Quantum Error Correction (QEC)

Encode **1 logical qubit** (error-free) into multiple **physical qubits** (noisy). Errors detected via **syndrome measurement** — ancilla qubits measure indirectly without collapsing data.

**Repetition Code (simplest):**
$$|0\rangle_L = |000\rangle, \quad |1\rangle_L = |111\rangle$$
3 physical qubits per logical qubit. Handles bit-flip only.

**Shor's 9-qubit Code:**
9 physical qubits per logical qubit. Handles ALL error types.

**Surface Code (most practical):**
- 2D grid of qubits
- Only nearest-neighbor interactions needed
- Error threshold: ~**1%**
- Most promising for real hardware

---

## 📊 QEC Codes Comparison

| Code | Physical qubits per logical | Errors corrected |
|------|----------------------------|------------------|
| Repetition | 3 | Bit-flip only |
| Shor's 9-qubit | 9 | All types |
| Surface code | ~1000 | All types |

---

## ❓ VIVA QUESTIONS — NOISE & QEC

**Q: What is decoherence?**
> The process by which a qubit loses its quantum state due to interaction with the environment (heat, vibrations, electromagnetic fields). The qubit "forgets" its superposition.

---

**Q: What is $T_1$ and $T_2$?**
> $T_1$: energy relaxation time — qubit decays from $|1\rangle$ to $|0\rangle$ naturally (amplitude damping). $T_2$: dephasing time — phase of superposition becomes random. Always $T_2 \leq 2T_1$.

---

**Q: What are the 3 types of quantum errors?**
> **Bit-flip** ($|0\rangle \leftrightarrow |1\rangle$), **phase-flip** ($|+\rangle \leftrightarrow |-\rangle$), **combined** (both happen simultaneously).

---

**Q: What is QEC?**
> Quantum Error Correction. Encoding 1 logical qubit into multiple physical qubits so errors can be detected and corrected without collapsing the logical state.

---

**Q: What is syndrome measurement?**
> Measuring ancilla qubits that have been entangled with data qubits. This reveals what type of error occurred on which qubit, without directly measuring (collapsing) the actual data qubit.

---

**Q: What is a surface code?**
> A 2D lattice of data and ancilla qubits used for error correction. Only needs nearest-neighbor interactions. Error threshold ~1%. Most practical for real superconducting hardware.

---

**Q: What is the error threshold?**
> The physical gate error rate below which fault-tolerant quantum computation is possible. For surface codes: ~**1%**. If physical error $< 1\%$, logical error can be made arbitrarily small by increasing code size.

---

**Q: What is a logical qubit vs physical qubit?**
> Physical qubit: actual noisy hardware qubit. Logical qubit: error-protected qubit encoded across many physical qubits. Surface code needs ~1000 physical qubits per 1 logical qubit.

---

**Q: Why can't we just measure the qubit to check for errors?**
> Measurement collapses the quantum superposition — it destroys the quantum state we're trying to protect. Syndrome measurement cleverly detects errors without collapsing the data.

---

**Q: What is depolarizing noise?**
> A noise model where each gate has probability $p$ of applying a random error ($X$, $Y$, or $Z$). Models imperfect gate operations.

---
---

# UNIT V — VARIATIONAL QUANTUM EIGENSOLVER (VQE)

## 🔵 The Big Idea

VQE finds the **minimum eigenvalue** (ground state energy) of a Hamiltonian matrix. Quantum computer prepares states and measures energies. Classical computer optimizes parameters. They work together in a loop — that's why it's called **hybrid**.

---

## 🧮 The Variational Principle

For any trial state $|\psi(\theta)\rangle$:
$$E(\theta) = \langle\psi(\theta)|H|\psi(\theta)\rangle \geq E_0$$

The energy from any trial state is **always ≥ true ground state energy $E_0$**. So if we minimize $E(\theta)$ over all $\theta$, we approach the true ground state.

---

## 🔄 The Feedback Loop

```
Classical computer                Quantum computer
     ↓                                  ↓
Initial θ  ──────────────────→  Build ansatz |ψ(θ)⟩
     ↑                                  ↓
New θ       ←── Optimizer ──←  Measure E(θ) = ⟨ψ|H|ψ⟩
     ↑                                  
 COBYLA/SPSA                    
     ↑                                  
Stop when E stops decreasing (convergence)
```

---

## 🔢 Hydrogen Example

Hamiltonian: $H = -8.5I - 5.1Z$

Using ansatz $|\psi(\theta)\rangle = R_y(\theta)|0\rangle$:

| $\theta$ | Energy |
|----------|--------|
| $0$ | $-13.6$ eV ← ground state! |
| $\pi/2$ | $-8.5$ eV |
| $\pi$ | $-3.4$ eV |

VQE minimizes → finds $\theta = 0$, $E_{\min} = -13.6$ eV ✅

---

## ❓ VIVA QUESTIONS — VQE

**Q: What is VQE?**
> Variational Quantum Eigensolver. A hybrid quantum-classical algorithm that finds the minimum eigenvalue (ground state energy) of a Hamiltonian using a parameterized quantum circuit optimized by a classical computer.

---

**Q: State the variational principle.**
> For any trial state $|\psi(\theta)\rangle$:
> $$E(\theta) = \langle\psi(\theta)|H|\psi(\theta)\rangle \geq E_0$$
> Minimizing over $\theta$ approaches the true ground state energy $E_0$.

---

**Q: What is an ansatz?**
> A parameterized quantum circuit $|\psi(\theta)\rangle$ representing an educated guess for the ground state structure. Its parameters $\theta$ are optimized to minimize energy. "Ansatz" = initial guess in German.

---

**Q: What does the quantum computer do in VQE?**
> Prepares the trial state $|\psi(\theta)\rangle$ from given parameters, and measures the energy expectation value $E(\theta) = \langle\psi(\theta)|H|\psi(\theta)\rangle$.

---

**Q: What does the classical computer do in VQE?**
> Receives the measured energy, runs an optimization algorithm (COBYLA, SPSA, gradient descent) to find better $\theta$ giving lower energy, sends new $\theta$ back. Continues until convergence.

---

**Q: What is COBYLA?**
> Constrained Optimization By Linear Approximation. A classical gradient-free optimizer used in VQE. Doesn't need quantum gradients — useful for noisy hardware where gradients are hard to compute.

---

**Q: What is convergence in VQE?**
> When energy stops decreasing significantly (change $< \varepsilon \approx 0.001$ eV) across successive iterations. The algorithm has found the minimum energy.

---

**Q: What is a Hamiltonian?**
> A matrix representing the total energy of a quantum system. Its minimum eigenvalue = ground state energy. Written as sum of Pauli operators: e.g., $H = -0.5ZZ + 0.25XX - 0.5IZ$.

---

**Q: What are Pauli operators?**
> The basic building blocks: $I = \begin{pmatrix}1&0\\0&1\end{pmatrix}$, $X = \begin{pmatrix}0&1\\1&0\end{pmatrix}$, $Y = \begin{pmatrix}0&-i\\i&0\end{pmatrix}$, $Z = \begin{pmatrix}1&0\\0&-1\end{pmatrix}$. Any Hamiltonian = weighted sum of their tensor products.

---

**Q: What is ground state energy of hydrogen?**
> $E_1 = -13.6$ eV for the ground state ($n=1$). VQE finds this as the minimum eigenvalue of the hydrogen Hamiltonian.

---

**Q: Why is VQE better than classical for chemistry?**
> For large molecules, the Hamiltonian matrix grows exponentially with system size. Classical eigenvalue solvers need exponential memory. VQE uses quantum superposition to represent the state efficiently.

---

**Q: What is SPSA?**
> Simultaneous Perturbation Stochastic Approximation. Another gradient-free optimizer for VQE. Estimates gradient by perturbing all parameters simultaneously with random signs — efficient on noisy hardware.

---

**Q: Applications of VQE?**
> Quantum chemistry (drug design, molecular simulation), materials science (superconductors), optimization (portfolio management), machine learning (PCA via VQE).

---
---

# UNIT V — QUANTUM APPROXIMATE OPTIMIZATION ALGORITHM (QAOA)

## 🔵 The Big Idea

QAOA solves **combinatorial optimization** problems (choosing the best option from exponentially many choices). Best example: **MaxCut** — partition graph nodes into two groups to maximize edges crossing between groups. This is NP-Hard classically.

---

## 📊 The MaxCut Problem

Given graph $G = (V, E)$, assign $x_i \in \{0,1\}$ to each vertex. Edge $(i,j)$ is "cut" if $x_i \neq x_j$.

**Maximize:**
$$C(x) = \sum_{(i,j) \in E} \frac{1 - z_iz_j}{2} \quad \text{where } z_i = 1-2x_i$$

For $n=50$ nodes: $2^{50} \approx 10^{15}$ possible partitions. Classical brute force: impossible.

---

## 🔧 QUBO Formulation

**QUBO** (Quadratic Unconstrained Binary Optimization):
$$\text{Minimize: } \sum_i q_i x_i + \sum_{i<j} q_{ij} x_i x_j, \quad x_i \in \{0,1\}$$

MaxCut, TSP, portfolio optimization → all written as QUBO → mapped to quantum Ising Hamiltonian → solved with QAOA.

---

## 📋 QAOA Circuit

**Step 1:** Start with all qubits in $|+\rangle$ state. Apply $H^{\otimes n}$.

**Step 2: Problem unitary $U_C(\gamma)$** — for each edge $(i,j)$:
$$\text{CX}_{ij} \cdot R_Z(2\gamma)_j \cdot \text{CX}_{ij}$$
Applies phase $e^{-i\gamma}$ when $x_i \neq x_j$ (rewards cut edges).

**Step 3: Mixer unitary $U_B(\beta)$** — for each qubit:
$$R_X(2\beta) \text{ on each qubit}$$
Enables quantum fluctuations → prevents getting stuck.

**Step 4:** Repeat Steps 2–3 for $p$ layers.

**Step 5:** Measure. Run many shots. Find bitstring with highest cut value.

**Step 6:** Classical optimizer updates $(\gamma, \beta)$ to maximize expected cut.

---

## ↔️ QAOA vs VQE

| | VQE | QAOA |
|-|-----|------|
| Purpose | Find ground state energy | Solve combinatorial optimization |
| Output | Continuous (energy number) | Discrete (bitstring/partition) |
| Ansatz | General parameterized | Problem-structured layers |
| Parameters | $\theta$ (arbitrary) | $\gamma$ and $\beta$ (alternating) |
| Problem type | Quantum chemistry | Discrete optimization |

---

## ❓ VIVA QUESTIONS — QAOA

**Q: What is QAOA?**
> Quantum Approximate Optimization Algorithm. A hybrid quantum-classical algorithm that approximately solves combinatorial optimization problems using alternating problem and mixer unitaries with classically optimized parameters.

---

**Q: What is MaxCut?**
> A graph partitioning problem: divide vertices into two sets to maximize the number of edges going between the sets. NP-Hard for general graphs.

---

**Q: What is QUBO?**
> Quadratic Unconstrained Binary Optimization. Minimizing $\sum_i q_i x_i + \sum_{ij} q_{ij}x_ix_j$ over binary $x_i \in \{0,1\}$. Many NP-Hard problems (MaxCut, TSP) can be written as QUBO and solved with QAOA.

---

**Q: What are the two unitaries in QAOA?**
> $U_C(\gamma)$: **problem unitary** — encodes objective function (CX-RZ-CX for each edge). $U_B(\beta)$: **mixer unitary** — RX rotations enabling quantum exploration of solution space.

---

**Q: What do $\gamma$ and $\beta$ do?**
> $\gamma$: controls how strongly we enforce the objective (problem unitary). $\beta$: controls mixing (exploration). Both are variational parameters optimized classically to maximize expected cut value.

---

**Q: What does QAOA circuit for one edge $(i,j)$ look like?**
> CX from $i$ to $j$, then $R_Z(2\gamma)$ on $j$, then CX from $i$ to $j$. This applies phase $e^{-i\gamma}$ when $x_i \neq x_j$.

---

**Q: What is $p$ in QAOA?**
> Number of alternating layers (depth). Higher $p$ = better approximation but deeper circuit. $p=1$ is simplest. As $p \to \infty$, QAOA approaches the exact solution.

---

**Q: Does QAOA guarantee the optimal solution?**
> **No.** QAOA is **approximate**. It finds a good solution with high probability, not necessarily the globally optimal one. "Approximate" is in the name.

---

**Q: How does QAOA differ from VQE?**
> VQE minimizes a continuous quantity (energy). QAOA maximizes a discrete objective (cut value). VQE uses general ansatz; QAOA uses problem-specific alternating layers.

---

**Q: Why is MaxCut hard classically?**
> NP-Hard: no known polynomial-time classical algorithm. $n=50$ nodes → $2^{50} \approx 10^{15}$ partitions. Even for $n=1000$, brute force is completely infeasible.

---

**Q: What is the mixer unitary's role in QAOA?**
> It applies $R_X(2\beta)$ to each qubit. This "mixes" computational basis states, allowing quantum tunneling between different partitions. Without it, the algorithm gets stuck in the initial superposition.

---
---

# PRACTICAL 1 — GROVER'S ALGORITHM (QISKIT)

## What This Experiment Does
- 3-qubit search: $N=8$, 1 Grover iteration, find 1 target
- 4-qubit search: $N=16$, 2 Grover iterations, multiple targets
- Simulator: AerSimulator (ideal, no noise)

## Key Code Concepts

```python
def make_oracle_3(target):
    circuit = QuantumCircuit(3)
    flipped = target[::-1]         # REVERSE: Qiskit is little-endian
    for pos, bit in enumerate(flipped):
        if bit == '0':
            circuit.x(pos)         # X gates on '0' positions
    circuit.h(2)
    circuit.ccx(0, 1, 2)           # H-CCX-H = phase flip on target
    circuit.h(2)
    for pos, bit in enumerate(flipped):
        if bit == '0':
            circuit.x(pos)         # Undo X gates
    return circuit

def run_grover_3(target, shots=1024):
    oracle = make_oracle_3(target)
    grover_op = GroverOperator(oracle)  # Oracle + Diffuser
    search = QuantumCircuit(3, 3)
    search.h([0, 1, 2])                 # Initial superposition
    search.append(grover_op, [0, 1, 2]) # One Grover iteration
    search.measure([0, 1, 2], [0, 1, 2])
    result = AerSimulator().run(transpile(search, AerSimulator()),
                                 shots=shots).result()
    return search, result.get_counts()
```

---

## ❓ VIVA QUESTIONS — PRACTICAL 1

**Q: Why is the target string reversed in the oracle code?**
> Qiskit uses **little-endian** bit ordering: qubit 0 is the **least significant** bit. The target string is reversed so that the oracle's X gates flip the correct qubit positions.

---

**Q: How does the oracle flip only the target state's phase?**
> X gates flip all '0' positions to '1', making the target state the all-$|1\rangle$ state. H-CCX-H then applies a phase flip specifically to the all-$|1\rangle$ state. Then X gates undo the flips. Net effect: only the target's phase is flipped.

---

**Q: What is GroverOperator in Qiskit?**
> A Qiskit library class that combines the oracle (phase flip) with the diffusion operator (invert about mean) into a single Grover iteration object.

---

**Q: What is AerSimulator?**
> A classical quantum circuit simulator from Qiskit Aer. Simulates ideal quantum operations without real hardware noise. Used for testing and learning.

---

**Q: What does shots=1024 mean?**
> The circuit is run 1024 times. Each run gives one measurement outcome. The histogram shows the frequency of each bitstring across all 1024 runs. More shots → more accurate probability estimate.

---

**Q: What output do you expect for 3-qubit Grover targeting $|100\rangle$?**
> The histogram should show $|100\rangle$ with close to **100%** of the 1024 shots. All other states should appear rarely or not at all.

---

**Q: What is transpile() used for?**
> Converts an abstract Qiskit circuit into native gate set supported by the specific backend. Maps abstract gates (like CCX) to physically implementable sequences.

---

**Q: How many iterations for 3 qubits ($N=8$)?**
> $R = \lfloor\frac{\pi}{4}\sqrt{8}\rfloor \approx 2.22 \to 2$ iterations optimal. But 1 iteration still gives good results for $N=8$.

---

**Q: What does get_counts() return?**
> A dictionary mapping bitstrings to their count. E.g., `{'100': 1020, '010': 2, '001': 2}` means $|100\rangle$ was measured 1020 out of 1024 times.

---
---

# PRACTICAL 2 — SHOR'S ALGORITHM (QISKIT)

## What This Experiment Does
- Demonstrates Shor's algorithm to factor $N=15$
- Shows classical period finding, quantum circuit, inverse QFT
- Classical GCD post-processing to extract factors

## Key Code Concepts

```python
def show_period_table(base, modulus):
    for x in range(1, modulus + 1):
        value = pow(base, x, modulus)
        if value == 1:
            print(f"Period r = {x}")
            return x
# With base=7, modulus=15: r=4

def extract_factors(base, period, modulus):
    half_power = pow(base, period // 2, modulus)  # a^(r/2) mod N
    factor_one = gcd(half_power - 1, modulus)     # gcd(a^r/2 - 1, N)
    factor_two = gcd(half_power + 1, modulus)     # gcd(a^r/2 + 1, N)
    return factor_one, factor_two
# For a=7, r=4, N=15: half_power=4, factors=3 and 5

def inverse_qft(num_qubits):
    circuit = QuantumCircuit(num_qubits)
    for i in range(num_qubits // 2):
        circuit.swap(i, num_qubits - i - 1)    # Reverse bit order
    for j in range(num_qubits):
        for m in range(j):
            circuit.cp(-pi / (2**(j-m)), m, j) # Controlled phase
        circuit.h(j)
    return circuit
```

---

## ❓ VIVA QUESTIONS — PRACTICAL 2

**Q: What does the quantum part of the experiment do?**
> Finds the period $r$ of $f(x) = a^x \bmod N$ using superposition + controlled modular exponentiation + inverse QFT. Period extraction cannot be done efficiently classically.

---

**Q: Why is modular exponentiation hardcoded for $N=15$?**
> General quantum modular exponentiation is very complex to implement. For the small case $N=15$, specific SWAP-based circuits are used for each base value.

---

**Q: What is the expected quantum circuit output for $N=15$?**
> Measurement histogram should show peaks at multiples of $2^n/r$. For $r=4$ with $n=8$ qubits ($2^8=256$): peaks at $0, 64, 128, 192$.

---

**Q: Why 8 counting qubits?**
> More counting qubits = higher precision in the fraction $s/r$. For $N=15$ (max period 15), 8 qubits gives $2^8=256$ resolution — enough to accurately extract $r$ via continued fractions.

---

**Q: What does inverse QFT circuit look like?**
> SWAP gates (reverse bit order) followed by: Hadamard on each qubit with controlled negative phase rotations $(-\pi/2^k)$ from all previous qubits. Reverse of forward QFT.

---

**Q: What is cp() gate in Qiskit?**
> Controlled-Phase gate. Applies phase rotation $e^{i\phi}$ to target qubit $|1\rangle$ only when control qubit is $|1\rangle$. Used in QFT circuit for controlled phase rotations.

---
---

# PRACTICAL 3 — TELEPORTATION & QKD (QISKIT)

## What This Experiment Does
- **Part A:** Teleport a random qubit state from Alice to Bob using 3 qubits
- **Part B:** Simulate BB84 QKD with and without eavesdropper (Eve)

## Key Code

```python
def make_bell_pair(circuit, a, b):
    circuit.h(a)        # H on qubit a → superposition
    circuit.cx(a, b)    # CNOT → entangle a and b

def alice_operations(circuit, psi, a):
    circuit.cx(psi, a)  # CNOT: psi controls Alice's Bell qubit
    circuit.h(psi)      # H on psi qubit

def bob_decode(circuit, b, bit_z, bit_x):
    circuit.x(b).c_if(bit_x, 1)  # Apply X if classical bit_x = 1
    circuit.z(b).c_if(bit_z, 1)  # Apply Z if classical bit_z = 1
```

---

## ❓ VIVA QUESTIONS — PRACTICAL 3

**Q: How many qubits in the teleportation circuit?**
> 3 qubits: **q0** = Alice's $|\psi\rangle$ to teleport, **q1** = Alice's Bell qubit, **q2** = Bob's Bell qubit.

---

**Q: How many classical registers and why?**
> **2 classical bits** (crz and crx). Store Alice's 2 measurement outcomes. Bob uses these to decide which correction gate ($X$, $Z$, $ZX$, or nothing) to apply.

---

**Q: What is c_if in Qiskit?**
> A conditional gate modifier. Applies the quantum gate only when the specified classical register equals a given value. Used for Bob's conditional $X$ and $Z$ corrections.

---

**Q: How do you verify teleportation worked?**
> Save the statevector after teleportation. Bob's qubit (q2) should point in the **same direction on the Bloch sphere** as Alice's original $|\psi\rangle$. States should be identical.

---

**Q: What is check_fraction=0.5 in QKD code?**
> 50% of sifted key bits are publicly compared to estimate QBER. Higher fraction = better Eve detection but fewer bits left for final key.

---

**Q: What does the QKD simulation output?**
> Sifted key, error rate (QBER), and whether Eve is detected. If QBER exceeds threshold, key is flagged as compromised.

---

**Q: Order of operations in teleportation circuit?**
> 1. Initialize $|\psi\rangle$ on q0. 2. Create Bell pair on q1, q2. 3. Alice: CNOT (q0→q1) then H on q0. 4. Measure q0 and q1. 5. Bob: conditional X on q2 (from crx), conditional Z on q2 (from crz).

---
---

# PRACTICAL 4 — VQE (QISKIT)

## What This Experiment Does
- Finds ground state energy of 2-qubit H$_2$ Hamiltonian
- Uses parameterized RY ansatz + COBYLA optimizer
- Verifies against exact classical eigenvalue

## Key Code

```python
# H2 Hamiltonian
hamiltonian = SparsePauliOp.from_list([
    ("ZZ", -0.5), ("ZI", -0.5), ("IZ", -0.5),
    ("XX",  0.25), ("YY",  0.25),
])

# Parameterized ansatz: RY rotations + CNOT entanglement
def build_ansatz(num_qubits=2, layers=2):
    angles = ParameterVector('theta', num_qubits * layers)
    circuit = QuantumCircuit(num_qubits)
    idx = 0
    for layer in range(layers):
        for q in range(num_qubits):
            circuit.ry(angles[idx], q); idx += 1
        if layer < layers - 1:
            for q in range(num_qubits - 1):
                circuit.cx(q, q+1)
    return circuit

# Cost function: measure energy
def cost(params):
    job = estimator.run([ansatz], [hamiltonian], [params])
    return job.result().values[0]

# Optimize
result = minimize(cost, start_params, method='COBYLA',
                   options={'maxiter': 500})
```

---

## ❓ VIVA QUESTIONS — PRACTICAL 4

**Q: What Hamiltonian is used?**
> $H = -0.5ZZ - 0.5ZI - 0.5IZ + 0.25XX + 0.25YY$ — a 2-qubit approximation of the H$_2$ (hydrogen molecule) interaction Hamiltonian.

---

**Q: What is SparsePauliOp?**
> A Qiskit class representing a Hamiltonian as a weighted sum of Pauli operator strings. E.g., `("ZZ", -0.5)` means $-0.5 \cdot Z \otimes Z$.

---

**Q: What is ParameterVector?**
> Creates symbolic parameter objects for a parameterized quantum circuit. These symbols are later replaced with numerical values during optimization.

---

**Q: How do you verify VQE found the right answer?**
> Compare with `np.linalg.eigvalsh(hamiltonian.to_matrix())[0]` — the exact minimum eigenvalue from classical linear algebra. VQE result should match closely.

---

**Q: What does the convergence plot show?**
> Energy vs iteration number. Should show decreasing energy that levels off (flattens) at the ground state energy. The flat region = convergence.

---

**Q: What is the Estimator?**
> A Qiskit primitive that efficiently computes expectation values $\langle\psi|H|\psi\rangle$ for a parameterized circuit and Hamiltonian. Central to the VQE cost function.

---

**Q: What optimizer is used?**
> COBYLA (Constrained Optimization By Linear Approximation). Gradient-free, works even on noisy hardware. From SciPy's `minimize` function.

---
---

# PRACTICAL 5 — NOISE & ERROR MITIGATION (QISKIT)

## What This Experiment Does
- Models realistic quantum noise: depolarizing + thermal relaxation + readout errors
- Applies two mitigation techniques: **MEM** and **ZNE**
- Target circuit: Bell state $|\Phi^+\rangle$ (ideal: 50% $|00\rangle$, 50% $|11\rangle$)

## Key Code

```python
def make_noise_model():
    model = NoiseModel()
    # Depolarizing: 0.1% on single-qubit, 1% on two-qubit gates
    model.add_all_qubit_quantum_error(depolarizing_error(0.001, 1), ['h','x'])
    model.add_all_qubit_quantum_error(depolarizing_error(0.01, 2), ['cx'])
    # Thermal relaxation: T1=50µs, T2=70µs, gate_time=100ns
    t1, t2, gate_time = 50_000, 70_000, 100
    thermal = thermal_relaxation_error(t1, t2, gate_time)
    model.add_all_qubit_quantum_error(thermal, ['h','x'])
    # Readout error: 2% chance of wrong measurement
    model.add_all_qubit_readout_error(
        ReadoutError([[0.98, 0.02], [0.02, 0.98]]))
    return model
```

---

## ❓ VIVA QUESTIONS — PRACTICAL 5

**Q: What is depolarizing noise?**
> Each gate has probability $p$ of applying a random error ($X$, $Y$, or $Z$). For 1-qubit gates: $p=0.001$ (0.1%). For CX gate: $p=0.01$ (1%).

---

**Q: What is thermal relaxation error?**
> Noise from energy decay ($T_1$) and phase decoherence ($T_2$) during gate operation time $t_{gate}$. More realistic than simple depolarizing noise.

---

**Q: What is readout error?**
> Probability of measuring the wrong bit. 2% chance: measuring $|0\rangle$ records "1" or measuring $|1\rangle$ records "0".

---

**Q: What is the ideal Bell state output?**
> Exactly **50% $|00\rangle$** and **50% $|11\rangle$**. Any $|01\rangle$ or $|10\rangle$ in results is due to noise/errors.

---

**Q: What is MEM (Measurement Error Mitigation)?**
> Build a calibration matrix by preparing each basis state ($|00\rangle$, $|01\rangle$, $|10\rangle$, $|11\rangle$) and measuring. Invert the matrix and multiply by noisy counts to recover corrected counts.

---

**Q: What is ZNE (Zero Noise Extrapolation)?**
> Run circuit at multiple amplified noise levels (scale 1, 2, 3). Fit a curve. Extrapolate back to noise level = 0 to estimate the ideal noiseless result.

---

**Q: How is noise amplified in ZNE?**
> By inserting gate-inverse pairs. Gate $G$ followed by $G^{-1}G^{-1}G$ has same logical effect but takes 3× longer, experiencing 3× more thermal noise.

---

**Q: What does the ReadoutError matrix `[[0.98, 0.02], [0.02, 0.98]]` mean?**
> Row 0: if qubit is $|0\rangle$, it's measured as 0 with prob 0.98 and as 1 with prob 0.02. Row 1: if qubit is $|1\rangle$, measured as 1 with prob 0.98, as 0 with prob 0.02.

---

**Q: Why is error mitigation different from error correction?**
> Error correction (QEC) uses extra qubits and works below a threshold error rate — not available on current hardware. Error mitigation is a software post-processing technique that works NOW on noisy hardware without extra qubits, but gives approximate improvement.

---
---

# PRACTICAL 6 — QAOA (QISKIT)

## What This Experiment Does
- MaxCut on a 5-node graph using QAOA with $p=1$ layer
- Also solves Traveling Salesman Problem (TSP) using QUBO encoding
- Optimizes $\gamma$ and $\beta$ using classical optimizer

## Key Code

```python
gamma = Parameter('gamma')
beta  = Parameter('beta')

def build_qaoa_circuit():
    circuit = QuantumCircuit(num_nodes)
    for node in range(num_nodes):
        circuit.h(node)               # Initial superposition
    for (i, j) in edge_list:          # Problem unitary
        circuit.cx(i, j)
        circuit.rz(2 * gamma, j)      # Phase for cut edges
        circuit.cx(i, j)
    for node in range(num_nodes):     # Mixer unitary
        circuit.rx(2 * beta, node)
    return circuit

def count_cut(bitstring):
    bits = [int(b) for b in bitstring[::-1]]  # Qiskit little-endian
    cut = 0
    for (i, j) in edge_list:
        if bits[i] != bits[j]:        # Edge crosses the cut
            cut += 1
    return cut
```

---

## ❓ VIVA QUESTIONS — PRACTICAL 6

**Q: What graph is used in Experiment 6?**
> 5 nodes (0–4) with 6 edges: $(0,1),(0,2),(1,3),(3,4),(4,2),(2,1)$.

---

**Q: What does the problem unitary do?**
> For each edge $(i,j)$: CX-RZ($2\gamma$)-CX applies phase $e^{-i\gamma}$ when nodes $i$ and $j$ are in different partitions. This energetically rewards cut edges.

---

**Q: What does the mixer unitary do?**
> $R_X(2\beta)$ on each qubit. Mixes computational basis states, enabling quantum exploration of different partitions. Without it, the algorithm stays stuck.

---

**Q: How do you find the best partition?**
> Measure many shots (e.g., 2048). For each output bitstring, compute cut value using `count_cut()`. The bitstring giving maximum cut is the best partition found.

---

**Q: What is assign_parameters() in Qiskit?**
> Substitutes numerical values for symbolic `Parameter` objects ($\gamma$, $\beta$) in the circuit, producing a concrete executable circuit.

---

**Q: What is TSP and how is it solved?**
> Traveling Salesman Problem: find the shortest route visiting all cities exactly once. Encoded as QUBO with penalty terms: one city per time step, each city visited exactly once. QAOA approximately minimizes the QUBO cost function.

---

**Q: What does count_cut() return?**
> The number of edges crossing the cut for a given bitstring partition. Higher = better. We maximize this over all measured bitstrings.

---
---

# COMMON TRAPS — DON'T GET FOOLED

> These are the TOP mistakes students make. If the examiner asks any of these, you'll answer perfectly.

---

**❌ TRAP: "Teleportation copies the qubit"**
> ✅ **CORRECT:** Alice's qubit is **DESTROYED** by measurement. Two copies NEVER exist simultaneously. No-cloning is not violated.

---

**❌ TRAP: "Teleportation is faster than light"**
> ✅ **CORRECT:** Bob's qubit is useless until he receives Alice's 2 classical bits. Classical bits travel at most at light speed. No FTL communication.

---

**❌ TRAP: "Grover's gives exponential speedup"**
> ✅ **CORRECT:** Grover's gives **quadratic** speedup ($\sqrt{N}$ vs $N$). **Shor's** gives exponential speedup (polynomial vs sub-exponential). Know the difference!

---

**❌ TRAP: "BB84 uses entanglement"**
> ✅ **CORRECT:** BB84 does **NOT** need entanglement. It uses superposition + no-cloning + measurement disturbance. (E91 protocol uses entanglement — that's a different QKD protocol.)

---

**❌ TRAP: "VQE is a pure quantum algorithm"**
> ✅ **CORRECT:** VQE is **HYBRID**. Quantum computer evaluates energy $\langle H \rangle$; classical computer optimizes $\theta$. That's the whole point.

---

**❌ TRAP: "QAOA gives the exact optimal solution"**
> ✅ **CORRECT:** QAOA is **APPROXIMATE** (it's in the name — "Approximate"). Higher depth $p$ improves solution quality but never guarantees global optimum.

---

**❌ TRAP: "Surface code can fix any error rate"**
> ✅ **CORRECT:** Surface code only works if physical error rate is **BELOW ~1%** (error threshold). Above threshold, adding more qubits makes things WORSE.

---

**❌ TRAP: "QFT directly reveals frequencies"**
> ✅ **CORRECT:** QFT transforms amplitudes, but you can only measure ONE outcome per run. Frequency information is extracted **statistically** across many measurements.

---

**❌ TRAP: "Shor's gives the same speedup as Grover's"**
> ✅ **CORRECT:** Shor's gives **exponential** speedup (factoring goes from sub-exponential to polynomial). Grover's gives only **quadratic** speedup. Very different.

---

**❌ TRAP: "The oracle tells you the answer directly"**
> ✅ **CORRECT:** The oracle only marks the target by flipping its phase — it does NOT reveal the answer. Phase flip is invisible to measurement. The diffusion step amplifies it into measurable probability.

---
---

# MASTER CHEAT SHEET

## All Algorithms in One Table

| Algorithm | Problem | Classical | Quantum | Key Trick |
|-----------|---------|-----------|---------|-----------|
| Grover's | Unstructured search | $O(N)$ | $O(\sqrt{N})$ | Amplitude amplification |
| Shor's | Factoring $N$ | Sub-exp | $O(n^3)$ | QFT + period finding |
| QFT | Basis change | $O(N\log N)$ | $O(n^2)$ | Phase rotations |
| Teleportation | Transfer qubit | Impossible | 1 Bell + 2 bits | Entanglement |
| Superdense | Send 2 bits | 2 classical bits | 1 qubit | Entanglement |
| BB84 | Shared secret key | Computationally hard | Physics-secure | Disturbance |
| VQE | Ground state energy | Exponential | Hybrid poly | Variational principle |
| QAOA | Combinatorial opt. | Exponential | Hybrid approx. | Cost + Mixer |

---

## Bell States — Memorize!

| State | Formula | Superdense: Alice sends |
|-------|---------|------------------------|
| $\|\Phi^+\rangle$ | $\frac{1}{\sqrt{2}}(\|00\rangle+\|11\rangle)$ | 00 → $I$ gate |
| $\|\Psi^+\rangle$ | $\frac{1}{\sqrt{2}}(\|01\rangle+\|10\rangle)$ | 01 → $X$ gate |
| $\|\Phi^-\rangle$ | $\frac{1}{\sqrt{2}}(\|00\rangle-\|11\rangle)$ | 10 → $Z$ gate |
| $\|\Psi^-\rangle$ | $\frac{1}{\sqrt{2}}(\|01\rangle-\|10\rangle)$ | 11 → $ZX$ gate |

---

## Key Formulas Cheat Sheet

| Formula | Meaning |
|---------|---------|
| $R \approx \frac{\pi}{4}\sqrt{\frac{N}{M}}$ | Grover's optimal iterations |
| $a^r \equiv 1 \pmod{N}$ | Period definition in Shor's |
| $\text{QFT}\|x\rangle = \frac{1}{\sqrt{N}}\sum_y e^{2\pi ixy/N}\|y\rangle$ | QFT definition |
| $E(\theta) = \langle\psi(\theta)\|H\|\psi(\theta)\rangle \geq E_0$ | Variational principle (VQE) |
| $T_2 \leq 2T_1$ | Decoherence time relationship |
| $\text{QBER}_\text{Eve} = 25\%$ | BB84 error from full interception |

---

## Resource Exchange (Critical!)

| Protocol | Resources |
|----------|-----------|
| Teleportation | 1 Bell pair + 2 classical bits → transfer 1 qubit state |
| Superdense Coding | 1 Bell pair + 1 qubit → send 2 classical bits |
| BB84 | $2n$ photons → $\approx n/2$ bits of secure key |
| Grover's | $O(\sqrt{N})$ oracle calls → find 1 item from $N$ |
| Shor's | $O(n^3)$ gates → factors of $n$-bit number $N$ |

---

## Speedup Summary

| Algorithm | Classical | Quantum | Speedup type |
|-----------|-----------|---------|--------------|
| Grover's | $O(N)$ | $O(\sqrt{N})$ | **Quadratic** |
| Shor's | $O(e^{n^{1/3}})$ | $O(n^3)$ | **Exponential** |
| QFT | $O(N\log N)$ | $O(n^2)$ | **Exponential** |

---

*All the best for your viva! You know this material — just stay calm and answer directly. If you don't know, say "I'm not sure of the exact value but the concept is..." — never bluff. 🎯*
