---
id: architecture
---

# Architecture

**Symbolic computing pipeline for form, and meaning**

---

## 🧩 Overview

Symp is composed of three independent but composable subsystems in a two parts of a structure:

```

Symbolmatch  →  Symbolverse / Symbolprose
(form)                  (meaning)

````

Each subsystem is implemented as a **frame** — a symbolic interpreter that takes
an input `(SEXPR …)` and produces a new symbolic result.

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

1. **input stage**
   The syntax stage must be preceeded by the input stage.
   
2. **Syntax Stage**
   The `<expr>` is passed to the `(SYNTAX …)` subprogram.
   This is typically an `(APPLY symbolmatch …)` expression that validates grammar.

   * ✅ On success → input passes unchanged to semantics.
   * ❌ On failure → returns an error path `(SEXPR (ERROR …))`.

3. **Semantics Stage**
   The validated `<expr>` is then passed to the `(SEMANTICS …)` subprogram.
   This is typically `(APPLY symbolverse …)` or `(APPLY symbolprose …)`,
   which transform or execute the expression.

4. **Output Stage**
   The semantics stage must end with a constant `(SEXPR …)` —
   the final symbolic result of computation.

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
                (ASGN RESULT ("Hello from" PARAMS)))
              (TARGET END)))))))
  
  (SEXPR "Symp"))
```

**Execution Steps:**

| Stage | Module                  | Action                              | Result                          |
| ----- | ----------------------- | ----------------------------------- | ------------------------------- |
| 1️⃣    | Input                   | Input must be applied to a frame    | `(APPLY <frame> (SEXPR "Symp"))`|
| 2️⃣    | Symbolmatch             | Validate input is atomic            | Pass                            |
| 3️⃣    | Symbolverse/Symbolprose | Perform computation                 | `(SEXPR ("Hello from" "Symp"))` |
| 4️⃣    | Output                  | Return constant symbolic expression | `("Hello from" "Symp")`         |

---

## 🔁 Data Flow

All components use a shared symbolic representation:

```
S-EXPRESSION = Atom | [S-EXPRESSION]
```

Each `(APPLY …)` passes these symbolic structures along the pipeline:

```
Input (…)
     ↓
Symbolmatch — verifies shape
     ↓
Symbolverse/Symbolprose — rewrites structure or executes as a graph
     ↓
Output (…)
```

Because the data format is uniform, any module can be swapped or nested.

---

## 💻 Backend Integration

Symp can act as a **backend framework** in multiple contexts:

### 🧠 1. CLI or REPL Mode

* Run standalone interpreter.
* Load frames from files.
* Evaluate `(APPLY …)` directly.

### 🌐 2. Web Service Mode

* Host a lightweight HTTP or WebSocket server.
* Receive `(APPLY …)` payloads as JSON:

  ```json
  { "apply": "(APPLY symbolmatch (SEXPR (RULES ...)))" }
  ```
* Respond with evaluated `(SEXPR …)` output.
* Ideal for web-based IDEs or symbolic assistants.

### 🔌 3. Embedded Library

* Expose the Symp engine as an API (JavaScript).
* Applications can define and run frames internally,
  using Symbolmatch, Symbolverse, and Symbolprose as composable services.

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

