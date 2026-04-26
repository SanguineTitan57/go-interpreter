# go-interpreter

> A fully hand-written language interpreter, built entirely in Go — from lexer to evaluator.

This is a tutorial-driven project designed to accomplish two goals: deepen practical knowledge of the Go programming language, and build a solid, ground-up understanding of how programming languages actually work — from raw source text all the way to evaluated output. It serves as the technical foundation and proving ground for [**Nightingale**](https://github.com/aerilabs/nightingale-lang.git), a memory-safe programming language currently in development.

---

## Motivation

Most developers use programming languages every day without understanding what happens between typing code and seeing output. This project tears that black box open. By implementing every stage of interpretation by hand — with no parser-generator libraries or shortcuts — you gain an intimate understanding of:

- How source code is tokenized and given structure
- How grammar rules become trees of meaning
- How those trees are walked and evaluated into real values
- Where memory, scope, and safety concerns actually live in a language runtime

This is the groundwork for thinking seriously about **Nightingale** — not as a continuation of this codebase, but as an answer to a harder question: *what should a memory-safe, zero-GC, systems-grade language look like if you weren't constrained by prior art?*

---

## How It Works

The interpreter follows the classic pipeline of a tree-walking interpreter:

```
Source Code
    │
    ▼
┌─────────┐
│  Lexer  │  Breaks raw text into a flat stream of tokens
└─────────┘
    │
    ▼
┌──────────┐
│  Parser  │  Consumes tokens and builds an Abstract Syntax Tree (AST)
└──────────┘
    │
    ▼
┌──────────────┐
│  AST (Tree)  │  A structured, hierarchical representation of the program
└──────────────┘
    │
    ▼
┌───────────────┐
│   Evaluator   │  Walks the AST and produces output / side effects
└───────────────┘
```

### Lexer (Tokenizer)
The lexer reads the source string character by character and emits tokens — categorized chunks of meaning like `IDENT`, `INT`, `PLUS`, `LET`, `FUNCTION`, etc. It handles keywords, operators, literals, and whitespace.

### Parser
The parser consumes the token stream and applies a grammar to produce an **Abstract Syntax Tree**. It implements **Pratt parsing** (top-down operator precedence) for expressions, making it easy to reason about operator precedence and associativity without complex grammar tables.

### AST (Abstract Syntax Tree)
Every construct in the language — let bindings, function literals, if expressions, infix operations — becomes a node in the tree. The AST is the heart of the interpreter; all later stages operate on it.

### Evaluator
The evaluator recursively walks the AST, resolving identifiers through an **environment** (a scoped symbol table), applying operators, calling functions, and producing values. It handles closures, first-class functions, and return values natively.

---

## Language Features

The interpreted language supports:

- **Integer and Boolean literals** — `5`, `true`, `false`
- **Prefix and Infix expressions** — `!true`, `5 + 10 * 2`
- **Let bindings** — `let x = 10;`
- **If / Else expressions** — `if (x > 5) { x } else { 0 }`
- **Functions as first-class values** — `let add = fn(a, b) { a + b; };`
- **Closures** — functions that capture their surrounding environment
- **Return statements** — explicit early return from functions
- **String literals** — `"hello, world"`
- **Built-in functions** — `len()`, `puts()`, and others
- **Arrays and Hash Maps** — `[1, 2, 3]`, `{"key": "value"}`

---

## Project Structure

```
go-interpreter/
├── lexer/          # Tokenization logic
├── token/          # Token type definitions
├── ast/            # AST node definitions
├── parser/         # Pratt parser implementation
├── object/         # Runtime value system (integers, booleans, functions, etc.)
├── evaluator/      # Tree-walking evaluator
├── environment/    # Scoped variable bindings
├── repl/           # Read-Eval-Print Loop
└── main.go         # Entry point
```

---

## Getting Started

### Prerequisites

- [Go](https://go.dev/dl/) `1.21+`

### Run the REPL

```bash
git clone https://github.com/aerilabs/go-interpreter.git
cd go-interpreter
go run main.go
```

You'll be dropped into an interactive REPL:

```
Hello! This is the go-interpreter REPL.
Type expressions to evaluate them. Ctrl+C to exit.
>> let add = fn(a, b) { a + b };
>> add(3, 7)
10
>> 
```

### Run the Tests

```bash
go test ./...
```

---

## Relationship to Nightingale

[**Nightingale**](https://github.com/aerilabs/nightingale-lang.git) is the real target — a compiled, zero-GC, memory-safe language built for critical software development. Think: **blazing fast, deeply safe, and not beholden to anyone else's design decisions.**

This Go interpreter is not Nightingale's codebase or even its direct prototype. It's the **mental calibration phase** — the work you do before you're ready to make original language design decisions with confidence. Understanding how scopes chain, how values live and die, how a runtime actually tracks state — that's the foundation you need before you can reason clearly about designing a *better* memory model from scratch.

Rust proved that you don't need a garbage collector to write safe systems software. But ownership and borrowing, while brilliant, come with a steep ergonomic cost. **Nightingale's goal is to go further** — to develop a memory safety model that is:

- **Novel** — not a copy of Rust's ownership/borrow system
- **Ergonomic** — safe by default without fighting the compiler
- **Zero-cost** — no GC pauses, no runtime overhead, no hidden allocations
- **Built for critical systems** — the kind of software where correctness and performance are both non-negotiable

This interpreter project is where that thinking starts.

---

## References & Inspiration

This project is heavily inspired by the book series:

- **_Writing An Interpreter In Go_** — Thorsten Ball
- **_Writing A Compiler In Go_** — Thorsten Ball

Both are highly recommended for anyone who wants to go from zero to a working language implementation.

---

## License

MIT — see [LICENSE](./LICENSE) for details.
