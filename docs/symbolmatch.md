# Symbolmatch

**Grammar and syntax engine of Symp**

---

## 🧩 Purpose

`Symbolmatch` defines the *form* of valid input.  
It’s a **PEG-like parser** expressed in S-expressions.  
Each grammar is a collection of `(RULE …)` definitions written entirely in symbolic notation.

Where other systems hide parsing inside compilers,  
Symbolmatch makes it an explicit, inspectable data structure.

---

## 📘 Grammar Overview

```
(RULES
    (FLAT <start> (ATOMIC))
    (NORM <rule> (GROUP (MUL "RULE" ATOMIC <expr>)))
    (ATOM <token> "identifier") )
````

| Rule type | Description                             |
| --------- | --------------------------------------- |
| **FLAT**  | Match the rule inline, without nesting. |
| **NORM**  | Match a normal tree structure.          |
| **ATOM**  | Match a literal atom or token.          |

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

```
(SEXPR "hello")
```

✅ Result → success
❌ `(SEXPR (hello world))` → parse error at position 1.

---

## 🔍 Notes

* `Symbolmatch` is the *syntax* pillar of Symp’s pipeline.
* It can be reused by other frames to define their expected input shape.
* Grammars are first-class values — you can generate them dynamically or load from files.

---

## 📘 See also

* [Symbolverse](symbolverse.md) — define *how meaning transforms*.
* [Symbolprose](symbolprose.md) — define *how programs execute*.

