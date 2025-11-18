---
layout: docs
---

# "hello world" From Symp

## TOC

- [1. Symp "hello world" example](#1-symp-hello-world-example)
- [2. The Foundations](#2-the-foundations)
  - [2.1. Symbolmatch](#21-symbolmatch)
  - [2.2. Symbolverse](#22-symbolverse)
  - [2.3. Symbolprose](#23-symbolprose)
  - [2.4. Symbolfront](#24-symbolfront)
- [3. Complete Example](#3-complete-example)
- [4. Conclusion](#4-conclusion)

## 1. Symp "hello world" example

**"hello world" example in a symbolic processing framework**

Symp is a modular symbolic programming environment designed to define, transform, and execute domain-specific languages using a unified representation. Instead of hard-coding syntax, semantics, and runtime behavior in a general-purpose language, Symp lets you build them declaratively, layer by layer, using interoperable components. It may find its use in AI symbolic processing, building logic solvers and theorem provers from its components, creating domain specific frameworks for solving various problems, or exploring declarative programming basics.

The purpose of this document is to write a comprehensive "hello world" example. We try to give an insight on how complex programs may be constructed in Symp framework. Although the simplest "hello world" program in Symp - choosing any of the offered methods - would be of much less complexity than the one in our ending section, we strive to establish the entire infrastructure that could be reused later to build more complex programs. The other purpose of this document is to tickle a bit of imagination and curiosity of the reader.

We start from the foundations of Symp, continuing to present the "hello world" programs in each of Symp programming components Symbolmatch, Symbolverse, Symbolprose, and Symbolfront, ending with the composite "hello world" example showing all of the described components united in action.

## 2. The Foundations

**designing form, meaning, execution, and intent**

#### Purpose

The fundamental purpose of Symp is to provide means to connect different programming tools used for performing different actions answering different requests. The frameworks are built from components spanning over classical tree type structures defined by the presented grammar. Each node in a tree is named, can have its content component, and may or may not branch further. To say something about the components - which we will cover in the following sections - they reflect the process of building programming frameworks through defining form as syntax, meaning as semantics, and runtime behavior as execution.

#### Grammar Overview

```
<start> := <tree>

<tree> := (NODE
              (NAME <ATOMIC>)
              (CONTENT (USING <ATOMIC>+)? <COMPONENT>)?
              (BRANCHES <tree>+)?
          )
```

### 2.1. Symbolmatch

**syntax checking**

#### Purpose

Each programming framework we want to define can be required to satisfy some form of our interest. This form is written as syntax grammar rules, and may help catching errors before proceeding with further processing. Also, knowing that an expression satisfies given form, we can exclude a considerable amount of later checking, relieving us of later tedious work. In expression parsing in Symp, we are dealing with standard S-expressions. This expectation boils down the syntax check to mere pattern matching which we perform by normalized rules behaving similarly to parsing expression grammar (PEG) rules. Seek for more details in accompanying documentation.

#### Grammar Overview

```
<start> := <grammar>

<grammar> := (GRAMMAR <elem>+)

<elem> := (RULE <IDENTIFIER> <metaExpr>)

<metaExpr> := (LIST <metaExpr> <metaExpr>)
            | <metaAtom>
         
<metaAtom> := (ATOM <ATOMIC> <metaAtom>)
            | <atomic>

<atomic> := <ATOMIC>
          | ()
```

#### Example

```
(GRAMMAR
    (RULE <start> (LIST "hello" (LIST "world" ()))))
```

Input:

```
(hello world)
```

Output

```
(hello world)
```

### 2.2. Symbolverse

**semantic transformation**

#### Purpose

After passing the syntax check, it may be required to translate the expression to a form suitable for final execution. This so-called semantic transformation is done in Symp using rules that represent a term rewriting system. The term rewriting is a complex matter on its own. It is based on replacing expected patterns by new expressions. The process repeats recursively until no more of the expected patterns occur in the whole expression. Seek for more details in accompanying documentation.

#### Grammar Overview

```
<start> := <ruleset>

<ruleset> := (REWRITE <elem>+)

<elem> := (RULE (READ <S-EXPR>) (WRITE <S-EXPR>))
```

#### Example

```
(REWRITE
    (RULE
        (READ ("greet" x))
        (WRITE ("hello" x))))
```

Input:

```
(greet world)
```

Output:

```
(hello world)
```

### 2.3. Symbolprose

**runtime execution**

#### Purpose

When we finally get the expression suitable for execution, we interpret it in a declarative environment resembling graphs known from graph theory. But our graphs hold instructions attached to edges. These instructions deal with states, assigning values to variables. Graphs may also be cyclic, requiring testing variables for expected values to break out of loops. Variable testing may also be used to control branching to one of the continuing nodes. Seek for more details in accompanying documentation.

#### Grammar Overview

```
<start> := <graph>

<graph> := (RUNGRAPH <elem>+)

<elem> := (EDGE (SOURCE <ATOMIC>) (INSTR <instr>+)? (TARGET <ATOMIC>))

<instr> := (TEST <ANY> <ANY>)
         | (ASGN <ATOMIC> <ANY>)
```

#### Example

```
(RUNGRAPH
    (EDGE
        (SOURCE BEGIN)
        (INSTR
            (ASGN RESULT ("hello" PARAMS)))
        
        (TARGET END)))
```

Input:

```
world
```

Output:

```
(hello world)
```

### 2.4. Symbolfront

**presentation**

#### Purpose

At last, we would want to present the computation we performed in some kind of user interface. This is where we use an environment that shows information, and may interact with a user. The user interface is minimalistic, and inspired by HTML output with a form submitting system. Seek for more details in accompanying documentation.

#### Grammar Overview

```
<start> := <gui>

<gui> := (PAGE <container> <ATOMIC>*)
        | (REF <ATOMIC>+)

<container> := (EXPIRES <DURATION> <body>)
             | <body>

<body> := (FRAMENS
              (NORTH <NAME> <HEIGHT> <gui>)?
              (CENTER <NAME> <gui>)
              (SOUTH <NAME> <HEIGHT> <gui>)?
          )
        | (FRAMEWE
              (WEST <NAME> <WIDTH> <gui>)?
              (CENTER <NAME> <MINWIDTH> <gui>)
              (EAST <NAME> <WIDTH> <gui>)?
          )
        | <formElem>+

<formElem> := (HDR1 <ATOMIC>)
            | (HDR2 <ATOMIC>)
            | (HDR3 <ATOMIC>)
            | (HDR4 <ATOMIC>)
            | (HDR5 <ATOMIC>)
            | (HDR6 <ATOMIC>)
            | (PARAG <ATOMIC>)
            | (ULIST <formElem>+)
            | (OLIST <formElem>+)
            | (CODE <ATOMIC>)
            | (CHECK <NAME> <formElem>+)
            | (RADIO <NAME> <formElem>+)
            | (EDIT <NAME> <ROWS> <ATOMIC>?)
            | (HBLIST (BUTTON <NAME> <ATOMIC> (PUT <ATOMIC>+)? (ERR <ATOMIC>+)? (GET <ATOMIC>+) <ATOMIC>)+)
            | (VBLIST (BUTTON <NAME> <ATOMIC> (PUT <ATOMIC>+)? (ERR <ATOMIC>+)? (GET <ATOMIC>+) <ATOMIC>)+)
```

#### Example

```
(PAGE
    (PARAG "hello world"))
```

Runs as a stand-alone application and outputs the "hello world" text on the screen.

## 3. Complete Example

**a comprehensive "hello world" example**

Finally we skimmed over enough building blocks to combine them all into a compound "hello world" example covering all the enumerated components.

```
(NODE
    (NAME greeting)
    (CONTENT
        (USING greeting)
        (PAGE
            (PARAG
                (runtime
                    (semantics
                        (syntax
                            (greet world)))))))
    (BRANCHES
        (NODE
            (NAME syntax)
            (CONTENT
                (GRAMMAR
                    (RULE START (LIST "greet" (LIST ATOMIC ()))))))
        
        (NODE
            (NAME semantics)
            (CONTENT
                (REWRITE
                    (RULE
                        (READ ("greet" x))
                        (WRITE ("greeting" x))))))
        
        (NODE
            (NAME runtime)
            (CONTENT
                (USING stdlib)
                (RUNGRAPH
                    (EDGE
                        (SOURCE BEGIN)
                        (INSTR
                            (TEST "greeting" (nth (PARAMS 0)))
                            (ASGN RESULT (strcat ("hello " (nth (PARAMS 1))))))
                        
                        (TARGET END)))))))
```

This example may seem as overly exaggerated for outputting the simple `hello world` on the screen, but the pattern in which it is written actually may already serve as a skeleton for more serious domain specific programming framework. 

## 4. Conclusion

**the machine**

In this short exposure, we saw how to form the component tree, and we saw each of the available components (Symbolmatch, Symbolverse, Symbolprose, and Symbolfront) in action of executing the "hello world" example. Each component in the tree structure may explicitly refer to each other, allowing us to choose the right tools for the right tasks. The available components include the syntax checking, semantic transformations, runtime execution, and user interface, while their arbitrary combination may form complex structures capable of performing any computation of our interest.
