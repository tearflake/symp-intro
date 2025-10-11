# Symp Architecture

**Symbolic computing pipeline for form, meaning, and execution**

---

## 🧩 Overview

Symp is composed of three independent but composable subsystems:

```

Symbolmatch  →  Symbolverse  →  Symbolprose
(form)         (meaning)       (execution)

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
  (SYNTAX   <apply>)
  (SEMANTICS <apply>))
````

When executed via `(APPLY <frame> <expr>)`, the following happens:

1. **Syntax Stage**
   The `<expr>` is passed to the `(SYNTAX …)` subprogram.
   This is typically an `(APPLY symbolmatch …)` expression that validates grammar.

   * ✅ On success → input passes unchanged to semantics.
   * ❌ On failure → returns an error path `(SEXPR (ERROR …))`.

2. **Semantics Stage**
   The validated `<expr>` is then passed to the `(SEMANTICS …)` subprogram.
   This is typically `(APPLY symbolverse …)` or `(APPLY symbolprose …)`,
   which transform or execute the expression.

3. **Output Stage**
   The semantics stage must end with a constant `(SEXPR …)` —
   the final symbolic result of computation.

---

## 🧮 Example: Execution Flow

```
(APPLY
  (FRAME
    (SYNTAX
      (APPLY symbolmatch
        (SEXPR (RULES (FLAT <start> ATOMIC)))))

    (SEMANTICS
      (APPLY symbolprose
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

| Stage | Module      | Action                              | Result                          |
| ----- | ----------- | ----------------------------------- | ------------------------------- |
| 1️⃣   | Symbolmatch | Validate `"Symp"` is atomic         | Pass                            |
| 2️⃣   | Symbolprose | Execute graph assigning to `RESULT` | `(SEXPR ("Hello from" "Symp"))` |
| ✅     | Output      | Return constant symbolic expression | Final result                    |

---

## 🔁 Data Flow

All components use a shared symbolic representation:

```
S-EXPRESSION = Atom | [S-EXPRESSION]
```

Each `(APPLY …)` passes these symbolic structures along the pipeline:

```
Input (SEXPR …)
     ↓
Symbolmatch — verifies shape
     ↓
Symbolverse — rewrites structure
     ↓
Symbolprose — executes as a graph
     ↓
Output (SEXPR …)
```

Because the data format is uniform, any module can be swapped or nested.

---

## 🧠 Reflection and `eval`

Symp is intentionally **first-order**, but includes an explicit reflective frame:

```
(APPLY eval (SEXPR (APPLY symbolverse (SEXPR ...))))
```

`eval` re-enters the pipeline with a new symbolic expression.
This provides **controlled metaprogramming** without unbounded recursion.

It allows higher-order behavior (frames creating frames)
while preserving logical consistency and termination guarantees.

---

## 🏗️ Frame Order and Evaluation

| Order | Meaning                    | Example                            |
| ----- | -------------------------- | ---------------------------------- |
| **0** | Constant expression (data) | `(SEXPR "hello")`                  |
| **1** | Executable frame           | `(FRAME (SYNTAX …) (SEMANTICS …))` |
| **2** | Frame that builds a frame  | Meta-frame (see mirror example)    |

The grammar restricts automatic evaluation beyond order 2.
Higher-order constructs are possible but must pass explicitly through `eval`.

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

* Expose the Symp engine as an API (JavaScript, Python, etc.).
* Applications can define and run frames internally,
  using Symbolmatch, Symbolverse, and Symbolprose as composable services.

---

## 🧰 Runtime Responsibilities

| Component      | Role                                                     |
| -------------- | -------------------------------------------------------- |
| **Parser**     | Converts source S-expressions into in-memory structures. |
| **Dispatcher** | Routes each `(APPLY …)` call to its target frame.        |
| **Evaluator**  | Executes the frame’s syntax and semantics subframes.     |
| **Serializer** | Converts result back to `(SEXPR …)` for transport.       |

---

## 📊 Example Integration Architecture

```
[ Web REPL ] → (APPLY …) → [ Symp Server ]
                         ↓
                  Symbolmatch (syntax)
                         ↓
                  Symbolverse (rewrite)
                         ↓
                  Symbolprose (execute)
                         ↓
                     (SEXPR result)
                         ↑
[ Browser Console ] ← response ← [ JSON API ]
```

---

## 🧩 Extensibility

Symp is fully modular:

* Define new frames using the same `(FRAME (SYNTAX …) (SEMANTICS …))` pattern.
* Store them as `.symp` files.
* Load dynamically into the runtime.
* Build DSLs, symbolic assistants, or logic engines without modifying the core.

---

## 🔮 Design Philosophy

> Symp is not a compiler.
> It’s a *conversation between symbols.*

Every stage — from grammar to meaning to execution —
is a distinct layer of that conversation, open for introspection and modification.

This is the foundation of **Symbolic Computing** in the world where symbols won.

---

## 📚 See Also

* [Symp Framework Overview](symp.md)
* [Symbolmatch](symbolmatch.md)
* [Symbolverse](symbolverse.md)
* [Symbolprose](symbolprose.md)
* [Examples](examples.md)

