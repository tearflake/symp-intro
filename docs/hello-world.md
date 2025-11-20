---
layout: docs
---

# "Hello world" From Symp

Symp is a modular symbolic programming environment designed to define, transform, and execute domain-specific languages using a unified representation. Instead of hard-coding syntax, semantics, and runtime behavior in a general-purpose language, Symp lets you build them declaratively, layer by layer, using interoperable components. The components include Symbolmatch (syntax), Symbolverse (semantics), Symbolprose (runtime), and Symbolfront (presentation). In this document, we shortly state the purpose of the components, and write a minimal "Hello world!" program in each of them.

#### "Hello world!" in Symbolmatch

The purpose of Symbolmatch is to perform a **syntax check** of an input. This is done by using input grammar rules. If the input conforms the rules, it is merely copied to the output.

Program:

```
(NODE
    (NAME helloWorld)
        (CONTENT
            (GRAMMAR
                (RULE START "Hello world!"))))
```

Input:

```
'Hello world!'
```

Output:

```
'Hello world!'
```

#### "Hello world!" in Symbolverse

The purpose of Symbolverse is to perform **semantic transformations** on the input. This is done by using a term rewriting system.

Program:

```
(NODE
    (NAME helloWorld)
        (CONTENT
            (REWRITE
                (RULE
                    (READ ())
                    (WRITE "Hello world!")))))
```

Input:

```
()
```

Output:

```
'Hello world!'
```

#### "Hello world!" in Symbolprose

The purpose of Symbolprose is to perform stateful **runtime execution**. This is done by using a finite state machine inspired computation graph. 

Program:

```
(NODE
    (NAME helloWorld)
        (CONTENT
            (RUNGRAPH
                (EDGE
                    (SOURCE BEGIN)
                    (INSTR
                        (ASGN RESULT ("Hello world!")))
                    
                    (TARGET END)))))
```

Input:

ignored

Output:

```
'Hello world!'
```

#### "Hello world!" in Symbolfront

The purpose of Symbolfront is to provide **presentation** means for our program. This is done by utilizing HTML forms inspired content pages.

Program:

```
(NODE
    (NAME helloWorld)
        (CONTENT
            (PAGE
                (PARAG "Hello world!"))))
```

Runs as a stand-alone application and outputs the "Hello world!" text on the screen.

#### Further Reading

Each Symp component has its role in a pipeline `syntax → semantics → runtime → presentation`. Together, they form a small but expressive environment for building symbolic systems, DSLs, and experimental interpreters. The components are described in more detail in the accompanying documentation.

