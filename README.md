# Symp

**A symbolic processing framework**

---

## 🧩 What Is Symp?

Symp is a **minimalist symbolic computation backend**. It unifies parsing, transformation, and execution into one explicit pipeline:

```

input → syntax → semantics → output

````

Instead of hiding these layers inside a compiler, Symp makes each stage programmable.

Symp lets you experiment with how programming languages themselves think — right from the grammar to execution.

You don’t define *functions*. You define **frames** — each with its own *syntax* (form) and *semantics* (meaning).

---

## 🧠 Core Components

#### Syntax validation:
  - **Symbolmatch**: PEG-like grammar engine

#### Semantic processing:
  - **Symbolverse**: Term rewriting and transformation engine
  - **Symbolprose**: graph-based execution of symbolic programs

These three systems for syntax analisys and semantic synthesis are connected through the **Symp glue language**, where you can write:

```
(APPLY
  (FRAME
    (SYNTAX (APPLY symbolmatch (SEXPR (RULES ...))))
    (SEMANTICS (APPLY symbolverse (SEXPR (REWRITE ...)))))
  (SEXPR myProgram))
```

Use it to prototype symbolic languages, theorem provers, or DSLs that think in trees instead of tokens.

---

## 🔮 Why It’s Different

* **Explicit meaning** – You can *see* how syntax maps to semantics.
* **Composable** – Each frame can use others as sub-frames.
* **Reflective** – Frames can define new frames (meta-programming built in).
* **Minimal** – Entire core fits in a few hundred lines.
* **Beautifully weird** – A computing system from an alternate reality where theory outran graphics.

---

## 🚀 Getting Started (coming soon)

* Install the runtime.
* Run the Symp web REPL or CLI.
* Try examples in `/examples/`.

---

## 🧬 Design Philosophy

> Every computation in Symp passes through **form** and **meaning**.
>
> Syntax validates structure.
> Semantics transforms intent.
> Output becomes new input.

It’s not a conventional programming language — it’s a framework for building *languages of meaning*.

---

## 📚 Learn More

* [About](docs/about.md)
* [Philosophy](docs/philosophy.md)
* [Architecture](docs/architecture.md)
* [Symbolmatch](docs/symbolmatch.md)
* [Symbolverse](docs/symbolverse.md)
* [Symbolprose](docs/symbolprose.md)
* [Examples](docs/examples.md)

---

## 🖤 Inspiration

Symp is inspired by Lisp, PEGs, term rewriting systems, and finite state machines.

