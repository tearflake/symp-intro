# Gentle Introduction to Symp - a Symbolic Processing Framework

## TOC

- [1. Symp](#1-symp)
- [2. The Foundations](#2-the-foundations)
  - [2.1. Symbolmatch](#21-symbolmatch)
  - [2.2. Symbolverse](#22-symbolverse)
  - [2.3. Symbolprose](#23-symbolprose)
- [3. Examples](#3-examples)
- [4. Conclusion](#4-conclusion)

## 1. Symp

**a framework from a parallel reality where symbols won**

---

#### 🔮 Philosophy

What if programming had evolved around **form** and **meaning** rather than machinery?

Syntax defines a form of expressions.  
Semantics decides what they mean.  
Execution connects the two.

Symp is a small, strange, and honest framework evolved from form and meaning.  
It carries a *thought experiment* from a reality in which symbols triumphed over flashing lights.

---

#### 💡 The Idea

In Symp, every computation follows one simple ritual:

```

input → syntax → semantics → output

````

Instead of a compiler that hides these stages,  
Symp makes each one **programmable**.

You don’t just write programs in Symp —  
you define *how* programs themselves should be understood.

---

#### 🧠 Why Symp Exists

Symp isn’t a Lisp dialect or a new syntax flavor.  
It’s a **computational philosophy**:

* Programs prove their *form* before they express their *meaning*.
* Syntax and semantics are equals — two halves of a single act.
* Everything can be described in its own syntax and semantics.

It’s deliberately colorless, minimalist, and transparent.  
A tool for anyone who loves **building languages more than writing in them**.

---

#### 🚀 What You Can Build

Symp is tiny, but composable.  
It’s a playground for symbolic systems:

* Custom DSLs and interpreters
* Meta-compilers and term rewriters
* Theorem provers, logic engines, or exotic REPLs

Anything that can be described as “action → form → meaning → reaction”.

---

#### 🖤 Inspiration

Symp is inspired by Lisp, PEGs, term rewriting systems, and finite state machines.

## 2. The Foundations

**Designing form and meaning**

---

### 📐 design

Symp is not a compiler. It’s a conversation between *form* and *meaning*. Syntax defines what is allowed. Semantics decides what it means. Execution is simply the dialogue between them. This is the foundation of **Symbolic Computing** in the world where symbols won.

---

#### 📘 Grammar Overview

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

#### 🧱 Architecture

Symp is a **minimalist symbolic computation backend**. It unifies parsing and transformation/execution into one explicit pipeline:

```
input → syntax → semantics → output
````

Instead of hiding these layers inside a compiler, Symp makes each stage programmable. Symp lets you experiment with how programming languages themselves think — right from the grammar to execution. You don’t define *functions*. You define **frames** — each with its own *syntax* (form) and *semantics* (meaning).

---

#### 🧩 What is a Frame?

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

A frame is like a function whose type signature is explicit syntax, and whose return type is explicit semantics.

---

#### 🧮 Example: Equality Simplifier

A small end-to-end demonstration:

```
(APPLY
  (FRAME
    (SYNTAX
      (APPLY
        symbolmatch
        (SEXPR
          (RULES
            (FLAT <start> <expr>)
            (FLAT <expr> ("eq" <expr> <expr>)
            (FLAT <expr> ("mul" <expr> <expr>)
            (FLAT <expr> ("pow" <expr> <expr>)
            (FLAT <expr> ATOMIC))))))))

    (SEMANTICS
      (APPLY
        symbolverse
        (SEXPR
          (REWRITE
            (RULE
              (READ ("mul" x x))
              (WRITE ("pow" x "2")))))))))
              
  (SEXPR (eq (mul x x) (pow x 2))))
```

**Step-by-step:**

* Input → `(eq (mul x x) (pow x 2))`
* `Symbolmatch` checks that the top-level input is a valid S-expression.
* `Symbolverse` rewrites `(mul x x)` into `(pow x 2)`.
* Output → `(eq (pow x 2) (pow x 2))`.

---

#### 🔍 Notes

Symp is a **glue language** where frame components are connected. In conventional languages, types check shape and functions produce values. In Symp, syntax checks shape and semantics produces structure — it’s a symbolic mirror of the same idea.

---

### 2.1. Symbolmatch

**grammar and syntax engine of symp**

---

#### 🧩 Purpose

Symbolmatch defines the *form* of valid input.  
It’s a **PEG-like parser** expressed in S-expressions.  
Each grammar is a collection of rule definitions written entirely in symbolic notation.

Where other systems hide parsing inside compilers, Symbolmatch makes it an explicit, inspectable data structure.

---

#### 📘 Grammar Overview

```
<start> := (RULES <rule>+)

<rule> := (FLAT <IDENTIFIER> <S-EXPRESSION>)
        | (NORM <IDENTIFIER> <S-EXPRESSION>)
        | (ATOM <IDENTIFIER> <S-EXPRESSION>)
```

---

#### 💡 Example

Validate that an expression is a single atom:

```
(APPLY
  symbolmatch
  (SEXPR
    (RULES
      (FLAT <start> ATOMIC))))
```

Input:

`hello` → success  
`(hello world)` → parse error at position 1.  

---

#### 🔍 Notes

* Symbolmatch is the *syntax* checker of Symp’s pipeline.
* It can be reused by other frames to define their expected input shape.
* Grammars are first-class values — you can generate them dynamically or load from files.

### 2.2. Symbolverse

**term rewriting and transformation engine**

---

#### 🧩 Purpose

Symbolverse defines the *semantics* of a frame.  
It describes how one symbolic structure is rewritten into another.  
This is where *meaning* happens — logic, inference, and transformation.

---

#### 📘 Grammar Overview

```
<start> := <ruleset>

<ruleset> := (REWRITE <elem>+)

<elem> := (RULE (READ <S-EXPR>) (WRITE <S-EXPR>))
        | (COMPUTE (NAME <ATOM>) <ruleset>)

<compute-call> := (RUN <ATOMIC> <ANY>)
```

---

#### 💡 Example

Replace every occurrence of `(mul x x)` with `(pow x 2)`:

```
(APPLY
  symbolverse
  (SEXPR
    (REWRITE
      (RULE
        (READ ("mul" x x))
        (WRITE ("pow" x "2"))))))
```

Input:

```
(SEXPR (eq (mul x x) (pow x 2)))
```

Output:

```
(SEXPR (eq (pow x 2) (pow x 2)))
```

---

#### 🔍 Notes

* Symbolverse can describe any symbolic logic transformation.
* Think of it as a pure function between *structures of meaning*.
* You can combine rules recursively, or invoke other computations via `(RUN name input)`.

### 2.3. Symbolprose

**the executable graph of symbolic computation**

---

#### 🧩 Purpose

Symbolprose also defines the *semantics* of a frame.  
Symbolprose returns result of executing directed symbolic graphs.  
This is where meaning is being represented as a result of stateful actions.  

---

#### 📘 Grammar Overview

```
<start> := <graph>

<graph> := (GRAPH <element>+)

<element> := (EDGE (SOURCE <ATOMIC>) (INSTR <instruction>+)? (TARGET <ATOMIC>))
           | (COMPUTE (NAME <ATOMIC>) <graph>)

<instruction> := (TEST <ANY> <ANY>)
               | (ASGN <ATOMIC> <ANY>)

<compute-call> := (RUN <ATOMIC> <ANY>)
````

---

#### 💡 Example

A simple multiplication program:

```
(APPLY
  symbolprose
  (SEXPR
    (GRAPH
      (EDGE
        (SOURCE BEGIN)
        (INSTR
          (ASGN RESULT (RUN stdlib (mul 6 7))))
        
        (TARGET END)))))
```

Output:

```
42
```

---

#### 🔍 Notes

* The graph model allows conditional and sequential computation.
* Each `ASGN` assigns symbolic values; `TEST` enables flow control.
* You can invoke other computations via `(RUN name input)`.

## 3. Examples

**a few small programs from a world where symbols won**

---

#### ✨ Remark

Each of the first three examples illustrates one pillar of the system — syntax, rewriting, and execution — before combining them into complete frames.

---

#### 1️⃣ Grammar Checker — Symbolmatch

Validate that input consists of a single atomic token. To be nested within a frame.

```
(APPLY
  symbolmatch
  (SEXPR
    (RULES
      (FLAT <start> ATOMIC))))
````

**Input**:

```
hello
```

Passes.

**Input**:

```
(hello world)
```

Fails — not atomic.

---

#### 2️⃣ Rewriter — Symbolverse

A symbolic rule that rewrites `(mul x x)` into `(pow x 2)`. To be nested within a frame.

```
(APPLY
  symbolverse
  (SEXPR
    (REWRITE
      (RULE
        (READ ("mul" x x))
        (WRITE ("pow" x "2"))))))
```

**Input**:

```
(eq (mul x x) (pow x 2))
```

**Output**:

```
(eq (pow x 2) (pow x 2))
```

---

#### 3️⃣ Executor — Symbolprose

A symbolic computation graph that multiplies 6 × 7. To be nested within a frame.

```
(APPLY
  symbolprose
  (SEXPR
    (GRAPH
      (EDGE
        (SOURCE BEGIN)
        (INSTR
          (ASGN RESULT (RUN stdlib (mul 6 7))))
        
        (TARGET END)))))
```

**Input**:

disregarded.

**Output**:

```
42
```

---

#### 4️⃣ Combined Frame — Syntax + Semantics

A frame that validates syntax *and* executes a symbolic computation. Complete example.

```
(APPLY
  (FRAME
    (SYNTAX
      (APPLY
        symbolmatch
        (SEXPR
          (RULES
            (FLAT <start> ATOMIC)))))

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

**Output**:

```
(Hello from Symp)
```

---

#### 5️⃣ DSL Example — Custom Math Frame

Define a small DSL to simplify addition and multiplication. Complete example.

```
(APPLY
  (FRAME
    (SYNTAX
      (APPLY
        symbolmatch
        (SEXPR
          (RULES
            (FLAT <start> <expr>)
            (FLAT <expr> ("add" <expr> <expr>)
            (FLAT <expr> ("mul" <expr> <expr>)
            (FLAT <expr> ATOMIC))))))))

    (SEMANTICS
      (APPLY
        symbolverse
        (SEXPR
          (REWRITE
            (RULE
              (READ ("add" 0 x))
              (WRITE x))

            (RULE
              (READ ("add" x 0))
              (WRITE x))

            (RULE
              (READ ("mul" 0 x))
              (WRITE 0))

            (RULE
              (READ ("mul" x 0))
              (WRITE 0))

            (RULE
              (READ ("mul" 1 x))
              (WRITE x))

            (RULE
              (READ ("mul" x 1))
              (WRITE x)))))))

  (SEXPR (mul (add 0 5) 1)))
```

**Output**:

```
5
```

## 4. Conclusion

**the quiet machine**

---

Symp is not just another programming framework; it is a question expressed as a system. It asks what computation really means when we remove the layers that usually conceal it—when syntax is no longer hidden inside compilers, and semantics is no longer assumed by convention. In Symp, every program must first demonstrate that it understands itself before it can act.

There are no opaque steps in this framework. Every transformation is written in the same symbolic language that describes it. Computation in Symp is transparent, traceable, and introspective. The system is small in size but deep in concept. It does not aim for efficiency or visual sophistication, but for clarity—a quality that has always been rare and fragile in computing.

Practically, Symp can be used to build parsers, interpreters, domain-specific languages, or experimental symbolic systems. Conceptually, it invites exploration into how programming might look if we treated the theory of meaning with the same importance we give to presentation.

Perhaps its real purpose lies there: to remind us that computation can still be a reflective act, not just a mechanical one. In an alternate history of technology, there might exist a world where machines never learned to paint but learned instead to understand. Symp is an echo from that world—a quiet machine designed to make the act of understanding visible.

---

*This document was written with the assistance of an AI language model, used as a collaborator in reflection and articulation.*

