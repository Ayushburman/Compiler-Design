# Compiler-Design
Here's where each topic actually shows up outside the exam — useful for building intuition and for GATE questions that frame things in an applied context.

### 1. Introduction to Compilers
- Understanding toolchains (gcc, clang, javac) and build pipelines
- JIT compilers in JVM, V8 (Chrome's JS engine), .NET CLR
- Cross-compilation for embedded systems (ARM binaries built on x86 machines)
- Bootstrapping — how new languages self-host (Rust compiler written in Rust)

### 2. Lexical Analysis
- Tokenizers in every compiler/interpreter (Python, Java, C)
- Syntax highlighting in editors (VS Code, Sublime) — uses regex/DFA-based scanners
- Search engines & text processing — regex matching (grep, sed, awk all use DFA/NFA internally)
- Network intrusion detection systems — pattern matching on packet payloads
- Lex/Flex — used to build scanners for DSLs (domain-specific languages), config file parsers
- Input validation (email/phone regex validators) — practical regex-to-DFA application

### 3. Syntax Analysis (Parsing)
- Every compiler's parser (recursive descent parsers are common in hand-written compilers like early GCC, Clang)
- Parser generators — Yacc/Bison (LALR), ANTLR (LL) — used to build parsers for custom languages, config formats (JSON, XML, YAML parsers)
- SQL query parsing (parses your SQL into an execution plan)
- Browser HTML/CSS parsing engines
- JSON/XML parsers in APIs — literally CFG-driven parsing
- Protocol parsers — parsing structured network protocols (HTTP headers, gRPC)

### 4. Syntax-Directed Translation
- Calculator/expression evaluators (annotate parse tree with computed values)
- Type checkers built during parsing (attribute-driven semantic checks)
- Code editors' "live evaluation" features (e.g., spreadsheet formula engines like Excel evaluate expressions using SDT-like attribute propagation)
- Template engines (Jinja2, Handlebars) — translate template syntax into output using synthesized attributes

### 5. Type Checking
- Static type checkers: TypeScript compiler, mypy for Python, Java/C++ compilers
- IDE type inference and autocomplete (IntelliSense relies on type equivalence rules)
- Catching bugs at compile time vs runtime (why Rust/Go catch more errors before execution)
- API contract validation (gRPC/Protobuf schema type-checking)

### 6. Runtime Environments
- Stack frame management — literally why you get "stack overflow" errors in recursion
- Debuggers (gdb, lldb) — inspect activation records/call stacks
- Memory profilers — track heap allocation strategies
- Closures in JS/Python — implemented via access to non-local names (static links)
- Function call conventions in OS/hardware interaction (calling conventions like cdecl, stdcall)
- Garbage collectors — operate on the heap storage model established here

### 7. Intermediate Code Generation
- LLVM IR — the most real-world example; GCC's GIMPLE, Java bytecode, .NET's CIL/MSIL all use TAC-like intermediate forms
- Portable compilation — same IR can target multiple architectures (this is literally why LLVM supports x86, ARM, RISC-V from one frontend)
- Optimizing compilers use TAC/DAG as the stage where most optimization passes actually operate
- Bytecode interpreters (Python's .pyc, Java's .class files)

### 8. Code Optimization
- Every production compiler's `-O1/-O2/-O3` flags (GCC, Clang, MSVC)
- JIT optimization in JVM HotSpot, V8 — profiles hot code paths and applies loop optimization, dead code elimination live
- Compiler-driven performance tuning — why identical code runs faster after -O2 (common subexpression elimination, constant folding)
- Data flow analysis — also used in static analysis tools for bug detection (finding uninitialized variables, unreachable code) — same algorithms as Coverity, SonarQube, linters
- Loop optimizations — critical in HPC/scientific computing compilers (auto-vectorization in Intel's ICC, GCC's loop unrolling)

### 9. Code Generation
- Backend of every compiler — turns IR into x86/ARM/RISC-V machine code
- Register allocation (graph coloring) — directly affects your program's real-world speed; this is why compilers matter for embedded/performance-critical systems
- WebAssembly compilation — compiling C/Rust/Go down to Wasm bytecode
- GPU shader compilers (compiling GLSL/HLSL to GPU-specific ISA)

---

### Why this matters for GATE specifically
GATE occasionally frames questions as "which real-world tool uses X" or gives an applied scenario (e.g., a code snippet and asks what optimization applies). Knowing LLVM as the concrete IR example, and knowing register allocation = graph coloring, are both patterns that have shown up as conceptual distractors in past papers.

Want this folded into the same HTML roadmap as an "applications" sidebar next to each phase, or kept separate as a quick-reference note?
