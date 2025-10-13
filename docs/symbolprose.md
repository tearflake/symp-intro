---
id: symbolprose
---

### Symbolprose

**The executable graph of symbolic computation**

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
