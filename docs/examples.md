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
(SEXPR "hello")
```

✅ Passes

Input:

```
(SEXPR ("hello" "world"))
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
(SEXPR (eq (mul x x) (pow x 2)))
```

Output:

```
(SEXPR (eq (pow x 2) (pow x 2)))
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
          (ASGN RESULT (mul 6 7)))
        (TARGET END)))))
```

Output:

```
(SEXPR 42)
```

---

## 4️⃣ Combined Frame — Syntax + Semantics

A frame that validates syntax *and* executes a symbolic computation.

```
(APPLY
  (FRAME
    (SYNTAX
      (APPLY symbolmatch
        (SEXPR
          (RULES
            (FLAT <start> ATOMIC)))))

    (SEMANTICS
      (APPLY symbolprose
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
(SEXPR ("Hello from" "Symp"))
```

---

## 5️⃣ Meta-Frame — The “Mirror” Example

A **second-order** frame that builds a new frame dynamically.

```
(APPLY
  (FRAME
    (SYNTAX
      (APPLY symbolmatch
        (SEXPR
          (RULES
            (FLAT <start> ATOMIC)))))

    (SEMANTICS
      (APPLY symbolprose
        (SEXPR
          (GRAPH
            (EDGE
              (SOURCE BEGIN)
              (INSTR
                (ASGN RESULT
                  ("FRAME"
                    ("SYNTAX"
                      ("APPLY" "symbolmatch"
                        ("SEXPR" ("RULES" ("FLAT" "<start>" "ATOMIC")))))
                    ("SEMANTICS"
                      ("APPLY" "symbolprose"
                        ("SEXPR"
                          ("GRAPH"
                            ("EDGE"
                              ("SOURCE" "BEGIN")
                              ("INSTR"
                                ("ASGN" "RESULT"
                                  ("Echo of" PARAMS "PARAMS")))
                              ("TARGET" "END"))))))))
              (TARGET END))))))))
  (SEXPR "myMirror"))
```

Result: a **new frame** named `"myMirror"`.

You can then apply it:

```
(APPLY myMirror (SEXPR "hello"))
```

Output:

```
(SEXPR ("Echo of" hello hello))
```

---

## 6️⃣ DSL Example — Custom Math Frame

Define a small DSL where `ADD`, `MUL`, and `POW` become executable math.

```
(APPLY
  (FRAME
    (SYNTAX
      (APPLY symbolmatch
        (SEXPR
          (RULES
            (NORM <start> (GROUP (ADD ATOMIC ATOMIC)))
            (NORM <start> (GROUP (MUL ATOMIC ATOMIC)))
            (NORM <start> (GROUP (POW ATOMIC ATOMIC)))))))

    (SEMANTICS
      (APPLY symbolprose
        (SEXPR
          (GRAPH
            (EDGE
              (SOURCE BEGIN)
              (INSTR
                (ASGN RESULT (EVAL PARAMS)))
              (TARGET END)))))))
  (SEXPR (ADD 2 3)))
```

Output:

```
(SEXPR 5)
```

*(Example assumes `EVAL` is a helper function or extension.)*

---

## 💡 Tips

* Every frame is an `(APPLY ...)` expression.
* Each stage can be tested independently — Symbolmatch, Symbolverse, Symbolprose.
* You can chain them or store them in files as reusable building blocks.
* Advanced users can use `(APPLY eval (SEXPR …))` for reflection and dynamic frame creation.

---

## 📚 See Also

* [Symp Framework Overview](symp.md)
* [Symbolmatch](symbolmatch.md)
* [Symbolverse](symbolverse.md)
* [Symbolprose](symbolprose.md)

---

> **Symp** — framework from a parallel reality where symbols won.

