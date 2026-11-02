# NEO --- Compiler Development Roadmap

## Project Overview

NEO is a statically typed, compiled programming language designed with
AI and machine learning workloads in mind. Its goals are readable
syntax, a straightforward developer experience, native code generation,
and useful language-level support for numerical computing and tensors.

The initial implementation will use:

-   **Implementation language:** C++20
-   **Build system:** CMake
-   **Compiler backend:** LLVM
-   **Source extension:** `.neo`
-   **Compiler executable:** `neo`

Development will proceed incrementally. Each milestone should produce a
working, testable result before moving to the next one.

------------------------------------------------------------------------

## M0 --- Project Foundation

**Goal:** Establish the C++ project and integrate LLVM.

Tasks: - Create the repository structure. - Configure CMake and C++20. -
Create the initial `neo` executable. - Configure CMake to locate the
LLVM installation. - Link the compiler against the required LLVM
libraries. - Initialize a basic command-line interface. - Add a simple
build and run workflow. - Document the development environment and build
instructions.

**Milestone result:** The project builds successfully and the compiler
can use the LLVM C++ API. The executable prints its name and version.

------------------------------------------------------------------------

## M1 --- Lexer

**Goal:** Convert NEO source code into a sequence of tokens.

Tasks: - Define token types. - Implement source character traversal. -
Recognize identifiers and keywords. - Recognize integer and
floating-point literals. - Recognize strings and characters. - Recognize
operators and punctuation. - Handle whitespace and comments. - Track
line and column positions. - Support indentation through `NEWLINE`,
`INDENT`, and `DEDENT` tokens. - Report lexical errors with useful
source locations. - Add lexer unit tests.

**Milestone result:** A source file can be tokenized, and lexical errors
produce readable diagnostics.

------------------------------------------------------------------------

## M2 --- Parser and Abstract Syntax Tree

**Goal:** Transform tokens into a structured representation of the
program.

Tasks: - Define AST node types. - Implement parsing for expressions. -
Implement operator precedence and associativity. - Parse variable
declarations using `let` and `var`. - Parse function declarations and
calls. - Parse return statements. - Parse structs. - Parse `if`, `else`,
`while`, and `for` statements. - Parse indentation-based code blocks. -
Parse imports. - Add parser error recovery where practical. - Add parser
tests.

**Milestone result:** Valid NEO programs produce an AST, while invalid
syntax produces useful parser diagnostics.

------------------------------------------------------------------------

## M3 --- Semantic Analysis and Type Checking

**Goal:** Determine whether a parsed program is meaningful and
type-correct.

Tasks: - Implement symbol tables and lexical scopes. - Resolve variable
and function names. - Enforce declaration and mutability rules. -
Implement primitive types. - Implement local type inference. - Require
explicit function parameter and return types. - Check function calls and
return statements. - Validate struct declarations and construction. -
Implement optional and result types as the type system develops. - Check
tensor element types and dimensions. - Report semantic errors with
source locations. - Add semantic analysis tests.

**Milestone result:** NEO rejects invalid programs before code
generation and produces a typed representation of valid programs.

------------------------------------------------------------------------

## M4 --- NEO Intermediate Representation and LLVM Backend

**Goal:** Translate valid NEO programs into native machine code through
LLVM.

Tasks: - Design a typed NEO intermediate representation (NEO IR). -
Represent values, operations, functions, and control flow. - Lower the
typed AST into NEO IR. - Create corresponding LLVM types and values. -
Generate LLVM IR for expressions and statements. - Generate functions
and control-flow blocks. - Integrate LLVM optimization passes. -
Generate native object files or executables. - Add compiler integration
tests.

**Milestone result:** A basic NEO program can be compiled into a native
executable.

------------------------------------------------------------------------

## M5 --- Core Runtime and Standard Library

**Goal:** Provide the runtime functionality needed by ordinary NEO
programs.

Tasks: - Implement basic input and output. - Implement string and
character support. - Implement runtime support for structs. - Implement
`Option` and `Result`. - Establish runtime error handling conventions. -
Design garbage-collector integration. - Implement an initial tracing,
non-moving, stop-the-world mark-and-sweep garbage collector. - Define GC
roots and compiler/runtime safepoint conventions. - Add basic
standard-library modules. - Test runtime behavior and memory management.

**Milestone result:** NEO programs can use core language features and
runtime services reliably.

------------------------------------------------------------------------

## M6 --- Numerical Computing and Tensors

**Goal:** Add the numerical foundation that makes NEO useful for
scientific computing and machine learning.

Tasks: - Define tensor types and runtime representation. - Implement
tensor allocation and storage. - Implement compile-time tensor shape
checking. - Support tensor indexing and basic manipulation. - Implement
element-wise arithmetic. - Implement matrix multiplication using `@`. -
Add numerical operations and math functions. - Consider CPU execution
first, keeping future accelerator support in mind. - Add correctness and
performance tests.

**Milestone result:** NEO can create and manipulate tensors, and invalid
tensor dimensions are detected during compilation where possible.

------------------------------------------------------------------------

## M7 --- Machine Learning Foundations

**Goal:** Build the first machine-learning capabilities on top of NEO's
tensor system.

Tasks: - Implement automatic differentiation. - Build foundational
neural-network operations. - Provide common activation functions. -
Implement loss functions. - Implement optimizers such as SGD and Adam. -
Provide dataset loading and batching utilities. - Build a small
neural-network API. - Implement model training and evaluation
workflows. - Add end-to-end machine-learning examples. - Benchmark basic
workloads.

**Milestone result:** NEO can express, compile, and run a basic
machine-learning training workflow.

------------------------------------------------------------------------

## Development Principles

-   Keep each milestone small enough to build and test independently.
-   Prefer understanding each compiler stage before adding the next one.
-   Keep the frontend independent from LLVM wherever practical.
-   Use a custom NEO IR between semantic analysis and LLVM code
    generation.
-   Keep numerical runtime functionality separate from compiler frontend
    logic.
-   Add tests as features are implemented.
-   Avoid adding AI-specific syntax until ordinary language constructs
    and library APIs prove insufficient.
-   Treat the roadmap as a guide; revise it when implementation
    experience reveals a better design.

## First End-to-End Target

The first complete language example should be:

``` neo
fn main() -> Void:
    let message = "Hello from NEO!"
    print(message)
```

The intended command is:

``` powershell
neo run hello.neo
```

Expected output:

``` text
Hello from NEO!
```

This is a later integration target, not a requirement for M0.
