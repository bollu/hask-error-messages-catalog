Specifically, I'm working in a small EDSL that uses lots of small
expressions composed together with a repurposed (.) operator.  The
pretty printer layout causes the context section of each type error to
become huge, for instance:

```
Derive/Solkattu/MridangamScore.hs:110:60:
    Couldn't match type ‘Derive.Solkattu.Sequence.Note
                           (Derive.Solkattu.Solkattu.Note
                              Derive.Solkattu.MridangamDsl.Stroke)’
                   with ‘[Derive.Solkattu.Sequence.Note
                            (Derive.Solkattu.Solkattu.Note
                               Derive.Solkattu.MridangamDsl.Stroke)]’
    Expected type: [Sequence]
      Actual type: [Derive.Solkattu.Sequence.Note
                      (Derive.Solkattu.Solkattu.Note
                         Derive.Solkattu.MridangamDsl.Stroke)]
    In the second argument of ‘($)’, namely
      ‘nadai 7
       $ repeat 2 kook
         . od
           . __
             . k
               . d
                 . __
                   . k
         [ keeps walking to the right for many many lines ]
    In the second argument of ‘($)’, namely
      ‘korvai adi
       $ nadai 7
         $ repeat 2 kook
           . od
             [ again ]
    In the second argument of ‘($)’, namely
       [ and again ]
```

You can probably easily reproduce this by making a lot of little
expressions and putting an error in someplace.  What I think it should
be is put more things on a horizontal line, or maybe cap the
expression at a certain number of lines.  Or maybe the "zooming out"
contexts could replace the previous expression with {- previous
expression -} to emphasize the difference.
