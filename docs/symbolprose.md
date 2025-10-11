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

## 📘 See also

* [Symbolmatch](symbolmatch.md) — define input form.
* [Symbolverse](symbolverse.md) — transform meaning before execution.

```

