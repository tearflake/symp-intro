---
id: symbolmatch
---

# Symbolmatch

**Grammar and syntax engine of Symp**

---

## 🧩 Purpose

`Symbolmatch` defines the *form* of valid input.  
It’s a **PEG-like parser** expressed in S-expressions.  
Each grammar is a collection of rule definitions written entirely in symbolic notation.

Where other systems hide parsing inside compilers, Symbolmatch makes it an explicit, inspectable data structure.

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

`hello` → success  
`(hello world)` → parse error at position 1.  

---

## 🔍 Notes

* `Symbolmatch` is the *syntax* pillar of Symp’s pipeline.
* It can be reused by other frames to define their expected input shape.
* Grammars are first-class values — you can generate them dynamically or load from files.

