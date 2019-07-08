---
published: true
title: "Abstract Data Types: The Other ADTs"
hidden: true
---

Abstract Data Types are a crucial construct in everyday programming. They constitute the vast majority of types which values in a program might have (possibly _all_ types which values might have, depending on your choice of definitions). For the sake of brevity, I will refer to Abstract Data Types in this article as ADTs, though not to be confused with _Algebraic_ Data Types, covered in [another article](https://ethanbell.me/algebraic-data-types/)).

ADTs can simply be described as “all the things that describe a type”. For instance, the ADT of `int` in C is the description of what values are valid for an `int` (whole numbers in the range of `INT_MIN` to `INT_MAX`) and the things an `int` can do (for instance, an `int` can be incremented, or decremented, or added to another `int`, or compared for equality with another int, etc). Important to note is that some things about `int`s are _not_ part of the `int` ADT. For instance, the fact that ints are usually represented as bit strings has nothing to do with the `int` ADT. Neither does the fact that `int`s can be used as pointers (since that is a language bug/feature rather than a property of the type).

In general, the set of values which are valid for an ADT are called that ADT’s _domain_, and the “things you can do” with an ADT are called the ADT’s _operations_, or sometimes, _API_ (meaning Application Programming Interface). What is not part of the ADT is how values are stored, or the specific steps operations use to do their jobs. This is why ADTs are called _Abstract_: the concerns of how these details work is _abstracted_ away from the developer.

In C, there are four primitive (sometimes called _atomic_) data types: `char`, `int`, `float`, and `double`. All other C data types are built from these. For example, in C, a `bool` is implemented as an `int` (where 1 has the value true and 0 has the value false). However, because `bool` has an ADT, a developer needn’t ever know this. In order to use a bool, you only need to know that the domain is {true, false} and the API is composed of 5 operations: `!` (negate), `&&` (logical AND), `||` (logical OR),  `==` (compare),`!=` (compare negation). Note that `=` (assignment) is not included in the list of operations: This is because assignment does not actually operate on `bool`s, only on variables of type `bool`. Consider `true = false;`. This statement does not compile (at least in clang-7) because `true` is not assignable. Therefore, `=` is not part of `bool`’s API.

According to [a very good article by MIT](https://web.archive.org/web/20180922184253/http://web.mit.edu/6.005/www/fa14/classes/08-abstract-data-types/), operations on an ADT can be classified as either Creators, Produces, Observers, or Mutators.
- Creators instantiate the ADT, for instance, the literals `0` (for `int`) or `'c'` (for `char`).
- Producers generate new ADT instances from extant ones: consider `int`’s `+`, which takes two `int`s and _produces_ a third.
- Observers take instances of the ADT and return a different type: `double`’s `floor`_observes_ a `double` but returns an `int`.
- Lastly, Mutators. Mutators change ADT instances without altering their identity. Atomic ADTs do not have mutators, but data structures (covered in the next article) do. As a simple example, an array’s dereference-and-assign operation (eg, `arr[3] = 5`) changes an element of the array without replacing the array itself, so we say it _mutates_ the array. Accordingly, this would be considered a mutator.

Some languages (like C) have _type qualifiers_ which modify a type’s ADT. For instance, an `int` might be modified to be an `unsigned int`, which changes its domain from (`INT_MIN` to `INT_MAX`) to (`0` to `UINT_MAX`). The same API applies, but the implementation might be different, as might the domain.