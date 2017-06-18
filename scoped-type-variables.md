A Haskell file named `scoped-type-variables.hs` with the following contents

```haskell
{-# LANGUAGE ScopedTypeVariables #-}

> func :: Show a => a -> String
> func x = show (x :: a)
```

produces the following errors when compiled,

```
scoped-type-variables.hs:4:10: error:
    • Could not deduce (Show a0) arising from a use of ‘show’
      from the context: Show a
        bound by the type signature for:
                   func :: Show a => a -> String
        at scoped-type-variables.hs:3:1-29
      The type variable ‘a0’ is ambiguous
      These potential instances exist:
        instance (Show b, Show a) => Show (Either a b)
          -- Defined in ‘Data.Either’
        instance Show Ordering -- Defined in ‘GHC.Show’
        instance Show Integer -- Defined in ‘GHC.Show’
        ...plus 23 others
        ...plus 44 instances involving out-of-scope types
        (use -fprint-potential-instances to see them all)
    • In the expression: show (x :: a)
      In an equation for ‘func’: func x = show (x :: a)

scoped-type-variables.hs:4:16: error:
    • Couldn't match expected type ‘a1’ with actual type ‘a’
      ‘a’ is a rigid type variable bound by
        the type signature for:
          func :: forall a. Show a => a -> String
        at scoped-type-variables.hs:3:9
      ‘a1’ is a rigid type variable bound by
        an expression type signature:
          forall a1. a1
        at scoped-type-variables.hs:4:21
    • In the first argument of ‘show’, namely ‘(x :: a)’
      In the expression: show (x :: a)
      In an equation for ‘func’: func x = show (x :: a)
    • Relevant bindings include
        x :: a (bound at scoped-type-variables.hs:4:6)
        func :: a -> String (bound at scoped-type-variables.hs:4:1)
```

The solution is either of the following:

  * Explicit quantification: `func :: forall a. Num a => a -> String`
  * Explicitly annotated argument: `func (x :: a) = show (x :: a)`
  * [Simply remove the type annotation on `x :: a` since this case doesn't need it; but this is a contrived example as it is.]

The error message could notice that we're reusing the type variable `a` but without any meaningful scoping. While it's possible the author means to introduce an entirely new type variable with the same name, it's more likely that they forgot to use one of these solutions. Why is it likely? Because the code is not type-checking as-is.

An even more impressive type error would actually *try* to see if introducing the scoping makes the code type-check and report back.
