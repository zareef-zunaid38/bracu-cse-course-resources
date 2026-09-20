# CSE331 Assignment 1 Summer 2025 (FDM)

Assignment question paper for CSE331 Automata and Computability Summer 2025 (Faculty: FDM), covering DFA state diagrams, product construction, NFA subset construction, regular expressions, RE to NFA conversions, and state elimination.
Docx last modified: 17-07-2025.

## Content

### Assignment Metadata
- **Course:** CSE331: Automata and Computability
- **Semester:** Summer 2025 | Assignment 1
- **Faculty:** FDM
- **Deadline:** 24th July, 2025

---

### Question 1 [10 Marks]
Draw state diagram for a DFA of the following regular languages:
- **$L_1$:** $\{w \in \text{string that has "b" in the second last digit}\}, \Sigma = \{a, b\}$
- **$L_2$:** $\{w \in \text{a string starts with 'ba' and contains 'bba'}\}, \Sigma = \{a, b\}$
- **$L_3$:** $\{w \in \text{a string where 0 is followed by at least one 1}\}, \Sigma = \{0, 1\}$
- **$L_4$:** $\{w \in \{0, 1\}^* \mid w \text{ ends with 0 and does not contain the substring 11}\}$
- **$L_5$:** $\{w \in \{a, b\}^* : \text{length of } w \text{ is multiple of } 3 \cap \text{contains at least two a's}\}$

---

### Question 2 [10 Marks]
Let $\Sigma = \{a, b\}$. Consider the following languages over $\Sigma$:
- $L_1 = \{w : w \text{ a string that starts and ends with different symbol}\}$
- $L_2 = \{w : \text{a string that has subsequence of 'aa'}\}$
- $L_3 = \{w : \text{a string that contains 'aba' and ends with 'b'}\}$
- $L_4 = \{w \in \{01, 1\}^*\}$

**Tasks:**
- **(a)** Give the state diagram for a DFA that recognizes $L_1$.
- **(b)** How many states will be there for $L_1 \cap L_2$, if using the cross product rule?
- **(c)** Draw a DFA state diagram for $L_1 \cap L_2$.
- **(d)** Find all the unique strings of length four for $L_4^*$.
- **(e)** Give the state diagram for a DFA that recognizes $L_3$.

---

### Question 3 [5 Marks]
Given an NFA specification:
- **(a)** [1 Mark] When converting the given NFA to an equivalent DFA using the subset construction method, what is the maximum number of states that the resulting DFA can have?
- **(b)** [1 Mark] Identify the subsets of states from the given NFA that will correspond to the rejecting states in its equivalent DFA.
- **(c)** [1 Mark] Determine the $\epsilon$-closure of state $q_2$ in the given NFA ($	ext{ECLOSE}(q_2)$).
- **(d)** [1 Mark] What is $\delta(\{q_0, q_4\}, a)$ in the given NFA? List all the states. [Recall: $\delta(\{q\}, a)$ refers to the set of states the NFA transitions to when in state $q$ with input $a$.]
- **(e)** [1 Mark] What would be the start state of the converted DFA? Write the subset of the start state.

---

### Question 4 [5 Marks]
Draw state diagram for an NFA of the following regular languages:
- **$L_1$:** $\{w \in \text{a string that contains 'aba'} \cap \text{ends with 'aab'}\}, \Sigma = \{a, b\}$
- **$L_3$:** $\{w \in \text{a string where third last symbol is 'b'}\}, \Sigma = \{a, b\}$

---

### Question 5 [20 Marks]
Let $\Sigma = \{0, 1\}$. Give regular expressions generating each of the following languages over $\Sigma$:
- **(a)** $\{w : w \text{ starts with a 1 and ends in a 0}\}$
- **(b)** $\{w : \text{the length of } w \text{ is even}\}$
- **(c)** $\{w : \text{every 1 in } w \text{ is followed by an even number of 0s}\}$
- **(d)** $\{w : w \text{ does not contain 10}\}$
- **(e)** $\{w : 10 \text{ appears in } w \text{ exactly once}\}$  
  *(Hint: If $w = x10y$, what can you say about $x$ and $y$?)*
- **(f)** $\{w : w \text{ containing strings where 0's and 1's are alternate}\}$
- **(g)** $\{w : w \text{ containing strings where every third position in } w \text{ is 1}\}$
- **(h)** $\{w : w \text{ containing strings where every 1 in } w \text{ is followed by at least two 0s}\}$
- **(i)** $\{w : w \text{ containing strings where every third position in } w \text{ is } 1 \cap \text{every 1 in } w \text{ is followed by at least two 0s}\}$
- **(j)** $\{w : w \text{ containing strings that end in three consecutive 1's}\}$

---

### Question 6 [5 Marks]
Convert Regular Expressions to equivalent NFAs (using Thompson's construction):
1. $(a^+ + b)^* b + a(ba)^*$
2. $(a + b + c)^* b + c^+$

---

### Question 7 [5 Marks]
Convert the given DFA into an equivalent regular expression using the **state elimination method**.
- **Constraint:** Must follow this exact sequence to eliminate states: $C, D, E, F, A, B$.
- Show all intermediate Generalized NFA (GNFA) steps and state transition expressions.
