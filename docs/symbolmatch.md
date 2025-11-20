# symbolmatch

> **[about document]**  
> Specification of *Symbolmatch*, a symbolic processing framework
>
> **[intended audience]**  
> Advanced programmers
> 
> **[abstract]**  
> Symbolmatch is a minimal parser combinator framework built on S-expressions. It defines grammars using the same notation it parses, making it self-describing and bootstrappable. By translating S-expressions into a meta-representation and applying deterministic, PEG-like semantics, Symbolmatch achieves predictable, structured parsing with no backtracking. Its compact design supports modular grammars and self-hosting within a unified formalism, ensuring that parsed expressions are suitable for further symbolic processing.

## table of contents

- [1. introduction](#1-introduction)  
- [2. theoretical background](#2-theoretical-background)
    - [2.1. formal syntax](#21-formal-syntax)
    - [2.2. informal semantics](#22-informal-semantics)  
- [3. examples](#3-examples)  
- [4. conclusion](#4-conclusion)  

## 1. introduction

Symbolmatch is a minimal parser combinator framework that operates on S-expressions. It defines grammars in the same notation that it parses, making the system self-describing and bootstrappable. Parsing is expressed as symbolic pattern matching, where rules match the meta-representation of S-expressions rather than raw text.

The framework adopts deterministic Parsing Expression Grammar (PEG) semantics without backtracking, ensuring predictable behavior and clear rule evaluation. Despite its minimal core, Symbolmatch supports recursion, modular composition, and self-hosting.

## 2. theoretical background

Symbolmatch combines elements of S-expression syntax and parsing production rules. It defines a small set of rules from which grammars for parsing formatted S-expressions can be built.

To make the parsing possible, one simple trick is being used. Prior to parsing, the input S-expression is being converted to its meta-form. The meta-form is inspired by the `cons` instruction from the Lisp family. Analogously to `cons`, Symbolmatch is using `LIST` and `ATOM` expressions, for constructing lists and atoms, respectively. That way, any S-expression is possible to be expressed by meta-rules that simply pattern matches against the meta S-expressions.

Each meta-rule is in fact a fixed length context free grammar rule that, on the meta level, is able to express even variable length meta S-expressions. However, following the minimalism we are set to fulfill, the rules are interpreted as parsed expression grammar rules, turning them to nondeterministic ordered choice matching expressions. We consciously choose to omit the backtracking to keep the minimalism constraints.

### 2.1. formal syntax

These are the syntax rules of Symbolmatch:

```
<symbolmatch> := <grammar>

<grammar> := (GRAMMAR <elem>+)

<elem> := (RULE <IDENTIFIER> <metaExpr>)

<metaExpr> := (LIST <metaExpr> <metaExpr>)
            | <metaAtom>
         
<metaAtom> := (ATOM <ATOMIC> <metaAtom>)
            | <atomic>

<atomic> := <ATOMIC>
          | ()
```

We apply the same syntax definition nomenclature as noted in Symp specification document.

### 2.2 informal semantics

Symbolmatch grammars are interpreted with PEG semantics without backtracking. Each rule matches deterministically, and choice is ordered (the first matching branch wins).

#### parsing mechanism

Before the algorithm starts parsing, the input expression is being translated into meta-expression in a translation phase. Any valid S-expression has a corresponding meta-representation. Meta-lists are right-recursive nested to form lists using `LIST` keyword, just like meta-atoms are right-recursive nested to form atoms using `ATOM` keyword. Meta-lists and meta-atoms recursive definitions are terminated by an empty list.

Algorithm then proceeds with the matching phase. As expected, the parsing entry point is the `START` rule that branches into matching depth. Finally, when the matching phase is done, the parser returns either input expression, or an error position.

Built-in constants are:

- `ATOMIC`: matches any S-expression atom.  
- `ANY`: matches any S-expression.  
- `()`: terminates `LIST` or `ATOM` meta-expressions

In addition to the above constants, it is possible to write double quote embraced terminals in grammars. This represents a shortcut that automatically unfolds into nested `ATOM` meta-expression on character split basis, ready to match against translated meta-input.

#### relating grammars in a parent structure

After declaring `(USING...)` sections, upon calling the used components, the components are automatically called with a parameter that equals to the current parsed sub-expression. That means that we never write parameters with the calls. 

#### error reporting

If no error occurred during parsing, the exact input expression is returned as a result of the computation. However, if a parsing error is encountered, the error reporting appears structural: Symbolmatch returns the **furthest path of indexes into the S-expression tree** that led to the error.

## 3. examples

### example 1: list of atoms

Grammar:

```
(NODE
    (NAME ex1)
    (CONTENT
        (GRAMMAR
            (RULE START atoms)
           
            (RULE atoms (LIST atom atoms))
            (RULE atoms (LIST atom ()))
            
            (RULE atom ATOMIC))))
```

Input S-expression:

```
(foo bar)
```

Result:

```
["foo", "bar"]
```

### example 2: nested lists

Grammar:

```
(NODE
    (NAME ex2)
    (CONTENT
        (GRAMMAR
            (RULE START (LIST "pairlist" pairs))
            
            (RULE pairs (LIST pair pairs))
            (RULE pairs (LIST pair ()))
            
            (RULE pair (LIST "pair" (LIST atom (LIST atom ()))))
            (RULE atom ATOMIC))))
```

Input:
```
(pairlist
    (pair foo bar)
    (pair alpha beta)
    (pair one two))
```

Result
```
["pairlist", ["pair", "foo", "bar"], ["pair", "alpha", "beta"], ["pair", "one", "two"]]
```

### example 3: custom atoms and computing section

Grammar:

```
(NODE
    (NAME ex3)
    (CONTENT
        (USING ex3/binNum)
        (GRAMMAR
            (RULE START binTree)
            
            (RULE binTree (LIST "bintree" (LIST binTree (LIST binTree ()))))
            (RULE binTree (LIST "leaf" (LIST binNum ())))))

    (BRANCHES
        (NODE
            (NAME binNum)
            (COMPUTE
                (GRAMMAR
                    (RULE START digits)
                    
                    (RULE digits (ATOM digit digits))
                    (RULE digits (ATOM digit ()))
                    
                    (RULE digit "0")
                    (RULE digit "1"))))))
```

Input S-expression:

```
(bintree
    (bintree
        (leaf 01)
        (leaf 10))
        
    (leaf 11))
```

Result:

```
["bintree", ["bintree", ["leaf", "01"], ["leaf", "10"]], ["leaf", "11"]]
```

However, the input S-expression:

```
(bintree
    (leaf 01)
    (leaf (leaf 11)))
```

fails because it doesn't follow the binary tree grammar. The error report returns:

```
["ERROR", [2, 1]]
```

This path identifies the location inside the nested expression where the parser expects a number.

## 4. conclusion

Symbolmatch demonstrates how a minimal set of S-expression-based rules can define and parse complex grammars. Its design emphasizes minimal core rule types with clear semantics and structural error reporting via index paths.

