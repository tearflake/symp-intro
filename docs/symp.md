---
layout: sidebar
---

# Symp Framework

**Symbolic Computing Pipeline — form, meaning, and execution**

---

## 🧩 Overview

Symp unites three symbolic subsystems into a single programmable pipeline:

```

input → syntax → semantics → output

````

Each stage is explicit, modular, and represented by its own symbolic language.  
Together, they make up the Symp framework — a computing substrate for defining and composing **frames**.

---

## 🧠 What is a Frame?

A **frame** is the fundamental computing unit in Symp.  
It combines *form* and *meaning* into one executable definition:

```
(FRAME
  (SYNTAX    (APPLY symbolmatch  (SEXPR (RULES ...))))
  (SEMANTICS (APPLY symbolverse (SEXPR (REWRITE ...)))))
````

A frame becomes *alive* when applied to an input:

```
(APPLY myFrame (SEXPR myInput))
```

During execution:

1. The **input** is passed to the system.
2. The **syntax** part validates the input.
3. The **semantics** part transforms or executes it.
4. The **output** is returned as a constant.

---

## ⚙️ Anatomy of the Pipeline

| Module          | Description                                                |
| --------------- | ---------------------------------------------------------- |
| **Symbolmatch** | Ensures the input follows the expected grammar.            |
| **Symbolverse** | Transforms the symbolic structure into a symbolic result.  |
| **Symbolprose** | Interprets the graph and produces a symbolic result.       |

---

## 🧮 Example: Equality Simplifier

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

1. `Symbolmatch` checks that the top-level input is a valid S-expression.
2. `Symbolverse` rewrites `(mul x x)` into `(pow x 2)`.
3. Result → `(eq (pow x 2) (pow x 2))`.

---

## 🌀 Nested and Custom Frames

Frames can reference other frames, or even *generate* new frames.
For example, a higher-order “builder” frame may output a new `(FRAME …)` definition.
This allows **metaprogramming** within a consistent symbolic model —
without uncontrolled self-reference.

---

## 🔍 Why Use Symp?

* **Explicit computation pipeline** — each stage visible and replaceable.
* **Composable DSLs** — build language fragments by defining new frames.
* **Small core** — few primitives, infinite combinations.
* **Self-describing** — the system can express its own syntax and semantics.

---

## 🖥️ Using Symp as a Backend

Symp can run as:

* A **CLI tool** evaluating symbolic frames.
* A **web service** over AJAX or WebSockets.
* A **library** embedded in other symbolic systems.

You can define your own frames, store them as files, and apply them dynamically.

---

## 🔮 Philosophy

> In Symp, computation is a conversation between *form* and *meaning*.
>
> Syntax defines what is allowed.
> Semantics decides what it means.
> Execution is simply the dialogue between them.

