---
id: foundations
---

## 2. The Foundations

**Designing form and meaning**

---

### 📐 Design

Symp is not a compiler. It’s a conversation between *form* and *meaning*. Syntax defines what is allowed. Semantics decides what it means. Execution is simply the dialogue between them. This is the foundation of **Symbolic Computing** in the world where symbols won.

---

### 📘 Grammar Overview

All components use a shared symbolic representation:

```
<s-expression> = <ATOM> | (<s-expression>+)
```

**Symp** specifies this representation to:

```
<start> := <apply>

<apply> := (APPLY <frame> <expr>)

<frame> := "symbolmatch"
         | "symbolverse"
         | "symbolprose"
         | (FRAME (SYNTAX <apply>) (SEMANTICS <apply>))
         | <FRAME-NAME>

<expr> := (SEXPR <S-EXPRESSION>)
        | <apply>
```

---

### 🧱 Architecture

Symp is a **minimalist symbolic computation backend**. It unifies parsing, transformation, and execution into one explicit pipeline:

```
input → syntax → semantics → output
````

Instead of hiding these layers inside a compiler, Symp makes each stage programmable. Symp lets you experiment with how programming languages themselves think — right from the grammar to execution. You don’t define *functions*. You define **frames** — each with its own *syntax* (form) and *semantics* (meaning).

---

### 🧩 What is a Frame?

A **frame** is the fundamental computing unit in Symp. It combines *form* and *meaning* into one executable definition:

```
(FRAME
  (SYNTAX <apply>)
  (SEMANTICS <apply>))
```

A frame becomes *alive* when applied to an input:

```
(APPLY myFrame (SEXPR myInput))
```

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

**Step-by-step:**

* Input → `(eq (mul x x) (pow x 2))`
* `Symbolmatch` checks that the top-level input is a valid S-expression.
* `Symbolverse` rewrites `(mul x x)` into `(pow x 2)`.
* Output → `(eq (pow x 2) (pow x 2))`.

---

### 🔍 Notes

Symp is a **glue language** where frame components are connected. In conventional languages, types check shape and functions produce values. In Symp, syntax checks shape and semantics produces structure — it’s a symbolic mirror of the same idea.

---

