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
"hello"
```

✅ Passes

Input:

```
("hello" "world")
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
(eq (mul x x) (pow x 2))
```

Output:

```
(eq (pow x 2) (pow x 2))
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
          (ASGN RESULT (RUN stdlib (mul 6 7))))
        (TARGET END)))))
```

Output:

```
42
```

---

## 4️⃣ Combined Frame — Syntax + Semantics

A frame that validates syntax *and* executes a symbolic computation.

```
(APPLY
  (FRAME
    (SYNTAX
      (APPLY
        symbolmatch
        (SEXPR
          (RULES
            (FLAT <start> ATOMIC)))))

    (SEMANTICS
      (APPLY
        symbolprose
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
("Hello from" "Symp")
```

---

## 5️⃣ DSL Example — Custom Math Frame

Define a small DSL where `ADD`, `MUL`, and `POW` become executable math.

```
(APPLY
  (FRAME
    (SYNTAX
      (APPLY symbolmatch
        (SEXPR
          (RULES
            (FLAT <start> <expr>)
            (FLAT <expr> ("add" <expr> <expr>)
            (FLAT <expr> ("mul" <expr> <expr>)
            (FLAT <expr> ("pow" <expr> <expr>)
            (FLAT <expr> ATOMIC)))))

    (SEMANTICS
      (APPLY symbolprose
        (SEXPR
          (GRAPH
            (EDGE
              (SOURCE BEGIN)
              (INSTR
                (ASGN RESULT (EVAL PARAMS)))
              (TARGET END)))))))
  (SEXPR (add 2 3)))
```

Output:

```
5
```

*(Example assumes `EVAL` is a helper function or extension.)*

---

> **Symp** — framework from a parallel reality where symbols won.

