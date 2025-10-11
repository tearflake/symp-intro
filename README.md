# Symp — A Symbolic Processing Framework

**Framework from a parallel reality where symbols won.**

---

## 🧩 What Is Symp?

Symp is a **minimalist symbolic computation backend**. It unifies parsing, transformation, and execution into one explicit pipeline:

```

input → syntax → semantics → output

````

Instead of hiding these layers inside a compiler, Symp makes each stage programmable.

You don’t define *functions*. You define **frames** — each with its own *syntax* (form) and *semantics* (meaning).

---

## 🧠 Core Components

- **Symbolmatch**: PEG-like grammar engine for syntax validation.
- **Symbolverse**: Term rewriting and transformation engine.
- **Symbolprose**: Graph-based virtual machine for executing symbolic programs.

These three systems are connected through the **Symp glue language**, where you can write:

```
(APPLY
  (FRAME
    (SYNTAX (APPLY symbolmatch (SEXPR (RULES ...))))
    (SEMANTICS (APPLY symbolverse (SEXPR (REWRITE ...)))))
  (SEXPR myProgram))
````

---

## 🔮 Why It’s Different

* **Explicit meaning** – You can *see* how syntax maps to semantics.
* **Composable** – Each frame can use others as sub-frames.
* **Reflective** – Frames can define new frames (meta-programming built in).
* **Minimal** – Entire core fits in a few hundred lines.
* **Beautifully weird** – A computing system from a world where theory outran graphics.

---

## 🚀 Getting Started (coming soon)

1. Install the runtime.
2. Run the Symp web REPL or CLI.
3. Try examples in `/examples/`

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

