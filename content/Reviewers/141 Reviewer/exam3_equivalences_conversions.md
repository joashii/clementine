# CMSC 141 Exam 3: Equivalences and Conversions, CFG to NPDA (Lecture 10)

**Items:** 20 | **Difficulty:** Average | **Type:** Application, multiple choice

**Directions:** Choose the best answer: **a, b, c, d**, or **e** (none of the above). Use the lecture's construction: a one-state NPDA (state q) that accepts by empty stack, with Σ = T, Γ = V ∪ T, and the start variable as the initial stack symbol. Each variable-production becomes an ε-transition; each terminal gets a matching pop transition. An ID is (remaining input, stack with top on the left).

**Grammars:** G1: S → ε | aSb. G2: S → aSa | bSb | c.

---

**1.** For G1, which is the transition for the terminal `b`?
- a. δ(q,b,S) = {(q,b)}
- b. δ(q,b,b) = {(q,ε)}
- c. δ(q,ε,b) = {(q,b)}
- d. δ(q,b,ε) = {(q,S)}
- e. None of the above

**2.** For G1, which transition comes from the production S → aSb?
- a. δ(q,a,S) = {(q,Sb)}
- b. δ(q,ε,S) contains (q,bSa)
- c. δ(q,ε,S) contains (q,aSb)
- d. δ(q,ε,aSb) = {(q,S)}
- e. None of the above

**3.** How many transitions does the NPDA for G1 have?
- a. 2
- b. 3
- c. 5
- d. 6
- e. None of the above

**4.** What is the stack alphabet Γ of the NPDA built from G2?
- a. {S, a, b, c}
- b. {S}
- c. {a, b, c, Z}
- d. {S, a, b}
- e. None of the above

**5.** What is the initial stack symbol of the NPDA built from G2?
- a. Z
- b. ε
- c. a
- d. S
- e. None of the above

**6.** Run the NPDA for G1 on `aabb`, choosing S → aSb, then matching a, then S → aSb. After these three moves, what is the stack (top first)?
- a. aSbb
- b. Sbb
- c. aSb
- d. SSb
- e. None of the above

**7.** Which transition set is the NPDA for G2?
- a. δ(q,ε,S) = {(q,aSa),(q,bSb),(q,c)} only
- b. δ(q,ε,S) = {(q,aSa),(q,bSb),(q,c)}; δ(q,a,a) = δ(q,b,b) = δ(q,c,c) = {(q,ε)}
- c. δ(q,a,S) = {(q,Sa)}; δ(q,b,S) = {(q,Sb)}; δ(q,c,S) = {(q,ε)}
- d. δ(q,ε,S) = {(q,aSa),(q,bSb)}; δ(q,c,c) = {(q,ε)}
- e. None of the above

**8.** Which string does the NPDA for G2 accept?
- a. `abcab`
- b. `abcba`
- c. `abba`
- d. `abc`
- e. None of the above

**9.** For G: S → AB, A → a, B → b, which transition comes from S → AB so that A is read first?
- a. δ(q,ε,AB) = {(q,S)}
- b. δ(q,a,S) = {(q,B)}
- c. δ(q,ε,S) = {(q,BA)}
- d. δ(q,ε,S) = {(q,AB)}
- e. None of the above

**10.** For G: S → SS | aSb | ε, how many transitions does the NPDA have?
- a. 5
- b. 3
- c. 4
- d. 6
- e. None of the above

**11.** In the NPDA for G: S → SS | aSb | ε, which production must be chosen first for the input `abab` to be accepted?
- a. S → ε
- b. Read a
- c. S → aSb
- d. S → SS
- e. None of the above

**12.** By which acceptance condition does the converted NPDA accept?
- a. Final state with empty stack
- b. Final state only
- c. Neither final state nor empty stack
- d. Empty stack
- e. None of the above

**13.** The palindrome grammar S → ε | a | b | aSa | bSb is converted. How many choices does δ(q,ε,S) contain?
- a. 5
- b. 3
- c. 4
- d. 6
- e. None of the above

**14.** An NPDA accepts { aⁿbⁿ | n > 0 }. By the equivalence of NPDAs and CFGs, which grammar generates the same language?
- a. S → aSb | ε
- b. S → aSb | ab
- c. S → aS | bS | ε
- d. S → SS | ab
- e. None of the above

**15.** Which language is accepted by **no** NPDA?
- a. Balanced parentheses
- b. Palindromes over {a,b}
- c. { aⁿbⁿ | n > 0 }
- d. { aⁿbⁿcⁿ | n > 0 }
- e. None of the above

**16.** How many moves does the NPDA for G1 make to accept `aabb` (using the sequence S → aSb, a, S → aSb, a, S → ε, b, b)?
- a. 7
- b. 5
- c. 6
- d. 8
- e. None of the above

**17.** Why is the NPDA built from G1 nondeterministic?
- a. δ(q,ε,S) offers two different choices
- b. It has two final states
- c. It reads two symbols per move
- d. It needs a second stack
- e. None of the above

**18.** For the production A → bBc, which transition is added?
- a. δ(q,ε,A) = {(q,cBb)}
- b. δ(q,b,A) = {(q,Bc)}
- c. δ(q,ε,A) = {(q,bBc)}
- d. δ(q,ε,bBc) = {(q,A)}
- e. None of the above

**19.** For G: S → aSb | ab, δ(q,ε,S) = {(q,aSb),(q,ab)}. How many moves are needed to accept `ab`?
- a. 2
- b. 4
- c. 3
- d. 5
- e. None of the above

**20.** For G: S → 0S1 | ε, what are Σ and Γ of the converted NPDA?
- a. Σ = {S}, Γ = {0,1}
- b. Σ = {0,1}, Γ = {0,1}
- c. Σ = {0,1,S}, Γ = {0,1,S}
- d. Σ = {0,1}, Γ = {S,0,1}
- e. None of the above

---

## Answer Key

| # | Ans | Why |
|---|-----|-----|
| 1 | b | terminal b: pop matching b |
| 2 | c | production pushes aSb with a on top |
| 3 | e | 4 transitions: a, b, S → ε, S → aSb |
| 4 | a | Γ = V ∪ T = {S,a,b,c} |
| 5 | d | the start variable S |
| 6 | a | (aabb,S) ⊢ (aabb,aSb) ⊢ (abb,Sb) ⊢ (abb,aSbb) |
| 7 | b | one ε-transition set plus one pop per terminal |
| 8 | b | abcba = x c xᴿ with x = ab |
| 9 | d | δ(q,ε,S) = {(q,AB)}, A on top |
| 10 | a | 2 terminals + 3 productions |
| 11 | d | any other first choice dies: abab = (ab)(ab) |
| 12 | d | acceptance by empty stack |
| 13 | a | five productions for S |
| 14 | b | S → aSb \| ab generates aⁿbⁿ, n > 0 |
| 15 | d | aⁿbⁿcⁿ is not context-free |
| 16 | a | 3 production moves + 4 matches = 7 |
| 17 | a | two ε-choices for S |
| 18 | c | every production becomes an ε-transition pushing its body |
| 19 | c | S → ab, match a, match b |
| 20 | d | Σ = T, Γ = V ∪ T |
