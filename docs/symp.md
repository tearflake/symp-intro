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
- [3. examples](#3-examples)  
- [4. conclusion](#4-conclusion)  

## 1. introduction

Symp provides the structural layer that integrates the Symbolmatch, Symbolverse, Symbolprose, and Symbolfront subsystems into runnable projects. Its primary purpose is to organize computation into a named, navigable tree rather than a monolithic program. Each node carries a single responsibility — such as syntax definition, semantic transformation, runtime evaluation, or UI construction — and nodes cooperate via explicit symbolic referencing, not global scope.

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
              (BRANCHES <tree>+)?
          )

<using> := <ATOMIC>
         | (ALIAS <ATOMIC> <ATOMIC>)

<component> := <frontend>
             | <backend>

<backend> := <assembly>
           | <functional>
           | <rewriting>
```

The above grammar defines the syntax of Symp. To interpret these grammar rules, we use special symbols: `<...>` for noting identifiers, `... := ...` for expressing assignment, `...+` for one or more occurrences, `...*` for zero or more occurrences, `...?` for optional appearance, and `... | ...` for alternation between expressions. All other symbols are considered parts of the Symp grammar.

Atoms may be enclosed between a pair of `'` characters if we want to include special characters used in the grammar. Strings are enclosed between a pair of `"` characters. Multiline atoms and strings are enclosed between an odd number of `'` or `"` characters.
 
In addition to the exposed grammar, user comments have no meaning to the system, but may be descriptive to readers, and may be placed wherever a whitespace is expected. Single line comments begin with `//` and span to the end of line. Multiline comments begin with `/*` and end with `*/`.

We will be using the same syntax definition nomenclature throughout the accompanying specification documents of assembly, functional, and rewriting components.

### 2.2 informal semantics

Every program in Symp is wrapped up within the tree data structure. Each tree consists of a node which may terminate as a leaf node, or may branch out further as a parent to a set of child nodes. This structure may indefinitely recursively repeat at each node. In theory, there exist cyclic and acyclic kinds of trees while Symp is initially set to handle the acyclic kind.

The tree structure, as one of the cornerstones in Symp, is not contained only in source code files, yet each tree in the code spans further up the file-directory system holding the files. Such feature makes the original file and directory structure seamlessly span into the source code, making no distinction between the low level file-directory structure and the trees expressed in source code files. To achieve this characteristic, Symp kind of file-directory tree must follow a strict form compatible with the low level OS filesystem. Thus, a file containing a node must have the same name as declared in the node code. Such file may be associated to a directory having the same name with the additional `.branch` extension. The branching may be defined either in the source code file, or in a directory, but not both. Such system of files and directories have a starting directory in the structure of the entire low level filesystem, and we will refer to that location as a `HOME` node. All the files and directories belonging directly, or indirectly to the `HOME` directory must follow these rules.

Within the structure branching from the `HOME` node, every tree node may contain a kind of computation component. The primary intention of the Symp tree structure is to assign names to those components. Components may refer to each other by noting other node paths in the `(USING ...)` sections after announcing `CONTENT`. During the reference to surrounding tree nodes, a component initially sees only its node and all its parent nodes. The child nodes of these parents are reached by appending the relative child node path, separating the path elements by the `/` character. After `(USING ...)` declaration, within the component definition, to refer to the nodes declared from `(USING ...)` section, we write only the last element of the path (or its alias if declared by `(ALIAS ...)` section). From that element, we may similarly append further path elements to access further child nodes.

#### backend computational components

In Symp, there are three backend computational components, namely assembly, functional, and rewriting. Each of these components are Turing complete minimalist programming segments. This computational triad serves as an utility bundle for creating new programming frameworks. The bundle is capable of defining any syntax, semantics, and runtime behavior of the newly created frameworks. Finally, the built-in computational triad closes the programming cycle of ranging between the imperative, functional, and logic programming, making Symp capable of expressing a whole plethora of domain specific frameworks. For the details about assembly, functional, and rewriting components functionality, the reader is kindly asked to refer to relevant documentation accompanying this distribution.

#### frontend user interface component

Upon running, and after all the computations are performed, each Symp source code file may output a structure that may serve as a user interface (UI) of a page system created in Symp framework. There may exist many variations of UI, but the Symp version opinionated to the one inspired by HTML forms and markdown file formats. It is called pagefront, and it is the fourth, and the last kind of component that may be held within the Symp trees. For the details about it, the reader is kindly asked to refer to relevant documentation accompanying this distribution.

## 3. example

The following example builds up a small user interface depicting a number and two buttons for incrementing and decrementing the number:

```
(NODE
    (NAME uiExample)
    (CONTENT
        (USING (ALIAS ui uiExample) (ALIAS sl stdlib))
        (PAGE
            (HDR1 ... "Increment/Decrement Value")
            (PARAG p (ui/init PARAMS))
            (HBLIST ...
                (BUTTON incr "increment" (GET (PARAG p (sl/incr p))) (TARGET p))
                (BUTTON decr "decrement" (GET (PARAG p (sl/decr p))) (TARGET p)))))
    
    (BRANCHES
        (NODE
            (NAME init)
            (CONTENT
                (REWRITE
                    (RULE ("init" ()) 0))))))
```

Let's store this example into a file named `uiExample`. When we run the Symp interpreter on the file, the UI opens, shows the number `0`, and waits for our action of pushing the buttons. The initial number `0` is constructed by the node `init` which is made accessible by `(ALIAS ui uiExample)` under `USING` section. The call to this example is supposed to be without parameters which gives us an empty list. The `init` node turns this input to `0`. Further button interactions reload the paragraph `p` with the content recalculated by `sl/incr` and `sl/decr`, incrementing or decrementing the current value.

## 4. conclusion

Symp defines an execution model where program structure, computation, and user interfaces are integrated through a uniform symbolic tree. Through this approach, Symp provides a foundation for constructing application runtimes and interactive tools without leaving the symbolic domain. Its deterministic referencing, distributed responsibility model, and filesystem-aligned structure allow incremental system growth.

