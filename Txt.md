## Compiler Design – GATE CSE Topic Breakdown


**Official GATE CSE syllabus scope:** Lexical analysis, parsing, syntax-directed translation, runtime environments, intermediate code generation, local optimization, data flow analysis for global optimization. Here's the full expansion with subtopics, ordered the way you'd actually want to study them (phases of a compiler).


### 1. Introduction to Compilers
- Compiler vs interpreter vs assembler vs translator
- Phases of a compiler (lexical → syntax → semantic → intermediate code → optimization → code gen)
- Analysis-synthesis model
- Front end vs back end
- Symbol table — structure, role across phases
- Error handling (types of errors, error recovery strategies per phase)
- Bootstrapping, cross-compilers (conceptual, low GATE weight but occasionally asked)


### 2. Lexical Analysis
- Role of lexical analyzer, tokens/lexemes/patterns
- Regular expressions → NFA (Thompson's construction)
- NFA → DFA (subset construction)
- DFA minimization
- Lex/Flex tool basics (conceptual)
- Input buffering (sentinel-based buffering — occasionally asked)
- Error recovery in lexical analysis


### 3. Syntax Analysis (Parsing)
- Context-free grammars — derivations, parse trees, ambiguity
- Left recursion elimination, left factoring
- **Top-down parsing**
  - Recursive descent parsing
  - Predictive parsing (LL(1))
  - FIRST and FOLLOW computation
  - LL(1) parsing table construction, conflicts
- **Bottom-up parsing**
  - Shift-reduce parsing, handle pruning
  - Operator precedence parsing
  - LR(0) items, canonical collection, DFA of items
  - SLR(1) parsing — construction, conflicts
  - CLR(1)/LR(1) parsing — construction
  - LALR(1) parsing — construction, merging states
  - Comparison: LL vs LR vs SLR vs CLR vs LALR (very high yield for GATE)
- Ambiguous grammar handling, operator precedence/associativity rules
- Parser conflicts: shift-reduce, reduce-reduce


### 4. Syntax-Directed Translation (SDT)
- Syntax-directed definitions (SDD)
- Synthesized vs inherited attributes
- S-attributed and L-attributed definitions
- Dependency graphs, evaluation order
- Annotated parse trees
- SDT schemes — translation during parsing
- Bottom-up evaluation of S-attributed definitions
  

### 5. Type Checking
- Type expressions, type systems
- Static vs dynamic type checking
- Type equivalence (structural vs name)
- Type conversion / coercion


### 6. Runtime Environments
- Storage organization: code, static, stack, heap
- Activation records — structure, fields
- Storage allocation strategies: static, stack-based, heap-based
- Parameter passing mechanisms: call by value, call by reference, call by name, call by value-result
- Static vs dynamic scoping
- Symbol table implementation across nested scopes
- Access to non-local names (static/dynamic links, displays) — mostly conceptual for GATE
  

### 7. Intermediate Code Generation
- Intermediate representations: postfix notation, three-address code (TAC), quadruples, triples, indirect triples
- Syntax trees / DAGs for expressions
- TAC generation for: expressions, assignment statements, boolean expressions, control flow (if/while/for), backpatching
- Short-circuit code for boolean expressions


### 8. Code Optimization
- **Local optimization**
  - Basic blocks — construction rules
  - Control flow graph (CFG) construction
  - DAG representation of basic blocks, common subexpression elimination
  - Peephole optimization techniques
- **Global (data-flow-based) optimization**
  - Data flow analysis: reaching definitions, available expressions, live variable analysis, use-def chains
  - Global common subexpression elimination
  - Copy propagation
  - Dead code elimination
  - Loop optimizations: loop invariant code motion, induction variable elimination, strength reduction
  - Constant folding/propagation
    

### 9. Code Generation
- Issues in code generator design
- Basic code generation algorithm (register/address descriptors)
- Register allocation and assignment (graph coloring — conceptual)
- Instruction selection basics
- (Low weight in GATE — usually 0–1 question; skip deep dive unless time permit's
  

---

### High-yield areas (based on past GATE trends)
- LL(1) vs SLR vs CLR vs LALR — table construction and comparison (almost guaranteed 1–2 questions)
- FIRST/FOLLOW computation
- Three-address code generation
- Basic blocks, CFG, DAG-based local optimization
- Activation records and parameter passing semantics
- Ambiguous grammar / grammar-to-language mapping (often mixed with TOC)

### Where this overlaps with your other subjects
- Regex → NFA → DFA overlaps directly with your TOC roadmap — study lexical analysis right after finishing DFA/NFA equivalence in TOC.
- Grammar/CFG concepts overlap with TOC's CFG-PDA section — sequence them back to back.
- SDT and type checking lean on Discrete Math (relations/functions) lightly, but mostly stand alone.

Given how you've been pairing subjects by conceptual dependency, Compiler Design slots naturally right after TOC (since parsing theory builds directly on CFGs and automata) and before/alongside COA (since runtime environments and code generation connect to your CPU/memory work).

Want me to turn this into your standard dark-themed single-file HTML roadmap with the six-phase structure and GATE trap callouts, like your other subject guides?
