This one liner:

       simplify x = y == y where y = foo x

causes GHC 8.0.1 to produce:

    a.hs:2:14: error:
        • Ambiguous type variable ‘a0’ arising from a use of ‘==’
          prevents the constraint ‘(Eq a0)’ from being solved.
          Probable fix: use a type annotation to specify what ‘a0’ should be.
          These potential instances exist:
            instance Eq Ordering -- Defined in ‘GHC.Classes’
            instance Eq Integer
              -- Defined in ‘integer-gmp-1.0.0.1:GHC.Integer.Type’
            instance Eq a => Eq (Maybe a) -- Defined in ‘GHC.Base’
            ...plus 22 others
            ...plus two instances involving out-of-scope types
            (use -fprint-potential-instances to see them all)
        • In the expression: y == y
          In an equation for ‘simplify’:
              simplify x
                = y == y
               where
                    y = foo x

    a.hs:2:31: error: Variable not in scope: foo :: [Int] -> t

Of course, the issue is simply that 'foo' is undefined, but that message comes afterwards and you end up looking at a
very confusing message with no idea what's going on.

Note that this came from a real example with lots of other code around where I happened to misspell `foo`. It took quite a while
to figure out what was going on, since I typically only look at the first error message first, which has no mention of `foo` in it!
