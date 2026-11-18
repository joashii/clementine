# CMSC 141 Exam 10: Extensions to the Turing Machine (Lecture 16)

**Items:** 20 | **Difficulty:** Average | **Type:** Application, multiple choice

**Directions:** Choose the best answer: **a, b, c, d**, or **e** (none of the above). A multi-tape transition `x ; y , D | x' ; y' , D'` lists tape 1 then tape 2.

## Reference Machine

- **C2 (2-tape counter, tape 1 holds the input over {a,b}, tape 2 is the blank output tape):** q0: `b;b,R | □;□,S` (stay in q0); `a;a,R | □;1,R` (stay in q0); `□;□,S | □;□,S` goes to q1 (final, halts).

---

**1.** What does C2 leave on tape 2 for the input `aba`?
- a. 1
- b. 11
- c. 111
- d. 1111
- e. None of the above

**2.** A multi-tape TM has k tapes. How many read/write heads does it have?
- a. 1
- b. 2
- c. k + 1
- d. k
- e. None of the above

**3.** Which statement about multi-tape TMs is true?
- a. Every language accepted by a multi-tape TM is accepted by a single-tape TM
- b. Multi-tape TMs accept more languages than single-tape TMs
- c. Single-tape TMs accept only regular languages
- d. Multi-tape TMs always halt
- e. None of the above

**4.** A 3-tape TM decides { aⁿbⁿcⁿ } with three heads moving right together, one per block. On `aaabbbccc`, how many times does it loop in q1 before moving to q2?
- a. 2
- b. 4
- c. 3
- d. 9
- e. None of the above

**5.** In a multi-track TM with k tracks, each tape cell contains:
- a. A stack
- b. A k-tuple of symbols
- c. A single symbol
- d. A state
- e. None of the above

**6.** A 2-tape enumerator prints the strings of ba*. Which strings have been printed after round 3?
- a. b and baa
- b. ba and baa only
- c. b only
- d. b, ba, and baa
- e. None of the above

**7.** What can be said about the enumerator for ba*?
- a. It never halts, but every string of ba* is eventually printed
- b. It halts after printing b
- c. It halts after three rounds
- d. It halts when tape 2 is full
- e. None of the above

**8.** A nondeterministic TM accepts an input when:
- a. All computation paths accept
- b. The first path tried accepts
- c. At least one computation path accepts
- d. No path loops
- e. None of the above

**9.** The monus machine computes f(m,n) = max(m − n, 0) on the input 0ᵐ10ⁿ and leaves 0^f(m,n) on the tape. What is the output for 4 ∸ 2?
- a. 0
- b. 000
- c. 0000
- d. 000000
- e. None of the above

**10.** Which is the encoded input for 3 ∸ 5?
- a. 00000100
- b. 000100000
- c. 0001000
- d. 00010
- e. None of the above

**11.** How is a PDA related to a Turing machine?
- a. A PDA is a one-tape TM
- b. A PDA is a TM with three tapes
- c. A PDA is a two-tape TM in which one tape acts as a stack
- d. A PDA cannot be simulated by a TM
- e. None of the above

**12.** Which ordering of machine power is correct?
- a. (DFA = NFA) < DPDA < NPDA < TM
- b. DPDA < DFA < NPDA < TM
- c. TM < NPDA < DPDA < DFA
- d. NPDA < DPDA < TM
- e. None of the above

**13.** Why can the Church–Turing thesis not be proven as a theorem?
- a. TMs are too weak
- b. It is false
- c. It depends on the tape alphabet
- d. "Algorithm" is an informal notion
- e. None of the above

**14.** A Universal TM takes which inputs?
- a. Only the input w
- b. An encoded description of a TM M and an input x
- c. Only an encoded TM
- d. The outputs of two TMs
- e. None of the above

**15.** Which statement about the extensions (multi-tape, multi-track, 2D tape, nondeterminism) is correct?
- a. Each one adds computational power
- b. A 2D tape adds power
- c. They make TMs easier to program, not more powerful
- d. Nondeterminism lets a TM decide more languages
- e. None of the above

**16.** In C2, what does the transition `a;a,R | □;1,R` do?
- a. Tape 1: read a, write a, move R; tape 2: read □, write 1, move R
- b. Tape 1: read □, write a; tape 2: read 1
- c. Both tapes read a
- d. Tape 2 reads a and writes 1
- e. None of the above

**17.** When C2 reads `b` on tape 1, what does it do?
- a. Writes 1 on tape 2
- b. Halts
- c. Moves both heads right
- d. Moves the tape 1 head right and leaves tape 2 unchanged
- e. None of the above

**18.** A nondeterministic TM has δ(q,0) = {(q,0,R), (q1,1,S)}. How many choices does it have in state q reading 0?
- a. 0
- b. 1
- c. 2
- d. 3
- e. None of the above

**19.** A multi-track TM can simulate t steps of a k-tape TM (fixed k) in at most:
- a. O(t) steps
- b. O(t²) steps
- c. O(2ᵗ) steps
- d. O(log t) steps
- e. None of the above

**20.** Which of these languages can be decided by a 2D-tape, 5-head nondeterministic TM but **not** by a basic single-tape deterministic TM?
- a. { aⁿbⁿcⁿ }
- b. Palindromes over {a,b}
- c. { x c x }
- d. { aⁿbⁿcⁿdⁿ }
- e. None of the above (extensions add no power)

---

## Answer Key

| # | Ans | Why |
|---|-----|-----|
| 1 | b | one 1 per a: two a's give 11 |
| 2 | d | one head per tape |
| 3 | a | multi-tape and single-tape have the same power |
| 4 | c | one pass per a, b, c triple |
| 5 | b | one symbol per track in each cell |
| 6 | d | b, ba, baa |
| 7 | a | an enumerator need not halt |
| 8 | c | acceptance needs one accepting path |
| 9 | e | output is 00 (not offered) |
| 10 | b | 000 1 00000 |
| 11 | c | a PDA is a two-tape TM, one tape used as a stack |
| 12 | a | (DFA = NFA) < DPDA < NPDA < TM |
| 13 | d | an informal notion cannot be formally proven |
| 14 | b | the encoding of M and the input x |
| 15 | c | extensions are for convenience, not power |
| 16 | a | tape 1 reads a and moves R, tape 2 writes 1 and moves R |
| 17 | d | skip b, tape 2 stays |
| 18 | c | two possible moves |
| 19 | b | O(t²) |
| 20 | e | all four languages are decidable by a basic TM, and extensions add no power |
