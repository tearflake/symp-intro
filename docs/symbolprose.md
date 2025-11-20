# symbolprose

> **[about document]**  
> Specification of *Symbolprose*, a symbolic processing framework
>
> **[intended audience]**  
> Advanced programmers
> 
> **[abstract]**  
> Symbolprose is a minimalist graph-based language where programs are expressed as nodes and edges with simple instructions. It serves as both a compact virtual machine model and a compilation target, emphasizing clarity, recursion, and fast execution through S-expression syntax.

## table of contents

- [1. introduction](#1-introduction)  
- [2. theoretical background](#2-theoretical-background)  
    - [2.1. formal syntax](#21-formal-syntax)  
    - [2.2. informal semantics](#22-informal-semantics)  
- [3. examples](#3-examples)  
- [4. conclusion](#4-conclusion)  

## 1. introduction

Symbolprose is a minimalist programming language designed to serve as both a virtual machine target and a concise medium for expression. Its purpose is twofold:  
1. To provide a low-level but structured execution model, allowing efficient compilation from higher-level languages.  
2. To preserve simplicity in syntax and semantics, lowering the barrier of adoption and making the execution model easy to reason about.  

At its core, Symbolprose embodies the philosophy that computational constructs should be both minimal and compositional. By expressing computation as graphs of nodes and edges with simple instructions, Symbolprose tries to find a balance between being close to the machine while still remaining human-readable.  

This document specifies the language, its syntax, semantics, and illustrates its use through a few examples.  

## 2. theoretical background

Symbolprose is intended to represent a fast-executing virtual machine and a compiling target from higher level languages. It provides a functionality trade-off between lower level and higher level programming constructs. The complexity of Symbolprose is intentionally kept simple to take advantage of the potential for its core being cached by the underlying processor unit. At the same time, simple grammar and semantics lower a barrier for a possible programming adoption.

### 2.1. formal syntax

These are the syntax rules of Symbolmatch:

```
<symbolprose> := <rungraph>

<rungraph> := (RUNGRAPH <element>+)

<element> := (EDGE (SOURCE <ATOMIC>) (INSTR <instruction>+)? (TARGET <ATOMIC>))

<instruction> := (TEST <ANY> <ANY>)
               | (ASGN <ATOMIC> <ANY>)
```

We apply the same syntax definition nomenclature as noted in Symp specification document.

### 2.2 informal semantics

Using S-expressions, Symbolprose defines its model of execution as instruction control flow based on graphs. Graphs consist of nodes and edges which connect nodes. Each node can have multiple edges, each representing a path that a program decides to take during its execution.

### programs

In Symbolprose, graphs represent programs. Here, special nodes `BEGIN` and `END` denote entry and exit point to a program depicted by a graph. In between, there can be arbitrary many other nodes with their own edges, drawing possible program execution lines from the `BEGIN` node to the `END` node.

### instructions

Edges are places where actual execution of instructions are done. Each edge `EDGE` defines `SOURCE` and `TARGET` nodes, while the edge `INSTR` section holds the instructions to execute in sequence. There are two types of instructions: `ASGN` and `TEST`.

`ASGN` instructions take two parameters: a variable name and a S-expression value that is going to be assigned to the variable. The variables are created when they are used for the first time. Before assignment of S-expressions to variables, S-expressions are evaluated, substituting contained variables for their values. `ASGN` instructions always succeed and the execution flow continues to the next instruction in the sequence.

`TEST` instructions also take two parameters: expressions which are about to be compared for being equal. Also using the variable substitution, if the parameters are equal to the last consisting atom, the execution flow continues to the next instruction in the sequence. If the parameters are not equal, the branching happens, and the next edge from the same source node is selected for execution. There is no backtracking which cancels the `ASGN` effects from the past instruction sequences. Instead, the control flow is just switched to the beginning of the next edge sequence. When all the edges from the same source node fail to execute, execution continues to the next parent edge in the sequence. When we reach the `BEGIN` node in this process, after all the execution branches failed, an error is reported.

Finally, when all the instructions from the current `INSTR` sequence succeed, the program control flow continues to the `TARGET` node, trying to execute further edges that branch from the `TARGET` node. We can consider each instruction sequence labeled by the `SOURCE` section name, while the `TARGET` section name serves as a kind of "goto" command mechanism to another instruction sequence. Naturally, it is possible to form loops targeting the past nodes, but we have to carefully set up the `TEST` conditions to branch out if we don't want an infinite loop to appear.

### internal variables

There are two predefined variables in each graph: `PARAMS` and `RESULT`. When executing a graph, we pass an input which is assigned to the `PARAMS` variable at the beginning of the execution. During the execution, we may assign a value to the `RESULT` builtin variable. On the end of execution, the contents of the `RESULT` builtin variable is returned as an output of the program.

### relating programs in a parent structure

After declaring `(USING...)` sections, upon calling the used components, we may pass a number of parameters. The parameters are forwarded to the component which is then responsible for their processing. When the called component is done with processing, it returns a value which takes the place of the call.

## 3. examples

Below are a few illustrative examples of Symbolprose programs. 

### example 1: echo

This program simply echoes its input:

```
(NODE
    (NAME ex1)
    (CONTENT
        (RUNGRAPH
           (EDGE
                (SOURCE BEGIN)
                (INSTR
                    (ASGN x PARAMS)
                    (ASGN RESULT x))
                (TARGET END)))))
```

### example 2: ping-pong

The program outputs `pong` when the input is `ping`, otherwise it raises an error:

```
(NODE
    (NAME ex2)
    (CONTENT
        (RUNGRAPH
           (EDGE
                (SOURCE BEGIN)
                (INSTR
                    (TEST PARAMS "ping")
                    (ASGN RESULT "pong"))
                (TARGET END)))))
```

Note that constants are represented as strings.

### example 3: hi-bye

Program answers `greeting` on input `hi`, `farewell` on input `bye`, or `unknown` otherwise:

```
(NODE
    (NAME ex3)
    (CONTENT
        (RUNGRAPH
            (EDGE
                (SOURCE BEGIN)
                (INSTR
                    (TEST PARAMS "hi")
                    (ASGN RESULT "greeting"))
                (TARGET END))

            (EDGE
                (SOURCE BEGIN)
                (INSTR
                    (TEST PARAMS "bye")
                    (ASGN RESULT "farewell"))
                (TARGET END))

            (EDGE
                (SOURCE BEGIN)
                (INSTR (ASGN RESULT "unknown"))
                (TARGET END)))))
```

### example 4: foo-bar

The program outputs `alpha` on `foo` input, `beta` on `bar` input, or `unknown` otherwise:

```
(NODE
    (NAME ex4)
    (CONTENT
        (RUNGRAPH
            // Step 1: Load input
            (EDGE
                (SOURCE BEGIN)
                (INSTR (ASGN x PARAMS))
                (TARGET check))

            // Step 2: Check for foo
            (EDGE
                (SOURCE check)
                (INSTR (TEST x "foo"))
                (TARGET match-foo))

            // Step 3: Check for bar
            (EDGE
                (SOURCE check)
                (INSTR (TEST x "bar"))
                (TARGET match-bar))

            // Step 4: Fallback if no match
            (EDGE
                (SOURCE check)
                (TARGET fallback))

            // Step 5: Match case foo
            (EDGE (SOURCE match-foo)
                (INSTR (ASGN RESULT "alpha"))
                (TARGET END))

            // Step 6: Match case bar
            (EDGE
                (SOURCE match-bar)
                (INSTR (ASGN RESULT "beta"))
                (TARGET END))

            // Step 7: Default case
            (EDGE
                (SOURCE fallback)
                (INSTR (ASGN RESULT "unknown"))
                (TARGET END)))))
```

### example 5: is-element-of

This example returns `true` if the first parameter is contained within second parameter list, `false` otherwise:

```
(NODE
    (NAME ex5)
    (USING stdlib)
    (CONTENT
        (RUNGRAPH
            // Load variables
            (EDGE
                (SOURCE BEGIN)
                (INSTR
                    (ASGN element (stdlib/nth (PARAMS "0")))
                    (ASGN list    (stdlib/nth (PARAMS "1"))))
                    
                (TARGET loop))
            
            // Loop condition: if input is ()
            (EDGE
                (SOURCE loop)
                (INSTR
                    (TEST list ())
                    (ASGN RESULT "false"))
                    
                (TARGET END)) // done
            
            // Loop condition: if element is found
            (EDGE
                (SOURCE loop)
                (INSTR
                    (TEST element (stdlib/first list))
                    (ASGN RESULT "true"))
                    
                (TARGET END)) // done
            
            // Fallback: process next element in list
            (EDGE
                (SOURCE loop)
                (INSTR (ASGN list (stdlib/rest list)))
                (TARGET loop))))) // Continue looping
```

### example 6: reverse list

This program reverses a list:

```
(NODE
    (NAME ex6)
    (CONTENT
        (RUNGRAPH

            // Load Input and initialize accumulator
            (EDGE
                (SOURCE BEGIN)
                (INSTR
                    (ASGN input PARAMS)
                    (ASGN acc ()))
                    
                (TARGET loop))

            // Loop condition: if input is ()
            (EDGE
                (SOURCE loop)
                (INSTR (TEST input ()))
                (TARGET done)) // go to done
            
            // Fallback: Process one element
            (EDGE
                (SOURCE loop)
                (INSTR
                    (ASGN head (stdlib/first input))
                    (ASGN tail (stdlib/rest input))
                    (ASGN acc (stdlib/prepend (head acc)))
                    (ASGN input tail))
                    
                (TARGET loop)) // Continue looping

            // Final step: store reversed RESULT
            (EDGE
                (SOURCE done)
                (INSTR (ASGN RESULT acc))
                (TARGET END)))))
```

### example 7: factorial function

Recursive factorial function - on input `5`, output is `120`:

```
(NODE
    (NAME ex7)
    (CONTENT
        (USING ex7)
        (EDGE
            (SOURCE BEGIN)
            (INSTR (ASGN RESULT (ex7/fact PARAMS)))
            (TARGET END)))

    (BRANCHES
        (NODE
            (NAME fact)
            (CONTENT
                (USING fact)
                (RUNGRAPH
                    // Base case: if PARAMS == 0 -> return 1
                    (EDGE
                        (SOURCE BEGIN)
                        (INSTR
                            (TEST PARAMS "0")
                            (ASGN RESULT "1"))
                            
                        (TARGET END))

                    // Recursive case
                    (EDGE
                        (SOURCE BEGIN)
                        (INSTR
                            (ASGN n PARAMS)
                            (ASGN n1 (stdlib/sub (n "1")))
                            (ASGN rec (fact n1))
                            (ASGN RESULT (stdlib/mul (n rec))))
                            
                        (TARGET END))))

```

## 4. conclusion

Symbolprose provides a foundation for computation that is both graph-structured and minimalistic. Its simplicity in syntax makes it a suitable compilation target, while its semantics offer a precise and predictable model of execution. By embracing graph-based control flow, Symbolprose enables flexible yet analyzable program representation.  

