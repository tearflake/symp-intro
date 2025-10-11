---
id: architecture
---

# Architecture

**Designing form and meaning**

---

## 🧩 Overview

Symp is composed of three independent but composable subsystems in two parts of a structure:

```

Symbolmatch  →  Symbolverse / Symbolprose
(form)                  (meaning)

````

Each subsystem is implemented as a **frame** — a symbolic interpreter that takes a symbolic input and produces a new symbolic result.

The **glue language** `(APPLY …)` binds them into a single computation.

---

## ⚙️ Core Evaluation Model

### The Frame Contract

A frame is a pair of subprograms:
```
(FRAME
  (SYNTAX <apply>)
  (SEMANTICS <apply>))
````

When executed via `(APPLY <frame> <expr>)`, the following happens:

* **Syntax Stage**: The `<expr>` is passed to the `(SYNTAX …)` subprogram. This is typically an `(APPLY symbolmatch …)` expression that validates grammar.
   * On success → input passes unchanged to semantics.
   * On failure → returns an error path.
* **Semantics Stage**: The validated `<expr>` is then passed to the `(SEMANTICS …)` subprogram. This is typically `(APPLY symbolverse …)` or `(APPLY symbolprose …)`, which transform or execute the expression.

---

## 🧮 Example: Execution Flow

```
(APPLY
  (FRAME
    (SYNTAX
      (APPLY
        symbolmatch
        (SEXPR (RULES (FLAT <start> ATOMIC)))))

    (SEMANTICS
      (APPLY
        symbolprose
        (SEXPR
          (GRAPH
            (EDGE
              (SOURCE BEGIN)
              (INSTR
                (ASGN RESULT ("Hello" "from" PARAMS)))
              (TARGET END)))))))
  
  (SEXPR Symp))
```

**Execution Steps:**

* Symbolmatch: Validate input is atomic - Pass
* Symbolprose: Perform computation - `(Hello from Symp)`

---

## 🔁 Data Flow

All components use a shared symbolic representation:

```
<s-expression> = <ATOM> | (<s-expression>+)
```

Each `(APPLY …)` passes these symbolic structures along the pipeline:

```
Input
  ↓
Symbolmatch — verifies shape
  ↓
Symbolverse/Symbolprose — rewrites structure or returns a graph execution result
  ↓
Output
```

Because the data format is uniform, any module can be swapped or nested.

---

## 💻 Backend Integration

Symp can act as a **backend framework** in multiple contexts:

### 1. 🧠 CLI or REPL Mode

* Run standalone interpreter.
* Load frames from files.
* Evaluate `(APPLY …)` directly.

### 2. 🌐 Web Service Mode

* Host a lightweight HTTP or WebSocket server.
* Send `(APPLY …)` payloads
* Receive evaluated S-expression output.
* Ideal for web-based IDEs or symbolic assistants.

### 3. 🔌 Embedded Library

* Expose the Symp engine as an API (JavaScript).
* Applications can define and run frames internally, using Symbolmatch, Symbolverse, and Symbolprose as composable services.

---

## 🧩 Extensibility

Symp is fully modular:

* Define new frames using the same `(FRAME (SYNTAX …) (SEMANTICS …))` pattern.
* Store them as `.frame` files.
* Load dynamically into the runtime.
* Build DSLs, symbolic assistants, or logic engines without modifying the core.

---

## 🔮 Design Philosophy

> Symp is not a compiler.
> It’s a *conversation between symbols.*

Grammar and meaning are distinct layers of that conversation, open for introspection and modification.

This is the foundation of **Symbolic Computing** in the world where symbols won.
