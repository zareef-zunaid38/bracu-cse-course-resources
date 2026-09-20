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
