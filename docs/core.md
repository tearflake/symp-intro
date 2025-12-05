---
layout: docs
---

# Symp Core Specs

> **[about document]**  
> Specification of *Symp core*, a symbolic processing framework
>
> **[intended audience]**  
> Advanced programmers
> 
> **[abstract]**  
> Symp core is a minimal symbolic computation engine built upon a strict S-expression grammar and a compact set of primitive operations. Its purpose is to provide a referentially transparent environment for expressing deterministic computations using function definitions and pure symbolic evaluation. This document formally defines the syntax, informal semantics, and fundamental computational model of Symp core. It also outlines the relationship between core functions, imported modules, and the built-in primitives that together form a complete and usable foundation for higher-level Symp components.


## Table of Contents

- [1. Introduction](#1-introduction)  
- [2. Theoretical Background](#2-theoretical-background)
    - [2.1. Formal Syntax](#21-formal-syntax)
    - [2.2. Informal Semantics](#22-informal-semantics)  
- [3. Examples](#3-examples)  
- [4. Conclusion](#4-conclusion)  

## 1. Introduction

The Symp core is designed as a small, self-contained language for symbolic computation. It provides only the essential constructs needed to define functions, structure data, and evaluate expressions. The intention is not to recreate a full programming language, but to offer a foundational substrate upon which more elaborate paradigms—imperative, functional, and rewriting systems—can be layered.

The design emphasizes simplicity, referential transparency, and structural clarity. Each program is an ordered collection of named functions, each with fixed arity and a single result expression. Evaluation proceeds through nested S-expressions, with no state, mutation, or side effects. The resulting system is predictable, deterministic, and amenable to formal reasoning, while still being expressive enough to define a wide range of symbolic transformations.

## 2. Theoretical Background

The Symp core rests on a formal syntactic structure that defines what constitutes a valid program, and an informal semantic model describing how such programs are evaluated. Although small, the system is computationally complete when combined with the six built-in primitive functions. This section introduces the grammar used in Symp core, its conventions, and the semantics guiding the evaluation process.

### 2.1. Formal Syntax

In computer science, the syntax of a computer language is the set of rules that defines the combinations of symbols that are considered to be correctly structured statements or expressions in that language. Symp core language itself resembles a kind of S-expression. S-expressions consist of lists of atoms or other S-expressions where lists are surrounded by parenthesis. In Symp core, the first list element to the left determines a type of a list. There are a few predefined list types depicted by the following relaxed kind of Backus-Naur form syntax rules:

```
<start> := (CORE (USING (ALIAS <ATOMIC> <ATOMIC>)+)? <func>+)

<func> := (FUNCTION <ATOMIC> (PARAMS <ATOMIC>+)? (RESULT <ANY>))
```

The above grammar defines the syntax of Symp core. To interpret these grammar rules, we use special symbols: `<...>` for noting identifiers, `... := ...` for expressing assignment, `...+` for one or more occurrences, `...*` for zero or more occurrences, `...?` for optional appearance, and `... | ...` for alternation between expressions. All other symbols are considered parts of the Symp core grammar.

Atoms may be enclosed between a pair of `'` characters if we want to include special characters used in the grammar. Strings are enclosed between a pair of `"` characters. Multiline atoms and strings are enclosed between an odd number of `'` or `"` characters.
 
In addition to the exposed grammar, user comments have no meaning to the system, but may be descriptive to readers, and may be placed wherever a whitespace is expected. Single line comments begin with `//` and span to the end of line. Multiline comments begin with `/*` and end with `*/`.

We will be using the same syntax definition nomenclature throughout the accompanying specification documents of imperative, functional, and rewriting extensions, and device components.

### 2.2 Informal Semantics

Semantics of Symp core is inspired by the functional programming paradigm and Lisp primitives. It is meant to be a very minimalist, but still practical computing environment. Program written in Symp core is a set of referentially transparent functions with a fixed arity. Each function returns an arbitrary S-expression evaluated using other functions or primitives. There are six built-in primitive functions which make Symp core a Turing complete computing environment:

- `CALL`: two or more parameters. calls a function named by the first parameter, possibly with arguments from the second parameter onward.
- `IF`: three parameters. If the first parameter is `true`, it returns the second one. If not, it returns the third one. 
- `EQ`: two parameters. If both parameters are atoms and are equal, it returns `true`; otherwise `false`.
- `HEAD`: one parameter as a list. Returns the first list element.
- `TAIL`: one parameter as a list. Returns the list without the first element.
- `CONS`: two parameters. The second parameter is a list. Returns a new list, placing the first parameter at the beginning of the second parameter list

Symp core program may import other programs stored in separate files, by `USING` section. Each program is given a unique name within `ALIAS` sections where the first parameter determines a referent name while the second parameter points to a separate Symp core file. Functions of imported programs are called by noting the referent name followed by the `.` and the function name.

## 3. Examples

We present seven examples each one building on the previous. Every example will be a full program with one or more functions, increasing in complexity.

#### Example 1 — A Constant Function

The absolute simplest Symp program: returns a hardcoded value.

```
(CORE
    (FUNCTION answer (PARAMS) (RESULT 42))
)
```

Calling:

```
(CALL answer)
```

→ `42`

---

#### Example 2 — Identity Function

A function with one parameter.

```
(CORE
    (FUNCTION id
        (PARAMS x)
        (RESULT x))
)
```

Call:

```
(CALL id hello)
```

→ `hello`

---

#### Example 3 — Boolean Branching

Use of built-in `IF`.

```
(CORE
    (FUNCTION is-hello
        (PARAMS x)
        (RESULT
            (IF (EQ x hello)
                true
                false)))
)
```

Call:

```
(CALL is-hello hello)
```

→ `true`

---

#### Example 4 — List Manipulation (HEAD/TAIL/CONS)

A function that returns the first element of a list.

```
(CORE
    (FUNCTION first
        (PARAMS xs)
        (RESULT
            (HEAD xs)))
)
```

Call:

```
(CALL first (a b c))
```

→ `a`

---

#### Example 5 — List Length (Recursive)

A classic example showing recursion.

```
(CORE
    (FUNCTION length (PARAMS xs)
        (RESULT
            (IF (EQ xs ())
                ()
                (CONS 1 (CALL length (TAIL xs))))))
)
```

This returns a unary count like `(1 1 1)` rather than a decimal number.

Call:

```
(CALL length (a b c))
```

→ `(1 1 1)`

---

#### Example 6 — Factorial (Direct Recursion + Conditionals)

Using symbolic numbers.

```
(CORE
    (FUNCTION factorial (PARAMS n)
        (RESULT
            (IF (EQ n 0)
                1
                (CALL multiply
                    n
                    (CALL factorial (CALL subtract n 1))))))
  
    (FUNCTION subtract (PARAMS a b)
        (RESULT
            (... your arithmetic ...)))

    (FUNCTION multiply (PARAMS a b)
        (RESULT
            (... your arithmetic ...)))
)
```

(You can plug in your preferred numeric encoding.)

---

#### Example 7 — Map Over a List

A higher-order structure without higher-order functions — you explicitly pass the function name.

```
(CORE
    (FUNCTION map (PARAMS f xs)
        (RESULT
            (IF (EQ xs ())
                ()
                (CONS
                    (CALL f (HEAD xs))
                    (CALL map f (TAIL xs))))))
)
```

Call:

```
(CALL map increment (1 2 3))
```

---

We skimmed over seven examples, ranging from literals to branching, to lists, to recursion, and to higher order constructs. The examples are not intended to show the full potential of Symp core, yet to provide an introduction to core capabilities. For more advanced examples of meta-programming, interested reader is invited to read the accompanying documentation where we deal with imperative, functional, and rewriting programming paradigms.

## 4. Conclusion

The Symp core provides a deliberately minimal foundation for symbolic computation. Its strict S-expression syntax, fixed-arity functions, and small set of primitives allow users to construct predictable and transparent computational structures. Although the core itself is intentionally sparse, it serves as the stable backbone for the richer paradigms defined in the broader Symp system. The examples provided illustrate how expressive behavior can emerge from a core symbolic substrate.

