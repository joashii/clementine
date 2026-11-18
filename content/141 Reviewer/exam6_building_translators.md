# CMSC 141 Exam 6: Application, Building Translators with flex and bison (Lecture 13)

**Items:** 20 | **Difficulty:** Average | **Type:** Application, multiple choice

**Directions:** Choose the best answer: **a, b, c, d**, or **e** (none of the above). Assume the lecture's calculator: `simple_calc.y` (bison grammar) and `scanner.lex` (flex scanner), with the rules

```
exp:    factor
      | exp ADD factor   { $$ = $1 + $3; }
      | exp SUB factor   { $$ = $1 - $3; } ;
factor: term
      | factor MUL term  { $$ = $1 * $3; }
      | factor DIV term  { $$ = $1 / $3; } ;
term:   NUMBER ;
```

---

**1.** In a compiler, which phase is built on regular languages?
- a. Syntax analysis
- b. Lexical analysis
- c. Semantic analysis
- d. Code generation
- e. None of the above

**2.** For the source line `int total = 25;`, which token does the lexeme `total` belong to?
- a. `<int>`
- b. `<number>`
- c. `<identifier>`
- d. `<=>`
- e. None of the above

**3.** What token sequence does a lexical analyzer produce for `x = 7;`?
- a. `<identifier> <=> <number> <;>`
- b. `<int> <identifier> <;>`
- c. `<identifier> <=> <identifier>`
- d. `<number> <=> <identifier> <;>`
- e. None of the above

**4.** Which tool generates the lexical analyzer from regular expressions?
- a. bison
- b. gcc
- c. flex
- d. JFLAP
- e. None of the above

**5.** What function does bison generate for you to call to parse the input?
- a. `yylex()`
- b. `main()`
- c. `lex.yy.c`
- d. `yyparse()`
- e. None of the above

**6.** What does the calculator print for the input `15 / 5 * 2 - 4 + 2` using the rules above?
- a. 4
- b. 3
- c. 6
- d. 11
- e. None of the above

**7.** In the rule `exp: exp ADD factor { $$ = $1 + $3; }`, what does `$2` refer to?
- a. The first `exp`
- b. The `factor`
- c. The token ADD
- d. The value of the whole rule
- e. None of the above

**8.** Which sequence of commands correctly builds the calculator?
- a. `gcc simple_calc.tab.c lex.yy.c -o parser_scanner`; `bison -d simple_calc.y`; `flex scanner.lex`
- b. `bison -d simple_calc.y`; `flex scanner.lex`; `gcc simple_calc.tab.c lex.yy.c -o parser_scanner`
- c. `flex scanner.lex`; `gcc lex.yy.c -o parser_scanner`; `bison -d simple_calc.y`
- d. `bison simple_calc.y`; `gcc scanner.lex -o parser_scanner`
- e. None of the above

**9.** Which file does `bison -d simple_calc.y` produce?
- a. `lex.yy.c`
- b. `scanner.lex`
- c. `parser_scanner`
- d. `yylex()`
- e. None of the above (it produces `simple_calc.tab.c` and `simple_calc.tab.h`)

**10.** What is stored in the global variable `yylval`?
- a. The parse tree
- b. The grammar rule being applied
- c. The start symbol
- d. The stack
- e. None of the above (it holds the semantic value of the last token)

**11.** Which bison rule matches the BNF `E → T | E + T`?
- a. `exp: term | exp ADD term ;`
- b. `exp → term | exp + term`
- c. `exp = term | exp ADD term`
- d. `exp: term, exp ADD term`
- e. None of the above

**12.** Suppose you replace the grammar with `exp: exp ADD exp | exp MUL exp | NUMBER`, with no precedence declarations. What is the main problem?
- a. Unreachable symbols
- b. Missing semantic actions
- c. The grammar is ambiguous (parser conflicts)
- d. Left recursion is forbidden
- e. None of the above

**13.** You want unary minus, so that `-3 * 2` evaluates to −6. Which added rule is correct?
- a. `factor: SUB factor { $$ = $2; }`
- b. `factor: SUB factor { $$ = -$2; }`
- c. `factor: SUB factor { $$ = -$1; }`
- d. `factor: SUB factor { $$ = $1 - $2; }`
- e. None of the above

**14.** You want right-associative exponentiation (`2^3^2` = 2^(3^2)). Which rule is correct?
- a. `power: base POW power`
- b. `power: power POW base`
- c. `power: base POW base`
- d. `power: POW power`
- e. None of the above

**15.** Which rule adds parentheses to the calculator?
- a. `term: '(' exp ')' { $$ = $1; }`
- b. `term: '(' exp ')' { $$ = $3; }`
- c. `term: exp`
- d. `term: '(' exp ')' { $$ = $2; }`
- e. None of the above

**16.** Which flex pattern matches an unsigned integer such as `25`?
- a. `[0-9]`
- b. `[0-9]+`
- c. `[0-9]*`
- d. `[1-9]`
- e. None of the above

**17.** Which flex pattern matches a floating-point number such as `3.14`?
- a. `[0-9]"."`
- b. `[0-9]*.[0-9]*`
- c. `[0-9]+"."[0-9]+`
- d. `"."[0-9]`
- e. None of the above

**18.** Which abstract machine does bison's parsing correspond to in theory?
- a. A pushdown automaton
- b. A DFA
- c. A Turing machine
- d. A linear bounded automaton
- e. None of the above

**19.** A calculator uses right-recursive rules `exp: term SUB exp | term`. What does it print for `10 - 4 - 3`?
- a. 3
- b. 5
- c. 9
- d. 11
- e. None of the above

**20.** Which rule lets the calculator accept chained assignment such as `x = y = 11`?
- a. `stmt: stmt '=' VAR`
- b. `stmt: VAR '=' exp`
- c. `stmt: exp '=' exp`
- d. `stmt: VAR '=' stmt | exp`
- e. None of the above

---

## Answer Key

| # | Ans | Why |
|---|-----|-----|
| 1 | b | flex handles tokens (regular languages); bison handles syntax |
| 2 | c | `total` is an identifier |
| 3 | a | identifier, `=`, number, `;` |
| 4 | c | flex builds the scanner from regular expressions |
| 5 | d | bison produces `yyparse()` |
| 6 | a | ((15/5)\*2 − 4) + 2 = 4 |
| 7 | c | `$1` = exp, `$2` = ADD, `$3` = factor |
| 8 | b | run bison and flex, then gcc links both generated files |
| 9 | e | `simple_calc.tab.c` and `simple_calc.tab.h` |
| 10 | e | `yylval` holds the token's semantic value |
| 11 | a | `:` for →, `;` ends the production |
| 12 | c | ambiguous grammars cause shift/reduce conflicts |
| 13 | b | `$2` is the operand, so negate it |
| 14 | a | right recursion gives right associativity |
| 15 | d | `$2` is the `exp` between the parentheses |
| 16 | b | one or more digits |
| 17 | c | digits, a literal dot, digits |
| 18 | a | CFG ↔ PDA (stack-based parsing); flex ↔ DFA |
| 19 | c | 10 − (4 − 3) = 9 |
| 20 | d | right recursion lets an assignment contain an assignment |
