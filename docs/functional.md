---
layout: docs
---

# functional specs

> **[about document]**  
> Specification of the *Functional* part of Symp
>
> **[intended audience]**  
> Intermediate programmers
> 
> **[abstract]**  
> ...

## table of contents

- [1. introduction](#1-introduction)  
- [2. theoretical background](#2-theoretical-background)
    - [2.1. formal syntax](#21-formal-syntax)
    - [2.2. informal semantics](#22-informal-semantics)  
- [3. examples](#3-examples)  
- [4. conclusion](#4-conclusion)  

## 1. introduction

The functional subsystem of Symp provides an implementation of the untyped lambda calculus extended with convenient multi-parameter syntax and symbolic data integration. Its purpose is to offer a compact computational core capable of expressing algorithms, transformations, and symbolic manipulations in a uniform and minimal manner.

In Symp, the lambda calculus is not presented as a separate language but as a structured subset of S-expressions. This allows functional expressions to coexist seamlessly with imperative instructions and rewriting rules. Because of this integration, functional nodes can operate both as self-contained computations and as intermediate processors within larger symbolic workflows.

The goal of this document is not to exhaustively cover the mathematics of lambda calculus, but to introduce the variant used in Symp and to highlight the model of evaluation, the role of S-expressions, and the interaction between abstraction, application, and symbolic constants. The reader is expected to be familiar with basic programming concepts but not necessarily with formal lambda calculus.

## 2. theoretical background

The functional component is grounded in the classical untyped lambda calculus, but adapted for practical use within the Symp ecosystem. The subsections below define its structure formally and describe its behavior informally, providing the foundation needed to understand the examples that follow.

### 2.1. formal syntax

These are the syntax rules of Functional part of Symp:

```
<functional> := (FUNCTIONAL <lmbd>)
 
<lmbd> := (LMBD <ATOMIC>+ <lmbd>)
        | (<lmbd> <lmbd>+)
        | <ATOMIC>
        | (EXP <ANY>)
```
We apply the same syntax definition nomenclature as noted in the Symp specification document.

### 2.2. informal semantics

Lambda calculus, as a foundation of the *functional* part of Symp, represents a very powerful programming paradigm. Solely, it really deserves a lot of its own space to be researched, but here we bring only a few important features that may serve as an introductory material to the rest of its inexhaustible theory.

In lambda calculus, we differentiate between abstractions (`(LMBD x1 x2 ... e)`), applications (`(e e1 e2 ...)`), and constants (atomic values or, in Symp, S-expressions noted like `(EXP e)`). Abstraction is a notation that declares a future variable substitution in an expression, while application is a process of substituting a variable with a value. In original notation, if a lambda expression is intended to abstract more than one variable, it is done by recursive abstraction in a process called currying. For convenience, Symp provides a syntax to abstract several variables at once, just like to apply several expressions at once. For example, lambda expression `(LMBD a b (a b))` declares a function that takes two parameters (`a` and `b`), and applies the first one to the second one. Thus, if we write `((LMBD a b (a b)) f 2)`, the expression evaluates to `(f 2)`. It is also possible to partially apply parameters in a way that `((LMBD a b (a b)) f)` evaluates to `(LMBD b (f b))`.

In the Symp version of lambda calculus, prior to their application, all the parameters are evaluated on their own. This behavior is called call-by-value which makes this interpretation eagerly computed. It is also worth noting that if a variable from an inner abstraction has the same name as any variable from an outer abstraction, the outer variable is being shadowed by the inner one.

S-expressions integrate to the Symp functional part by enclosing them in `(EXP ...)` sections. When an S-expression is placed at the body of the function, upon parameter application, it behaves just like ordinary lambda expressions, replacing all the elements that match the abstracted variable name with the applied value.

Initially, lambda expressions are pure which means that calling the same function with the same parameters always yields the same result. However, when using functions like some from the `stdlib` package, the lambda expressions may depend on externally altered states making them impure.

In lambda calculus, there is a term called recursion, mostly programmed with the famous Y combinator. Because Symp implementation binds a name to each node, there is a possibility to use the functional node name within the contents of the node, making a case for the simplified recursive calls. We have to be careful with recursions since it is possible to form an infinite one resulting in a stack overflow error.

In the end, all the lambda expressions can be considered as expression trees, making the evaluation process equal to systematic substitution of abstracted variables with their values. The resulting expression trees may be further processed by other parts of the Symp framework.

We skimmed over a few important aspects of the lambda calculus, but there is a lot more to say and research about it. For example, Church encodings enable sets like numbers and booleans to be expressed in lambda calculus, while the world of combinators expressed in lambda calculus can make very interesting structures. Interested readers are invited to explore further the true beauty of the lambda calculus in a vast range of resources scattered over the Internet.

## 3. examples

Here, we bring three simple examples grasping the surface of the lambda calculus.

#### identity function

This example is using "identity function" to return the passed parameter:

```
(NODE
    (NAME hello)
    (CONTENT
        (FUNCTIONAL
            ((LMBD x x) "hello"))))
```

The result of this expression is a string `hello`.

#### symbolic data

We can combine abstracted parameters in a symbolic expression:

```
(NODE
    (NAME hiThere)
    (CONTENT
        (FUNCTIONAL
            (USING stdlib)
            ((LMBD a b c (EXP (a b c)))
                0
                1
                2))))
```

which results in the expression `(0 1 2)`.

#### fibonacci numbers

And for the end, we bring a slightly more complex, but concrete example of calculating the n-th fibonacci number:

```
(NODE
    (NAME fib)
    (CONTENT
        (USING
            fib
            (ALIAS sl stdlib))
        
        (FUNCTIONAL
            (LMBD n
                (sl/iff (sl/lt n 2)
                    1
                    (sl/add
                        (fib (sl/sub n 1))
                        (fib (sl/sub n 2))))))))
```

The example uses recursion and references to the `stdlib` built-in library. Function `iff` takes three parameters. If the first parameter evaluates to `true`, the second parameter is returned as a result, and if it evaluates to `false`, the third parameter is returned. The function `lt` returns `true` if its first parameter is less than its second parameter, otherwise `false`. Lastly, `sub` function subtracts the second parameter from the first one.

## 4. conclusion

The functional layer of Symp provides a compact and expressive computational model based on the lambda calculus, extended with convenient multi-parameter abstractions, symbolic constants, and native integration with S-expressions. Its semantics remain intentionally simple: evaluation is driven by substitution, variable binding follows lexical scoping, and expressions reduce to values through a small and predictable set of rules.

Because lambda expressions in Symp are first-class symbolic structures, they interoperate smoothly with the imperative and rewriting subsystems, forming a flexible foundation for transforming data, constructing symbolic representations, or implementing small algorithms. While the examples shown here only scratch the surface, the model is expressive enough to encode higher-order functions, recursion, structured symbolic data, and many classical lambda-calculus abstractions.

The material presented in this document should provide enough background to understand how functional nodes behave and how they may be used in practice. Developers interested in deeper theoretical aspects — including combinatory logic, Church encodings, and evaluation strategies — are encouraged to explore these topics further, as the lambda calculus continues to be a rich and influential foundation for programming language theory.

