# CMSC 141 Exam 1: Pushdown Automata (Lecture 8)

**Items:** 20 | **Difficulty:** Average | **Type:** Application, multiple choice

**Directions:** Choose the best answer: **a, b, c, d**, or **e** (none of the above). Notation: transition δ(q, input, top) = (p, replacement for top); ID = (state, remaining input, stack with top on the left); Z is the bottom-of-stack marker.

## Reference Machines

- **M** (final-state acceptance): Q = {q0,q1,q2}, Σ = {a,b}, Γ = {A,Z}, F = {q2}.
  δ(q0,a,ε) = (q0,A); δ(q0,b,A) = (q1,ε); δ(q1,b,A) = (q1,ε); δ(q1,ε,Z) = (q2,ε).
- **N** (final-state acceptance): Q = {q0,q1,q2}, Σ = {a,b}, Γ = {A,Z}, F = {q2}.
  δ(q0,a,ε) = (q0,A); δ(q0,b,A) = (q1,ε); δ(q1,b,A) = (q1,ε); δ(q1,b,Z) = (q2,ε).
- **N0** (empty-stack acceptance): δ(q0,a,ε) = (q0,A); δ(q0,b,A) = (q0,ε); δ(q0,ε,Z) = (q0,ε).

---

**1.** Which is the correct sequence of IDs of M on the input `ab`?
- a. (q0,ab,Z) ⊢ (q0,b,Z) ⊢ (q1,ε,Z) ⊢ (q2,ε,ε)
- b. (q0,ab,Z) ⊢ (q0,b,AZ) ⊢ (q1,ε,AZ) ⊢ (q2,ε,ε)
- c. (q0,ab,Z) ⊢ (q1,b,AZ) ⊢ (q1,ε,Z) ⊢ (q2,ε,ε)
- d. (q0,ab,Z) ⊢ (q0,b,AZ) ⊢ (q1,ε,Z) ⊢ (q2,ε,ε)
- e. None of the above

**2.** What happens when M runs on `aab`?
- a. Accepted; it ends at (q2,ε,ε)
- b. Rejected; it gets stuck at (q1,ε,AZ)
- c. Rejected; it gets stuck at (q0,ε,AAZ)
- d. Accepted; it ends at (q1,ε,Z)
- e. None of the above

**3.** After M (starting in q0) reads `aaaa`, what is on the stack, top first?
- a. A
- b. AAAA
- c. AAAAZ
- d. ZAAAA
- e. None of the above

**4.** How many moves (⊢ steps) does M make to accept `aaabbb`?
- a. 6
- b. 7
- c. 8
- d. 9
- e. None of the above

**5.** Which string is accepted by M?
- a. ε
- b. `abb`
- c. `aabb`
- d. `ba`
- e. None of the above

**6.** You want M changed so it also accepts ε (so that L = { aⁿbⁿ | n ≥ 0 }) without accepting anything else new. Which change works?
- a. Add δ(q0,ε,Z) = (q2,ε)
- b. Add δ(q0,ε,A) = (q2,ε)
- c. Add δ(q1,a,Z) = (q2,ε)
- d. Make q0 a final state as well
- e. None of the above

**7.** Which language requires a nondeterministic PDA (a DPDA is not enough)?
- a. { aⁿbⁿ | n > 0 }
- b. { x c xᴿ | x ∈ {a,b}* }
- c. { aⁿbⁿ⁺¹ | n > 0 }
- d. All palindromes over {a,b}
- e. None of the above

**8.** Which pair of transitions makes a PDA nondeterministic?
- a. δ(q0,a,ε) = (q0,A) and δ(q0,b,A) = (q1,ε)
- b. δ(q0,a,Z) = (q0,AZ) and δ(q0,ε,Z) = (q1,Z)
- c. δ(q0,a,A) = (q0,AA) and δ(q0,a,Z) = (q0,AZ)
- d. δ(q0,a,A) = (q0,ε) and δ(q1,a,A) = (q1,ε)
- e. None of the above

**9.** N0 accepts by empty stack. Which of these strings is **not** accepted by N0?
- a. ε
- b. `ab`
- c. `aabb`
- d. `abab`
- e. None of the above (all of them are accepted)

**10.** In M, which ID immediately follows (q1,b,AZ)?
- a. (q1,ε,AZ)
- b. (q2,ε,ε)
- c. (q1,ε,Z)
- d. (q0,ε,Z)
- e. None of the above

**11.** Which string does N accept?
- a. `abb`
- b. `aabb`
- c. `ab`
- d. `abbb`
- e. None of the above

**12.** What is L(N)?
- a. { aⁿbⁿ⁻¹ | n > 1 }
- b. { aⁿbⁿ | n > 0 }
- c. { aⁿ⁺¹bⁿ | n > 0 }
- d. { aⁿbⁿ⁺¹ | n > 0 }
- e. None of the above

**13.** You are designing a PDA for { x c xᴿ | x ∈ {a,b}* } that pushes each symbol of x. What should the transition on the symbol `c` do?
- a. Push C onto the stack
- b. Pop the top symbol
- c. Switch to the matching (popping) state, leaving the stack unchanged
- d. Accept immediately
- e. None of the above

**14.** What stack alphabet Γ does the PDA in item 13 need?
- a. {A, Z}
- b. {A, B, Z}
- c. {A, B, C, Z}
- d. {Z}
- e. None of the above

**15.** In N, δ(q1,b,A) = (q1,ε). From the ID (q1,bbb,AAZ), what is the next ID?
- a. (q1,bb,AZ)
- b. (q1,bb,AAZ)
- c. (q1,bbb,AZ)
- d. (q1,bb,AAAZ)
- e. None of the above

**16.** A PDA has read its whole input and is in a non-final state, with an empty stack. Under which acceptance mode is the string accepted?
- a. Final state only
- b. Empty stack only
- c. Both final state and empty stack
- d. Neither
- e. None of the above

**17.** Which machine is enough to recognize { aⁿbⁿcⁿ | n > 0 }?
- a. A DFA
- b. A DPDA
- c. An NPDA
- d. A PDA that accepts by empty stack
- e. None of the above (a more powerful machine is needed)

**18.** How many moves does N make to accept `abb`?
- a. 2
- b. 4
- c. 5
- d. 3
- e. None of the above

**19.** A PDA for all palindromes over {a,b} reads `abba`, pushing the first two symbols and then popping. What lets it know when to switch?
- a. It looks ahead to the end of the input
- b. It counts the input length in its state
- c. It nondeterministically guesses the middle with an ε-transition
- d. It uses a second stack
- e. None of the above

**20.** Which input makes M get stuck before consuming all of the input?
- a. `aabb`
- b. `ab`
- c. `abb`
- d. `aaabbb`
- e. None of the above

---

## Answer Key

| # | Ans | Why |
|---|-----|-----|
| 1 | d | push A, pop A in q1, ε-move on Z |
| 2 | b | (q0,aab,Z) ⊢ (q0,ab,AZ) ⊢ (q0,b,AAZ) ⊢ (q1,ε,AZ); top is A, not Z |
| 3 | c | four A's pushed above Z |
| 4 | b | 3 pushes + 3 pops + 1 ε-move |
| 5 | c | aabb = a²b² |
| 6 | a | ε-move at (q0,ε,Z) accepts ε; making q0 final would accept `aaa` |
| 7 | d | no center marker, so the middle must be guessed |
| 8 | b | an input move and an ε-move share (q0, Z) |
| 9 | e | all four strings are accepted by N0 |
| 10 | c | pop one A: (q1,ε,Z) |
| 11 | a | abb = a¹b² |
| 12 | d | a^n b^(n+1), n > 0 |
| 13 | c | c marks the middle; no stack change |
| 14 | b | one symbol each for a, b, plus Z |
| 15 | a | pop one A, consume one b |
| 16 | b | acceptance by empty stack |
| 17 | e | not context-free (needs a Turing machine) |
| 18 | d | push A, pop A (to Z), pop Z |
| 19 | c | nondeterministic guess of the middle |
| 20 | c | (q1,b,Z) has no transition |
