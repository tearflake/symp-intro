---
layout: docs
---

# "Hello world!" From Symp

Symp is a modular symbolic programming environment designed to define, transform, and execute domain-specific frameworks using a unified representation. Its backend framework triad, together with the frontend, Symp lets you build frameworks using interoperable components. The components include assembly, functional, and rewriting frameworks, together with the frontend user interface. In this document, we briefly state the purpose of the components, and write a minimal "Hello world!" program in each of them.

#### "Hello world!" in assembly component

The purpose of the assembly component is to run code in a fast environment where dealing with variable states apply.

Program:

```
(NODE
    (NAME helloWorld)
    (CONTENT
        (ASSEMBLY
            (ASGN RESULT "Hello world!"))))
```

Input:

ignored

Output:

```
'Hello world!'
```

#### "Hello world!" in functional component

The purpose of the functional component is to utilize untyped lambda calculus to perform computations.

Program:

```
(NODE
    (NAME helloWorld)
    (CONTENT
        (FUNCTIONAL
            "Hello world!")))
```

Input:

none

Output:

```
'Hello world!'
```

#### "Hello world!" in rewriting component

The purpose of the rewriting component is to perform **semantic transformations** on the input. This is done by using a term rewriting system. 

Program:

```
(NODE
    (NAME helloWorld)
    (CONTENT
        (REWRITER
            (RULE ("helloWorld" x) "Hello world!"))))
```

Input:

ignored

Output:

```
'Hello world!'
```

#### "Hello world!" in pagefront

The purpose of the pagefront is to provide a user interface for our program. This is done by utilizing HTML forms inspired content pages.

Program:

```
(NODE
    (NAME helloWorld)
    (CONTENT
        (PAGE
            (PARAG ... "Hello world!"))))
```

Runs as a stand-alone application and outputs the "Hello world!" text on the screen.

#### Further Reading

Each Symp component has its role in a set of programming frameworks. Together, they form a small but expressive environment for building symbolic systems, domain specific frameworks, and experimental interpreters. The components are described in more detail in the accompanying documentation.

