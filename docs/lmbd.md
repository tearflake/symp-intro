---
layout: docs
---
# Lambda Calculus Awakening in Symp

```
(NODE
    (NAME lmbd)
    (CONTENT
        (USING lmbd)
        (RUNGRAPH
            (EDGE
                (SOURCE BEGIN)
                (INSTR
                    (ASGN checked (syntax PARAMS))
                    (TEST (nth checked 0) "ERROR"))
                    (ASGN RESULT ("parsing error" (nth checked 1)))
                (TARGET END))
             
            (EDGE
                (SOURCE BEGIN)
                (INSTR
                    (ASGN RESULT (runtime (semantics checked))))
                
                (TARGET END))))

    (BRANCH
        (NODE
            (NAME runtime)
            (CONTENT
                (REWRITE
                    (RULE (READ ("<I>" x)) (WRITE x))
                    (RULE (READ (("<K>" x) y)) (WRITE x))
                    (RULE (READ ((("<S>" x) y) z)) (WRITE ((x z) (y z)))))))
        
        (NODE
            (NAME semantics)
            (CONTENT
                (REWRITE
                    (RULE (READ ("lmbd" x x)) (WRITE "<I>"))
                    (RULE (READ ("lmbd" x (E1 E2))) (WRITE ("<S>" ("lmbd" x E1) ("lmbd" x E2))))
                    (RULE (READ ("lmbd" x y)) (WRITE ("<K>" y))))))
        
        (NODE
            (NAME syntax)
            (CONTENT
                (GRAMMAR
                    (RULE START term)
                    
                    (RULE term (LIST "lmbd" (LIST term (LIST term ()))))
                    (RULE term (LIST term (LIST term ())))
                    (RULE term var)
                    
                    (RULE var (ATOM letter varNext))
                    (RULE var (ATOM letter ()))

                    (RULE varNext (ATOM letterDigit varNext))
                    (RULE varNext (ATOM letterDigit ()))
                    
                    (RULE letter "A")
                    ...
                    (RULE letter "Z")
                    (RULE letter "a")
                    ...
                    (RULE letter "z")

                    (RULE letterDigit letter)
                    (RULE letterDigit "0")
                    ...
                    (RULE letterDigit "9"))))))
                    
```

