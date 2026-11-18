# CMSC 141 Automata and Language Theory: Summary of Lectures 8 to 18

Each lecture has three parts:

- **A. Topics:** the topics and their classifications, with no explanation.
- **B. Summary:** a short explanation of the lecture.
- **C. Remember!:** important facts, formulas, and notes.

## Contents

| Lecture | Topic |
|---|---|
| [Lecture 8](#lecture-8-pushdown-automata) | Pushdown Automata |
| [Lecture 9](#lecture-9-context-free-grammars) | Context-Free Grammars |
| [Lecture 10](#lecture-10-equivalences-and-conversions-cfg-to-npda) | Equivalences and Conversions (CFG to NPDA) |
| [Lecture 11](#lecture-11-ambiguous-grammars) | Ambiguous Grammars |
| [Lecture 12](#lecture-12-normal-forms-of-grammars) | Normal Forms of Grammars |
| [Lecture 13](#lecture-13-application-building-translators) | Application: Building Translators |
| [Lecture 14a](#lecture-14a-closure-properties-and-the-pumping-lemma-for-cfls) | Closure Properties and the Pumping Lemma for CFLs |
| [Lecture 14b](#lecture-14b-l-systems) | L-Systems |
| [Lecture 15](#lecture-15-turing-machines) | Turing Machines |
| [Lecture 16](#lecture-16-extensions-to-the-turing-machine) | Extensions to the Turing Machine |
| [Lecture 17](#lecture-17-context-sensitive-and-unrestricted-grammars) | Context-Sensitive and Unrestricted Grammars |
| [Lecture 18](#lecture-18-limits-of-computation) | Limits of Computation |
| [Quick Reference](#quick-reference-language-classes-grammars-and-machines) | Language classes, grammars, and machines |

---

## Lecture 8: Pushdown Automata

### A. Topics

- Context-free languages, CFG, and NPDA
- Formal definition of a PDA (7-tuple)
- PDA transition function
- Instantaneous descriptions (IDs) and the turnstile (⊢)
- Acceptance in a PDA
  1. Acceptance by final state
  2. Acceptance by empty stack
- Two types of PDA
  1. Nondeterministic PDA (NPDA)
  2. Deterministic PDA (DPDA)
- Relationships among automata
- Creating PDAs in JFLAP
- Example PDAs
  - { aⁿbⁿ : n > 0 }
  - Palindromes (NPDA)
  - Palindromes with a distinct middle character (DPDA)

### B. Summary

- A **pushdown automaton** is a finite automaton with a stack that can store a string of arbitrary length. The stack is read and changed only at its top.
- Context-free languages are exactly the languages recognized by NPDAs.
- A computation is shown as a sequence of **IDs** (state, remaining input, stack), where each ⊢ is one move.
- In { aⁿbⁿ }, the PDA pushes an A for every `a`, pops one A for every `b`, and accepts when the top of the stack is Z at the end of the input.
- An **NPDA** may have zero, one, or many possible moves in a configuration. A **DPDA** has at most one.
- Palindromes need an NPDA because the machine must guess the middle. A distinct middle character removes the guess, so a DPDA is enough.

### C. Remember!

- PDA = M = (Q, Σ, Γ, δ, q₀, Z, F). Z is the bottom-of-stack marker.
- Transition: δ(q, a, x) = (p, y). Here q is the current state, a the input symbol, x the stack top, p the next state, and y the replacement for the stack top.
- Move notation: (q, aw, Zα) ⊢ (p, w, βα).
- Acceptance by final state: L(M) = { w | (q₀, w, Z) ⊢* (p, ε, γ), p ∈ F }. The whole string is consumed and the machine ends in a final state.
- Acceptance by empty stack: L(M) = { w | (q₀, w, Z) ⊢* (p, ε, ε) }. The whole string is consumed and the stack is empty (Z popped). A final state is optional.
- The two acceptance forms are essentially equivalent.
- DPDA rule: no conflict between an input-consuming move and an ε-move for the same state and stack top.
- FA ⊂ DPDA ⊂ NPDA.
- PDAs cannot accept { aⁿbⁿcⁿ : n > 0 } because they have only one stack.
- Example transitions for { aⁿbⁿ }: δ(q₀,a,ε) = (q₀,A); δ(q₀,b,A) = (q₁,ε); δ(q₁,b,A) = (q₁,ε); δ(q₁,ε,Z) = (q₂,ε); F = {q₂}.

---

## Lecture 9: Context-Free Grammars

### A. Topics

- Context-free grammar (CFG) definition (4-tuple)
- Regular grammars as a special case of CFGs
- Context-free languages
- Why "context-free"
- Building a CFG
  1. Identify the base case
  2. Identify the recursive rules
- Ways to show a string is in L(G)
  1. Derivation (leftmost or rightmost)
  2. Parse tree
- CFG examples
  - { aⁿbⁿ : n > 0 }
  - Palindromes over {a,b}
  - { aᵐbⁿ : m > n > 0 }
- Creating CFGs in JFLAP
- Exercise types: counting-based languages, palindrome variants, balanced parentheses and brackets, equal counts of a's and b's

### B. Summary

- A CFG describes a language by recursive rules called productions. Every production has a single variable on the left side.
- It is **context-free** because a rule can be applied to a variable regardless of the symbols around it.
- To design a CFG, start from the simplest valid strings (the base case) and add recursive rules for the longer ones.
- To check a string, start from S and repeatedly replace a variable by the body of one of its productions. A parse tree shows the same derivation as a tree.
- A language can have several correct grammars. For { aᵐbⁿ : m > n > 0 } the lecture gives two.

### C. Remember!

- CFG = G = (V, T, P, S) with P: V → (V + T)*. S ∈ V is the start variable.
- Regular grammar productions have the form V → VT* or V → T*V, so RG ⊂ CFG.
- L(G) = { w ∈ Σ* | S ⇒* w }.
- { aⁿbⁿ : n > 0 }: S → ab | aSb. Base case S → ab; recursive rule S → aSb.
- Palindromes: S → ε | a | b | aSa | bSb (three base cases, two recursive rules). To exclude ε, remove S → ε and add S → aa | bb.
- { aᵐbⁿ : m > n > 0 }: S → AX, A → a | aA, X → ab | aXb. Or S → aab | aSB, B → b | ε.
- Derivation of `aaabbb`: S ⇒ aSb ⇒ aaSbb ⇒ aaabbb.

---

## Lecture 10: Equivalences and Conversions (CFG to NPDA)

### A. Topics

- Equivalence of CFGs and NPDAs
  1. Every language accepted by an NPDA has a CFG
  2. Every language generated by a CFG has an NPDA
- Conversion: CFG to NPDA (8 steps)
  1. Create a one-state NPDA
  2. Set the input alphabet
  3. Set the stack alphabet
  4. Set the initial stack symbol
  5. Add a transition for each terminal
  6. Add a transition for every ε-production
  7. Add a transition for every other production
  8. Accept by empty stack
- Examples
  - { aⁿbⁿ : n ≥ 0 }
  - Palindromes

### B. Summary

- NPDAs and CFGs describe exactly the same class of languages (the context-free languages).
- To convert a CFG into a PDA, build one state and let the stack carry the derivation. The stack starts with the start variable, and each production becomes an ε-transition that replaces a variable by its body.
- Each terminal gets a transition that pops it when it matches the input.
- The machine accepts when the whole input has been read and the stack is empty.
- Tracing `aabb`: the NPDA's stack changes follow the same steps as the CFG derivation S ⇒ aSb ⇒ aaSbb ⇒ aabb.

### C. Remember!

- NPDA M = (Q, Σ, Γ, δ, q₀, Z, F) with Q = {q₀}, Σ = T, Γ = V ∪ T, Z = S.
- For every terminal a: δ(q, a, a) = {(q, ε)}.
- For every production A → ε: δ(q, ε, A) = {(q, ε)}.
- For every production A → B₁B₂…Bₙ: δ(q, ε, A) = {(q, B₁B₂…Bₙ)}. The symbols are pushed in reverse order so that B₁ ends up on top.
- The NPDA needs no extra states to track the derivation; the stack does all the work.
- Palindrome grammar S → ε | a | b | aSa | bSb: δ(q, ε, S) has five choices.
- Exercises: build the CFG, then the one-state NPDA, for palindromes over {a,b,c} and for { aᵐbⁿcᵐ⁺ⁿ : m, n ≥ 0 }.

---

## Lecture 11: Ambiguous Grammars

### A. Topics

- Definition of an ambiguous grammar
- Examples of ambiguity
  1. { aᵐbⁿ : m > n > 0 } with S → aab | aSB
  2. English: "The boy saw the man with a telescope"
  3. Expression grammar: `3-2-2`
- Removing ambiguity from CFGs
  1. Expression grammars (precedence and associativity)
  2. Dangling-else ambiguity
- Final notes on ambiguity
  1. No algorithm decides ambiguity
  2. Inherently ambiguous languages
- Exercises: unambiguous Boolean expressions, regular expressions, a meta-CFG for CFGs

### B. Summary

- A grammar is **ambiguous** if some string it generates has more than one parse tree.
- `3-2-2` can be grouped as (3-2)-2 = −1 or 3-(2-2) = 3, so the grammar S → S-S | 3 | 2 is ambiguous (subtraction is not associative).
- Ambiguity matters for translators, which need exactly one parse tree.
- There is no universal method to remove ambiguity, but some grammars can be rewritten to enforce the intended meaning.
- For expressions, put each operator at its own grammar level. For if/else, make each `else` belong to the nearest unmatched `if`.

### C. Remember!

- Grammar level gives **precedence** (deeper level = binds tighter). Direction of recursion gives **associativity** (left recursion = left-associative).
- Unambiguous subtraction: E → T | E-T; T → 3 | 2 | (E).
- Unambiguous + and *: E → T | E+T; T → F | T*F; F → 3 | 2 | (E). The expression must follow PEMDAS.
- Dangling else: with S → B | if C then S | if C then S else S, the string `if c then if c then b else b` has two parses. The usual convention attaches `else` to the nearest `if`.
- No algorithm can tell whether an arbitrary CFG is ambiguous.
- Inherently ambiguous language: { aᵖbᵠcʳ | p = q or q = r }. Every CFG for it is ambiguous.
- Exercise on { aᵖbᵠcʳ | p = q or q = r }: give a CFG, show it is ambiguous, and explain why every CFG for L is ambiguous.

---

## Lecture 12: Normal Forms of Grammars

### A. Topics

- Normal form: definition
- Two normal forms
  1. Chomsky Normal Form (CNF)
  2. Greibach Normal Form (GNF)
- Transforming CFG → CNF → GNF
  - Step I: Clean up the CFG
    1. Eliminate ε-productions
    2. Eliminate unit productions
    3. Eliminate useless symbols
  - Step II: CFG → CNF
    1. Make all bodies of length 2 or more consist only of variables
    2. Break bodies of length 3 or more into cascades of two variables
  - Step III: CNF → GNF
    1. Eliminate left recursion (direct and indirect)
    2. Repeatedly substitute the leftmost variable until each body starts with a terminal

### B. Summary

- A **normal form** is a standardized way of writing a CFG by restricting its production rules. The grammar changes, but the language does not.
- **CNF** allows only A → BC or A → a. Its parse trees are binary trees.
- **GNF** allows only A → aα, where a is a terminal and α is zero or more variables. A string x in L(G) has a derivation of exactly |x| steps.
- To convert, first clean the grammar (remove ε-productions, unit productions, and useless symbols). Then rewrite to CNF. Then rewrite to GNF.

### C. Remember!

- Both forms generate L − {ε} (every CFL without ε).
- CNF: A → BC or A → a (V → VV or V → T). GNF: A → aα (V → TV*).
- **Nullable:** X is nullable if X → ε, or X → C₁…Cₙ with every Cᵢ nullable. To eliminate ε-productions, create every keep-or-omit combination of the nullable variables in each body. It is not allowed to make all symbols absent.
- **Unit production:** A → B with both variables. Copy the non-unit productions of every reachable variable, then remove the unit productions.
- **Useful symbol:** both *generating* (derives a terminal string) and *reachable* from S. Remove non-generating symbols first, then unreachable ones.
- To CNF: replace each terminal `a` in a body of length ≥ 2 by a new variable Xₐ (Xₐ → a). Break a body of length k ≥ 3 into k − 1 productions with two variables each.
- **Direct left recursion:** A → Aα | β becomes A → βA′, A′ → αA′ | ε.
- **Indirect left recursion:** substitute to make it direct, then eliminate it.
- Worked example: S → AB, A → aAA | ε, B → bBB | ε simplifies to S → AB | a | aA | aAA | b | bB | bBB, A → a | aA | aAA, B → b | bB | bBB. It then goes to CNF using new variables Xₐ, X_b, Y, Z, and finally to GNF.

---

## Lecture 13: Application: Building Translators

### A. Topics

- From formal languages to compilers
- A compiler as a language translator
- Phases of compiler execution
  1. Lexical analyzer
  2. Syntax analyzer
  3. Semantic analyzer
  4. Intermediate code generator
  5. Code optimizer
  6. Code generator
- Lexical analysis (regular languages; tool: flex)
  - Lexemes and tokens
- Syntax analysis (context-free languages; tool: bison)
  - BNF
  - Writing a bison grammar (`.y` file)
- Putting it together: the calculator
  - Scanner (`yylex()`), parser (`yyparse()`), `yylval`
  - Semantic actions: `$$`, `$1`, `$3`
  - Build and run steps
- Extending the calculator
  1. Parentheses and binary subtraction
  2. Floating-point numbers and division
  3. Right-associative exponentiation and negative values
  4. One-letter variables and single or multiple assignment

### B. Summary

- A **compiler** translates a source program (C, Java, Python) into a target language (assembly or machine code), in phases.
- **Lexical analysis** turns source text into tokens using regular expressions. **Syntax analysis** checks that the tokens form a valid structure using a CFG.
- flex generates the scanner and bison generates the parser. The scanner hands tokens to the parser one at a time, and C actions in the grammar rules compute the translation.
- The theory maps directly onto the tools: regular expressions to a DFA-based scanner, CFG to a stack-based parser.

### C. Remember!

- **Lexeme** = actual text found; **token** = classification of the lexeme. Example: for `int total = 25;` the tokens are `<int> <identifier> <=> <number> <;>`.
- flex (stable release 2.6.4) is for lexical analysis; bison (stable release 3.8.2) is for syntax analysis. Both can be installed with `brew install`.
- BNF (Backus-Naur Form): `→` becomes `:`, alternatives use `|`, and `;` ends a production. Terminals are declared with `%token`. The start symbol is the left side of the first rule.
- In an action like `{ $$ = $1 + $3; }`, `$$` is the value of the target symbol and `$1`, `$3` are the values of the symbols in the body.
- `yylval` is the global variable where the actual value of a token is stored. `simple_calc.tab.h` (made by `bison -d`) is `#include`d in `scanner.lex` so both sides agree on token names.
- Build: `bison -d simple_calc.y` → `flex scanner.lex` → `gcc simple_calc.tab.c lex.yy.c -o parser_scanner` → `./parser_scanner`. Example: `15 / 5 * 2 - 4 + 2` gives `= 4`.
- RegEx ↔ DFA (flex); CFG ↔ PDA (bison).
- A translator needs exactly one parse tree per input, so resolving ambiguity is critical. Update the CFG first, then the `.lex` and `.y` files.

---

## Lecture 14a: Closure Properties and the Pumping Lemma for CFLs

### A. Topics

- Two approaches to reason about CFLs
  1. Closure properties (prove a language is context-free)
  2. Pumping lemma (prove a language is not context-free)
- Closure properties of CFLs
  1. Closed under union, concatenation, and Kleene star
  2. CFL ∩ regular language is a CFL
  3. Not closed under intersection
  4. Deterministic CFLs are closed under complement
- Proving context-freeness using closure properties
- Pumping lemma for CFLs
  - Special property
  - Pumping condition and negated condition
  - Proof strategy
- Examples of languages that are not CFLs
  - { aⁿbⁿcⁿ }, { xcx }, { aᵐbⁿcᵐdⁿ }, { aᵖ : p prime }
- Exercises: PAL ∩ (ab)*(a+ε), complement of PAL, a*b* − { aⁿbⁿ }, closure under reversal and homomorphism

### B. Summary

- **Closure properties** let you build a new language from known CFLs and conclude it is context-free. The proofs construct a CFG (union, concatenation, star) or a PDA (intersection with a regular language).
- CFLs are **not** closed under intersection: { 0ⁿ1ⁿ2ᵐ } ∩ { 0ᵐ1ⁿ2ⁿ } = { 0ⁿ1ⁿ2ⁿ }, which is not context-free.
- The **pumping lemma** says every long enough string in a CFL can be split into five parts with two pumpable pieces. To prove L is not context-free, show the pumping condition fails.
- Proof outline for { 0ᵐ1ᵐ2ᵐ }: pick z = 0ⁿ1ⁿ2ⁿ. Because |vwx| ≤ n, vwx cannot touch both 0s and 2s. Pumping down changes at most two of the counts, so the string leaves L.

### C. Remember!

- Constructions: union Sᵤ → S₁ | S₂; concatenation S_c → S₁S₂; Kleene star S_k → ε | S₁S_k.
- PDA + DFA → PDA (so CFL ∩ regular is a CFL). PDA + PDA ↛ PDA (so intersection can fail).
- DCFL complement: normalize the DPDA so it always reaches a clear accept or reject, then swap the outcomes.
- **Pumping lemma:** if L is a CFL there is a constant n such that every z ∈ L with |z| ≥ n can be written z = uvwxy with (1) |vwx| ≤ n, (2) vx ≠ ε, (3) uvⁱwxⁱy ∈ L for all i ≥ 0.
- **Negated condition** (for a proof by contradiction): for the language L there is an integer n > 0 and a string z ∈ L with |z| ≥ n such that for every partition z = uvwxy with vx ≠ ε and |vwx| ≤ n, there is some i ≥ 0 with uvⁱwxⁱy ∉ L.
- You choose z (as a function of n) and i. The adversary chooses the partition.
- Regular languages ⊂ context-free languages, so a regular language can be used as one of the known CFLs.
- Context-free "look-alikes" (these are CFLs): { aⁿbⁿ }, { xcxᴿ }, { aᵐbᵐcⁿdⁿ }.
- Closure under complement fails in general; it follows from De Morgan once union is closed and intersection is not (this is a consequence, not a separate slide).

---

## Lecture 14b: L-Systems

### A. Topics

- Recall: languages that are not CFLs, and their look-alikes
- Going beyond context-free
  1. Stronger machines and grammars (Turing machines, context-sensitive and unrestricted grammars)
  2. Different rewriting (L-systems)
- Lindenmayer systems (L-systems)
  - Origin (modeling biological growth) and applications (fractals, plant modeling, procedural graphics)
  - Definition: L = (V, S, ω, P)
  - Sequential vs. parallel rewriting
- Example: Lindenmayer's algae
- Turtle graphics
  - Turtle commands (as in JFLAP)
- Examples
  1. Koch curve and Koch snowflake
  2. A branching plant
- Running L-systems: three tools
  1. JFLAP
  2. Python (turtle)
  3. GAMA (GAML)
- Exercises: change θ, Sierpinski triangle, dragon curve, algae strings are not a CFL

### B. Summary

- An **L-system** is like a grammar, but every applicable symbol is rewritten **at the same time** in each step (parallel rewriting). A CFG rewrites one nonterminal per step.
- Because of parallel rewriting, even the simple rule a → aa produces a language outside the CFLs.
- Lindenmayer's algae models cell division: A → AB, B → A. The strings grow in length 1, 2, 3, 5, 8, 13, 21 (Fibonacci).
- A **turtle** turns the strings into pictures: each symbol is a drawing command. Brackets let the turtle branch.
- The same (ω, P, θ) runs in JFLAP, GAMA, or a few lines of Python.

### C. Remember!

- L = (V, S, ω, P): V replaceable variables, S non-replaceable constants (optional), ω the axiom (initial string), P the production rules.
- Rule a → aa with ω = a: **parallel** gives a, aa, aaaa, aaaaaaaa (not a CFL); **sequential** gives a regular language.
- Algae: V = {A, B}, ω = A, P = { A → AB, B → A }. Strings: AB, ABA, ABAAB, ABAABABA, …, lengths 2, 3, 5, 8, 13, 21.
- Turtle commands (JFLAP): `g` forward with pen down; `f` forward with pen up; `+` / `-` turn right / left by θ; `[` / `]` push / pop state; `%` turn around 180°. A square is `g+g+g+g` with θ = 90°.
- Koch curve: ω = g, P = { g → g-g++g-g }, θ = 60°. Koch snowflake: ω = g++g++g.
- Plant: ω = X, P = { X → g[+X]g[-X]+X, g → gg }, θ = 25°. X draws nothing and marks where growth happens.
- Sierpinski triangle: ω = F-G-G, F → F-G+F+G-F, G → GG, θ = 120°. Dragon curve: ω = FX, X → X+YF+, Y → -FX-Y, θ = 90°.
- Changing θ changes the drawing but not the derived string.
- The algae strings are not a CFL because their lengths are Fibonacci numbers, so the gaps between lengths keep growing.
- L-system = a grammar for growth.

---

## Lecture 15: Turing Machines

### A. Topics

- Why Turing machines (PDAs cannot accept { aⁿbⁿcⁿ })
- Turing's 1936 paper
- Turing machine basics
  1. Infinite tape (memory)
  2. Read/write head (access)
  3. Finite control (control)
- Visualizing a Turing machine
- Formal definition of a TM (6-tuple)
  1. States
  2. Alphabets
  3. Transitions
- TM in JFLAP
- Language of a Turing machine (three roles)
  1. Acceptor
  2. Decider
  3. Transducer
- Examples
  1. Acceptor: strings that start with `a`
  2. Decider: strings of the form xcx
  3. Transducer: successor function
  4. Transducer: counting a's
- Acceptor, decider, and { aⁿbⁿcⁿ } as exercises

### B. Summary

- A **Turing machine** uses the tape itself as storage, read and written one cell at a time by a head that moves left, right, or stays. A finite set of states decides every move.
- In each step the TM reads the symbol under the head, overwrites it, moves, and goes to the next state.
- A TM can be designed as an **acceptor** (accepts strings in L; may loop on others), a **decider** (halts on every input), or a **transducer** (computes a function by leaving an output on the tape).
- A string x is accepted if M ends in a final state and halts.

### C. Remember!

- TM = M = (Q, Σ, Γ, δ, q₀, F). Σ ⊆ Γ, and Γ includes the blank □.
- δ : Q × Γ → Q × Γ × D with D = { L, S, R }. The notation `a ; b , R` means read a, write b, move right.
- Turing's paper "On Computable Numbers" (1936) gave a definition of computability, the Turing machine, the Universal TM (stored programs), and the halting problem.
- L(M) = { x | x is accepted by M }. Loops are possible, for example `a ; a , S` repeated.
- Acceptor example: for `ba` the machine runs forever (never accepted, never rejected). To make it a decider, add `□ ; □ , S` to a reject state.
- Decider example (xcx): erase the first symbol and remember it, cross the `c`, mark the matching symbol A or B, walk back, repeat, then clean up and accept.
- Successor transducer on binary x: scan right to the last bit; turn trailing 1s into 0s; the first 0 or blank becomes 1; walk back and halt. Examples: 10111 → 11000, 1000 → 1001.
- Counting transducer: replace x ∈ {a,b}* by the unary count of its a's. Examples: baaba → 111, baabaaab → 11111.
- A decider is a stronger notion than an acceptor. Either way, a decider must halt.

---

## Lecture 16: Extensions to the Turing Machine

### A. Topics

- Extensions to the basic TM
  1. Multiple tapes
  2. Multiple tracks
  3. Two-dimensional tape
  4. Nondeterministic moves
  5. Combinations
- Multi-tape TM: definition and JFLAP
  - Example: 2-tape TM counting a's
  - Example: 3-tape TM deciding { aⁿbⁿcⁿ }
  - Example: 2-tape TM enumerating ba*
- Fourth role of a TM: enumerator
- Multi-track TM: definition
  - Example: 3-track TM deciding { aⁿbⁿcⁿ }
- Nondeterministic TM: definition
  - Example: 1-tape NDTM for the monus function
- PDA as a special case of a TM
- The Universal Turing Machine
- The Church–Turing thesis

### B. Summary

- Extensions make TMs **easier to program, not more powerful**. A language accepted by a multi-tape TM is also accepted by a single-tape TM.
- A **multi-tape TM** has k tapes with k independent heads that can read and write in a single transition. A **multi-track TM** has one tape divided into k tracks and one head; each cell holds a k-tuple.
- A **nondeterministic TM** can have several transitions for a state and symbol, and accepts if at least one path accepts.
- An **enumerator** generates and prints every string of a language, one by one, without necessarily halting.
- A PDA is a two-tape TM in which one tape acts as the stack.
- A **Universal TM** takes ⟨M⟩ and x and simulates M on x. The **Church–Turing thesis** says anything an algorithm can compute, a TM can compute.

### C. Remember!

- Multi-track: δ : Q × Γᵏ → Q × Γᵏ × D. A multi-track TM has the same power as a k-tape TM, but simulating t steps takes at most O(t²) steps.
- 2D tape: D = { L, R, U, D, S }.
- 2-tape counter: tape 1 (input) moves R on every symbol; tape 2 (output) writes a 1 only when tape 1 reads an `a`.
- 3-tape decider for { aⁿbⁿcⁿ }: three heads move right together over the a-block, b-block, and c-block. All three blocks must end at the same time.
- Enumerator for ba*: write b on both tapes, then repeat forever: add `a`, copy tape 1 onto tape 2, add `#`. It never halts, but every string of ba* gets printed.
- Monus: f(m, n) = max(m − n, 0) with input 0ᵐ10ⁿ and output 0^f(m,n). Examples: 5 ∸ 2 = 000; 3 ∸ 5 = ε.
- NDTM transition: δ(q, X) = { (q₁, γ₁, D), …, (qₖ, γₖ, D) }.
- The Universal TM accepts iff M accepts and loops if M loops. It works by fetch-decode-execute.
- The Church–Turing thesis is widely accepted but is not a provable theorem. It treats "algorithm" (informal) as equal to "Turing machine" (formal). λ-calculus and recursive functions have the same power.
- Power ordering: (DFA = NFA) < DPDA < NPDA < TM.
- The four TM roles: acceptor (may loop), decider (always halts), transducer (computes f(x)), enumerator (lists L).

---

## Lecture 17: Context-Sensitive and Unrestricted Grammars

### A. Topics

- Chomsky hierarchy of formal grammars
  1. Regular
  2. Context-free
  3. Context-sensitive
  4. Unrestricted
- Non-context-free languages
  1. Context-sensitive language
  2. Recursively enumerable language
- Context-sensitive grammars (CSG)
  - Definition and normal form
  - Example: { aⁿbⁿcⁿ : n > 0 }
  - Example: equal numbers of a's, b's, and c's
- Linear bounded automata (LBA) for CSLs
- Unrestricted grammars (UG)
  - Example: { a^(2ᵏ) : k > 0 }
- Recursively enumerable languages, unrestricted grammars, and Turing machines
- Universal Turing machines

### B. Summary

- The **Chomsky hierarchy** has four classes of languages and grammars: regular, context-free, context-sensitive, and unrestricted. Each class strictly contains the one before it.
- A **context-sensitive grammar** has productions α → β with |α| ≤ |β|, so a derivation never gets shorter. The name comes from the normal form α₁Aα₂ → α₁βα₂, where A is replaced only in the context α₁, α₂.
- For { aⁿbⁿcⁿ }, the CSG builds a(BC) blocks, sorts them with CB → BC, then converts to lowercase left to right.
- An **LBA** is a nondeterministic TM whose tape is limited to the input plus two endmarkers. LBAs accept the context-sensitive languages.
- An **unrestricted grammar** has no restriction on productions, and generates exactly the recursively enumerable languages, which are the languages accepted by Turing machines.
- A **Universal TM** simulates any TM from its encoded description and an input.

### C. Remember!

- All grammars are 4-tuples G = (V, T, P, S).
- CSG: P has productions α → β with |α| ≤ |β|, and α contains at least one variable. Normal form: α₁Aα₂ → α₁βα₂ with β ≠ ε.
- Regular G ⊂ CFG ⊂ CSG ⊂ Unrestricted grammar.
- CSG for { aⁿbⁿcⁿ }: S → aBC | aSBC; CB → BC; aB → ab; bB → bb; bC → bc; cC → cc.
- CSG for equal counts: S → ABC | ABCS; AB → BA; BC → CB; AC → CA; A → a; B → b; C → c.
- LBA: M = (Q, Σ, Γ, δ, <, >, F). No move is permitted left of `<` or right of `>`, and nothing can be printed over `<` or `>`.
- UG for { a^(2ᵏ) : k > 0 }: S → <Ca>; Ca → aaC; C> → D>; C> → E; aD → Da; <D → <C; aE → Ea; <E → ε. C is a doubler: it passes through the a's, doubling each, then either prepares to double again (D) or stops (E).
- A language can be generated by an unrestricted grammar if and only if it is accepted by some TM.
- Recursively enumerable: a TM can list the strings, but may not be able to decide that a string is not in the language.
- UTM idea: fetch-decode-execute over an encoded TM on a tape.

---

## Lecture 18: Limits of Computation

### A. Topics

- The Church–Turing thesis and Gödel
- Gödel's incompleteness theorems
- The limit of Turing machines
  1. Decidable (recursive)
  2. Acceptable (recursively enumerable)
  3. Neither (not recursively enumerable)
- Closure properties of TM languages
- Enumerable sets
  1. Reals in (0, 1) are not enumerable
  2. The set of all languages is not enumerable
- Problems as languages
- The halting problem
  - HALT and co-HALT
  - Why HALT is not decidable
- Other undecidable problems
- From decidability to computational complexity
  1. P
  2. NP
  3. NP-hard
  4. NP-complete
- Problems in P and problems in NP
- The P vs NP problem
- Questions to think about

### B. Summary

- Church–Turing: TMs capture all effective computation. **Gödel**: in any sound, consistent, effectively axiomatized system that can express arithmetic, some true statements cannot be proven inside it.
- A language is **decidable** if a TM always halts on it; **acceptable** if a TM halts on every string in L (and may loop otherwise); some languages are neither.
- Cantor's **diagonal argument** shows the reals in (0, 1) cannot be listed. The same argument on languages (0/1 membership bits) shows the set of all languages is not enumerable. TMs are enumerable, so some language has no TM.
- **HALT** is acceptable but not decidable: assuming a decider H, build D that does the opposite of H on its own code; H is then wrong either way. **co-HALT** is not even acceptable.
- Some properties of CFGs are also undecidable.
- Even decidable problems may need unreasonable time. **P** is solvable in polynomial time; **NP** has solutions verifiable in polynomial time; **NP-complete** problems are the hardest in NP.

### C. Remember!

- decidable ⊂ acceptable ⊂ all languages. Examples: { aⁿbⁿcⁿ } is decidable; HALT is acceptable but not decidable; co-HALT is not acceptable.
- Closure of TM languages: decidable languages are closed under union, concatenation, Kleene star, intersection, and complement. Acceptable languages are closed under all of these except complement.
- If both a language and its complement are acceptable, the language is decidable. Since HALT is acceptable but not decidable, co-HALT is not acceptable.
- Gödel's sentence G: "This statement is not provable in F". If G were provable, F would prove a false statement; so G is true but unprovable. First theorem: F is incomplete. Second theorem: F cannot prove its own consistency.
- Diagonalization steps: pair ℕ with the list, read the diagonal, change each diagonal entry (add 1 to a digit with 9 → 0, or flip a bit), and the new object differs from every row.
- Pigeonhole argument: there are more languages than Turing machines.
- HALT = { ⟨M, x⟩ | M halts on x }. co-HALT = { ⟨M, x⟩ | M runs forever on x }.
- Undecidable questions about grammars: Is L(G) regular for a CFG G? Is a CFL inherently ambiguous? Is L(G) context-free for a CSG G?
- P ⊆ NP. P = NP is an open Millennium Prize Problem ($1 million).
- NP-hard: at least as hard as every NP problem; need not be a decision problem or be in NP. NP-complete: in NP and NP-hard.
- SAT was the first problem proven NP-complete (Cook–Levin Theorem, 1971).
- In P: graph reachability (BFS/DFS), shortest path (Dijkstra, nonnegative weights), minimum spanning tree (Kruskal).
- In NP: Clique (verify all pairs, O(k²), brute force O(2ⁿ)); Hamiltonian cycle (verify in O(n), brute force O(n!)).
- Overall chain: P ⊆ NP ⊆ decidable.

---

## Quick Reference: Language Classes, Grammars, and Machines

| Language class | Grammar | Machine | Example language |
|---|---|---|---|
| Regular | Regular grammar | DFA / NFA | strings ending in `ab` |
| Context-free | CFG | NPDA (DPDA for a subset) | { aⁿbⁿ }, palindromes |
| Context-sensitive | CSG | LBA | { aⁿbⁿcⁿ } |
| Recursively enumerable | Unrestricted grammar | Turing machine | HALT |

- Regular ⊂ CFL ⊂ CSL ⊂ RE.
- Decidable (recursive) languages lie between CSL and RE.
- (DFA = NFA) < DPDA < NPDA < TM.
