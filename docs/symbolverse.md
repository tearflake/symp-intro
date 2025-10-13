---
id: symbolverse
---

### Symbolverse

**Term rewriting and transformation engine**

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
