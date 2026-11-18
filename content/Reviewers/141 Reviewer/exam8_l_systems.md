# CMSC 141 Exam 8: L-Systems (Lecture 14b)

**Items:** 20 | **Difficulty:** Average | **Type:** Application, multiple choice

**Directions:** Choose the best answer: **a, b, c, d**, or **e** (none of the above). An L-system is L = (V, S, ω, P): variables, constants, axiom, and productions, with **parallel rewriting** (every applicable symbol is replaced in each step). Strings are numbered by derivation step n; n = 0 is the axiom.

**Turtle commands (as in JFLAP):** `g` forward with pen down; `f` forward with pen up; `+` turn right by θ; `-` turn left by θ; `[` push state; `]` pop state; `%` turn around 180°.

**Lindenmayer's algae:** V = {A, B}, ω = A, P = { A → AB, B → A }. Steps: n=1: AB; n=2: ABA; n=3: ABAAB.

---

**1.** What is the algae string at n = 4?
- a. ABAAB
- b. ABAABAB
- c. ABABAABA
- d. ABAABABA
- e. None of the above

**2.** What is the length of the algae string at n = 5?
- a. 8
- b. 13
- c. 21
- d. 10
- e. None of the above

**3.** What is the length of the algae string at n = 7?
- a. 21
- b. 55
- c. 13
- d. 89
- e. None of the above

**4.** Rule a → aa with axiom ω = a, rewritten in parallel. How long is the string after 3 steps?
- a. 8
- b. 6
- c. 4
- d. 3
- e. None of the above

**5.** Parallel rewriting of a → aa from ω = a gives the strings a^(2ⁿ). Which describes this set of strings?
- a. Regular
- b. Context-free but not regular
- c. Not context-free
- d. Finite
- e. None of the above

**6.** With θ = 90°, what does the turtle draw for `g+g+g+g`?
- a. A straight line
- b. A closed square
- c. A triangle
- d. A spiral
- e. None of the above

**7.** Which symbol moves the turtle forward **without** drawing?
- a. `g`
- b. `+`
- c. `[`
- d. `f`
- e. None of the above

**8.** What do `[` and `]` do?
- a. Save and restore the turtle's position and heading (used for branching)
- b. Start and end a filled polygon
- c. Turn the turtle left and right
- d. Change the pen color
- e. None of the above

**9.** The Koch curve is ω = g, P = { g → g-g++g-g }, θ = 60°. How many `g` symbols does the string at n = 2 contain?
- a. 8
- b. 12
- c. 16
- d. 64
- e. None of the above

**10.** What axiom draws the Koch snowflake from three Koch curves (turning right 120° between them)?
- a. g
- b. g++g++g
- c. g+g
- d. g+g+g+g
- e. None of the above

**11.** The plant has ω = X, P = { X → g[+X]g[-X]+X, g → gg }, θ = 25°. What is the string at n = 1?
- a. g[+X]g[-X]+X
- b. gg[+X]g[-X]+X
- c. g[+X][-X]
- d. X[+X]X[-X]+X
- e. None of the above

**12.** In the same plant, how many `g` symbols are in the string at n = 2?
- a. 6
- b. 8
- c. 12
- d. 10
- e. None of the above

**13.** In the plant L-system, what does the symbol `X` do when the turtle reads it?
- a. Draws a line
- b. Turns right
- c. Draws nothing; it marks where new growth happens
- d. Pops the saved state
- e. None of the above

**14.** The lengths of the algae strings at n = 0, 1, 2, 3, ... follow which sequence?
- a. Powers of 2
- b. Fibonacci numbers
- c. Squares
- d. Primes
- e. None of the above

**15.** Sierpinski triangle: ω = F-G-G, P = { F → F-G+F+G-F, G → GG }, θ = 120°. What is the string at n = 1?
- a. F-G+F+G-F-GG-GG
- b. F-G+F+G-F-G-G
- c. FF-GG-GG
- d. F-G+F+G-F+GG+GG
- e. None of the above

**16.** Dragon curve: ω = FX, P = { X → X+YF+, Y → -FX-Y }, θ = 90°. What is the string at n = 1?
- a. FX+YF
- b. FXX+YF+
- c. F+YF+
- d. FX+YF+
- e. None of the above

**17.** You change θ from 25° to 35° in the plant L-system. What changes?
- a. The derived string
- b. The axiom
- c. Nothing in the string, but the drawing bends the branches more
- d. The productions
- e. None of the above

**18.** Why can the set of algae strings {A, AB, ABA, ABAAB, ...} not be a CFL?
- a. Consecutive lengths are Fibonacci numbers with ever-growing gaps, which the CFL pumping lemma does not allow
- b. All the strings have the same length
- c. The strings contain nested parentheses
- d. The alphabet is infinite
- e. None of the above

**19.** What is the key difference between L-system derivation and CFG derivation?
- a. L-systems have no axiom
- b. L-systems rewrite all applicable symbols simultaneously; a CFG rewrites one nonterminal per step
- c. CFGs rewrite all variables at once
- d. L-systems cannot have variables
- e. None of the above

**20.** With θ = 90°, what does the turtle draw for `g+g+g`?
- a. A closed square
- b. An equilateral triangle
- c. A straight line
- d. A closed octagon
- e. None of the above (it draws an open path of three sides)

---

## Answer Key

| # | Ans | Why |
|---|-----|-----|
| 1 | d | ABAAB ⇒ ABAABABA |
| 2 | b | lengths 1, 2, 3, 5, 8, 13 at n = 0..5 |
| 3 | e | length 34 at n = 7 (not offered) |
| 4 | a | a → aa → aaaa → aaaaaaaa |
| 5 | c | lengths 2ⁿ need more than one stack's counting |
| 6 | b | four sides with right turns of 90° |
| 7 | d | `f` is forward with pen up |
| 8 | a | push/pop of turtle state creates branches |
| 9 | c | each g becomes four g's: 4² = 16 |
| 10 | b | three curves, turning right 120° (two `+` at 60°) |
| 11 | a | X is rewritten; there are no g's to double at n = 0 |
| 12 | d | 2 old g's become 4; 3 X's contribute 2 g's each: 4 + 6 = 10 |
| 13 | c | X is only a placeholder for growth |
| 14 | b | Fibonacci numbers |
| 15 | a | F is rewritten, `-` and `+` stay, each G becomes GG |
| 16 | d | F has no rule, X → X+YF+ |
| 17 | c | the angle only affects the interpretation |
| 18 | a | pumping gives bounded gaps between lengths |
| 19 | b | parallel versus sequential rewriting |
| 20 | e | three sides of a square: an open path |
