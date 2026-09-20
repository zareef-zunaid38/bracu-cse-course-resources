# Pumping Lemma — 20 Worked Non-Regularity Proofs

Practice sheet condensed from a YouTube walkthrough ("20 Pumping Lemma Examples") shared by TNF as supplementary Pumping Lemma material for CSE331. Cleaned up from a raw auto-caption transcript into standard proof-by-contradiction write-ups — filler, timestamps, and spoken asides removed; all technical content and reasoning preserved.

**Standard proof skeleton used throughout:** Assume $L$ is regular, so a pumping length $p$ exists. Choose a witness string $w \in L$ with $|w|\ge p$. For any decomposition $w=xyz$ with $|xy|\le p$, $|y|>0$, exhibit some $i\ge0$ such that $xy^iz \notin L$ — contradicting the Pumping Lemma, so $L$ is not regular.

---

## 1. $\{0^n1^n \mid n \geq 0\}$

Witness: $w = 0^p1^p$. Since $|xy|\le p$ and the first $p$ symbols are 0's, $x=0^\alpha$, $y=0^\beta$ ($\beta\ge1$), $z=0^{p-\alpha-\beta}1^p$.

$$xy^iz = 0^{p+(i-1)\beta}1^p \in L \iff (i-1)\beta = 0 \iff i=1$$

Choose $i=2$: not in $L$. **Technique:** the baseline block-comparison proof every other proof below builds on.

## 2. Equal number of 0's and 1's (any order)

$L' = \{w \in \{0,1\}^* : \#0(w) = \#1(w)\}$ — broader than problem 1, since ordering isn't fixed.

Two shortcuts avoid repeating the full argument:
- **Closure under intersection:** $L' \cap 0^*1^* = \{0^n1^n\}$. $0^*1^*$ is regular; if $L'$ were regular, the intersection would be too (regular languages closed under intersection) — but $\{0^n1^n\}$ is already known non-regular. Contradiction.
- **Reuse problem 1's proof verbatim:** the witness $0^p1^p$ and every decomposition step are identical between the two languages, since "equal count" and "$n$ 0's then $n$ 1's" agree on this specific string.

**Technique:** adapting/reusing an already-proven language's proof, or using closure properties, instead of re-deriving from scratch.

## 3. More 1's than 0's

$L = \{w : \#1(w) > \#0(w)\}$. Witness: $w = 0^p1^{p+1}$ — chosen to be *barely* in the language (exactly one more 1 than 0).

Decompose as usual: $x=0^\alpha$, $y=0^\beta$, $z=0^{p-\alpha-\beta}1^{p+1}$.

$$xy^iz = 0^{p+(i-1)\beta}1^{p+1} \in L \iff (i-1)\beta < 1$$

Since $\beta\ge1$, this holds only for $i\in\{0,1\}$. Choose $i=2$: not in $L$.

**Technique:** pick a witness that sits *right on the boundary* of the language, with the deficit/surplus symbol at the very start of the string (in the pumpable region) — pumping the majority symbol at the front, rather than the minority symbol, guarantees you can cross the boundary. Putting the "extra" symbol elsewhere risks a witness that can never leave the language no matter how you pump (e.g. pumping the minority symbol only ever adds to it, never resolves the surplus).

## 4. $\{0^{2n}1^n \mid n \geq 0\}$

Witness: $w = 0^{2p}1^p$. $x=0^\alpha$, $y=0^\beta$, $z=0^{2p-\alpha-\beta}1^p$.

$$xy^iz = 0^{2p+(i-1)\beta}1^p \in L \iff (i-1)\beta = 0 \iff i=1$$

Choose $i=2$: not in $L$. **Technique:** same block-comparison as problem 1, just with a fixed ratio (2:1) instead of equality.

## 5. $\{0^n1\,0^n \mid n \geq 0\}$

Witness: $w = 0^p1\,0^p$. $x=0^\alpha$, $y=0^\beta$ (from the leading block), $z=0^{p-\alpha-\beta}1\,0^p$.

$$xy^iz = 0^{p+(i-1)\beta}1\,0^p \in L \iff (i-1)\beta=0 \iff i=1$$

Choose $i=2$: not in $L$. **Technique:** identical to problem 1's structure with a middle separator instead of a middle boundary between two blocks.

## 6–7. $\{0^m1^n0^{m+n}\}$ and $\{0^m1^n0^{mn}\}$

Both proved at once via a general language $L_3(f) = \{0^m1^n0^{f(m,n)} \mid m,n\ge0\}$, specializing $f(m,n)=m+n$ (problem 6) and $f(m,n)=mn$ (problem 7).

Witness: $w = 0^p1^p0^{f(p,p)}$. $x=0^\alpha$, $y=0^\beta$ (leading block), $z=0^{p-\alpha-\beta}1^p0^{f(p,p)}$.

Pumping changes only the leading count to $p+(i-1)\beta$; the $1$-count ($p$) and trailing count ($f(p,p)$) are untouched.

- **Sum case:** in $L$ iff $[p+(i-1)\beta] + p = 2p \iff (i-1)\beta=0 \iff i=1$.
- **Product case:** in $L$ iff $[p+(i-1)\beta]\cdot p = p^2 \iff p+(i-1)\beta=p \iff i=1$.

Choosing $i=2$ breaks **both** simultaneously.

**Technique:** generalize several similar languages into one parametrized language, prove the general case once, then specialize. Caution: $L_3(f)$ itself isn't automatically non-regular for *every* $f$ — e.g. $f\equiv 0$ makes $L_3 = 0^*1^*$, which is regular. The technique only transfers to specific instantiations of $f$ that were actually checked.

## 8. $\{0^n1^m \mid n \neq m\}$ — the $p!$ trick

Naive witness $0^{p+1}1^p$ fails: pumping the 0-block by an unknown amount $\beta\in[1,p]$ might never land exactly back on equality — you could overshoot without knowing it.

Fix: witness $w = 0^p1^{p+p!}$. Any $\beta\in[1,p]$ divides $p!$, so an integer $i$ can always be found that closes the gap exactly.

$x=0^\alpha$, $y=0^\beta$, $z=0^{p-\alpha-\beta}1^{p+p!}$.

$$xy^iz \in L \iff p+(i-1)\beta \neq p+p! \iff (i-1)\beta \neq p!$$

Since $\beta \mid p!$, choose $i = p!/\beta + 1$ (an integer) $\Rightarrow (i-1)\beta = p!$ exactly $\Rightarrow$ the two counts become **equal** $\Rightarrow xy^iz\notin L$.

**Technique — the $p!$ trick:** to force an unknown pump amount $\beta \in [1,p]$ to hit an exact target after repeated addition, make the target a multiple of $\mathrm{lcm}(1,\dots,p)$ (e.g. $p!$) — $\beta$ is then guaranteed to divide it, so the needed number of pumps is always an integer. Reuse this whenever you need to force two quantities to become *equal* (harder) rather than merely *different* (easy).

## 9. $\{0^n1^m \mid n < 3m\}$

Witness: $w = 0^{3p-1}1^p$ — barely in the language, one 0 short of leaving.

$x=0^\alpha$, $y=0^\beta$, $z=0^{3p-1-\alpha-\beta}1^p$.

$$xy^iz \in L \iff 3p-1+(i-1)\beta < 3p \iff (i-1)\beta < 1$$

True only for $i\in\{0,1\}$ (since $\beta\ge1$). Choose $i=2$: not in $L$.

**Technique:** same "barely in the language" boundary witness as problem 3.

## 10. Perfect Squares: $\{0^n \mid n \text{ is a perfect square}\}$

Witness: $w = 0^{p^2}$ — length is a perfect square *by construction*, regardless of what $p$ actually is (don't try to force $p$ itself to be a perfect square — $p$'s value is fixed but unknown).

$x=0^\alpha$, $y=0^\beta$ ($1\le\beta\le p$), $z=0^{p^2-\alpha-\beta}$.

Pumping with $i=2$: new length $= p^2+\beta$.

$$p^2 < p^2+\beta \leq p^2+p < p^2+2p+1 = (p+1)^2$$

Strictly between two consecutive perfect squares $\Rightarrow$ not a perfect square $\Rightarrow xy^2z \notin L$.

**Technique — squeeze between consecutive targets:** $i=3$ also works ($p^2+2\beta$, still $<p^2+2p<(p+1)^2$); $i=0,1$ weren't checked in the source. Larger $i$ risks overshooting into the next square.

## 11. Powers of Two: $\{0^n \mid n = 2^k, k\ge0\}$

Witness: $w = 0^{2^p}$. $x=0^\alpha$, $y=0^\beta$ ($1\le\beta\le p$).

Pumping with $i=2$: new length $=2^p+\beta$. Using the extra fact $p < 2^p$ for all $p\ge1$:

$$2^p < 2^p+\beta \leq 2^p+p < 2^p+2^p = 2^{p+1}$$

Strictly between two consecutive powers of two $\Rightarrow$ not itself a power of two.

**Technique:** same squeeze-between-consecutive-targets idea as problem 10, with the extra step of justifying $p<2^p$.

## 12. Primes: $\{0^n \mid n \text{ is prime}\}$

Witness: $w=0^r$, where $r$ is the smallest prime $\ge p+2$ (using that primes are infinite, so such $r$ always exists). The "+2" guarantees at least 2 characters remain outside the pumpable region $|xy|\le p$.

Decomposition-**dependent** choice of $i$: for any decomposition, let $i = |xz|$ (not fixed in advance — it depends on which decomposition is being tested). Then:

$$|xy^iz| = |xz| + i\cdot|y| = |xz| + |xz|\cdot|y| = |xz|\,(1+|y|)$$

Both factors are $\ge 2$: $|xz|\ge2$ (since $r\ge p+2$ guarantees $|z|\ge2$ even in the worst case $|xy|=p$), and $1+|y|\ge2$ (since $|y|\ge1$). A product of two factors $\ge2$ is composite $\Rightarrow$ not prime $\Rightarrow xy^iz\notin L$.

**Technique:** the Pumping Lemma only requires *some* $i$ per decomposition — $i$ is allowed to depend on the specific decomposition rather than being fixed once for all of them.

## 13. $\{0^n1^m \mid n/m \in \mathbb{Z}\}$ (0-count a multiple of 1-count)

Naive witness $0^{2p}1^p$ fails: any pump amount $\beta\in[1,p]$ only ever shifts the ratio between two adjacent integer multiples of $p$, so the string never actually leaves the language.

Fix: witness $w=0^{4p}1^{2p}$ — spacing the valid ratios $2p$ apart instead of $p$ apart, wide enough that a pump of $\beta\in[1,p]$ can't land on another valid multiple.

$x=0^\alpha$, $y=0^\beta$. Pumping with $i=2$: new 0-count $=4p+\beta$.

$$\frac{4p+\beta}{2p} \in \mathbb{Z} \iff \beta \in \{0, 2p\}$$

Impossible since $1\le\beta\le p<2p$. Not in $L$.

**Technique:** when a naive witness's "gap" between valid values is too narrow relative to the pump range $[1,p]$, widen the witness so the gap exceeds $p$.

## 14. Equal number of "00" and "11" occurrences (substrings, overlapping allowed)

Witness: $w=0^p1^p$ — deliberately chosen so both counts start equal at $p-1$ occurrences each (a string of $p$ identical symbols contains $p-1$ overlapping occurrences of that pair).

$x=0^\alpha$, $y=0^\beta$. Pumping with $i$: new leading run length $=p+(i-1)\beta$, giving $p+(i-1)\beta-1$ occurrences of "00" vs. an unchanged $p-1$ occurrences of "11".

$$\text{Equal} \iff (i-1)\beta = 0 \iff i=1$$

Choose $i=2$: not in $L$.

**Technique:** the witness matters not just for symbol counts but for *occurrence counts* of a pair — chosen so both start exactly equal in the unperturbed string. (Source notes, as an open follow-up not resolved here: replacing "00 vs 11" with other symbol pairs like "01 vs 10" flips the answer for some pairs — worth exploring separately.)

## 15–16. $\{ww \mid w\in\{0,1\}^*\}$ and $\{w\#w\}$

Witness: $w = 0^p1\,0^p1$ — **not** an all-0's string like $0^p0^p$, which would fail: pumping an even number of characters in/out of an undifferentiated 0-block keeps both halves equal (still of the form $0^m0^m$), giving no contradiction. The trailing marker `1` is what breaks the symmetry once the pump shifts the midpoint off-center.

$x=0^\alpha$, $y=0^\beta$ (first block). For any $i\ge2$ (or $i=0$), the split point moves, and the two resulting halves end in different symbols (one in `0`, one in `1`) — so they cannot be equal.

$xy^iz \notin \{ww\}$ for $i\ne1$. The $\{w\#w\}$ version reuses the identical witness and proof, just with an explicit separator inserted (put the pumped region before the `#`).

**Technique:** for "two halves must match" languages, the witness needs an asymmetric marker inside the pumpable region — an undifferentiated block alone can't break symmetry under pumping.

## 17. $\{w_1\#\cdots\#w_n \mid \text{all } w_i \text{ distinct}\}$

Witness: a leading run of exactly $p$ 0's, followed by `#`-separated runs of strictly increasing length $p+1, p+2, \dots, 2p$ (each run distinct by construction, so the whole string is in $L$).

The leading $p$-run is the only pumpable region ($|xy|\le p$). Pumping it with $i=2$ (adding $\beta\in[1,p]$ more characters) changes its length to $p+\beta \in [p+1, 2p]$ — which **matches one of the other runs exactly**, producing a duplicate.

$xy^2z \notin L$.

**Technique:** construct a witness where the pumpable region's possible new lengths are guaranteed to collide with an already-present, deliberately-spread-out set of "forbidden" lengths.

## 18. Non-Palindromes: $\{w \mid w \neq w^R\}$ — the $p!$ trick again

Witness: $w = 0^p\,11\,0^{p+p!}$ — asymmetric run lengths ($p$ vs $p+p!$) with a fixed two-character separator, so the run lengths alone determine palindrome-ness.

$x=0^\alpha$, $y=0^\beta$ ($1\le\beta\le p$). String is a palindrome iff the two runs are equal: $p+(i-1)\beta = p+p! \iff (i-1)\beta = p!$.

Since $\beta \mid p!$, choose $i = p!/\beta+1$ (integer) $\Rightarrow$ the runs become equal $\Rightarrow$ the pumped string **is** a palindrome $\Rightarrow \notin L$ (the language of *non*-palindromes).

**Technique:** same $p!$ trick as problem 8, applied to force equality where the language requires inequality.

## 19. $\{xy \mid |x|=|y|,\, x\neq y\}$ (even-length, unequal halves) — $p!$ trick

Witness: $w = 0^p1\,0^{p+p!}1$ (assume $p\ge2$ so the length is even — if not, use $\max(p,2)$; still a valid pumping length). The two halves are unequal at the start: the first half ends in `0`, the second in `1`.

$x=0^\alpha$, $y=0^\beta$ ($1\le\beta\le p$). The two halves become equal in run-length iff $(i-1)\beta = p!$.

Since $\beta\mid p!$, choose $i=p!/\beta+1$ (integer) $\Rightarrow$ both halves become **identical** $\Rightarrow xy^iz \notin L$.

**Technique:** same $p!$ trick again — note it doesn't matter whether a pumped string's length stays even; if odd, it's automatically outside $L$ anyway, so the argument only needs to handle the case where length is preserved even.

## 20. Factorials: $\{0^{n!} \mid n \geq 0\}$

Witness: $w = 0^{p!}$. $x=0^\alpha$, $y=0^\beta$ ($1\le\beta\le p$).

Pumping with $i=2$: new length $= p! + \beta$.

$$p! < p!+\beta \leq p!+p < p!\cdot(p+1) = (p+1)!$$

Strictly between two consecutive factorials $\Rightarrow$ not itself a factorial $\Rightarrow xy^2z \notin L$.

**Technique:** same squeeze-between-consecutive-targets family as problems 10 and 11 (perfect squares, powers of two) — this pattern generalizes to any "next value" function $f(n)$ where $f(p) < f(p)+p < f(p+1)$ can be shown.

---

## Techniques Index

Quick lookup by recurring technique, for review:

- **Block-comparison (baseline):** 1, 2, 4, 5, 6–7, 9, 14 — pump a block and compare resulting counts/lengths directly.
- **"Barely in the language" boundary witness:** 3, 9 — choose a witness one unit away from leaving the language, so any pump in the right direction crosses the boundary.
- **Generalize-then-specialize:** 6–7 — prove a parametrized language once, specialize at the end.
- **The $p!$ trick (force an unknown bounded quantity to hit an exact target):** 8, 18, 19 — use when the language requires two quantities to become *equal*.
- **Squeeze between two consecutive target values:** 10, 11, 20 — use when membership depends on hitting a sparse target set (squares, powers, factorials).
- **Decomposition-dependent pump value:** 12 — $i$ may depend on the specific decomposition being tested, not fixed in advance.
- **Widen the witness's gap:** 13 — if a naive witness's valid-value gap is too narrow relative to $[1,p]$, choose a wider-spaced witness.
- **Asymmetric marker for "halves must match" languages:** 15–16, 19 — an undifferentiated block can't break symmetry under pumping; needs a distinguishing marker.
- **Closure properties as a shortcut:** 2 — intersection or complement can avoid re-deriving a proof from scratch.
