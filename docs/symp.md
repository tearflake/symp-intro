# symp intro

> **[about document]**  
> Introduction to *Symp*, a symbolic processing framework
>
> **[intended audience]**  
> Advanced programmers
> 
> **[abstract]**  
> Symp is a symbolic orchestration framework that embeds computation, semantics, and user interface construction within a unified tree-structured program model. It connects Symbolmatch, Symbolverse, Symbolprose, and Symbolfront into an executable composition layer, where each component is named, loaded, referenced, and executed through a declarative node system. Symp turns file-system hierarchy into a first-class part of program structure, enabling programs to be organized, executed, and reasoned about as symbolic trees rather than linear code files.


## table of contents

- [1. introduction](#1-introduction)  
- [2. theoretical background](#2-theoretical-background)
    - [2.1. formal syntax](#21-formal-syntax)
    - [2.2. informal semantics](#22-informal-semantics)  
- [3. examples](#3-examples)  
- [4. conclusion](#4-conclusion)  

## 1. introduction

Symp provides the structural layer that integrates the Symbolmatch, Symbolverse, Symbolprose, and Symbolfront subsystems into runnable projects. Its primary purpose is to organize computation into a named, navigable tree rather than a monolithic program. Each node carries a single responsibility — such as syntax definition, semantic transformation, runtime evaluation, or UI construction — and nodes cooperate via explicit symbolic referencing, not global scope.

The framework removes the distinction between *program organization* and *program execution model*: the same symbolic representation is used to store, locate, and run components. This minimizes hidden state, avoids implicit linking, prevents namespace ambiguity, and enables deterministic resolution of dependencies at runtime.

## 2. theoretical background

Symp models programs as acyclic trees that directly map to a directory structure. Every node represents a computational or interactive element and is uniquely identified by its position in that tree. Node relationships replace import systems, module loaders, and runtime registries — visibility and access are controlled structurally rather than through component-level scoping rules.

By aligning symbolic node trees with real file paths, Symp ensures that program layout, documentation hierarchy, build steps, and UI composition can be derived automatically from structure alone. The interpreter does not rely on implicit search paths or environment configuration; all resolution follows tree navigation rules. This makes program behavior inspectable, relocatable, and self-describing.

To visualize the structure and control flow in a Symp application, we can draw a diagram connecting all the main components of the Symp programming framework:

```
                  [symp components connection diagram]

• • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • •
•                                                                       •
•                            R E N D E R E R                            •
•                                                                       •
• • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • •
•                                                                       •
•                                                                       •
•                           • • • • • • • • •                           •
•                           •               •                           •
•                           •  SYMBOLFRONT  •                           •
•                           •               •                           •
•                           • • • • • • • • •                           •
•                                   •                                   •
•                                   •                                   •
•             • • • • • • • • • • • • • • • • • • • • • • •             •
•             •                     •                     •             •
•             •                     •                     •             •
•     • • • • • • • • •     • • • • • • • • •     • • • • • • • • •     •
•     •               •     •               •     •               •     •
•     •  SYMBOLMATCH  •     •  SYMBOLVERSE  •     •  SYMBOLPROSE  •     •
•     •               •     •               •     •               •     •
•     • • • • • • • • •     • • • • • • • • •     • • • • • • • • •     •
•                                                                       •
•                                                                       •
• • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • •
•                                                                       •
•                   A P P L I C A T I O N     C O D E                   •
•                                                                       •
• • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • • •
```

The diagram reads from the ground up. Symp reads the application code, processes it by applying Symbolmatch, Symbolverse, and Symbolprose in a sequence required by the Symbolfront that provides the UI to the renderer. The renderer may send feedback information to the application, regenerating its UI in each cycle. Resulting application maintains its internal states in the renderer while it is alive. External states may be altered by modifying the application code.

### 2.1. formal syntax

In computer science, the syntax of a computer language is the set of rules that defines the combinations of symbols that are considered to be correctly structured statements or expressions in that language. Symp language itself resembles a kind of S-expression. S-expressions consist of lists of atoms or other S-expressions where lists are surrounded by parenthesis. In Symp, the first list element to the left determines a type of a list. There are a few predefined list types used for data transformation depicted by the following relaxed kind of Backus-Naur form syntax rules:

```
<start> := <tree>

<tree> := (NODE
              (NAME <ATOMIC>)
              (CONTENT (USING <ATOMIC>+)? <COMPONENT>)?
              (BRANCHES <tree>+)?
          )
```

The above grammar defines the syntax of Symp. To interpret these grammar rules, we use special symbols: `<...>` for noting identifiers, `... := ...` for expressing assignment, `...+` for one or more occurrences, `...*` for zero or more occurrences, `...?` for optional appearance, and `... | ...` for alternation between expressions. All other symbols are considered as parts of the Symp grammar.

Atoms may be enclosed between a pair of `'` characters if we want to include special characters used in the grammar. Strings are enclosed between a pair of `"` characters. Multiline atoms and strings are enclosed between an odd number of `'` or `"` characters.
 
In addition to the exposed grammar, user comments have no meaning to the system, but may be descriptive to readers, and may be placed wherever a whitespace is expected. Single line comments begin with `//` and span to the end of line. Multiline comments begin with `/*` and end with `*/`.

### 2.2 informal semantics

Every program in Symp is wrapped up within the tree data structure. Each tree consists of a parent node which may terminate as a leaf node, or may branch out further to a set of child nodes. This structure may indefinitely recursively repeat. There exist cyclic and acyclic kinds of trees while Symp is initially set to handle the acyclic kind.

The tree structure, as one of the cornerstones in Symp, is not contained only in source code files, yet each tree in the code spans further up the file-directory system holding the files. Such feature makes the original file and directory structure seamlessly span into the source code, making no distinction between the low level file-directory structure and the trees expressed in code files. To achieve this characteristic, Symp kind of file-directory tree must follow a strict form compatible with the low level OS filesystem. Thus, a file containing a node must have the same name as declared in node. Such file may be accompanied by a directory having the same name having the additional `.branch` extension. The branching may be defined either in the source code file, or a directory, but not both. Such system of files and directories have to start in some directory in the structure of the entire low level filesystem, and we will refer to that location as a `HOME` node.

Within the structure branching from the `HOME` node, every tree node may contain some kind of computation component, or hold an use interface component. The primary Symp tree structure intention is to assign names to those components. Components may refer to other components by noting other components paths in the `(USING ...)` sections when declaring nodes `CONTENT`. During the reference, in the surrounding tree, a node may see only adjacent, parents, and adjacent to their parents nodes. To access any other node, Symp uses the initially visible tree node names as a starting point for reaching deeper nodes by enumerating node names in a sequence, separated by the `/` character. Such sequence then forms a path to an unique node containing the component of our interest.

#### computational components

In Symp, there are three computational components, namely Symbolmatch, Symbolverse, and Symbolprose. Each of these functions are minimalist programming segments. They take an input parameter, and produce some output. This computational triad serves as an utility bundle for creating new programming frameworks. The bundle is capable of defining any syntax, semantics, and runtime behavior of the newly created frameworks. The purpose of Symbolmatch is to declare syntaxes of newly created frameworks. The purpose of Symbolverse is to define semantics of newly created frameworks. The purpose of Symbolprose is to serve as a runtime for executing programs of newly created frameworks.

Finally, the built-in computational triad closes the programming cycle of ranging between the form, meaning, and execution, making Symp capable of expressing a whole plethora of domain specific framework. For the details about Symbolmatch, Symbolverse, and Symbolprose functionality, the reader is kindly asked to refer to relevant documentation accompanying this distribution.

#### user interface component

Upon running, and after all the computations are performed, each Symp source code file may output a structure that may serve as a user interface (UI) of a page system created in Symp framework. There may exist many variations of UI, but the Symp version opinionated to the one inspired by a combination of HTML and markdown file formats. It is called Symbolfront, and it is the fourth kind of component that may be held within the Symp trees. For the details about it, the reader is kindly asked to refer to relevant documentation accompanying this distribution.

## 3. examples

We bring a two simplistic examples describing how components held within Symp tree are accessed. For understanding examples, the reader is expected to have a certain level of familiarity with the Symbolmatch, Symbolverse, Symbolfront, and Synbolfront, as attainable from the documentation supporting these frameworks.

### example 1: building a UI application

The following example builds up a small user interface depicting a number and two buttons for incrementing and decrementing the number:

```
(NODE
    (NAME uiExample)
    (CONTENT
        (USING THIS)
        (PAGE
            (HDR1 "Increment/decrement value")
            (PARAG (calc PARAMS))
            (HBLIST
                (BUTTON incr "increment" (GET uiExample (incr PARAMS)))
                (BUTTON decr "decrement" (GET uiExample (decr PARAMS))))))
    (BRANCHES
        (NODE
            (NAME calc)
            (CONTENT
                (USING stdlib)
                (REWRITE
                    (RULE (READ ()) (WRITE "0"))
                    (RULE (READ ("incr" x)) (WRITE (add (x "1"))))
                    (RULE (READ ("decr" x)) (WRITE (sub (x "1")))))))))

```

Let's store this example into a file named `uiExample`. When we run the Symbolback interpreter on the file, the UI opens, shows the number `0`, and waits for our action of pushing its buttons. According to our further actions, the number increments/decrements by one.

The root node constructs the user interface with a number calculated by the node `calc` which is made accessible by `(USING THIS)` section. The initial ignition of the file is supposed to be without an input which is then assumed to be of a default value `()`. The `calc` node turns this input to `0`. Further UI interactions reload the `uiExample` with action parameters, causing the number to change and recalculate its value.

### example 2: simple math optimizer using syntax checking, semantic transformation, and runtime execution

The following example shows how to apply the `syntax`, `semantics`, and `runtime` functions to validate, compile, and execute a domain specific kind of computation. It performs an optimization of a math expression involving addition and multiplication of integers:

```
(NODE
    (NAME optimizer)
    (CONTENT
        (USING THIS stdlib)
        (PAGE
            (HDR1 "Simple Math Optimizer")
            (EDIT expr 10 (stringify PARAMS))
            (HBLIST (BUTTON opt "Optimize" (GET optimizer expr)))
            (PARAG (runtime PARAMS)))))))
    
    (BRANCHES
        (NODE
            (NAME syntax)
            (CONTENT
                (GRAMMAR
                    (RULE START addmul)
                    
                    (RULE addmul (LIST "add" (LIST math (LIST math ()))))
                    (RULE addmul (LIST "mul" (LIST math (LIST math ()))))
                    (RULE addmul digits)
                    
                    (RULE digits (ATOM digit digits))
                    (RULE digits (ATOM digit ()))

                    (RULE digit "0")
                    (RULE digit "1")
                    (RULE digit "2")
                    (RULE digit "3")
                    (RULE digit "4")
                    (RULE digit "5")
                    (RULE digit "6")
                    (RULE digit "7")
                    (RULE digit "8")
                    (RULE digit "9"))))
        
        (NODE
            (NAME semantics)
            (CONTENT
                (REWRITE
                    (RULE (READ ("add"  x  "0")) (WRITE  x ))
                    (RULE (READ ("add" "0"  x )) (WRITE  x ))
                    (RULE (READ ("mul"  x  "1")) (WRITE  x ))
                    (RULE (READ ("mul" "1"  x )) (WRITE  x ))
                    (RULE (READ ("mul"  x  "0")) (WRITE "0"))
                    (RULE (READ ("mul" "0"  x )) (WRITE "0")))))

        (NODE
            (NAME runtime)
            (CONTENT
                (USING math stdlib)
                (RUNGRAPH
                    (EDGE
                        (SOURCE BEGIN)
                        (INSTR
                            (ASGN expr (syntax PARAMS))
                            (TEST (nth syn 0) "ERROR")
                            (ASGN RESULT "syntax error"))
                        (TARGET END))
                        
                    (EDGE
                        (SOURCE BEGIN)
                        (INSTR (ASGN RESULT (stringify (semantics expr))))
                        (TARGET END)))))
```

Root node sets up a small interface containing  an editor, a button, and a feedback paragraph. Below the root node, the example declares `syntax`, `semantics`, and `runtime` nodes. Symbolprose `runtime` node utilizes Symbolmatch `syntax` and Symbolverse `semantics` nodes. After entering a math expression and pressing the `Optimize` button in the user interface, the `runtime` node is executed. Here, the `syntax` node checks the syntax of entered math expressions. If the syntax rules decide it is valid, the same input is passed through the `semantics` node that performs the optimization. Finally, optimized expression is outputted to the UI paragraph.

In summary, if we pass a parameter like `(add 0 (mul 7 1))` to this example, we may expect an output like `7` in the output. The constructions used in this specific example represent a pattern capable of handling more complex domain specific programming frameworks.

## 4. conclusion

Symp defines an execution model where program structure, computation, and user interfaces are integrated through a uniform symbolic tree. Through this approach, Symp provides a minimal but complete foundation for constructing new micro-languages, application runtimes, and interactive tools without leaving the symbolic domain. Its deterministic referencing, distributed responsibility model, and filesystem-aligned structure allow incremental system growth with minimal coordination overhead.

