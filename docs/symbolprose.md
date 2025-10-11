---
id: symbolprose
---

# Symbolprose

**The executable graph of symbolic computation**

---

## 🧩 Purpose

`Symbolprose` also defines the *semantics* of a frame.  
Where `Symbolverse` rewrites meaning, `Symbolprose` executes the computation through **directed symbolic graphs**.

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
          (ASGN RESULT (RUN stdlib (mul 6 7))))
        (TARGET END)))))
```

Output:

```
42
```

---

## 🔍 Notes

* The graph model allows conditional and sequential computation.
* Each `ASGN` assigns symbolic values; `TEST` enables flow control.
* You can invoke other computations via `(RUN name input)`.

