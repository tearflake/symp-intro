# Symp Examples

**A few small programs from a world where symbols won.**

---

## 1️⃣ Grammar Checker — Symbolmatch

Validate that input consists of a single atomic token. To be nested within a frame.

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

A symbolic rule that rewrites `(mul x x)` into `(pow x 2)`. To be nested within a frame.

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

A symbolic computation graph that multiplies 6 × 7. To be nested within a frame.

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

Input disregarded.

Output:

```
42
```

---

## 4️⃣ Combined Frame — Syntax + Semantics

A frame that validates syntax *and* executes a symbolic computation. Complete example.

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

Define a small DSL to simplify addition and multiplication. Complete example.

```
(APPLY
  (FRAME
    (SYNTAX
      (APPLY
        symbolmatch
        (SEXPR
          (RULES
            (FLAT <start> <expr>)
            (FLAT <expr> ("add" <expr> <expr>)
            (FLAT <expr> ("mul" <expr> <expr>)
            (FLAT <expr> ATOMIC))))))))

    (SEMANTICS
      (APPLY
        symbolverse
        (SEXPR
          (REWRITE
            (RULE
              (READ ("add" 0 x))
              (WRITE x))

            (RULE
              (READ ("add" x 0))
              (WRITE x))

            (RULE
              (READ ("mul" 0 x))
              (WRITE 0))

            (RULE
              (READ ("mul" x 0))
              (WRITE 0))

            (RULE
              (READ ("mul" 1 x))
              (WRITE x))

            (RULE
              (READ ("mul" x 1))
              (WRITE x)))))))

  (SEXPR (mul (add 0 5) 1)))
```

Output:

```
5
```

