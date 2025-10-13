---
id: introduction
---

## 2. Introduction

**Symbolic Computing Pipeline — form and meaning**

---

### 🧩 Overview

Symp organizes symbolic subsystems into a single programmable pipeline:

```

input → syntax → semantics → output

````

Each stage is explicit, modular, and represented by its own symbolic language. Together, they make up the Symp framework — a computing substrate for defining, composing, and interacting with **frames**.

---

### 🧠 What is a Frame?

A **frame** is the fundamental computing unit in Symp. It combines *form* and *meaning* into one executable definition:

```
(FRAME
  (SYNTAX    (APPLY symbolmatch  (SEXPR (RULES ...))))
  (SEMANTICS (APPLY symbolverse (SEXPR (REWRITE ...)))))
```

A frame becomes *alive* when applied to an input:

```
(APPLY myFrame (SEXPR myInput))
```

During execution:

* The **input** is passed to the system.
* The **syntax** part validates the input.
* The **semantics** part transforms or executes it.
* The **output** is returned as a constant.

---

### ⚙️ Anatomy of the Pipeline

Each pipeline stage may choose to be constructed from the three symbolic subsystems: 
* **Symbolmatch**: Ensures the input follows the expected grammar.
* **Symbolverse**: Transforms the symbolic structure into a symbolic result.
* **Symbolprose**: Interprets the graph and produces a symbolic result.

---

### 🧮 Example: Equality Simplifier

A small end-to-end demonstration:

```
(APPLY
  (FRAME
    (SYNTAX
      (APPLY symbolmatch
        (SEXPR
          (RULES
            (FLAT <start> <expr>)
            (FLAT <expr> ("eq" <expr> <expr>)
            (FLAT <expr> ("mul" <expr> <expr>)
            (FLAT <expr> ("pow" <expr> <expr>)
            (FLAT <expr> ATOMIC)))))

    (SEMANTICS
      (APPLY symbolverse
        (SEXPR
          (REWRITE
            (RULE
              (READ ("mul" x x))
              (WRITE ("pow" x "2"))))))))
              
  (SEXPR (eq (mul x x) (pow x 2))))
```

🧩 **Step-by-step:**

* Input → `(eq (mul x x) (pow x 2))`
* `Symbolmatch` checks that the top-level input is a valid S-expression.
* `Symbolverse` rewrites `(mul x x)` into `(pow x 2)`.
* Output → `(eq (pow x 2) (pow x 2))`.

---

### 🌀 Nested and Custom Frames

Frames can reference other frames, or even *generate* new frames. For example, a higher-order “builder” frame may output a new `(FRAME …)` definition. This allows **metaprogramming** within a consistent symbolic model — without uncontrolled self-reference.

---

### 💡 Type Analogy

In conventional languages, types check shape and functions produce values. In Symp, syntax checks shape and semantics produces structure — it’s a symbolic mirror of the same idea.

---

### 🔍 Why Use Symp?

* **Explicit computation pipeline** — each stage visible and replaceable.
* **Composable DSLs** — build language fragments by defining new frames.
* **Small core** — few primitives, infinite combinations.
* **Self-describing** — the system can express its own syntax and semantics.

---

### 🖥️ Using Symp as a Backend

Symp can run as:

* A **CLI tool** evaluating symbolic frames.
* A **web service** over AJAX or WebSockets.
* A **library** embedded in other symbolic systems.

You can define your own frames, store them as files, and apply them dynamically.

---

### 🔮 Philosophy

In Symp, computation is a conversation between *form* and *meaning*.

Syntax defines what is allowed.  
Semantics decides what it means.  
Execution is simply the dialogue between them.  
