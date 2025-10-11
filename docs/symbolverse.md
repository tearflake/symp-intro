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
(REWRITE
  (RULE
    (READ ("mul" x x))
    (WRITE ("pow" x "2"))))
````

| Form        | Description                                     |
| ----------- | ----------------------------------------------- |
| **RULE**    | Basic pattern-rewrite mapping.                  |
| **VAR**     | Optional list of variables.                     |
| **COMPUTE** | Nested rewriting scope for complex definitions. |

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

## 📘 See also

* [Symbolmatch](symbolmatch.md) — syntax validation.
* [Symbolprose](symbolprose.md) — execution engine.

