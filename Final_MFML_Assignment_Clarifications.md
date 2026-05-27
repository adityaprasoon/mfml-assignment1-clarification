# MFML Assignment 1 — Comprehensive Clarifications

> **This document is AI-generated.** It was produced by synthesizing information from the following sources:
>
> 1. **Assignment_1_MFML.pdf** — The original assignment document with questions and instructions.
> 2. **Announcement by faculty.txt** — Two official announcements posted by the MFML team on the course website (Taxila).
> 3. **WhatsApp Chat with BITS MFML With Prof. Saurabh** — Clarifications posted by Prof. Saurabh in the course WhatsApp group between May 16–27, 2026.
> 4. **Assignment_1_Approach_and_Hints.pdf** — A detailed approach and hints document shared by Prof. Saurabh.
> 5. **Particular_and_Homogeneous_Solutions.pdf** — A companion note on solving Ax = b (particular and homogeneous solutions).
> 6. **three_decompositions.pdf** — A companion note on LU, Cholesky, and QR decompositions.

---

## Table of Contents

- [General Instructions & Submission Guidelines](#general-instructions--submission-guidelines)
- [Q1 — Finding Solutions of Linear Systems](#q1--finding-solutions-of-linear-systems)
  - [Q1(a) — REF and RREF](#q1a--ref-and-rref)
  - [Q1(b) — Pivot/Non-Pivot Columns, Particular & Null-Space Solutions](#q1b--pivotnon-pivot-columns-particular--null-space-solutions)
  - [Q1(c) — Random 6×9 System (End to End)](#q1c--random-69-system-end-to-end)
- [Q2 — Matrix Decompositions](#q2--matrix-decompositions)
  - [Q2(a) — LU Decomposition via Elementary Matrices](#q2a--lu-decomposition-via-elementary-matrices)
  - [Q2(b) — Cholesky Decomposition](#q2b--cholesky-decomposition)
  - [Q2(c) — QR Decomposition (Gram–Schmidt)](#q2c--qr-decomposition-gramschmidt)
  - [Q2(d) — Random 7×5 Matrix and Diagonal of R](#q2d--random-75-matrix-and-diagonal-of-r)
- [Deadline & Logistics](#deadline--logistics)

---

## General Instructions & Submission Guidelines

### Programming Language
- Use **any programming language** of your choice: Python, R, C, Java, MATLAB, Octave, Julia, etc.
- **Excel is explicitly not allowed** (original assignment instruction 1).
- **Python is recommended** by Prof. Saurabh for its universality in ML.
  > *"My recommendation going forward is Python, simply because it's universal in modern ML and the skills compound across every later course and ML role."*
  > — Saurabh, 5/18/26, 11:10

- If new to coding, a **Jupyter notebook or Google Colab** is the easiest starting point (no installation needed for Colab).
  > *"If you're new to running code, a Jupyter notebook or Google Colab is the easiest starting point — no installation needed for Colab."*
  > — Saurabh, 5/18/26, 10:45

### What "No Built-In Functions" Means

This rule targets **functions that do the mathematical work for you**. You must implement the algorithms yourself; the language's ordinary plumbing is fine.

#### NOT Allowed (these defeat the purpose of the question):
- `numpy.linalg.solve`, `numpy.linalg.matrix_rank`, `numpy.linalg.inv`
- `scipy.linalg.lu`, `scipy.linalg.qr`, `scipy.linalg.cholesky`
- `sympy.Matrix(...).rref()`, `.LUdecomposition()`, `.QRdecomposition()`, `.cholesky()`
- MATLAB/Octave: `rref()`, `lu()`, `qr()`, `chol()`, `A\b`, `inv()`, `linsolve()`
- Any "one-line" function that returns the echelon form, pivots, L, U, Q, R, or the solution directly
  > — Saurabh, 5/18/26, 10:40

#### Allowed (these are language features/utilities, not algorithms):
- Loops, conditionals, arithmetic operators, indexing, slicing
- Container creation — `np.zeros`, `np.eye`, lists of lists, array copying
- Reading `.shape`, basic broadcasting
- Printing / formatting
- Random-number generation (`np.random.*`, `random.*`)
- `math.sqrt` (or `** 0.5`) for the Cholesky diagonal
- `abs()`, `len()`, `enumerate()`, `.append()`, list comprehensions
- `Fraction()` — allowed for Q2
- `np.eye()` for identity matrix generation
- `@` operator for matrix multiplication (for Q2)
- `.T` for transpose (for Q2)
- `np.dot()`, `np.norm()` (for Q2 verification)
  > — Saurabh, 5/18/26, 10:40; Hints PDF §0; Saurabh, 5/26/26, 13:01; Saurabh, 5/26/26, 14:27; Saurabh, 5/26/26, 20:03

#### Scope of the "No Built-In" Rule:
- **Q1**: The restriction applies strictly. Use numpy only for storage and random generation — not for the algorithm itself. No `np.linalg.*`.
  > — Faculty Announcement 1, Q3 answer
- **Q2**: Don't call ready-made decompositions (`np.linalg.lu/cholesky/qr`). Build L/U/Cholesky/QR yourself. But `numpy` for arrays and the final norm check is fine. Matrix multiply (`@`), transpose (`.T`), determinant, and inverse are all fine **for Q2**.
  > *"For Q2, just don't call ready-made decompositions (no np.linalg.lu/cholesky/qr) → build L/U/Cholesky/QR yourself. numpy for arrays and the final norm check is fine."*
  > — Saurabh, 5/26/26, 13:01

**Quick self-test** (from Hints PDF): If your REF function is three lines long, you have called a built-in. A correct hand-written REF/RREF/LU/Cholesky/QR is roughly 15–25 lines of loops.

### What "Code Snippet" Means
- You must **actually run the code** in a programming language — not pseudocode, not a flowchart, not a verbal description.
- "Code snippet" means **the portion of your working code** that performs the specific activity asked.
- You do **not** need to submit the full script/notebook — just the relevant snippet per sub-question.
- The output shown in your handwritten PDF must be **the output your code produced** — not numbers you wrote by hand.
  > — Saurabh, 5/18/26, 10:45

### What "Random Entries" Means
- **Entries of A and b must be system-generated** (instruction 3 — deterministic/hand-typed entries get no marks).
- Integers (`random.randint`) or reals (`random.uniform`) are both fine.
- The **dimensions** may be taken from the user (or hardcoded where specified, e.g. 6×9); the **entries** must be random.
- The range of random numbers is not restricted. Students used values like -10 to 10, -5 to 5, -2 to 2, 0 to 9 — all are fine.
  > *"There are no restrictions on them."*
  > — Saurabh, 5/27/26, 12:29

### Numerical Care
- Work in **floating point** throughout. Elimination naturally produces decimals — that is expected.
- When checking for a zero pivot, do **not** test `== 0`. Use a tolerance: treat `abs(value) < 1e-9` as zero.
- Decimal output is fine. Using `round(value, 2)` or preserving fractions — both acceptable.
  > *"It's okay."* (in response to decimal-valued outputs)
  > — Saurabh, 5/24/26, 15:46

### Freezing Random Runs
- Use a **seed** (`np.random.seed()` or `random.seed()`) so every run produces the same A. This ensures your handwritten matrix, code, and output all match.
  > *"Set a seed (np.random.seed) so every run produces the same A."*
  > — Saurabh, 5/26/26, 13:19

### Submission Format ("Handwritten")
- **Handwritten** means written by hand — pen on paper, scanned/photographed, compiled into a **single PDF** named `<YOUR_BITSID>.pdf`.
- Writing on an **iPad/tablet with a stylus** is also acceptable — it is still handwritten, just digital ink. Export as a single PDF.
- **Not acceptable**: typed text in a "handwriting" font, or only screenshots of code/output without the worked steps.
  > — Saurabh, 5/18/26, 10:27

- **Code snippets** can be pasted as images/screenshots — or handwritten. Both are acceptable.
  > *"You can take snapshots of code snippets and attach to the handwritten stuff and bundle them as a single pdf."*
  > — Faculty Announcement 2, Q3 answer

- **Input and output** can be shown as screenshots from your code environment.
  > *"Shouldn't be an issue"* (in response to sharing Google Colab code snippet screenshots)
  > — Saurabh, 5/23/26, 13:01

- **Derivations, observations, and explanations** must be in your own handwriting.
  > *"code itself can be screenshots but the explanation of what it does must be handwritten"*
  > — Confirmed by Saurabh, 5/23/26, 13:02

- **Helper functions**: For Q1(a) and Q1(b), it is sufficient to include only the **core logic** for REF/RREF computation. Helper functions (like `absolute(x)`, `augmented_matrix()`) do not need to be included.
  > — Faculty Announcement 2, Q1 answer

  However, for Q2, helper functions **should** be included (e.g., transpose, matrix multiply helpers for Cholesky verification).
  > *"yes they should be"* (regarding helper functions for Q2)
  > — Saurabh, 5/25/26, 11:08

### What Your Page Should Contain (Per Sub-Question)
1. The random input (A, b)
2. The code snippet
3. A few lines explaining the logic (handwritten)
4. The output (REF, RREF, pivots, particular solution, null-space basis, L, U, Q, R, etc.)
5. The verification (residual ≈ 0)
6. Any observation explicitly asked for (e.g., Q2(d) on diag(R))

> *"Keep it tight → one to two handwritten pages per sub-question is plenty. Quality of reasoning > length."*
> — Saurabh, 5/18/26, 10:38

### Edge Cases
- **Edge-case handling** (m > n, empty input, etc.) is **not expected** for this assignment.
  > — Saurabh, 5/24/26, 16:32

### Partial/Step Marking
- Yes, there will be partial/step marking.
  > — Saurabh, 5/18/26, 10:56

### Coding in Exams
- There are **very bleak chances** of coding questions in the midterm/endterm exams. The exams are paper-based.
  > — Saurabh, 5/18/26, 10:30; Ganeshkumar BITS, 5/20/26, 09:48

---

## Q1 — Finding Solutions of Linear Systems

**Overview**: The whole of Q1 is one pipeline: build the augmented matrix, reduce it, read off the solution structure. Parts (a) and (b) build the tools; part (c) runs them on a random 6×9 system. (Hints PDF §1)

### Q1(a) — REF and RREF

**What it asks** (2 marks: 1 for REF + 1 for RREF):

Write a code taking as input a matrix A of size m×n and a vector b of size m×1, where m and n are arbitrarily large numbers and m < n, constructing the augmented matrix and performing REF and RREF — without using built-in functions.

**Deliverables**: The code snippet showing the procedure for REF and RREF.

#### Detailed Clarifications:

1. **Input dimensions**: Your code should accept any m, n with m < n as inputs (parameters to the function). m and n can be user inputs. The "random entries" requirement refers to the entries of A and b, not the dimensions.
   > *"For Q1(a), your code should accept any m, n with m < n as inputs (parameters to the function)."*
   > — Saurabh, 5/18/26, 10:30

2. **Division by zero**: This won't happen if you implement properly. The pivot is always a nonzero entry you pick, so just search the column for a nonzero value and swap rows if needed before dividing. If the whole column below is zero, skip it (that's a free column, perfectly fine). No need to switch matrices.
   > — Saurabh, 5/20/26, 06:14

3. **Approach** (from Hints PDF):
   - **REF (forward elimination)**: Keep a current pivot row. For each column, look for a non-zero entry at or below the pivot row; swap it up; if the whole column below is zero, move on. Use the pivot to clear every entry below it (`factor = entry / pivot`, then subtract `factor × pivot-row`). Advance the pivot row.
   - **RREF (from REF)**: Divide each pivot row by its pivot so the pivot becomes 1, then clear the entries above each pivot the same way you cleared below.
   - Use a **tolerance** for zero-checking: treat `abs(value) < 1e-9` as zero.

4. **Hand-worked example**: Show understanding by hand-solving a small **3×4** (or 3×3) matrix on paper with brief explanation. This proves you understand what the loop is doing without requiring a long handwritten derivation.
   > *"If you want to demonstrate that your understanding is solid show 2–3 representative row operations by hand on a small 3×3 or 3×4 example somewhere in your Q1(a) submission."*
   > — Saurabh, 5/19/26, 11:23

5. **Can Q1(a) and Q1(c) use the same code?** Yes.
   > — Saurabh, 5/23/26, 22:18

#### Summary of What to Submit for Q1(a):
- A small hand-worked example (e.g., 3×4 matrix) showing REF and RREF with brief explanation
- Code snippet for the REF function
- Code snippet for the RREF function
- Screenshot of the code producing the same result as the hand example

---

### Q1(b) — Pivot/Non-Pivot Columns, Particular & Null-Space Solutions

**What it asks** (1 mark):

Identify the pivot and non-pivot columns and find the particular solution and solutions to Ax = 0.

**Deliverables**: The code snippet showing the pivot and non-pivot columns, particular solution and the solutions to Ax = 0.

#### Detailed Clarifications:

1. **What to compute**:
   - **Pivot columns**: Columns of the RREF that contain a leading 1 in some row → basic variables.
   - **Non-pivot (free) columns**: Columns that do not contain a leading 1 → free variables.
   - **Particular solution (xp)**: Set every free variable to 0. Then each pivot variable equals the right-hand-side entry in its row.
   - **Null-space basis (Ax = 0)**: Each free variable gives one basis vector: set that free variable to 1, the others to 0, and read off the pivot variables as the negatives of that free column's entries in the pivot rows.
   - **General solution**: x = xp + α₁n₁ + α₂n₂ + ···

2. **The code should output both**: the particular solution and the null-space basis.
   > *"The code should output both: the particular solution and the null space basis. Q1(b) explicitly asks for 'the particular solution and the solutions to Ax = 0,' so both are code deliverables."*
   > — Saurabh, 5/25/26, 17:59

3. **Pivots are a property of A**, not of "Ax = 0" specifically.
   > — Saurabh, 5/18/26, 18:51

4. **With m < n**, you have at most m pivots and at least n − m free columns — so there are infinitely many solutions.

#### Summary of What to Submit for Q1(b):
- Code snippet identifying pivot and non-pivot columns
- Code snippet computing particular solution
- Code snippet computing null-space basis (solutions to Ax = 0)
- Brief handwritten explanation of the logic
- Output screenshots

---

### Q1(c) — Random 6×9 System (End to End)

**What it asks** (2 marks: ¼ × 8 quantities):

Consider a random 6×9 matrix A and a suitable b and show the REF, RREF, pivot columns, non-pivot columns, the particular solution, the solutions to Ax = 0, the general solution and verify the general solution.

**Deliverables**: The random matrix A, vector b and other quantities mentioned in the question.

#### Detailed Clarifications:

1. **Size is fixed at 6×9** — do not ask the user for dimensions. Hard-coding 6×9 in Q1(c) is correct.
   > *"No change needed. Hard-coding 6×9 in Q1(c) is correct — the question fixes that size."*
   > — Saurabh, 5/25/26, 10:21

2. **Only the entries are random** — dimensions are fixed. The "dimensions may be input" hint was only for Q1(a), the general routine.
   > *"'random' refers to the entries, not the dimensions. For Q1(c) the size is fixed at 6×9; only the entries are randomly generated."*
   > — Saurabh, 5/23/26, 23:08

3. **Choosing b — the critical trap**: If you draw b at random independently of A, the system is almost always inconsistent (no solution exists). The clean fix:
   > **Draw a random vector x_seed and set b = A · x_seed.** Now b lies in the column space of A by construction, so the system is guaranteed consistent.
   > — Saurabh, 5/18/26, 10:30; Hints PDF §1 Q1(c)

4. **No hand-solving needed for the 6×9** — the code does that. You show its input and output.
   > *"Q1(c): Run the same code on a random 6×9 matrix. Add screenshots of the logic, the input (A and b), and the output. No hand-solving needed here."*
   > — Saurabh, 5/23/26, 23:08

5. **Code snippet for Q1(c)**: Yes, code snippet should be included (for verification of the general solution at minimum).
   > *"Code snippet should be included."* (in response to whether Q1(c) verification code should be shown)
   > — Faculty Announcement 2, Q2 answer

6. **Verification**: Pick random coefficients α for the free variables, form x = xp + Σ αk nk, and check that A · x equals b (within tolerance). Report the residual A·x − b ≈ 0.

7. **The 8 quantities to show** (¼ mark each):
   1. The random matrix A (6×9)
   2. The vector b
   3. REF of [A|b]
   4. RREF of [A|b]
   5. Pivot columns
   6. Non-pivot columns
   7. Particular solution
   8. Solutions to Ax = 0 (null-space basis), general solution, and verification

#### Summary of What to Submit for Q1(c):
- The randomly generated A (6×9) and b — copied from code output
- The final REF of [A|b] — copied from code output
- The final RREF of [A|b] — copied from code output
- Pivot columns, non-pivot columns
- Particular solution
- Null-space basis
- General solution
- Verification residuals (A·xp − b ≈ 0 and A·x_general − b ≈ 0)
- Code snippet (can reuse Q1(a)/Q1(b) code — just show input/output for the 6×9 case)

---

## Q2 — Matrix Decompositions

**General rules for Q2**:
- Q2 needs **only code snippets + output**. No pen-paper dry run required.
  > *"Q2 needs only code snippets + output. No pen-paper dry run required."*
  > — Saurabh, 5/26/26, 13:01
- The "no built-in" rule for Q2: Don't call ready-made decompositions. But numpy for arrays, `@` for matrix multiply, `.T` for transpose, and norm checks are fine.
- You can use the same SPD matrix for Q2(a) and Q2(b).
- A short explanation of the code should be included to make it easy for evaluators.
  > — Saurabh, 5/26/26, 14:17

### Q2(a) — LU Decomposition via Elementary Matrices

**What it asks** (2 marks):

Consider a symmetric and positive definite matrix A of size n×n. Write a code to construct an elementary matrix for every elementary row operation that is performed on A and using the theory explained in the class write the decomposition of A as LU.

**Deliverables**: The code snippet showing the generation of elementary matrices for a given elementary row operation and getting L and U, and verification of A = LU.

#### Detailed Clarifications:

1. **Elementary matrices are the deliverable**: Build each elementary matrix explicitly. Storing multipliers directly in L (the Doolittle shortcut) skips them and answers a different question.
   > *"The elementary matrices are themselves the deliverable. Storing multipliers directly in L (the Doolittle shortcut) skips them and answers a different question."*
   > — Hints PDF §2 Q2(a)

2. **What is an elementary matrix**: The identity matrix with one entry below the diagonal made non-zero. For example, to subtract 2×row1 from row2, multiply on the left by the identity with −2 at position (2,1).

3. **The process**:
   - Apply one such E per elimination step: Em ··· E₁ A = U
   - Then A = E₁⁻¹ ··· Em⁻¹ U = L U
   - Each E⁻¹ is the identity with the sign of its off-diagonal entry flipped
   - The product of these inverses is L
   - Start U = A, L = I. For each below-diagonal position, compute f, build E, multiply U = E · U, and fold the inverse into L.

4. **SPD matrix generation**: A need not be randomly generated. A hardcoded SPD matrix is fine. No SPD check routine needed.
   > *"Q2(a) → A need not be random. A hardcoded SPD matrix is fine. No SPD check routine needed."*
   > — Saurabh, 5/26/26, 18:40

   If you want to generate one by construction: `A = B^T B + nI` for a random B (always SPD).
   > — Hints PDF §2 Q2(a); Saurabh, 5/26/26, 18:40

5. **Why SPD**: SPD guarantees no zero pivots, so you never need row swaps — every elementary matrix is the simple "add a multiple of one row to another" type.

6. **Verification**: Show that A = L × U (compute L @ U and compare to A).

7. **The L from LU is NOT the same as the L from Cholesky** — they are different matrices produced by different algorithms. Q2(b) requires its own separate code.
   > *"You're right that Cholesky is 'the extra part,' but the extra part is a different way of producing L, so it needs its own snippet. Q2(a)'s code won't produce the Cholesky L."*
   > — Saurabh, 5/25/26, 22:00

#### Summary of What to Submit for Q2(a):
- The SPD matrix A (hardcoded or generated)
- Code snippet showing elementary matrix construction and LU decomposition
- The resulting L and U matrices
- Verification: A = L × U
- Brief handwritten explanation

---

### Q2(b) — Cholesky Decomposition

**What it asks** (1 mark):

For the above problem (same SPD A), work out the Cholesky's decomposition.

**Deliverables**: The code snippet showing the generation of L and verification of A = LL^T.

#### Detailed Clarifications:

1. **Cholesky is a "matrix square root"**: Find a lower-triangular L with positive diagonal such that A = L L^T.

2. **The algorithm** — two rules cover everything:
   - **Diagonal**: `L[i][i] = sqrt( A[i][i] − sum of squares of L entries already placed in row i )`
   - **Below diagonal**: `L[i][j] = ( A[i][j] − sum of products L[i][k]·L[j][k] for k < j ) / L[j][j]`

3. **Why only for SPD**: Symmetry lets L L^T match A; positive-definiteness keeps the quantity under each square root positive.

4. **Cholesky is about half the work of LU** since L and L^T share entries.

5. **Verification**: Show that A = L × L^T.

#### Summary of What to Submit for Q2(b):
- Code snippet for Cholesky decomposition
- The resulting L matrix
- Verification: A = L × L^T
- Brief handwritten explanation

---

### Q2(c) — QR Decomposition (Gram–Schmidt)

**What it asks** (1 mark):

Given n linearly independent vectors in m dimensions (the corresponding matrix with these as columns is A of size m×n), with m > n, read about the QR decomposition of a matrix and generate Q and R.

**Deliverables**: The code snippet generating Q and R for the given A.

#### Detailed Clarifications:

1. **Algorithm**: Use **Gram–Schmidt process** (Classical Gram–Schmidt is sufficient for full marks; Modified Gram–Schmidt is welcome but not required).
   > *"QR → Classical Gram-Schmidt (in the material) is enough for full marks. Modified GS is welcome if you prefer it, but not required."*
   > — Saurabh, 5/26/26, 18:40

2. **The idea**: Process columns left to right. Normalize the first to get q₁. For each later column, subtract its projection onto every q already built; what remains is perpendicular — normalize it. The projection coefficients fill R above the diagonal; the residual lengths fill R's diagonal.

3. **Verification**: Include checks A = QR and Q^T Q = I (orthonormality).
   > — Hints PDF §2 Q2(c)

4. **Include verification for Q2(c)**: Yes.
   > *"Yes → include verification for (c) QR (Q^TQ=I and A=QR)."*
   > — Saurabh, 5/26/26, 13:01

#### Summary of What to Submit for Q2(c):
- Code snippet for QR decomposition via Gram–Schmidt
- The resulting Q and R matrices
- Verification: A = QR and Q^T Q ≈ I
- Brief handwritten explanation

---

### Q2(d) — Random 7×5 Matrix and Diagonal of R

**What it asks** (1 mark):

Take a random 7×5 matrix having all its columns as linearly independent and decompose into Q and R. What is your observation on the diagonal elements of R?

**Deliverables**: The random matrix, Q and R and your observation on the diagonal elements of R.

#### Detailed Clarifications:

1. **Matrix**: Random 7×5 matrix. A random matrix has linearly independent columns with probability one, so Gram–Schmidt runs without breaking down.

2. **Set a seed** so the matrix matches your output across runs.
   > — Saurabh, 5/26/26, 18:40

3. **The observation on R's diagonal** (this is a written/theoretical answer, not code):
   - Every diagonal entry of R comes out **strictly positive**.
   - R[j][j] is the length of the residual left after removing projections — a length, so it is ≥ 0, and it cannot be 0 because a zero residual would mean that column is a combination of the earlier ones, contradicting linear independence.
   - Because the diagonal is non-zero, R is invertible (det R = product of the diagonal ≠ 0).
   - A near-zero diagonal entry would be a warning sign of near-linear-dependence (collinearity).
   > — Hints PDF §2 Q2(d)

4. **For Q2(d), the observation on R's diagonal is required — written, not code.**
   > — Saurabh, 5/26/26, 13:01

5. **No intermediate steps needed**: Just submit the random 7×5 matrix, final Q, final R, and your written observation.
   > — Saurabh, 5/26/26, 18:40

#### Summary of What to Submit for Q2(d):
- The random 7×5 matrix (with seed set)
- The resulting Q and R matrices
- Your handwritten observation on the diagonal elements of R
- No intermediate steps needed

---

## Deadline & Logistics

| Item | Detail |
|------|--------|
| **Original deadline** | 27th May, 2026 19:00 hrs (per assignment PDF instruction 6) |
| **Extended deadline** | **31st May, 2026** (extended on Taxila; confirmed by Saurabh) |
| **Recommended submission** | By 30th May |
| **File format** | Single PDF named `<YOUR_BITSID>.pdf` |
| **File size limit** | 10 MB on Taxila |
| **Submission platform** | Taxila (e-learn portal) |
| **Email submissions** | Not accepted (assignment instruction 7) |
| **Individual work** | This is not a group activity. Each student submits individually. Copying is strictly prohibited. |

> *"I think the last date is 31st.. take your time but submit by 30th."*
> — Saurabh, 5/25/26, 19:57

> *"In the instructions document it was mentioned as 27th May! But as you requested it is extended to 31st."*
> — +91 91418 62372, 5/25/26, 20:12

---

## ⚠ Conflicting / Ambiguous Information

The following areas had conflicting or evolving information across sources. Both versions are presented for transparency.

### 1. Q1(c) — Is Code Required?

- **Assignment PDF wording**: Deliverables say *"The random matrix A, vector b and other quantities mentioned in the question"* — does not explicitly mention "code snippet."
- **Faculty Announcement 2**: *"For Q1(c), is showing the final output sufficient, or should the code snippet used for verifying the general solution also be included?"* → Answer: **"Code snippet should be included."**
- **Prof. Saurabh (5/18/26, 10:56)**: Initially said the deliverable wording "is asking you to show the results, not implying that the results are produced manually. But I will clarify after the meet today."
- **Final resolution**: Code snippet **should** be included for Q1(c), at minimum for verification. The same code from Q1(a)/Q1(b) is reused — just show input/output for the 6×9.

### 2. Q1(c) — Hand-Solving vs Code Only

- **Saurabh (5/23/26, 22:24)** clarified: *"Q1(c) specifically asks you to run it on a random 6×9 matrix. This part must use 6×9, not 3×4."* and *"You don't need to hand-solve the 6×9 — the code does that."*
- **Conclusion**: For Q1(c), run the code on 6×9 and show screenshots. No hand-solving of the 6×9 is needed. The 3×4 hand example belongs in Q1(a)/Q1(b).

### 3. Q2(a) — Random vs Hardcoded SPD Matrix

- **Assignment PDF**: Says *"Consider a symmetric and positive definite matrix A"* — does not specify random.
- **Saurabh (5/26/26, 18:40)**: *"A need not be random. A hardcoded SPD matrix is fine."*
- **Conclusion**: Hardcoded SPD matrix is acceptable for Q2(a) and Q2(b). If you want to generate one: `A = B^T B + nI`.

### 4. Q1(a) Input — User Input vs Hardcoded Dimensions

- **Saurabh (5/23/26, 23:28)**: *"Function should take m, n"* as parameters.
- **Saurabh (5/21/26, 21:45)**: Confirmed m and n from user, entries random.
- **For Q1(c)**: Hard-coding 6×9 is correct since the question fixes that size.
- **Conclusion**: Q1(a) function should accept m, n as parameters. Q1(c) can hardcode 6×9.

### 5. Using Pure Python Lists vs NumPy

- **Saurabh (5/26/26, 14:27)**: numpy, randint, abs, len are all fine for Q1. The "no built-in functions" rule only bars ready-made solvers.
- **A student asked (5/26/26, 14:47)**: *"We can implement the code using pure python lists and loops as well right?"* — No explicit answer recorded, but based on the general guidance, pure Python is acceptable.
- **Conclusion**: Both pure Python lists and numpy arrays are acceptable for implementation.

---

## Quick Reference: Submission Structure

### Q1(a) — REF and RREF
| Component | Format |
|-----------|--------|
| Hand-worked 3×4 example | Handwritten |
| Code snippet (REF + RREF) | Screenshot or handwritten |
| Logic explanation | Handwritten |
| Code output matching hand example | Screenshot |

### Q1(b) — Pivots, Particular & Null-Space Solutions
| Component | Format |
|-----------|--------|
| Code snippet | Screenshot or handwritten |
| Logic explanation | Handwritten |
| Output | Screenshot |

### Q1(c) — 6×9 Random System
| Component | Format |
|-----------|--------|
| Code snippet (reuse from Q1a/b) | Screenshot |
| Random A (6×9) and b | Screenshot from code |
| REF, RREF | Screenshot from code |
| Pivot/non-pivot columns | Screenshot from code |
| Particular solution, null-space, general solution | Screenshot from code |
| Verification (residual ≈ 0) | Screenshot from code |

### Q2(a) — LU Decomposition
| Component | Format |
|-----------|--------|
| SPD matrix A | Screenshot or handwritten |
| Code snippet (elementary matrices + LU) | Screenshot or handwritten |
| L and U output | Screenshot |
| Verification A = LU | Screenshot |
| Logic explanation | Handwritten |

### Q2(b) — Cholesky
| Component | Format |
|-----------|--------|
| Code snippet | Screenshot or handwritten |
| L output | Screenshot |
| Verification A = LL^T | Screenshot |
| Logic explanation | Handwritten |

### Q2(c) — QR Decomposition
| Component | Format |
|-----------|--------|
| Code snippet (Gram–Schmidt) | Screenshot or handwritten |
| Q and R output | Screenshot |
| Verification A = QR and Q^TQ ≈ I | Screenshot |
| Logic explanation | Handwritten |

### Q2(d) — Random 7×5 and Diagonal Observation
| Component | Format |
|-----------|--------|
| Random 7×5 matrix | Screenshot from code |
| Q and R output | Screenshot from code |
| Observation on diagonal of R | **Handwritten** (required — not code) |
