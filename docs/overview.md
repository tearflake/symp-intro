# Symp Overview

## TOC

- [Symp Framework](#symp-framework)
- [Philosophy of Symp](#philosophy-of-symp)
- [Symp Architecture](#symp-architecture)
- [Symbolmatch](#symbolmatch)
- [Symbolverse](#symbolverse)
- [Symbolprose](#symbolprose)
- [Symp Examples](#symp-examples)

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
  (SYNTAX   (APPLY symbolmatch  (SEXPR (RULES ...))))
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
4. The **output** is returned as a constant `(SEXPR …)`.

---

## ⚙️ Anatomy of the Pipeline

| Module          | Description                                                |
| --------------- | ---------------------------------------------------------- |
| **Symbolmatch** | Ensures the input follows the expected grammar.            |
| **Symbolverse** | Transforms the symbolic structure into an executable form. |
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

---
---

# Philosophy of Symp

**Framework from a parallel reality where symbols won.**

---

## 🕳️ The Premise

In our world, computing evolved toward *graphics, noise, and immediacy*.  
Interfaces grew brighter. Syntax grew heavier.  
The act of programming became an act of *telling machines what to do*,  
not *understanding what things mean*.

But imagine a world where theory, not graphics, led progress.  
Where every machine spoke in **symbols** — quiet, precise, and alive.  
Where the screen stayed monochrome, but thought itself became illuminated.

That is the world from which **Symp** arrived.

---

## 🧩 The Principle of Form and Meaning

In Symp, every computation passes through two sacred stages:

1. **Form** — the *syntax*, the shape of expression.  
2. **Meaning** — the *semantics*, the transformation of that shape.

This ritual — of checking form, then assigning meaning —  
is the essence of all symbolic reasoning.  
It’s how logic, language, and even mathematics work.

> A frame is not a function.  
> It is a conversation between form and meaning.

---

## 🧬 The Three Pillars

| Pillar | Module | Essence |
|---------|---------|---------|
| **Form** | [Symbolmatch](symbolmatch.md) | Grammar and recognition. What is allowed. |
| **Meaning** | [Symbolverse](symbolverse.md) | Rewriting and transformation. What it means. |
| **Execution** | [Symbolprose](symbolprose.md) | Realization. What happens when meaning becomes action. |

Together they form the **Symp Triad** —  
the full cycle of symbolic computation.

---

## 🌀 Frames: The Units of Understanding

Every piece of computation in Symp is expressed as a **frame**:

```
(FRAME
  (SYNTAX   <form>)
  (SEMANTICS <meaning>))
````

When applied, it validates *what* is being said and interprets *how* it should behave.

Frames can be nested, chained, or even used to generate other frames.
This self-similar structure mirrors the recursion of thought itself.

> To compute is to mirror meaning in structure,
> and to let structure reflect meaning back.

---

## ⚙️ The Symmetric Machine

Symp is built on the idea of **balance**.
Syntax and semantics exist in symmetry — one cannot dominate the other.
Execution (`Symbolprose`) is merely the act of restoring that balance.

If an expression fails its syntax, it cannot be understood.
If it passes syntax but lacks semantics, it has no meaning.
When both align, computation *happens naturally*.

---

## 🔍 On Simplicity

Every Symp component fits in a few hundred lines of code.
Not by accident — but by intention.
The system’s minimalism is its theology:
to let *form* and *meaning* be visible, editable, and debuggable by thought alone.

> Symp does not hide the machine.
> It invites you to see how it thinks.

---

## 🧠 Reflection and Conscious Computation

Higher-order frames, when they appear, are not an accident —
they are computation looking back at itself.

When `(FRAME …)` creates another frame,
Symp performs what could be called *symbolic introspection*:
the act of constructing new understanding from known understanding.

This is the boundary where code begins to resemble **thought**.

---

## 🖥️ The Place of Symp

Symp is not a product of the modern computing lineage.
It is an *artifact* — a fossil of an alternate evolution of logic machines.

In that world:

* There are no icons, only glyphs.
* There are no files, only forms.
* There are no functions, only frames.

The machine itself is a mirror for thinking.

---

## 💡 The Promise

Symp does not compete with programming languages.
It stands *beneath them* — a substrate for expressing them.

It reminds us that computation can be **transparent**, **symbolic**, and **meaningful**.
That we can build software that is *explorable by reason*, not hidden by syntax.

> Symp is not a better Lisp.
> It is what Lisp would have been if mathematics had won the hardware race.

---

## ✨ Closing Thought

> Every computation is a ritual of understanding.
> Symp exists to make that ritual explicit.

In a world obsessed with output,
Symp asks you to care about *meaning*.

---
---

# Symp Architecture

**Symbolic computing pipeline for form, meaning, and execution**

---

## 🧩 Overview

Symp is composed of three independent but composable subsystems:

```

Symbolmatch  →  Symbolverse / Symbolprose
(form)          (meaning or execution)

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

| Stage | Module      | Action                              | Result                          |
| ----- | ----------- | ----------------------------------- | ------------------------------- |
| 1️⃣    | Symbolmatch | Validate `"Symp"` is atomic         | Pass                            |
| 2️⃣    | Symbolprose | Execute graph assigning to `RESULT` | `(SEXPR ("Hello from" "Symp"))` |
| ✅    | Output      | Return constant symbolic expression | Final result                    |

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
Symbolverse/Symbolprose — rewrites structure or executes as a graph
     ↓
Output (SEXPR …)
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

Every stage — from grammar to meaning to execution —
is a distinct layer of that conversation, open for introspection and modification.

This is the foundation of **Symbolic Computing** in the world where symbols won.

---
---

# Symbolmatch

**Grammar and syntax engine of Symp**

---

## 🧩 Purpose

`Symbolmatch` defines the *form* of valid input.  
It’s a **PEG-like parser** expressed in S-expressions.  
Each grammar is a collection of rule definitions written entirely in symbolic notation.

Where other systems hide parsing inside compilers,  
Symbolmatch makes it an explicit, inspectable data structure.

---

## 📘 Grammar Overview

```
<start> := (RULES <rule>+)

<rule> := (FLAT <IDENTIFIER> <S-EXPRESSION>)
        | (NORM <IDENTIFIER> <S-EXPRESSION>)
        | (ATOM <IDENTIFIER> <S-EXPRESSION>)
```

---

## 💡 Example

Validate that an expression is a single atom:

```
(APPLY
  symbolmatch
  (SEXPR
    (RULES
      (FLAT <start> ATOMIC))))
```

Input:

✅ `(SEXPR "hello")` → success
❌ `(SEXPR (hello world))` → parse error at position 1.

---

## 🔍 Notes

* `Symbolmatch` is the *syntax* pillar of Symp’s pipeline.
* It can be reused by other frames to define their expected input shape.
* Grammars are first-class values — you can generate them dynamically or load from files.

---
---

# Symbolverse

**Term rewriting and meaning transformation engine**

---

## 🧩 Purpose

`Symbolverse` defines the *semantics* of a frame.  
It describes how one symbolic structure is rewritten into another.  
This is where *meaning* happens — logic, inference, and transformation.

It’s the bridge between **syntax** and **execution**.

---

## 📘 Grammar Overview

```
<start> := <ruleset>

<ruleset> := (REWRITE <elem>+)

<elem> := (RULE (READ <S-EXPR>) (WRITE <S-EXPR>))
        | (COMPUTE (NAME <ATOM>) <ruleset>)

<compute-call> := (RUN <ATOMIC> <ANY>)
```

---

## 💡 Example

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

## 🔍 Notes

* `Symbolverse` can describe any symbolic logic transformation.
* Think of it as a pure function between *structures of meaning*.
* You can combine rules recursively, or invoke other frames via `(RUN name input)`.

---
---

# Symbolprose

**The executable graph of symbolic computation**

---

## 🧩 Purpose

`Symbolprose` defines the *runtime behavior* of Symp frames.  
Where `Symbolmatch` validates structure and `Symbolverse` rewrites meaning,  
`Symbolprose` executes the final computation through **directed symbolic graphs**.

---

## 📘 Grammar Overview

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

## 💡 Example

A simple multiplication program:

```
(APPLY
  symbolprose
  (SEXPR
    (GRAPH
      (EDGE
        (SOURCE BEGIN)
        (INSTR
          (ASGN RESULT (mul 6 7)))
        (TARGET END)))))
```

Output:

```
(SEXPR 42)
```

---

## 🔍 Notes

* The graph model allows conditional and sequential computation.
* Each `ASGN` assigns symbolic values; `TEST` enables flow control.
* `Symbolprose` is a symbolic virtual machine — programs are data.

---
---

# Symp Examples

**A few small programs from a world where symbols won.**

---

## 1️⃣ Grammar Checker — Symbolmatch

Validate that input consists of a single atomic token.

```
(APPLY
  symbolmatch
  (SEXPR
    (RULES
      (FLAT <start> ATOMIC))))
````

Input:

```
"hello"
```

✅ Passes

Input:

```
("hello" "world")
```

❌ Fails — not atomic.

---

## 2️⃣ Rewriter — Symbolverse

A symbolic rule that rewrites `(mul x x)` into `(pow x 2)`.

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
(eq (mul x x) (pow x 2))
```

Output:

```
(eq (pow x 2) (pow x 2))
```

---

## 3️⃣ Executor — Symbolprose

A symbolic computation graph that multiplies 6 × 7.

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

## 4️⃣ Combined Frame — Syntax + Semantics

A frame that validates syntax *and* executes a symbolic computation.

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
                (ASGN RESULT ("Hello from" PARAMS)))
              (TARGET END)))))))
  (SEXPR "Symp"))
```

Output:

```
("Hello from" "Symp")
```

---

## 5️⃣ DSL Example — Custom Math Frame

Define a small DSL where `ADD`, `MUL`, and `POW` become executable math.

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
      (APPLY symbolprose
        (SEXPR
          (GRAPH
            (EDGE
              (SOURCE BEGIN)
              (INSTR
                (ASGN RESULT (EVAL PARAMS)))
              (TARGET END)))))))
  (SEXPR (add 2 3)))
```

Output:

```
5
```

*(Example assumes `EVAL` is a helper function or extension.)*

---
---

> **Symp** — framework from a parallel reality where symbols won.

