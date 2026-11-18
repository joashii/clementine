# CMSC 141 Exam 9: Turing Machines (Lecture 15)

**Items:** 20 | **Difficulty:** Average | **Type:** Application, multiple choice

**Directions:** Choose the best answer: **a, b, c, d**, or **e** (none of the above). A transition `x ; y , D` means: read x, write y, move the head D ∈ {L, S, R}. □ is the blank. The head starts on the first symbol of the input.

## Reference Machines

- **T1 (successor, input is a binary number):** q0: `0;0,R` and `1;1,R` stay in q0; `□;□,L` goes to q1. q1: `1;0,L` stays in q1; `0;1,L` and `□;1,L` go to q2. q2: `0;0,L` and `1;1,L` stay in q2; `□;□,R` goes to q3 (final, halts).
- **T2 (counts a's, output in unary):** q0: `a;□,R` goes to q1; `b;□,R` stays in q0; `1;1,S` goes to q3. q1: `a;a,R`, `b;b,R`, `1;1,R` stay in q1; `□;1,L` goes to q2. q2: `a;a,L`, `b;b,L`, `1;1,L` stay in q2; `□;□,R` goes to q0. q3 is final.
- **A (acceptor for strings starting with a):** q0: `a;a,S` goes to q2 (final); `b;b,R` goes to q1. q1: `a;a,R`, `b;b,R`, `□;□,R` stay in q1.
- **X (decider for { x c x | x ∈ {a,b}* }):** it erases the first symbol and remembers it in its state, crosses the `c`, marks the matching symbol on the right, walks back left, and repeats.

---

**1.** What does the transition `a ; b , R` mean?
- a. Read b, write a, move right
- b. Read a, write b, move right
- c. Read a, move right, go to state b
- d. Read a, write b, move left
- e. None of the above

**2.** How many components does the TM tuple M = (Q, Σ, Γ, δ, q0, F) have?
- a. 4
- b. 5
- c. 6
- d. 7
- e. None of the above

**3.** What is the output of T1 on `1011`?
- a. 1010
- b. 1000
- c. 1111
- d. 1100
- e. None of the above

**4.** What is the output of T1 on `0111`?
- a. 1000
- b. 0000
- c. 1001
- d. 0111
- e. None of the above

**5.** How many moves does T1 make on the input `1` before halting?
- a. 4
- b. 5
- c. 6
- d. 7
- e. None of the above

**6.** What does A do on the input `ba`?
- a. Accepts
- b. Rejects
- c. Loops forever
- d. Halts in q1
- e. None of the above

**7.** How can A be turned into a decider?
- a. Add `□;□,S` from q1 to a new non-final halting state
- b. Make q1 a final state
- c. Remove q0
- d. Make q2 non-final
- e. None of the above

**8.** A string x is accepted by a TM M when:
- a. M halts in a non-final state
- b. M reads all of x
- c. M loops forever
- d. M halts in a final state
- e. None of the above

**9.** What is the output of T2 on `baaba`?
- a. 11
- b. 111
- c. 1111
- d. 11111
- e. None of the above

**10.** What is the output of T2 on `bbab`?
- a. 11
- b. ε (blank tape)
- c. 1
- d. 111
- e. None of the above

**11.** A TM halts on every input, ending in a final state exactly for the strings of L. What kind of machine is it?
- a. A decider
- b. An acceptor only
- c. A transducer
- d. An enumerator
- e. None of the above

**12.** Which string does A accept?
- a. b
- b. ba
- c. ε
- d. ab
- e. None of the above

**13.** Which string is in L = { x c x | x ∈ {a,b}* } and is therefore accepted by X?
- a. `abcba`
- b. `acb`
- c. `abcab`
- d. `abab`
- e. None of the above

**14.** When X (reference) first reads `a` in state q0, what does it do?
- a. Marks it A and moves left
- b. Erases it, moves right, and remembers `a` in its state
- c. Moves left
- d. Rejects
- e. None of the above

**15.** To compute x + 1 in T1, the machine first:
- a. Scans right to the end of x
- b. Turns every 1 into 0
- c. Erases x
- d. Writes a 1 on the left
- e. None of the above

**16.** In T1, state q1 reads a `1`. Which transition applies?
- a. `1;1,R`
- b. `1;1,L`
- c. `1;0,R`
- d. `1;0,L`
- e. None of the above

**17.** In T2, what does q0 do when it reads `b`?
- a. Keeps b, moves left
- b. Writes 1
- c. Erases b, moves right, stays in q0
- d. Halts
- e. None of the above

**18.** What does `□ ; □ , S` mean?
- a. Stay in the same cell (blank rewritten as blank)
- b. Move left over a blank
- c. Move right over a blank
- d. Halt immediately
- e. None of the above

**19.** Which language can be decided by a TM but not accepted by any PDA?
- a. { aⁿbⁿ | n ≥ 0 }
- b. { aⁿbⁿcⁿ | n ≥ 0 }
- c. Palindromes over {a,b}
- d. { x c xᴿ | x ∈ {a,b}* }
- e. None of the above

**20.** What is the output of T1 on `1001`?
- a. 1000
- b. 1011
- c. 1100
- d. 1101
- e. None of the above

---

## Answer Key

| # | Ans | Why |
|---|-----|-----|
| 1 | b | read a, write b, move right |
| 2 | c | Q, Σ, Γ, δ, q0, F |
| 3 | d | 1011 + 1 = 1100 |
| 4 | a | 0111 + 1 = 1000 |
| 5 | b | R, L (to q1), 1→0 L, □→1 L (to q2), □ R: five moves, output 10 |
| 6 | c | q0 reads b, then q1 loops over blanks |
| 7 | a | halting in a non-final state makes it a decider |
| 8 | d | accept means halting in a final state |
| 9 | b | three a's |
| 10 | c | one a |
| 11 | a | decider |
| 12 | d | starts with a |
| 13 | c | abcab = x c x with x = ab |
| 14 | b | erase, move right, remember a |
| 15 | a | scan right to the last bit, then carry |
| 16 | d | trailing 1 becomes 0, move left |
| 17 | c | b's are not counted |
| 18 | a | S is stay |
| 19 | b | aⁿbⁿcⁿ is not context-free |
| 20 | e | 1001 + 1 = 1010 (not offered) |
