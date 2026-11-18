# CMSC 141 Exam 4: Ambiguous Grammars (Lecture 11)

**Items:** 20 | **Difficulty:** Average | **Type:** Application, multiple choice

**Directions:** Choose the best answer: **a, b, c, d**, or **e** (none of the above). A grammar is ambiguous if some string it generates has more than one parse tree.

---

**1.** G: S → S-S | 3 | 2. How many different parse trees does `3-2-2` have?
- a. 1
- b. 2
- c. 3
- d. 4
- e. None of the above

**2.** Evaluating the two parse trees of `3-2-2` from item 1 gives which pair of values?
- a. −1 and 7
- b. 1 and 3
- c. −1 and 1
- d. 3 and 3
- e. None of the above

**3.** G: S → S+S | S*S | (S) | 3. The two parse trees of `3+3*3` evaluate to which values?
- a. 12 and 18
- b. 9 and 18
- c. 12 and 27
- d. 18 and 27
- e. None of the above

**4.** Which grammar is an unambiguous grammar for +, * with usual precedence and left associativity?
- a. E → E+E | E*E | 3
- b. S → S+S | S*S | (S) | 3
- c. E → E+T | T; T → T*F | F; F → 3 | (E)
- d. E → E+E | T
- e. None of the above

**5.** In E → E-T | T; T → 3 | 2 | (E), what enforces left associativity of `-`?
- a. The number of nonterminals
- b. The choice of terminals
- c. The start variable
- d. The left recursion in E → E-T
- e. None of the above

**6.** To make subtraction right-associative, E → E-T | T should be changed to:
- a. E → E-E | T
- b. E → T-E | T
- c. T → T-F | F
- d. E → T-T | E
- e. None of the above

**7.** Which grammar is ambiguous?
- a. S → aSb | ε
- b. S → aS | a
- c. S → SS | (S) | ε
- d. S → (S)S | ε
- e. None of the above

**8.** With the "nearest if" convention, to which `if` does the `else` belong in `if c then if c then b else b`?
- a. The inner if
- b. The outer if
- c. Both ifs
- d. Neither
- e. None of the above

**9.** Which grammar removes the dangling-else ambiguity of S → B | if C then S | if C then S else S?
- a. S → B | if C then S | if C then S else S
- b. S → if C then S else S | B
- c. S → if C then S | B
- d. S → M | U; M → if C then M else M | B; U → if C then S | if C then M else U
- e. None of the above

**10.** G: S → aab | aSB, B → b | ε. Which string has more than one parse tree?
- a. `aaab`
- b. `aaaab`
- c. `aaaabb`
- d. `aab`
- e. None of the above

**11.** Which unambiguous grammar generates the same language { aᵐbⁿ | m > n > 0 } as the grammar of item 10?
- a. S → aab | aSB; B → b | ε
- b. S → AX; A → a | aA; X → ab | aXb
- c. S → aSb | a
- d. S → aS | Sb
- e. None of the above

**12.** In E → E+T | T; T → T*F | F; F → 3 | (E), which operator binds tighter and why?
- a. `+`, because it appears first
- b. Both equally
- c. It depends on the input string
- d. `*`, because it is generated at a deeper grammar level
- e. None of the above

**13.** You must add a unary minus so that `-3*2` is unambiguously parsed as (−3)*2 in E → E+T | T; T → T*F | F; F → 3 | 2 | (E). Which change works?
- a. E → -E | E+T | T
- b. F → -F | 3 | 2 | (E)
- c. S → -S | E (S is a new start symbol)
- d. T → -T | T*F | F
- e. None of the above

**14.** G: S → true | false | S or S | S and S | not S | (S). How many parse trees does `not true and false` have?
- a. 4
- b. 3
- c. 2
- d. 1
- e. None of the above

**15.** In S → S or A | A; A → A and N | N; N → not N | F; F → true | false | (S), how is `true or false and false` grouped?
- a. true or (false and false)
- b. (true or false) and false
- c. Both parses are possible
- d. It cannot be parsed
- e. None of the above

**16.** Which language is inherently ambiguous (every CFG for it is ambiguous)?
- a. { aⁿbⁿ | n > 0 }
- b. Palindromes over {a,b}
- c. { aᵖbᵠcʳ | p = q or q = r }
- d. Balanced parentheses
- e. None of the above

**17.** You are asked to write a program that decides whether any given CFG is ambiguous. What is the correct situation?
- a. No such algorithm exists
- b. An O(n³) algorithm exists
- c. It works only for grammars with no ε-productions
- d. It works via CYK parsing
- e. None of the above

**18.** The English sentence "The boy saw the man with a telescope" has how many parses in the lecture's example?
- a. 1
- b. 3
- c. 4
- d. 2
- e. None of the above

**19.** Which grammar is an unambiguous grammar for the same language as S → SS | (S) | ε?
- a. S → (S)S | ε
- b. S → (S)(S) | ε
- c. S → (S) | ε
- d. S → SS | ε
- e. None of the above

**20.** G: S → S-S | 3 | 2. How many parse trees does `3-2-2-2` have?
- a. 3
- b. 4
- c. 5
- d. 6
- e. None of the above

---

## Answer Key

| # | Ans | Why |
|---|-----|-----|
| 1 | b | (3-2)-2 and 3-(2-2) |
| 2 | e | the values are −1 and 3 |
| 3 | a | (3+3)*3 = 18 and 3+(3*3) = 12 |
| 4 | c | precedence by levels, associativity by recursion direction |
| 5 | d | left recursion groups from the left |
| 6 | b | right recursion groups from the right |
| 7 | c | many trees for `()` (ε and SS) |
| 8 | a | else attaches to the nearest unmatched if |
| 9 | d | matched/unmatched split |
| 10 | c | `aaaabb`: B → b, B → ε or B → ε, B → b |
| 11 | b | unique split into extra a's and aⁿbⁿ |
| 12 | d | deeper level means it is grouped first |
| 13 | b | unary minus at the factor level; d is itself ambiguous |
| 14 | c | (not true) and false, or not (true and false) |
| 15 | a | `and` is at a deeper level than `or` |
| 16 | c | the lecture's inherently ambiguous language |
| 17 | a | ambiguity of CFGs is undecidable |
| 18 | d | telescope with the boy, or with the man |
| 19 | a | S → (S)S \| ε is unambiguous |
| 20 | c | Catalan number C₃ = 5 |
