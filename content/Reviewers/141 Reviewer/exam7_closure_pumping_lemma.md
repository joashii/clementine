# CMSC 141 Exam 7: Closure Properties and the Pumping Lemma for CFLs (Lecture 14a)

**Items:** 20 | **Difficulty:** Average | **Type:** Application, multiple choice

**Directions:** Choose the best answer: **a, b, c, d**, or **e** (none of the above).

**Pumping lemma for CFLs:** if L is a CFL there is a constant n such that every z ∈ L with |z| ≥ n can be written z = uvwxy with |vwx| ≤ n, vx ≠ ε, and uvⁱwxⁱy ∈ L for all i ≥ 0.

---

**1.** Which closure property shows that L = { 0ⁿ1ⁿ2ᵐ | n, m ≥ 0 } is a CFL?
- a. Intersection of two CFLs
- b. Complement of a DCFL
- c. Concatenation of the CFL { 0ⁿ1ⁿ } with the regular language 2*
- d. Reversal of a CFL
- e. None of the above

**2.** S1 and S2 are the start variables of CFGs for L1 and L2. Which start production gives a grammar for L1 ∪ L2?
- a. Su → S1 | S2
- b. Su → S1S2
- c. Su → ε | S1Su
- d. Su → S2S1
- e. None of the above

**3.** Which start production gives a grammar for L1* (S1 is the start variable of L1's grammar)?
- a. Sk → S1S1
- b. Sk → S1 | ε
- c. Sk → S1Sk only
- d. Sk → ε | S1Sk
- e. None of the above

**4.** What language is PAL ∩ a*b*, where PAL is the set of palindromes over {a,b}?
- a. { aⁿbⁿ | n ≥ 0 }
- b. a* ∪ b*
- c. ∅
- d. a*b*
- e. None of the above

**5.** The language a*b* − { aⁿbⁿ | n ≥ 0 } is:
- a. Regular
- b. Not a CFL
- c. A CFL but not regular
- d. Finite
- e. None of the above

**6.** Let L1 = { 0ⁿ1ⁿ2ᵐ } and L2 = { 0ᵐ1ⁿ2ⁿ }, both CFLs. What is L1 ∩ L2?
- a. { 0ⁿ1ⁿ2ⁿ }, which is not a CFL
- b. { 0ⁿ1ⁿ2ⁿ }, which is a CFL
- c. ∅
- d. { 0ⁿ1ⁿ }
- e. None of the above

**7.** L is a deterministic CFL. What can we say about its complement L'?
- a. L' is not a CFL
- b. L' is regular
- c. Membership in L' is undecidable
- d. L' is a deterministic CFL
- e. None of the above

**8.** Which grammar generates the reversal of { aⁿb²ⁿ | n ≥ 0 }?
- a. S → aSbb | ε
- b. S → bbSa | ε
- c. S → SS | ε
- d. S → aSb | bb
- e. None of the above

**9.** Applying the homomorphism h(0) = ab, h(1) = c to { 0ⁿ1ⁿ | n ≥ 0 }, which CFG generates h(L)?
- a. S → aSbc | ε
- b. S → abSc | c
- c. S → abSc | ε
- d. S → 0S1 | ε
- e. None of the above

**10.** To prove L = { 0ᵐ1ᵐ2ᵐ | m ≥ 1 } is not a CFL, which string z is a good choice for pumping length n?
- a. 0ⁿ1ⁿ
- b. 0ⁿ1ⁿ2ⁿ⁺¹
- c. (012)ⁿ
- d. 0ⁿ1ⁿ2ⁿ
- e. None of the above

**11.** With z = 0ⁿ1ⁿ2ⁿ, why can vwx not contain both a 0 and a 2?
- a. Any substring containing a 0 and a 2 has length at least n + 2, but |vwx| ≤ n
- b. 0 and 2 are different symbols
- c. vx must be nonempty
- d. Pumping down removes them
- e. None of the above

**12.** Suppose vwx has no 2's and vx has a zeros and b ones (a + b > 0). Pumping down (i = 0) gives uwy = 0ⁿ⁻ᵃ1ⁿ⁻ᵇ2ⁿ. Why is uwy ∉ L?
- a. It is no longer of the form 0*1*2*
- b. It has more 2's than 0's
- c. The numbers of 0's, 1's, and 2's can no longer all be equal
- d. It is longer than z
- e. None of the above

**13.** To prove { aᵖ | p is prime } is not a CFL, take z = aᵖ (p prime, p ≥ n) with |vx| = k ≥ 1. For which exponent i is the length of uvⁱwxⁱy surely composite?
- a. i = 0
- b. i = p + 1
- c. i = 1
- d. i = 2
- e. None of the above

**14.** Is { aⁿbᵐcⁿdᵐ | n, m ≥ 1 } context-free?
- a. No; the a–c and b–d dependencies cross, and the pumping lemma applies
- b. Yes; S → aSc | bSd
- c. Yes; it is regular
- d. Yes; S → aSdb | ε
- e. None of the above

**15.** Which of these languages is context-free?
- a. { x c x | x ∈ {a,b}* }
- b. { aⁿbⁿcⁿ | n ≥ 1 }
- c. { aᵐbᵐcⁿdⁿ | m, n ≥ 1 }
- d. { aᵐbⁿcᵐdⁿ | m, n ≥ 1 }
- e. None of the above

**16.** To show { x ∈ {a,b,c}* | #a = #b = #c } is not a CFL, which regular language do you intersect it with to get { aⁿbⁿcⁿ }?
- a. a*b*c*
- b. a*b*
- c. (abc)*
- d. {a,b,c}*
- e. None of the above

**17.** Which statement is the negated pumping condition used to prove a language is not a CFL?
- a. For every n and every z ∈ L with |z| ≥ n, some split pumps correctly
- b. There is an n such that every z ∈ L has a valid split
- c. For every z ∈ L there is an n with a split such that all i keep the string in L
- d. For every n there is z ∈ L with |z| ≥ n such that every valid split has some i with uvⁱwxⁱy ∉ L
- e. None of the above

**18.** CFLs are closed under union but not under intersection. What follows for complement?
- a. CFLs are closed under complement
- b. CFLs are closed under complement only when finite
- c. CFLs are not closed under complement
- d. The complement of any CFL is regular
- e. None of the above

**19.** What is the complement of PAL (the non-palindromes over {a,b})?
- a. Regular
- b. A CFL but not regular
- c. Not a CFL
- d. Finite
- e. None of the above

**20.** Which of these operations does **not** always give a CFL when applied to CFLs?
- a. Union
- b. Concatenation
- c. Kleene star
- d. Reversal
- e. None of the above (all of them preserve CFLs)

---

## Answer Key

| # | Ans | Why |
|---|-----|-----|
| 1 | c | { 0ⁿ1ⁿ } is a CFL, 2\* is regular, and concatenation preserves CFLs |
| 2 | a | union: Su → S1 \| S2 |
| 3 | d | star: Sk → ε \| S1Sk |
| 4 | b | aⁿbᵐ is a palindrome only if n = 0 or m = 0 |
| 5 | c | equals { aᵐbⁿ \| m ≠ n } |
| 6 | a | intersection of two CFLs need not be a CFL |
| 7 | d | DCFLs are closed under complement (DPDA with outcomes swapped) |
| 8 | b | reversal gives b²ⁿaⁿ |
| 9 | c | replace 0 by ab, 1 by c in S → 0S1 \| ε |
| 10 | d | 0ⁿ1ⁿ2ⁿ is in L and has length ≥ n |
| 11 | a | the 1ⁿ block separates 0's from 2's |
| 12 | c | at least one count drops while another stays n |
| 13 | b | length p + pk = p(1 + k) is composite |
| 14 | a | crossing (interleaved) dependencies |
| 15 | c | aᵐbᵐ followed by cⁿdⁿ (nested pairs) |
| 16 | a | L ∩ a\*b\*c\* = { aⁿbⁿcⁿ } |
| 17 | d | for every n, some z, every split, some i fails |
| 18 | c | De Morgan: closure under complement would give closure under intersection |
| 19 | b | grammar S → aSa \| bSb \| aTb \| bTa, T → aT \| bT \| ε; PAL is not regular |
| 20 | e | all four preserve CFLs |
