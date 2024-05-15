Hyperbole
=========

Create dynamic HTML applications in server-side Haskell. Inspired by HTMX


Obligatory Counter Example
--------------------------


```haskell
page :: (Hyperbole :> es, Concurrent :> es) => TVar Int -> Page es Response
page var = do
  hyper $ counter var

  load $ do
    pure $ col (pad 20 . gap 10) $ do
      el h1 "Counter"
      viewId Counter (viewCount 0)


data Counter = Counter
  deriving (Show, Read, Param)
instance HyperView Counter where
  type Action Counter = Count


data Count
  = Increment
  | Decrement
  deriving (Show, Read, Param)


counter :: (Hyperbole :> es, Concurrent :> es) => TVar Int -> Counter -> Count -> Eff es (View Counter ())
counter var _ Increment = do
  n <- modify var $ \n -> n + 1
  pure $ viewCount n
counter var _ Decrement = do
  n <- modify var $ \n -> n - 1
  pure $ viewCount n


viewCount :: Int -> View Counter ()
viewCount n = col (gap 10) $ do
  row id $ do
    el (bold . fontSize 48 . border 1 . pad (XY 20 0)) $ text $ pack $ show n
  row (gap 10) $ do
    button Increment Style.btn "Increment"
    button Decrement Style.btn "Decrement"
```

