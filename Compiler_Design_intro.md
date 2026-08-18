# 🛠️ Compiler Design — GATE CSE 2027 Roadmap
### From Zero to Mastery — Full Phase-Wise Study Plan

> **Track:** Discrete Mathematics → Theory of Computation → **Compiler Design**
> **Weightage:** ~3–5 marks (small subject, high tractability)
> **Standard Text:** Aho, Sethi, Ullman — *Compilers: Principles, Techniques, and Tools* (the "Dragon Book")
> **Prerequisite:** Theory of Computation (Regular Expressions, DFA/NFA, CFG, PDA) — do not start Compiler Design before TOC is solid.

---

## 📌 Why This Subject Matters

Compiler Design is one of the **highest ROI-per-hour** subjects in the GATE CSE syllabus. It's small, self-contained, and almost entirely mechanical once the core parsing algorithms click — no sprawling memorization like OS or DBMS. Historically it delivers **1–2 guaranteed questions** on parsing table construction (LL(1)/SLR/CLR/LALR) and 1 on syntax-directed translation or intermediate code. Students who "finish it in two weeks" often walk away with a near-perfect score in this section because the question patterns are highly repetitive across PYQs (1990–2026).

The catch: it is **conceptually cumulative**. If your grammar/parsing table construction has an error in step 2, every downstream answer (FOLLOW sets, action/goto tables, conflict detection) collapses. This roadmap is built to catch those failure points early.

---

## 🧭 Six-Phase Study Plan

| Phase | Focus | Approx. Duration | Depends On |
|---|---|---|---|
| **Phase 0** | Prerequisite check (TOC recall) | 1–2 days | Theory of Computation |
| **Phase 1** | Lexical Analysis | 2–3 days | Regular Expressions, DFA/NFA |
| **Phase 2** | Syntax Analysis — Top-Down Parsing | 3–4 days | CFG, Ambiguity, Left Recursion |
| **Phase 3** | Syntax Analysis — Bottom-Up Parsing | 5–6 days | Phase 2 |
| **Phase 4** | Syntax-Directed Translation & IR | 2–3 days | Phase 3 |
| **Phase 5** | Runtime Environments, Optimization, Code Gen | 2–3 days | Phase 4 |

**Total: ~2–2.5 weeks of focused study**, then fold into rolling PYQ revision cycles alongside TOC.

---

## Phase 0 — Prerequisite Gate Check

Before opening a compiler design chapter, you must be able to do the following **without hesitation**:

- Convert a Regular Expression → NFA (Thompson's Construction) → DFA (Subset Construction)
- Identify and remove **left recursion** and perform **left factoring** on a grammar
- Compute **FIRST** and **FOLLOW** sets for any CFG by hand, fast
- Distinguish ambiguous vs. unambiguous grammars, and know why ambiguity breaks parsers

> ⚠️ **GATE Trap:** A huge fraction of "wrong LL(1)/LALR table" mistakes trace back to a wrong FIRST/FOLLOW set, not a misunderstanding of the parsing algorithm itself. Drill FIRST/FOLLOW until it's mechanical.

---

## Phase 1 — Lexical Analysis (The Front Door)

### Core Topics
- Role of the lexical analyzer (scanner) in the compiler pipeline
- Tokens, Lexemes, Patterns — the three-way distinction (a classic MCQ trap)
- Specifying tokens via Regular Expressions
- Recognizing tokens: transition diagrams, DFA-based scanners
- **Lex/Flex** tool basics (conceptual, rarely numerically tested but shows up in theory MCQs)
- Error recovery strategies in lexical analysis (panic mode)
- Maximal Munch principle — why lexers always match the longest possible token

### GATE Traps
- Confusing **token** (category, e.g. `ID`) with **lexeme** (actual string, e.g. `count`) with **pattern** (rule that generates the lexeme, e.g. regex `[a-zA-Z_][a-zA-Z_0-9]*`)
- Off-by-one errors when counting states in a DFA-based scanner built from combined REs for multiple token classes
- Questions that require minimizing a DFA before counting states — don't skip minimization

### Practice Signal
If you can look at a language spec (a small set of token REs) and draw a single combined DFA recognizing all token classes with distinct accepting states, you're done with this phase.

---

## Phase 2 — Top-Down Parsing

### Core Topics
- Parsing as a search over derivations — leftmost derivation for top-down
- **Recursive Descent Parsing** — direct grammar-to-code mapping, backtracking issues
- **Predictive Parsing** — why it needs no backtracking (LL(1) condition)
- Grammar transformations required before predictive parsing:
  - Left recursion elimination (direct and indirect)
  - Left factoring
- **FIRST() and FOLLOW() computation** — the single most tested mechanical skill in this subject
- Constructing the **LL(1) parsing table**
- Detecting **LL(1) conflicts** (multiple entries in one table cell → grammar is not LL(1))
- Parsing a string using the LL(1) table + stack simulation

### GATE Traps
- Forgetting that **FOLLOW(A)** only includes `$` if A can be the start symbol (or reachable to end of derivation from start)
- Missing **ε (epsilon)** propagation rules when computing FIRST of a production with multiple symbols
- Assuming a grammar is automatically not LL(1) just because it's left-recursive — you must first eliminate recursion and *then* re-check
- Confusing "grammar is ambiguous" with "grammar is not LL(1)" — ambiguous ⇒ not LL(1), but not-LL(1) does **not** imply ambiguous (it might still be handled by a stronger parser)

### Worked Skill Checklist
- [ ] Eliminate left recursion from a 3+ production grammar in under 3 minutes
- [ ] Compute FIRST/FOLLOW for 5+ non-terminals without errors
- [ ] Fill an LL(1) table and correctly flag conflict cells

---

## Phase 3 — Bottom-Up Parsing (The Heavy Middle)

This is the **highest-yield sub-topic** in the entire subject. Budget the most time here.

### Core Topics
- Shift-Reduce parsing — the general bottom-up strategy
- **Handle** pruning and rightmost derivation in reverse
- **LR(0) parsing** — items, closure, goto, constructing the LR(0) automaton (DFA of states)
- **SLR(1) parsing** — using FOLLOW sets to resolve reduce actions; SLR conflicts
- **CLR(1) / LR(1) parsing** — canonical collection with lookahead built into items (larger but conflict-free for a wider grammar class)
- **LALR(1) parsing** — merging LR(1) states with identical cores; why it's the practical middle ground (used by YACC/Bison)
- The parser generator hierarchy: **LL(1) ⊂ SLR(1) ⊂ LALR(1) ⊂ CLR(1)** — memorize this containment, it is asked directly and indirectly almost every year
- Shift-Reduce and Reduce-Reduce conflicts — what causes each, and which parser class resolves them

### GATE Traps
- **The single most common GATE trap in this entire subject**: being asked "is this grammar SLR(1)?" and computing FIRST/FOLLOW correctly but making an arithmetic slip while checking for conflicts in the action table
- Believing LALR(1) has the *same power* as CLR(1) — it doesn't; LALR can introduce **reduce-reduce conflicts** that don't exist in the full CLR(1) table, even though state *count* matches CLR's core count after merging
- Miscounting the number of states in an LR(0)/SLR(1) automaton — always double check by explicitly listing item sets, don't estimate
- Confusing when a grammar is SLR(1) but not LR(0) (SLR uses FOLLOW to resolve conflicts LR(0) can't)
- Forgetting the **augmented grammar** (adding S' → S) before constructing any LR automaton — skipping this silently breaks the "accept" condition

### Worked Skill Checklist
- [ ] Construct the full LR(0) item-set automaton for a 4–5 production grammar
- [ ] Build the SLR(1) parsing table and correctly identify any conflicts
- [ ] Explain, with an example, a grammar that is SLR(1) but not LR(0)
- [ ] Explain, with an example, a grammar that is LALR(1) but not SLR(1)
- [ ] State the exact containment hierarchy from memory: LR(0) ⊂ SLR(1) ⊂ LALR(1) ⊆ CLR(1)

> 💡 **Study Tip:** Don't try to memorize CLR(1) item-set construction cold. Instead, master LR(0)/SLR(1) construction completely first — CLR(1) is the same closure/goto machinery with a lookahead symbol tacked onto every item. The delta in effort is small if the foundation is solid.

---

## Phase 4 — Syntax-Directed Translation & Intermediate Code

### Core Topics
- Syntax-Directed Definitions (SDDs) — attaching semantic rules to grammar productions
- **Synthesized vs. Inherited attributes** — the classic distinction (synthesized flows bottom-up, inherited flows top-down/sideways)
- **S-attributed** vs **L-attributed** grammars — which can be evaluated during a single bottom-up / top-down pass
- Annotated parse trees — evaluating attributes at each node
- Dependency graphs for attribute evaluation order
- Intermediate representations:
  - **Three-Address Code (TAC)**
  - Quadruples, Triples, Indirect Triples
  - Postfix notation / Abstract Syntax Trees (AST)
- Translation schemes for common constructs: expressions, if-else, while loops, boolean expressions (short-circuit / backpatching basics)

### GATE Traps
- Mixing up synthesized and inherited when an attribute depends on a **sibling** node (that's inherited, not synthesized, even though it "feels" like it should flow up)
- Assuming every L-attributed grammar is automatically S-attributed — it's the other way around (S-attributed ⊂ L-attributed)
- Errors in converting an expression like `a = b + c * d` into TAC — precedence mistakes are the #1 source of wrong TAC answers
- Miscounting temporary variables generated for a given expression tree — count strictly by the number of internal (operator) nodes, not operands

### Worked Skill Checklist
- [ ] Convert 5 different arithmetic/boolean expressions into correct TAC by hand
- [ ] Classify a given SDD's attributes as synthesized or inherited, and the grammar as S-attributed / L-attributed / neither

---

## Phase 5 — Runtime Environments, Optimization & Code Generation

### Core Topics
**Runtime Environments**
- Activation records — structure (return address, saved registers, local vars, parameters)
- Storage allocation strategies: static, stack-based, heap-based
- Parameter passing: call-by-value vs. call-by-reference (conceptual, occasionally numerical)
- Symbol table structure and scope resolution (static vs. dynamic scoping)

**Code Optimization**
- Basic blocks and control flow graphs (CFGs)
- Local optimization: constant folding, common subexpression elimination, dead code elimination
- Loop optimization basics: loop-invariant code motion
- **Data flow analysis** basics — reaching definitions (conceptual level is enough for GATE; deep numeric DFA problems are rare but have appeared)

**Target Code Generation**
- Conceptual only for GATE — issues in code generation, register allocation basics (spilling, live ranges) at a high level

### GATE Traps
- Confusing **static scoping** (resolved at compile time, based on lexical nesting) with **dynamic scoping** (resolved at runtime, based on call stack) — GATE loves giving a nested-function code snippet and asking for output under each scoping rule
- Skipping basic block boundary rules — a basic block *ends* at a jump/branch and *starts* right after one, or at a label; miscounting basic blocks cascades into wrong CFG answers

### Worked Skill Checklist
- [ ] Given a small program with nested functions, evaluate output under static vs. dynamic scoping
- [ ] Partition a code snippet into correct basic blocks and draw the CFG

---

## 📊 Full Topic-Weightage Snapshot (for revision prioritization)

| Topic | Typical GATE Marks | Priority |
|---|---|---|
| SLR / CLR / LALR table construction | ★★★★★ | Highest — spend the most time |
| FIRST/FOLLOW & LL(1) tables | ★★★★☆ | High |
| Grammar hierarchy (LL/SLR/LALR/CLR containment) | ★★★☆☆ | High (theory MCQ, low effort) |
| Synthesized/Inherited attributes | ★★★☆☆ | Medium |
| Three-Address Code generation | ★★★☆☆ | Medium |
| Lexical analysis (tokens/lexemes/DFA scanners) | ★★☆☆☆ | Medium-low |
| Static vs dynamic scoping | ★★☆☆☆ | Medium-low |
| Runtime environments / activation records | ★★☆☆☆ | Low-medium |
| Code optimization / data flow analysis | ★☆☆☆☆ | Low (conceptual only) |

---

## 🔗 Cross-Subject Dependency Map

```
Theory of Computation (CFG, PDA, Ambiguity)
            │
            ▼
   Compiler Design — Parsing
            │
            ├──► Discrete Math (Set theory reused in FIRST/FOLLOW as sets)
            │
            └──► Programming & Data Structures (stacks, trees — used conceptually
                  in parsing algorithms and AST construction)
```

Study Compiler Design **immediately after** finishing Theory of Computation while CFG/ambiguity concepts are still fresh — this is the single biggest time-saver in this pairing.

---

## 🎯 PYQ Strategy (1990–2026)

1. **First pass:** Solve all PYQs on LL(1)/SLR(1)/LALR(1) table construction — these repeat in structure year after year with different grammars.
2. **Second pass:** Solve all PYQs on TAC generation and attribute classification.
3. **Third pass:** Solve theory-only MCQs (grammar hierarchy, scoping, tokens/lexemes) — these are quick points, don't skip them even though they feel "soft."
4. **Timed mock:** Attempt a mixed 8–10 question Compiler Design PYQ set in **under 25 minutes** — this subject should never be a time sink relative to its weightage.

---

## 📚 Resource Stack

- **Primary text:** Aho, Sethi, Ullman — *Compilers: Principles, Techniques, and Tools* (Dragon Book) — Chapters on Lexical Analysis, Syntax Analysis, Syntax-Directed Translation, Run-Time Environments
- **Supplementary:** Standard GATE-focused video series for Compiler Design (visual walk-throughs of LR automaton construction are worth the time investment)
- **Practice:** GATE Overflow / GateQA-style PYQ banks, filtered by subject tag "Compiler Design," sorted by year

---

## ✅ Final Self-Assessment Before Moving On

You are ready to consider this subject "GATE-ready" when you can, from a blank page and without notes:

1. Take any small grammar (4–6 productions) and construct its LL(1) table, flagging conflicts correctly
2. Construct the LR(0) automaton and SLR(1) table for the same grammar, and explain any discrepancy between the two results
3. State the LL(1)/SLR(1)/LALR(1)/CLR(1) containment hierarchy and give a one-line reason for each strict inclusion
4. Convert a 3-operator arithmetic expression into three-address code without counting errors
5. Classify a 3-rule SDD's attributes as synthesized/inherited and state whether the grammar is L-attributed

If all five are fluent, Compiler Design is locked in — fold it into weekly rolling revision with TOC and move to the next subject pairing.
