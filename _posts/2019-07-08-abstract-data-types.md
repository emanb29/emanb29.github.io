---
published: true
title: 'Abstract Data Types: The Other ADTs'
hidden: true
---
Abstract Data Types are a crucial construct in everyday programming. They effectively constitute all types which values might have. For the sake of brevity, I will refer to Abstract Data Types in this article as ADTs, though not to be confused with _Algebraic_ Data Types, covered in [another article](https://ethanbell.me/algebraic-data-types/)).

Note: While the concepts in this article apply to all programming languages, the specific examples are drawn from the C programming language.

ADTs can simply be described as “all the things that describe a type”. For instance, 

`int`’s ADT specifies which values are valid for an `int` (whole numbers in the range of `INT_MIN` to `INT_MAX`) and the operations that are defined for an int (for example, incrementing, decrementing, adding with another `int`, etc). Important to note is that some things about `int`s are _not_ part of `int`’s ADT. For instance, the fact that ints are usually represented as bit strings has nothing to do with the `int` ADT. Neither has the fact that `int`s can be used as pointers (since that is a language bug/feature rather than a property of the type).

In general, the set of values which are valid for an ADT are called the _domain_, and the “things you can do” with an ADT are called the ADT’s _operations_, or sometimes, _API_ (Application Programming Interface). The ADT does not include how values are stored, nor the specific steps by which operations do their jobs. This is why they are referred to as _abstract_: the concerns of how these details work is hidden (or _abstracted_) from the developer.

In C, there are four _primitive_ (sometimes called _atomic_) data types: `char`, `int`, `float`, and `double`. All other data types are built from these. As an example, a `bool` is implemented as an `int` (where `1` has the value `true` and `0` has the value `false`). However, because `bool` has an ADT, a developer needn’t ever know this. In order to use a bool, you only need to know that the domain is {`true`, `false`} and the API is composed of 5 operations: `!` (negate), `&&` (logical AND), `||` (logical OR), `==` (logical equality), and `!=` (logical inequality / XOR). Note that `=` (assignment) is not included in the list of operations: This is because assignment does not actually operate on `bool`s, only on _variables_ of type `bool`. Consider `true = false`: This statement does not compile because `true`, while a valid `bool` value, is not an assignable variable. Therefore, `=` is not part of `bool`’s API.

According to [a very good article by MIT](https://web.archive.org/web/20180922184253/http://web.mit.edu/6.005/www/fa14/classes/08-abstract-data-types/), operations on an ADT can be classified as either Creators, Produces, Observers, or Mutators.



*   _Creators_ instantiate the ADT, such as the literals `0` (for `int`) or `'c'` (for `char`). 
*   _Producers_ generate new ADT instances from existing ones (eg, `int`’s `+`, which takes two `int`s and produces a third).
*   _Observers_ take instances of the ADT and return a different type (eg, `double`’s `floor` observes a `double` but returns an `int`).
*   _Mutators _change ADT instances without altering their identity. Atomic ADTs do not have mutators, but data structures (covered in the next article) do. For example, an array’s dereference-and-assign operation (eg, `arr[3] = 5`) changes an element of the array without replacing the array itself, so we say it _mutates_ the array.

Some languages have _type qualifiers_ which modify a type’s ADT, such as `unsigned`. An `unsigned int` has a domain of `0` to `UINT_MAX`, as opposed to an `int`’s domain of `INT_MIN` to `INT_MAX`. The same API applies, but the implementation of the operations might be different, as might the domain.