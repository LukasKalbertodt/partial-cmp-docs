`PartialOrd` Documentation
==========================

[Strict partial ordering relation](https://en.wikipedia.org/wiki/Partially_ordered_set#Strict_and_non-strict_partial_orders).

This trait extends the partial equivalence relation provided by `PartialEq` (`==`) with `partial_cmp(a, b) -> Option<Ordering>`, which is a [trichotomy](https://en.wikipedia.org/wiki/Trichotomy_(mathematics)) of the ordering relation when its result is `Some`:

* if `a < b` then `partial_cmp(a, b) == Some(Less)`
* if `a > b` then `partial_cmp(a, b) == Some(Greater)`
* if `a == b` then `partial_cmp(a, b) == Some(Equal)`

and the absence of an ordering between `a` and `b` when `partial_cmp(a, b) == None`. Furthermore, this trait defines `<=` as `a < b || a == b` and `>=` as `a > b || a == b`.

The comparisons must satisfy, for all `a`, `b`, and `c`:

- _transitivity_: if `a < b` and `b < c` then `a < c`
- _duality_: if `a < b` then `b > a`

The `lt` (`<`), `le` (`<=`), `gt` (`>`), and `ge` (`>=`) methods are implemented in terms of `partial_cmp` according to these rules. The default implementations can be overridden for performance reasons, but manual implementations must satisfy the rules above.

From these rules it follows that `PartialOrd` must be implemented symmetrically and transitively: if `T: PartialOrd<U>` and `U: PartialOrd<V>` then `U: PartialOrd<T>` and `T: PartialOrd<V>`.

The following corollaries follow from transitivity of `<`, duality, and from the definition of `<` et al. in terms of 
the same `partial_cmp`:

- irreflexivity of `<`: `!(a < a)`
- transitivity of `>`: if `a > b` and `b > c` then `a > c`
- asymmetry of `<`: if `a < b` then `!(b < a)`
- antisymmetry of `<`: if `a < b` then `!(a > b)`

Stronger ordering relations can be expressed by using the `Eq` and `Ord` traits, where the `PartialOrd` methods provide:

- a [non-strict partial ordering relation](https://en.wikipedia.org/wiki/Partially_ordered_set#Strict_and_non-strict_partial_orders) if `T: PartialOrd + Eq`
- a [total ordering relation](https://en.wikipedia.org/wiki/Total_order) if `T: Ord`
