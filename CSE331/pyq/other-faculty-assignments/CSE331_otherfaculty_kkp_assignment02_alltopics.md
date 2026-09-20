---
source: CSE331_otherfaculty_kkp_assignment02_alltopics.docx (the original, included in this repo)
fidelity: the summary above the divider is paraphrased and may omit detail; the final section 'Full extracted text' is a complete script extraction of the source text (images and diagrams are not transcribed there — 10 embedded image(s); see the summary or the source)
source-mtime: 2026-06-02T12:20:10
generated: summary earlier (2026-09); header and full-text section added 2026-09-20
---
# CSE331 Assignment 02 & Practice Sheet (KKP)

CSE331 comprehensive theory problem set and practice sheet prepared by faculty KKP covering theoretical use cases, Pumping Lemma non-regularity proofs, CFG construction, PDA design, derivations/parse trees/ambiguity, and Turing Machine reductions.
Docx last modified: 02-06-2026.

## Content

### Part 1: CSE331 Assignment 02

#### Question 1: Course Use Cases
- List five use cases for applying the knowledge from our CSE331 course. Describe each in detail.

#### Question 2: Pumping Lemma Proof
- Prove $L$ is a non-regular language using the Pumping Lemma:
  $$L = \{w \in \{a,b\}^* : w = a^i b^j \text{ where } i > j \text{ and } j \ge 0\}$$

#### Question 3: Context-Free Grammar (CFG) Design
Design CFGs for each of the following languages:
1. $L = \{w \in \{a,b,c,p,q,r,\#\}^* : a^i \#^n c^j p^{2x} q^y r^z b^k \text{ where } i=j+k, y=3x+z, z \text{ is even}, n \text{ is odd, and } i,j,k,n,x,y,z \ge 0\}$
2. $L = \{w \in \{0,1,2\}^* : w = 0^i 2^j 1^k \text{ where conditions below apply}\}$:
   - Condition a: $i = k$, $i,k \ge 1$, and $j \ge 2$
   - Condition b: $i = 3k$, $j$ is odd, and $i,j,k \ge 0$
   - Condition c: $i$ is a multiple of two ($i \equiv 0 \pmod 2$), $k$ is two more than a multiple of 3 ($k \equiv 2 \pmod 3$), $j = k+i$, and $i,j,k \ge 0$
   - Condition d: $i+j > k$ and $i,j,k \ge 0$
   - Condition e: $i+k$ is even, $j = i+k$, and $j \ge 1$
3. $L = \{w \in \{0,1\}^* : \text{the parity of 0s and 1s is different in } w\}$
4. $L = \{w \in \{0,1\}^* : \text{the number of 0s and 1s are different in } w\}$ *(Hint: First solve for equal number of 0s and 1s)*
5. $L = \{w \in \{a, b\}^* : w \text{ is a palindrome}\}$. Design a CFG for $\overline{L}$ (Complement of $L$).
6. $L = \{w \in \{a, b\}^* : w \text{ does not contain any palindromic substring of length two}\}$ (i.e. no $aa$ or $bb$).
7. $L = \{w\#x \mid w^R \text{ is a substring of } x \text{ where } w \in \{0, 1\}^*\}$ where $w^R$ denotes the reverse of string $w$.
8. $L = \{1^i 0^{2j} 1^k \mid i, j, k \ge 0, 3i \ge 4k + 2, j \text{ is not divisible by 3}\}$.
9. Let $\Sigma = \{0,1\}$, $L_1 = \{w \in \Sigma^* : w \text{ contains exactly two 1s}\}$, $L_2 = \{x\#y : x \in \Sigma^*, y \in L_1, |x| = |y|\}$. Construct a CFG for $L_2$.
10. Let $\Sigma = \{0,1\}$, $L_1 = \{w \in \Sigma^* : w \text{ contains at least three 1s}\}$, $L_2 = \{x\#y : x \in (\Sigma\Sigma)^*, y \in L_1, |x| = |y|\}$. Construct a CFG for $L_2$.
11. Let $\Sigma = \{a,b\}$, $L_1 = \{w \in \Sigma^* : w \text{ contains } bb \text{ as a substring}\}$, $L_2 = \{w_1\#w_2 : w_1, w_2 \in L_1 \text{ and } |w_1| = |w_2|\}$. Construct a CFG for $L_2$.

#### Question 4: Pushdown Automata (PDA)
- Construct Pushdown Automata (PDA) for all languages given in Question 3.

#### Question 5: Derivation, Parse Tree, and Ambiguity
- **Grammar (i):**
  $$A \to A1 \mid 0A1 \mid 01$$
  - (a) Give a leftmost derivation for the string `001111`.
  - (b) Sketch the parse tree corresponding to the derivation in (a).
  - (c) Demonstrate that there are two more parse trees for the same string.
  - (d) Find a string $w$ of length six such that $w$ has exactly one parse tree in the grammar above.
- **Grammar (ii):**
  $$A \to 1A \mid 1C \mid 0B \mid 00A$$
  $$B \to 0A \mid 1B \mid 00B$$
  $$C \to 0C0 \mid 0C1 \mid 1C0 \mid 1C1 \mid \varepsilon$$
  - (a) Give a leftmost derivation for the string `01011001`.
  - (b) Sketch the parse tree corresponding to (a).
  - (c) Demonstrate ambiguity by showing two more parse trees for the same string.
  - (d) Find a string $w$ of length six such that $w$ has exactly one parse tree.
  - (e) Design an unambiguous Context-Free Grammar for the language represented by this grammar.

#### Question 6: Turing Machines & Computability
- Construct TMs for:
  1. $L = \{w \in \{0,1,2\}^* : 0^i 1^j 2^k \text{ where } i=j=k \text{ and } i,j,k \ge 0\}$
  2. $L = \{w \in \{0,1,2\}^* : 0^i \# 1^j \# 2^k \text{ where } i \cdot j = k \text{ and } i,j,k \ge 0\}$
  3. $L = \{w \in \{a,b\}^* : w \text{ is an odd length palindrome}\}$
  4. $L = \{w\#w^R\#w \mid w \in \{a,b\}^*\}$
- Let $\mathrm{HALT}_{\mathrm{TM}} = \{\langle M, w \rangle : M \text{ is a TM and } M \text{ halts on input } w\}$. Given that $\mathrm{HALT}_{\mathrm{TM}}$ is undecidable, prove that the collection of Turing-recognizable languages is not closed under complement.

---

### Part 2: CSE331 Practice Sheet (Prepared by KKP)

#### Question 1: Designing Context-Free Grammar
Construct CFGs generating:
1. $L = \{w \in \{0,1\}^* : w \text{ starts with '0' and the length of } w \text{ is even}\}$
2. $L = \{w \in \{a,b\}^* : \text{every second letter in } w \text{ is a 'b'}\}$
3. $L = \{w \in \{a,b\}^* : \text{the length of } w \text{ is divisible by 3}\}$
4. $L = \{w \in \{0,1\}^* : w \text{ starts with and ends with a different symbol}\}$
5. $L = \{w \in \{a,b\}^* : \text{the number of 'a' is at least the number of 'b' in } w\}$
6. $L = \{w \in \{a,b\}^* : \text{the length of } w \text{ is two more than a multiple of six, i.e., } |w| = 6k+2\}$
7. $L = \{w \in \{0,1\}^* : w = 0^n 1^{2n+3}, n \ge 0\}$
8. $L = \{w \in \{0,1\}^* : w = 0^n 1^n, \text{where } n \text{ is odd}\}$
9. $L = \{w \in \{0,1,2\}^* : w = 0^i 1^j 2^k \text{ where } j \ge 2i + 3k, \text{and } i, k \ge 0\}$
10. $L = \{w \in \{0,1,2,3\}^* : w = 0^i 1^j 2^k 3^m \text{ where } i=m, j \ge 3k+2, \text{and } m, k \ge 0\}$
11. $L = \{w \in \{0,1,2\}^* : w = 0^i 1^j 2^k \text{ where } i > 2j + 3k, \text{and } j, k \ge 0\}$
12. If $A = \{w \in \{0, 1\}^* : w \text{ contains at least two 0s}\}$, construct $L = \{w \in \{0, 1\}^* : w = 0^{3i} v 1^{2i} \text{ where } v \in A \text{ and } i \ge 0\}$
13. $\Sigma = \{0,1\}$, $L_1 = \{w \in \Sigma^* : w \text{ contains } 11\}$, $L_2 = \{x\#y : y \in L_1, x \in \Sigma^*, |x| = |y|\}$. Construct CFG for $L_2$.
14. $L = \{w \in \{0,1\}^* : w_1\#w_2 : \text{number of 0s in } w_1 \text{ is equal to number of 1s in } w_2\}$
15. $L = \{w \in \{0,1\}^* : w_1\#w_2 : \text{length of } w_2 \text{ is double of length of } w_1\}$
16. Convert Regular Expression into CFG: $((ab)^* + (a+b^3)cb)^*$
17. Convert Regular Expression into CFG: $(a^*b^* + (ac+b^*c)b)^*$
18. Convert DFA into CFG.

#### Question 2: Derivation, Parse Tree, Ambiguity
- **Grammar 1:**
  $$S \to ASB \mid SS \mid SAS \mid A$$
  $$A \to ASS \mid BS \mid B$$
  $$B \to 00 \mid 11 \mid 01 \mid 1$$
  - (a) Show the leftmost derivation of `00010111`.
  - (b) Sketch the parse tree corresponding to (a).
  - (c) Show the rightmost derivation of `00010111`.
  - (d) Sketch the parse tree corresponding to (c).
- **Grammar 2:**
  $$S \to 0S1 \mid 1S0 \mid SS \mid 01 \mid 10$$
  - (a) Show that the grammar is ambiguous by demonstrating two different parse trees for `011010`.
  - (b) Find a string $w$ of length six such that $w$ has exactly one parse tree.

#### Question 3: Pushdown Automata (PDA)
Construct PDAs for:
1. $L = \{w \in \{0,1\}^* : w = 0^n 1^n, \text{where } n \text{ is odd}\}$
2. $L = \{w \in \{0,1\}^* : w = 0^m 1^n, \text{where } m,n \ge 1 \text{ and } m \ge n\}$
3. $L = \{w \in \{0,1\}^* : \text{length of } w \text{ is divisible by 4}\}$
4. $L_1 = \{w \in \{0,1\}^* : \text{number of 1s in } w \text{ is a multiple of 3}\}$, $L_2 = \{w \in \{0,1\}^* : w \text{ contains even number of 0s}\}$. Construct PDA for $L = \{w \in \{0,1\}^* : w = uv, u \in L_1, v \in L_2, |u| = |v|\}$.
5. $L = \{w \in \{0,1\}^* : w = 0^i 1^j 0^k \text{ where } j = i+k \text{ and } i, k \ge 0\}$
6. $L = \{w \in \{0,1\}^* : w = 0^i 1^j 0^k \text{ where } i+j = k \text{ and } i, k \ge 0\}$
7. $L = \{w \in \{0,1\}^* : w_1\#w_2 : \text{number of } 00 \text{ substrings in } w_1 = \text{number of } 11 \text{ in } w_2\}$
8. $L = \{w w^{RC} : w \in \{0,1\}^*\}$ where $w^{RC}$ denotes the reverse complement of string $w$ (e.g. for $w = 010$, complement is $101$, reverse complement is $101$).

---

## Full extracted text

<!-- script-extracted 2026-09-20 from the source: all text, table cells, speaker notes, image alt text, link targets, headers/footers, footnotes, comments; list numbering, formatting and images are not reproduced -->

CSE331: Automata and Computability

Assignment 02

Solve all the questions given below:

List five use cases for applying the knowledge from our CSE331 course. Describe in detail.

Proof L is a non-regular language using the Pumping Lemma: [If you are not seeing any Qs, then View in the light theme]

<https://www.codecogs.com/eqnedit.php?latex=L%20%3D%20%5C%7B%20w%20%5Cin%20%5C%7Ba%2Cb%5C%7D%5E*%20%5Cmid%20w%20%3D%20a%5Ei%20b%5Ej%2C%5C%3B%20i%20%3C%20j%2C%5C%3B%20i%20%5Cge%200%20%5C%7D#0>

<https://www.codecogs.com/eqnedit.php?latex=L%20%3D%20%5C%7B%200%5En%201%5Em%20%5Cmid%20n%20%3D%203q%20%2B%20r_1%2C%5C%3B%20m%20%3D%203q%20%2B%20r_2%2C%5C%3B%20q%20%5Cge%200%2C%5C%3B%200%20%5Cle%20r_1%2C%20r_2%20%5Cle%202%20%5C%7D#0>

<https://www.codecogs.com/eqnedit.php?latex=L%20%3D%20%5C%7B%20w%20%5Cin%20%5C%7B0%2C1%5C%7D%5E*%20%5Cmid%20w%20%3D%200%5E%7Bn!%7D%2C%5C%3B%20n%20%5Cge%200%20%5C%7D#0>

<https://www.codecogs.com/eqnedit.php?latex=L%20%3D%20%5C%7B%20w%20%5Cin%20%5C%7B0%2C1%5C%7D%5E*%20%5Cmid%20w%20%3D%201%200%5E%7Bn%5E2%7D%2C%5C%3B%20n%20%5Cge%200%20%5C%7D#0>

<https://www.codecogs.com/eqnedit.php?latex=L%20%3D%20%5C%7B%20w%20%5Cin%20%5C%7B0%2C1%5C%7D%5E*%20%5Cmid%20w%20%3D%200%5Ea%201%5Eb%201%5Ec%200%5Ed%2C%5C%3B%20a%20%2B%20b%20%3D%20c%20%2B%20d%2C%5C%3B%20a%2Cb%2Cc%2Cd%20%5Cge%200%20%5C%7D#0>

<https://www.codecogs.com/eqnedit.php?latex=L%20%3D%20%5C%7B%20w%20%5Cin%20%5CSigma%5E*%20%5Cmid%20w%20%3D%20a%5En%201%5Em%200%5Ek%2C%5C%3B%20%5Ctext%7Bwhere%20%7D%20n%20%3D%20m%20%5Ctext%7B%20or%20%7D%20n%20%5Cne%20k%20%5C%7D#0>

<https://www.codecogs.com/eqnedit.php?latex=%20L%20%3D%20%5C%7B%20w%20%5Cin%20%5C%7B0%2C1%2C2%5C%7D%5E*%20%5Cmid%20w%20%3D%200%5En%201%5Em%202%5E%7B%5C%2Cn-m%7D%2C%5C%3B%20n%20%5Cge%20m%20%5Cge%200%20%5C%7D#0>

L = {w ∈ {a,b}∗: w = aibj, where i > j, and j ≥ 0}

Design a Context Free Grammar for the Language:

L = {w ∈ {a,b,c,p,q,r,#}*: ai#ncjp2xqyrzbk where i=j+k, y=3x+z, z is even, n is odd, and i,j,k,n,x,y,z ≥ 0}

L = {w ∈ {0,1,2}*: w = 0i2j1k, [where ……conditions…… ] }

where…

i = k, i,k ≥ 1 and j ≥ 2

i = 3k, j is odd, and i,j,k ≥ 0

i is a multiple of two, k is two more than a multiple of 3, j = k+i, and i,j,k ≥ 0

i+j > k and i,j,k ≥ 0

i+k is even, j = i+k and j≥1

L = {w ∈ {0,1}*: the parity of 0s and 1s is different in w}

L = {w ∈ {0,1}*: the number of 0s and 1s are different in w}

[Hint: First, try to solve for an equal number of 0s and 1s in w]

L = {w ∈ {a, b}∗: w is a palindrome}. Design a CFG for <https://www.codecogs.com/eqnedit.php?latex=%5Coverline%7BL%7D#0> (Complement of L)

L = {w ∈ {a, b}∗: w does not contain any palindromic substring of length two}

L = {w#x | wR is a substring of x where w ∈ {0, 1}∗}. wR Denotes reverse of string, w.

L = {1i02j1k| i, j, k ≥ 0, 3i ≥ 4k + 2, j is not divisible by three}

Recall that for a string w, |w| denotes the length of w. Σ = {0,1}

L1 = {w ∈ Σ∗: w contains exactly two 1s}

L2 = {x#y : x ∈ Σ∗, y ∈ L1, |x| = |y|}

Construct a CFG for L2.

Recall that for a string w, |w| denotes the length of w. Σ = {0,1}

L1 = {w ∈ Σ∗: w contains at least three 1s}

L2 = {x#y : x ∈ (ΣΣ)∗, y ∈ L1, |x| = |y|}

Construct a CFG for L2.

Recall that for a string w, |w| denotes the length of w. Σ = {a,b}

L1 = {w ∈ Σ∗: w contains bb as a substring}

L2 = {w1#w2 : w1, w2 ∈ L1 and |w1| = |w2|}

Construct a CFG for L2.

For all the problems in Question(3), now construct the Pushdown Automata (PDA).

Derivation, Parse Tree, Ambiguity

i) Given the Context-Free Grammar, answer the following questions

A → A1 | 0A1 | 01

(a) Give a leftmost derivation for the string 001111.

(b) Sketch the parse tree corresponding to the derivation you gave in (a).

(c) Demonstrate that there are two more parse trees (apart from the one you already found in (b)) for the same string.

(d) Find a string w of length six such that w has exactly one parse tree in the grammar above. (1 point)

ii) Given the Context-Free Grammar, answer the following questions:

A → 1A | 1C | 0B | 00A

B → 0A | 1B | 00B

C → 0C0 | 0C1 | 1C0 | 1C1 | ε

(a) Give a leftmost derivation for the string 01011001. (3 points)

(b) Sketch the parse tree corresponding to the derivation you gave in (a). (2 points)

(c) Demonstrate that the given grammar is ambiguous by showing two more parse trees (apart from the one you already found in (b)) for the same string. (3 points)

(d) Find a string w of length six such that w has exactly one parse tree in the grammar above. (1 point)

(e) Design an unambiguous Context Free Grammar for the language represented by the given ambiguous grammar. (1 point)

Turing Machine

L = {13n| n ≥ 0}

L = {w ∈ {0,1,2}*:0i1j2k where i=j=k and i,j,k ≥ 0}

L = {w ∈ {0,1,2}*:0i#1j#2k where i*j=k and i,j,k ≥ 0} [Idea described in Sipser’s Book]

L = {w ∈ {a,b}*: w is an odd length palindrome}

L = {w#wR#w | w ∈ {a,b}*} [wR means reverse of w]

Let HALT_TM = { <M, w>: M is a TM and M halts on input w }. Given that HALT_TM is undecidable, prove that the collection of Turing-recognizable languages is not closed under complement.

—----------------------------------------------------------------------

—----------------------------------------------------------------------

CSE331: Automata and Computability

Practice Sheet

Prepared By: KKP

Question 1: Designing Context-Free Grammar

Give Context-free Grammar that generates the language

L = {w ∈ {0,1}*: w starts with ‘0’ and the length of w is even.}

L = {w ∈ {a,b}*: every second letter in w is a ‘b’.}

L = {w ∈ {a,b}*: the length of w is divisible by three}

L = {w ∈ {0,1}*: w starts with and ends with a different symbol.}

L = {w ∈ {a,b}*: the number of ‘a’ is at least the number of ‘b’ in w.}

L = {w ∈ {a,b}*: the length of w is two more than a multiple of six}

L = {w ∈ {0,1}*: w = 0n12n+3, n ≥ 0 }

L = {w ∈ {0,1}*: w = 0n1n, where n is odd.}

L = {w ∈ {0,1,2}*: w=0i1j2k where j ≥ 2i + 3k, and i, k ≥ 0}

L = {w ∈ {0,1,2,3}*: w=0i1j2k3m where i=m, j ≥ 3k+2, and m, k ≥ 0}

L = {w ∈ {0,1,2}*: w=0i1j2k where i > 2j + 3k, and j, k ≥ 0}

If A = {w ∈ {0, 1}* : w contains at least two 0s}, then construct L = {w ∈ {0, 1}∗: w = 03iv12i where v ∈ A and i ≥ 0} [marked as problem j in the solution]

Recall that for a string w, |w| denotes the length of w. Σ = {0,1}

L1 = {w ∈ Σ∗: w contains 11}

L2 = {x#y : y ∈ L1, x ∈ Σ∗, |x| = |y|}

Construct a CFG for L2.

L = {w ∈ {0,1}* : w1#w2: number of 0s w1 is equal to number of 1s in w2.}

L = {w ∈ {0,1}* : w1#w2: length of w2 is double of length of w1.}

Construct a Context-free Grammar for the language expressed by the following Finite Automata:

Convert the Regular Expression into a CFG: ( (ab)* + (a+b3) cb)*

Convert the Regular Expression into a CFG: ( a*b* + (ac+b*c) b)*

Convert the following DFA into a CFG:

Question 2: Derivation, Parse tree, Ambiguity

S→ASB|SS|SAS|A

A→ASS|BS|B

B→00|11|01|1

Given the above Context-Free Grammar, answer the following questions:

a) Show the Leftmost Derivation of 00010111

b) Sketch the parse tree corresponding to the derivation you gave in (a).

c) Show the Rightmost Derivation of 00010111

d) Sketch the parse tree corresponding to the derivation you gave in (c).

S → 0S1 | 1S0 | SS | 01 | 10

Given the above Context-Free Grammar, answer the following questions:

a) Show that the grammar above is ambiguous by demonstrating two different parse trees for 011010.

b) Find a string w of length six such that w has exactly one parse tree in the grammar above.

Question 3: Pushdown Automata

L = { w ∈ {0,1}* : w = 0n1n, where n is odd.}

L = { w ∈ {0,1}* : w = 0m1n, where m,n ≥ 1 and m≥n.}

L = { w ∈ {0,1}*: w | the length of w is divisible by four}

L1 = { w ∈ {0,1}*: the number of 1s in w is multiple of 3.}

L2 = { w ∈ {0,1}* : w contains even numbers of 0.}

Construct a PDA for L = { w ∈ {0,1}*: w = uv, where u ∈ L1,v ∈ L2 and |u| = |v| }

L = { w ∈ {0,1}* : w = 0i1j0k, where j = i+k and i, k ≥ 0.}

L = { w ∈ {0,1}* : w = 0i1j0k, where i+j = k and i, k ≥ 0.}

L = { w ∈ {0,1}* : w1#w2: number of 00 substrings in w1 is equal to number of 11 in w2.}

L = { wwR: w ∈ {0,1}* }

Here, wR denotes the reverse complement of the string w.

For example, 01001101 ∈ L, because the second half of the string, 1101, is the reverse complement of the first half, 0100, i.e. 1101 =0100R
