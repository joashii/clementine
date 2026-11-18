# CMSC 141 Exam 5: Normal Forms of Grammars (Lecture 12)

**Items:** 20 | **Difficulty:** Average | **Type:** Application, multiple choice

**Directions:** Choose the best answer: **a, b, c, d**, or **e** (none of the above). Uppercase letters are variables, lowercase letters are terminals.

**Reminders:** Chomsky Normal Form (CNF): every production is A → BC or A → a. Greibach Normal Form (GNF): every production is A → aα, where a is a terminal and α is a string of zero or more variables. Both forms describe the language without ε.

---

**1.** Which grammar is in Chomsky Normal Form?
- a. S → aSb | ab
- b. S → A | AB
- c. S → AB | a
- d. S → ε | AB
- e. None of the above

**2.** Which grammar is in Greibach Normal Form?
- a. S → aSB | b; B → b
- b. S → Ab | a
- c. S → aSb | ab
- d. S → Sa | a
- e. None of the above

**3.** For P = { S → AB, A → aAA | ε, B → bBB | ε }, which variables are nullable?
- a. {A}
- b. {S}
- c. {A, B}
- d. {S, A, B}
- e. None of the above

**4.** After eliminating ε-productions from P in item 3, the S-productions are:
- a. S → AB
- b. S → AB | A | B
- c. S → AB | A | B | ε
- d. S → A | B
- e. None of the above

**5.** Eliminate the unit productions from { A → B | a, B → aC | C | b, C → A | c }. The result is:
- a. A → a; B → aC | b; C → c
- b. A → B | a; B → C | b; C → A | c
- c. A → a | aC | b | c; B → a | aC | b | c; C → a | aC | b | c
- d. A → aC | b | c; B → a | b; C → a | c
- e. None of the above

**6.** Remove the useless symbols of { S → AB | a, A → b }. The result is:
- a. S → AB | a; A → b
- b. S → AB
- c. S → a; A → b
- d. S → a
- e. None of the above

**7.** In { S → aA, A → a, B → b }, which variable is unreachable?
- a. S
- b. B
- c. A
- d. None of them
- e. None of the above

**8.** In the first CNF step, S → abAB (terminals mixed with variables) becomes:
- a. S → XaXbAB; Xa → a; Xb → b
- b. S → aXbAB; Xb → b
- c. S → XabAB; Xab → ab
- d. S → aSb | ab
- e. None of the above

**9.** To convert S → ABC to CNF, you rewrite it as:
- a. S → AB | C
- b. S → AX | BC
- c. S → AX; X → BC
- d. S → ABX; X → C
- e. None of the above

**10.** Which grammar is a CNF for S → ab | aSb?
- a. S → ab | aX; X → Sb
- b. S → AB | ASB; A → a; B → b
- c. S → AB | AX; X → S; A → a; B → b
- d. S → AB | AX; X → SB; A → a; B → b
- e. None of the above

**11.** G is in GNF. A string x in L(G) has length 6. How many derivation steps does x need?
- a. 5
- b. 6
- c. 7
- d. 12
- e. None of the above

**12.** G is in CNF and generates a string of length 4. How many production applications are needed?
- a. 4
- b. 5
- c. 6
- d. 8
- e. None of the above

**13.** Eliminate the direct left recursion in A → Aa | b:
- a. A → bX; X → aX | ε
- b. A → aA | b
- c. A → bA | a
- d. A → Ab | a
- e. None of the above

**14.** In { A → Ba | c, B → Ab | d }, substituting A into the B-production gives:
- a. B → Aab | d
- b. A → Bab | c
- c. B → Bab | cb | d
- d. B → abB | cb | d
- e. None of the above

**15.** Eliminating the direct left recursion in B → Bab | cb | d gives:
- a. B → cbX | dX; X → aX | ε
- b. B → cbX | dX; X → abX | ε
- c. B → Xcb | Xd; X → abX
- d. B → cb | d; X → abX | ε
- e. None of the above

**16.** In the grammar { S → aSB | b, B → b }, which production violates GNF?
- a. S → aSB
- b. S → b
- c. B → b
- d. All three violate GNF
- e. None of the above

**17.** In P = { S → A | AX, A → a | aA, X → ab | aXb }, which productions violate GNF?
- a. Only S → A
- b. S → A, S → AX, X → ab, and X → aXb
- c. A → a and A → aA
- d. Only X → aXb
- e. None of the above

**18.** Convert X → ab | aXb to GNF:
- a. It is already in GNF
- b. X → aB | aXb; B → b
- c. X → aB | XB; B → b
- d. X → aB | aXB; B → b
- e. None of the above

**19.** In the CNF-to-GNF conversion, Z → BB is fixed by substituting B → b | bB | bZ for the leftmost B. The result is:
- a. Z → bB | bBB | bZB
- b. Z → bB | bB | bZ
- c. Z → B | bBB
- d. Z → bBB | bZB
- e. None of the above

**20.** A GNF for the grammar S → aSb | ab (nothing else) is:
- a. S → aSb | ab (already GNF)
- b. S → aSB | b; B → b
- c. S → aSB | aB; B → b
- d. S → aS | b
- e. None of the above

---

## Answer Key

| # | Ans | Why |
|---|-----|-----|
| 1 | c | bodies are two variables or one terminal; ε is not allowed |
| 2 | a | each body is one terminal followed by variables |
| 3 | d | A and B go to ε directly; S → AB makes S nullable |
| 4 | b | keep or drop A and B, but not both (no S → ε) |
| 5 | c | every variable inherits the non-unit productions of A, B, C |
| 6 | d | B is non-generating; then A is unreachable |
| 7 | b | B cannot be reached from S |
| 8 | a | Xa → a, Xb → b replace terminals in long bodies |
| 9 | c | cascade of two-variable bodies |
| 10 | d | S → AB \| AX, X → SB (A → a, B → b) |
| 11 | b | GNF derivation takes exactly \|x\| steps |
| 12 | e | CNF needs 2n − 1 = 7 steps |
| 13 | a | A → βA', A' → αA' \| ε with α = a, β = b |
| 14 | c | B → (Ba \| c)b \| d |
| 15 | b | α = ab, β = cb \| d |
| 16 | e | S → aSB, S → b, B → b are all in GNF |
| 17 | b | productions starting with a variable, or with a terminal followed by a terminal |
| 18 | d | X → aB \| aXB with B → b |
| 19 | a | Z → (b \| bB \| bZ)B |
| 20 | c | S → aSB \| aB, B → b |
