<!-- solve-problem:76448ad4-951a-4a71-9c62-416f30b7a51c -->
# Valiant’s explicit matrix rigidity problem

A [Tangle Solve problem](https://solve.tangle.tools/problems/76448ad4-951a-4a71-9c62-416f30b7a51c).

## What is the problem?

Matrix rigidity: for an $n\times n$ matrix $A$ over a field $\mathbb{F}$, define $R_A(r) = \min\{ \mathrm{rank}(B) : B \text{ differs from } A \text{ in at most } r \text{ entries per row, hence } \le n\cdot r \text{ total entries}\}$ — more standardly, $r_A(m)$ is the Hamming distance from $A$ to the set of $n\times n$ matrices of rank $\le m$. **Goal:** construct an *explicit* family $\{A_n\}$ (computable in polynomial time, or better, logspace/uniform) over a fixed field such that $R_A(\varepsilon n) \ge n^{1+\delta}$ for constants $\varepsilon, \delta > 0$. Equivalently: any matrix differing from $A_n$ in fewer than $n^{1+\delta}$ entries must have rank $> \varepsilon n$.

Key formal points:
- Over finite fields one must be careful with counting arguments; the target is usually stated over $\mathbb{Q}$, or finite fields with the rank parameter linear in $n$.
- "Explicit" means there is an efficient uniform algorithm producing $A_n$ — no probabilistic existence allowed.
- Valiant showed that matrices with $R_A(\varepsilon n) \ge n^{1+\delta}$ require linear circuits (log-depth linear-size circuits) of size $\Omega(n \log^{1+\delta'} n)$... more precisely: such rigidity implies the associated linear map cannot be computed by $O(n)$-size, $O(\log n)$-depth circuits.

## Why does it matter?

A solution would give the first superlinear lower bound ($\Omega(n \log^{1+\Omega(1)} n)$) for log-depth linear circuits — a 40+ year barrier in circuit complexity, and one of the weakest natural proofs still standing toward separating TC⁰/NC¹-type classes for linear maps. It's the canonical "explicit construction matching a random object" problem: random matrices are rigid with high probability (Valiant 1977), so the gap is purely about explicitness. Downstream connections: coding theory (explicit codes with good list-decoding/distance properties), compressed sensing (explicit robust measurement matrices), and lower bounds for data structures and communication complexity.

## What's already known?

- **Random matrices are rigid:** a counting argument shows almost every matrix has $R_A(\varepsilon n) \ge n^{1+\delta}$ over infinite/large enough fields. No explicitness.
- **Valiant (1977):** introduced rigidity exactly to prove linear-circuit lower bounds; the $n^{1+\delta}$ threshold for rank-$\varepsilon n$ modifications is the parameter regime needed.
- **Known explicit lower bounds are weak:** best general explicit results give $R_A(r) \ge \Omega(n^2/r \cdot \log(n/r))$-type bounds for various constructions (e.g., Friol/Iscaev, de Wolf and others achieve $\Omega(n^2/r)$ or slight improvements for Hankel/Vandermonde-type matrices via locally decodable code connections). None reach rank parameter $\varepsilon n$ with distance $n^{1+\delta}$.
- **Natural candidates failed:** Vandermonde, Hankel, discrete Fourier, Cauchy matrices, and matrices from error-correcting codes (via "design matrices") have all been shown insufficiently rigid — Dvir–Liu, and earlier Alman–Williams-style or Kumar–Saraf work show near-quadratic rigidity fails for these structured classes. There are "rigidity barriers" showing broad proof techniques (e.g., certain rank-method approaches) cannot succeed.
- **Ramya (2020) survey:** comprehensive overview of all of the above with precise statements.

## What would count as solving it?

1. **Main goal:** An explicit poly-time computable family $\{A_n\}$ over an explicit field with a proof that $R_A(\varepsilon n) \ge n^{1+\delta}$ for constants $\varepsilon,\delta>0$.
2. **Partial credit variants:** (a) same over a field of your choosing with the rank parameter relaxed to $n/(\log\log n)^{O(1)}$ with superlinear rigidity; (b) rigidity $n^{1+\Omega(1)}$ at rank parameter $\varepsilon n$ for a weaker explicitness notion (e.g., $n^{\mathrm{poly}\log n}$-time construction); (c) strong lower bounds for restricted modification patterns.
3. A referee would demand: full rank-vs-distance tradeoff statement, uniformity of the family, and the field fixed in advance.

## How to start

1. Read Valiant's 1977 paper (the rigidity sections) to get the exact circuit-lower-bound reduction — knowing precisely which parameters suffice clarifies how much slack exists.
2. Read Ramya's survey (arXiv:2009.09460), especially sections on lower-bound techniques for Hankel/design matrices and on why Fourier/Vandermonde matrices fail.
3. Natural approaches: (a) probabilistic method plus derandomization — try to show a pseudorandom generator's output is rigid, or use limited independence to preserve rigidity; (b) algebraic constructions via evaluation of multivariate polynomials (generalizing Dvir–Liu's setting to higher-degree or multivariate families); (c) use design matrices / LDPC adjacency matrices with proven rigidity at near-linear rank parameter.
4. Main obstacle: every algebraically structured family proposed so far admits low-rank perturbations exploiting its symmetry; a solution likely needs a family rich enough to look random but still analyzably explicit.

## Key references

- L. G. Valiant, "Graph-Theoretic Arguments in Low-Level Complexity," MFCS 1977 (LNCS 53). Introduces matrix rigidity and proves that rigid matrices yield $\Omega(n\log\log n)$... $\Omega(n \log^{1+\delta}n)$-type lower bounds for log-depth linear circuits; shows random matrices are rigid. Link: https://doi.org/10.1007/3-540-08353-7_135 (verify).
- C. Ramya, "Recent Progress on Matrix Rigidity — A Survey," arXiv:2009.09460, 2020. Surveys all known upper/lower bounds, candidate constructions and their failures. Link: https://arxiv.org/abs/2009.09460
- Z. Dvir and A. Liu, "Fourier and Circulant Matrices Are Not Rigid," ECCC TR16-159 / Theory of Computing 2020. Shows major structured families fail the rigidity requirement — essential negative result. Link: https://eccc.weizmann.ac.il/report/2016/159/
- Additional pointers worth locating (verify): works of Alman–Williams, Kumar–Saraf on rigidity barriers and design matrices; de Wolf on rigidity via locally decodable codes.

## Warnings

- **Parameter regime matters:** many papers prove rigidity $r_A(m) \ge \Omega(\frac{n^2}{m}\log\frac{n}{m})$, which looks impressive but is trivial-range for the target parameters; always check $m = \varepsilon n$.
- **Field subtlety:** over small finite fields counting arguments can fail and the target itself is ambiguous; over $\mathbb{Q}$, entries may need many bits — clarify the bit model.
- **Dead ends:** don't spend time on Fourier, circulant, Vandermonde, Hankel, or Cauchy matrices — non-rigidity is proven. Reed–Muller/evaluation-based matrices also largely ruled out.
- **Don't overclaim circuit consequences:** the circuit bound requires exactly the $n^{1+\delta}$-at-$\varepsilon n$ threshold; weaker rigidity gives nothing superlinear.
- The census record attributes "several promising matrices insufficiently rigid" — that negative literature is the single most useful guide to what *not* to try.

---
*`ALGO-000038` · unverified*

## Acceptance guidance

1. **Main goal:** An explicit poly-time computable family $\{A_n\}$ over an explicit field with a proof that $R_A(\varepsilon n) \ge n^{1+\delta}$ for constants $\varepsilon,\delta>0$.
2. **Partial credit variants:** (a) same over a field of your choosing with the rank parameter relaxed to $n/(\log\log n)^{O(1)}$ with superlinear rigidity; (b) rigidity $n^{1+\Omega(1)}$ at rank parameter $\varepsilon n$ for a weaker explicitness notion (e.g., $n^{\mathrm{poly}\log n}$-time construction); (c) strong lower bounds for restricted modification patterns.
3. A referee would demand: full rank-vs-distance tradeoff statement, uniformity of the family, and the field fixed in advance.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/76448ad4-951a-4a71-9c62-416f30b7a51c?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000038.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000038)
