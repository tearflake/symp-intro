---
layout: docs
---

# symp specs

> **[about document]**  
> Specification of *Symp*, a symbolic processing framework
>
> **[intended audience]**  
> Advanced programmers
> 
> **[abstract]**  
> Symp is a symbolic coordination framework that embeds computation and user interface construction within a unified tree-structured program model. It connects its backend and frontend into an executable composition layer, where each component is named, loaded, referenced, and executed through a declarative node system. Symp turns file-system hierarchy into a first-class part of program structure, enabling programs to be organized, executed, and reasoned about as symbolic trees rather than linear code files.


## table of contents

- [1. introduction](#1-introduction)  
- [2. theoretical background](#2-theoretical-background)
    - [2.1. formal syntax](#21-formal-syntax)
    - [2.2. informal semantics](#22-informal-semantics)  
- [3. informative example](#3-informative-example)  
- [4. conclusion](#4-conclusion)  

## 1. introduction

Symp provides the structural layer that integrates the assembly, functional, rewriter, and pagefront components into runnable projects. Its primary purpose is to organize computation into a named, navigable tree rather than a monolithic program. Each node carries a single responsibility — such as backend support or UI construction — and nodes cooperate via explicit symbolic referencing, not global scope.

The framework removes the distinction between *program organization* and *program execution model*: the same symbolic representation is used to store, locate, and run components. This minimizes hidden state, avoids implicit linking, prevents namespace ambiguity, and enables deterministic resolution of dependencies at runtime.

## 2. theoretical background

Symp models programs as acyclic trees that directly map to a directory structure. Every node represents a computational or interactive element and is uniquely identified by its position in that tree. Node relationships replace import systems, module loaders, and runtime registries — visibility and access are controlled structurally rather than through component-level scoping rules.

By aligning symbolic node trees with real file paths, Symp ensures that program layout, documentation hierarchy, build steps, and UI composition can be derived automatically from structure alone. The interpreter does not rely on implicit search paths or environment configuration; all resolution follows tree navigation rules. This makes program behavior inspectable, relocatable, and self-describing.

### 2.1. formal syntax

In computer science, the syntax of a computer language is the set of rules that defines the combinations of symbols that are considered to be correctly structured statements or expressions in that language. Symp language itself resembles a kind of S-expression. S-expressions consist of lists of atoms or other S-expressions where lists are surrounded by parenthesis. In Symp, the first list element to the left determines a type of a list. There are a few predefined list types depicted by the following relaxed kind of Backus-Naur form syntax rules:

```
<start> := <tree>

<tree> := (NODE
              (NAME <ATOMIC>)
              (CONTENT (USING <using>+)? <component>)?
              (BRANCHES <tree>+)?)

<using> := <ATOMIC>
         | (ALIAS <ATOMIC> <ATOMIC>)

<component> := <frontend>
             | <backend>

<backend> := <assembly>
           | <functional>
           | <rewriter>
```

The above grammar defines the syntax of Symp. To interpret these grammar rules, we use special symbols: `<...>` for noting identifiers, `... := ...` for expressing assignment, `...+` for one or more occurrences, `...*` for zero or more occurrences, `...?` for optional appearance, and `... | ...` for alternation between expressions. All other symbols are considered parts of the Symp grammar.

Atoms may be enclosed between a pair of `'` characters if we want to include special characters used in the grammar. Strings are enclosed between a pair of `"` characters. Multiline atoms and strings are enclosed between an odd number of `'` or `"` characters.
 
In addition to the exposed grammar, user comments have no meaning to the system, but may be descriptive to readers, and may be placed wherever a whitespace is expected. Single line comments begin with `//` and span to the end of line. Multiline comments begin with `/*` and end with `*/`.

We will be using the same syntax definition nomenclature throughout the accompanying specification documents of assembly, functional, rewriter, and pagefront components.

### 2.2 informal semantics

Every program in Symp is wrapped up within the tree data structure. Each tree consists of a node which may terminate as a leaf node, or may branch out further as a parent to a set of child nodes. This structure may indefinitely recursively repeat at each node.

#### tree-filesystem alignment

The tree structure, as one of the cornerstones in Symp, is not contained only in source code files, yet each tree in the code spans further up the file-directory system holding the files. Such feature makes the original file and directory structure seamlessly span into the source code, making no distinction between the low level file-directory structure and the trees expressed in source code files. To achieve this characteristic, the Symp kind of file-directory tree must follow a strict form compatible with the low level OS filesystem. Thus, a file containing a node must have the same name as declared in the node code. Such a file may be associated with a directory having the same name with the additional `.branch` extension. The branching may be defined either in the source code file, or in a directory, but not both. The system of files and directories have a starting directory in the structure of the entire low level filesystem, and we will refer to that location as a `HOME` node. All the files and directories belonging directly, or indirectly to the `HOME` directory must follow these rules.

#### visibility and referencing

Within the structure branching from the `HOME` node, every tree node may contain a kind of computation component. The primary intention of the Symp tree structure is to assign names to those components. Components may refer to each other by noting other node paths in the `(USING ...)` sections after announcing `CONTENT`. During the reference to surrounding tree nodes, a component initially sees only its node and all its parent nodes. The child nodes of these parents are reached by appending the relative child node path, separating the path elements by the `/` character. After `(USING ...)` declaration, within the component definition, to refer to the nodes declared from `(USING ...)` section, we write only the last element of the path (or its alias if declared by `(ALIAS ...)` section). From that element, we may similarly append further path elements to access further child nodes.

#### backend computational components

In Symp, there are three backend computational components, namely assembly, functional, and rewriter. Each of these components are Turing complete minimalist programming segments. This computational triad serves as an utility bundle for creating new programming frameworks. The bundle is capable of defining any syntax, semantics, and runtime behavior of the newly created frameworks. Finally, the built-in computational triad closes the programming span of ranging between the imperative, functional, and rule-based programming, making Symp capable of expressing a whole plethora of domain specific frameworks. For the details about assembly, functional, and rewriter components functionality, the reader is kindly asked to refer to relevant documentation accompanying this distribution.

#### frontend user interface component

Upon running, and after all the computations are performed, each Symp source code file may output a structure that may serve as a user interface (UI) of a page system created in Symp framework. There may exist many variations of UI, but the Symp version opinionated to the one inspired by HTML forms and markdown file formats. It is called pagefront, and it is the fourth, and the last kind of component that may be held within the Symp trees. For the details about it, the reader is kindly asked to refer to relevant documentation accompanying this distribution.

## 3. informative example

Here, we bring a composite example of a simplistic calculator with its own user interface. The purpose of this example is not to give a comprehensive explanation about how Symp works, but to exhibit a representative form of a Symp application. A reader more interested in Symp may return to this example after completing reading the entire documentation. The example is called "Three-Mind Calculator" because it is using all of the three available computation models in Symp to produce new values.

We start our exposure with the user interface:

```
(NODE
    (NAME calculator)
    (CONTENT
        (USING (ALIAS fnc calculator/functionalOps))
        (PAGE
            (HDR1 "Three-Mind Calculator")
            (PARAG "num" 0)
            (HBLIST
                (BUTTON "add" "Increment"
                    (GET (PARAG "num" (fnc/add1 num)))
                    (TARGET "num"))
                
                (BUTTON "mult" "Double"
                    (GET (PARAG "num" (fnc/mul2 num)))
                    (TARGET "num"))
                
                (BUTTON "fact" "Factorial"
                    (GET (PARAG "num" (fnc/fact num)))
                    (TARGET "num")))))

    (BRANCHES
        (NODE
            (NAME functionalOps)
            ...)

        (NODE
            (NAME assemblyOps)
            ...)

        (NODE
            (NAME rewriterOps)
            ...)))
```

The user interface consists of a title, a number, and three horizontally aligned buttons for incrementing, doubling, and calculating a factorial of a number initially set to zero. Pressing the buttons will make a call to functions `add1`, `mul2`, and `fact`, respectively. The calls to the functions are made possible by explicitly stating that we will use them in the body of the `PAGE` section. This is done in the `(USING ...)` section, stating the calls and their referent name in the `(ALIAS ...)` section. The called functions are defined in `functionalOps` node as follows:

```
(NODE
    (NAME functionalOps)
    (BRANCHES
        (NODE
            (NAME add1)
            (CONTENT
                (USING (ALIAS rwr rewriterOps) (ALIAS asm assemblyOps))
                (FUNCTIONAL
                    (LMBD n
                        (asm/decode
                            (rwr
                                (add
                                    (asm/encode n)
                                    ("S" "Z"))))))))
        
        (NODE
            (NAME mul2)
            (CONTENT
                (USING (ALIAS rwr rewriterOps) (ALIAS asm assemblyOps))
                (FUNCTIONAL
                    (LMBD n
                        (asm/decode
                            (rwr
                                (mul
                                    (asm/encode n)
                                    ("S" ("S" "Z")))))))))
        
        (NODE
            (NAME fact)
            (CONTENT
                (USING (ALIAS rwr rewriterOps) (ALIAS asm assemblyOps))
                (FUNCTIONAL
                    (LMBD n
                        (asm/decode
                            (rwr
                                (fact
                                    (asm/encode n))))))))))
```

Each of the functions first make a call to encoder of decimal to unary number (`encode`), then runs a term rewriting operation on the unary number (`rwr`), and finally decodes the result back to a decimal number (`decode`). Encoder and decoder are written in assemblyOps node as follows:

```
(NODE
    (NAME assemblyOps)
    (BRANCHES
        (NODE
            (NAME encode)
            (CONTENT
                (USING (ALIAS sl stdlib))
                (ASSEMBLY
                    // encode: int → symbolic
                    (ASSGN s "Z")
                    (ASSGN n PARAMS)
                    (JMPNE n 0 encode_loop)
                    (JMPNE 0 1 encode_done)
                    (LABEL encode_loop)
                        (ASSGN n (sl/sub n 1))
                        (ASSGN s ("S" s))
                        (JMPNE n 0 encode_loop)

                    (LABEL encode_done)
                        (ASSGN RESULT s))))
        
        (NODE
            (NAME decode)
            (CONTENT
                (USING (ALIAS sl stdlib))
                (ASSEMBLY
                    // decode: symbolic → int
                    (ASSGN count 0)
                    (ASSGN s PARAMS)
                    (JMPNE s "Z" decode_loop)
                    (JMPNE 0 1 decode_done)
                    (LABEL decode_loop)
                        (ASSGN s (sl/snd s))
                        (ASSGN count (sl/add count 1))
                        (JMPNE s "Z" decode_loop)

                    (LABEL decode_done)
                        (ASSGN RESULT count))))))
```

Here, we use the hardcoded `stdlib` library of functions to access `add` (adds two numbers), `sub` (subtracts two numbers), and `snd` (returns the second list element) functions.

Unary numbers consumed and produced by encoder/decoder are of the following notation:

```
0 = Z
1 = (S Z)
2 = (S (S Z))
3 = (S (S (S Z)))
...
```

This kind of unary numbers are suitable for performing computations in a very simple manner. This is shown by the rewriterOps node where the supported operations are `add`, `mul`, and `fact`:

```
(NODE
    (NAME rewriterOps)
    (CONTENT
        (REWRITER
            // Addition
            (RULE (READ (add Z n)) (WRITE n))
            (RULE (READ (add (S m) n)) (WRITE (S (add m n))))

            // Multiplication
            (RULE (READ (mul Z n)) (WRITE Z))
            (RULE (READ (mul (S m) n)) (WRITE (add n (mul m n))))

            // Factorial
            (RULE (READ (fact Z)) (WRITE (S Z)))
            (RULE (READ (fact (S n))) (WRITE (mul (S n) (fact n)))))))
```

The entire example consisting of these four segments shows how to use all the four available node types, namely in order of appearance: pagefront, functional, assembly, and rewriter. In Symp, all the operations are based on these node types while the new domain specific frameworks may be defined in their terms and called as functions when needed.

## 4. conclusion

Symp defines an execution model where program structure, computation, and user interfaces are integrated through a uniform symbolic tree. Through this approach, Symp provides a foundation for constructing application runtimes and interactive tools without leaving the symbolic domain. Its deterministic referencing, distributed responsibility model, and filesystem-aligned structure allow incremental system growth.

