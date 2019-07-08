---
published: true
title: Algebraic Data Types
hidden: false
---
Recently, type theory is quite the hot topic in the languages and theoretical CS research spaces. Seeing this, and not wanting to exclude any major sub-disciplines or research areas from consideration as I review prospective graduate programs, I decided to try to teach myself the intuitive basics of the theory. This necessarily (or at least I hope it was necessary, for if not then I don’t want to think about how much time I’ve wasted) led me down a few rabbit holes, and introduced me to a _ton_ of new vocabulary. Among the definitions, some terms stood out as both somewhat opaque, and particularly critical to a foundational understanding of many topics. Algebraic data types fall into this intersection.

While I first came across algebraic data types (ADTs -- not to be confused with _abstract_ data types) in the context of type-theoretic mathematics, I quickly saw the applications to my everyday coding. Interestingly, the relevance to regular programming was clear across a wide breadth of languages. From my personal favorite Scala, to the language I wish I was smart enough to have as my favorite Haskell, to my company’s language of choice at the time C#, to the university-mandated C and C++, all of these languages had type systems that could be modeled using ADTs. While the usefulness of this model was certainly more apparent in the strongly-typed functional languages of Scala and Haskell, thinking about data in terms of ADTs helped me gain clarity in designing data structures independent of language.

So, what are ADTs? Algebraic data types are composite types (ie, types made of other types) that have properties that mimic set-theoretic properties. If that’s just word soup to you, don’t worry, it would have been to me too. I find it easier to get a basic grasp of the concept using examples. Let’s start with a tuple, basically a container for other types in some particular order, kind of like a class with no methods and no property names. One tuple type may be `(Boolean, Int, Byte)`. Another may be `(String, Boolean)`. Yet another could be `(String, Nothing)`. A tuple is a very simple algebraic data type. In particular, it is a _product type_, the meaning of which we will get to later. Other product types include classes (which, as mentioned, are basically tuples with names) and C/C++ structs.

Let’s consider another ADT: `Either`. `Either` has a “left type” and a “right type”, and the value of the `Either` is _either_ an instance of the left type or one of the right type. Some `Either` types may be `Either[Byte, Boolean]`, or `Either[Int, String]`. An `Either` is another algebraic data type. It is a _sum type_, the meaning of which, again, I hope will be clear very soon. Other sum types include `Option` (aka `Maybe` in Haskell/Scalaz), `Try`, and even C `union`-s.

Hopefully by now you have some intuition of what sorts of things may be product types, and what may be sum types. In my mind, when the above was all I had to go on, I basically thought the following: “A product type is like those children’s pegboard games with different shapes and slots to match the shapes. Each hole is a type and the whole board is a product type. You need to fill each hole to complete the product type, and each inner type is available side-by-side. A sum type is like if the pegboard had only one hole, constructed in such a way that it it will fit only certain pegs, but not all. For example, the hole may fit a square or a triangle, but not a circle. Further, to complete the sum type, you can only put 1 shape in the hole at a time. If a triangle is in the hole, a square cannot fit, and visa versa. Similarly, you can never put in a shape the hole wasn’t designed for. A rhombus won’t fit a hole designed to take only squares and circles!”.

If you think me crazy after reading the above, I won’t blame you. Obviously, that approach to thinking about ADTs is brittle and hard to convey, and doesn’t really explain why these things are called “products” and “sums”. For a better explanation (actually two), we turn to our old friend, counting.

Consider our above tuple types. In particular, let’s first look at the types they’re composed of. Boolean, Int, String, Byte, and Nothing. For each, let’s jot down how many values of that type can be instantiated.


<table>
  <tr>
   <td><strong>Type</strong>
   </td>
   <td><strong>Count</strong>
   </td>
  </tr>
  <tr>
   <td>{% highlight scala %}Boolean{% endhighlight %}
   </td>
   <td>\(2\) (true and false)
   </td>
  </tr>
  <tr>
   <td>{% highlight scala %}Int{% endhighlight %}
   </td>
   <td>\(2^{31} - 1\) or similar (depending on context)
   </td>
  </tr>
  <tr>
   <td>{% highlight scala %}String{% endhighlight %}
   </td>
   <td>Depends on implementation, but usually \(\text{a lot}\)
   </td>
  </tr>
  <tr>
   <td>{% highlight scala %}Byte{% endhighlight %}
   </td>
   <td>\(256\)
   </td>
  </tr>
  <tr>
   <td>{% highlight scala %}Nothing{% endhighlight %}
   </td>
   <td>\(0\)
   </td>
  </tr>
</table>


One type which may not be familiar is Nothing. Nothing is the name of the “bottom type” (denoted ⏊) in Scala, meaning it is a subtype of every other type in the language. It has no values and cannot be instantiated, much like void in many other C-like languages.

Now, consider the tuple types themselves in a similar fashion. To determine the total number of possible values for a tuple, we need to consider every possible combination of the values of its constituent types. If we were dealing with a tuple composed of 2 booleans, we’d have 4 options: `(true, true)`, `(false, true)`, `(true, false)`, and `(false, false)`. Using the technique for counting problems taught in any discrete mathematics class, we multiply the potential value counts for each member of the tuple to get the count for the tuple overall.


<table>
  <tr>
   <td><strong>Type</strong>
   </td>
   <td><strong>Count</strong>
   </td>
  </tr>
  <tr>
   <td>{% highlight scala %}(Boolean, Boolean){% endhighlight %}
   </td>
   <td>\((2)*(2) = 4\)
   </td>
  </tr>
  <tr>
   <td>{% highlight scala %}(Boolean, Int, Byte){% endhighlight %}
   </td>
   <td>\((2)*(2^{31} - 1)*(256) = (2^{40})-512\)
   </td>
  </tr>
  <tr>
   <td>{% highlight scala %}(String, Boolean){% endhighlight %}
   </td>
   <td>\((\text{a lot})*(2) = \text{a lot}\)
   </td>
  </tr>
  <tr>
   <td>{% highlight scala %}(String, Nothing){% endhighlight %}
   </td>
   <td>\((\text{a lot})*(0) = 0\)
   </td>
  </tr>
</table>


As noted, the total number of values possible for these tuple types is precisely the product of the number of possible values of each of their constituent types! Therefore, it makes sense to call tuples a _product type_! Let’s repeat the same exercise for the sum types we called out earlier. Any guesses how that might go?

<table>
  <tr>
   <td><strong>Type</strong>
   </td>
   <td><strong>Count</strong>
   </td>
  </tr>
  <tr>
   <td>{% highlight scala %}Either[Byte, Boolean]{% endhighlight %}
   </td>
   <td>\(256 + 2 = 258\)
   </td>
  </tr>
  <tr>
   <td>{% highlight scala %}Either[Int, String]{% endhighlight %}
   </td>
   <td>
   \((2^{31} - 1) + \text{a lot} = \text{a lot}\)
   </td>
  </tr>
  <tr>
   <td>{% highlight scala %}Option[Byte]{% endhighlight %}
   </td>
   <td>\(256 + 1 = 257\) (ie, 256 <code>Some[Byte]</code>) + 1 <code>None</code>) 
   </td>
  </tr>
</table>


_Sum types_ have a possible number of values equal to the _sum_ of their composite types! Who’d have guessed?

The astute reader may notice that I promised two mathematical explanations for the definition of sum and product types, but have so far only offered one. Well, remember how I mentioned set theoretic properties? What if we define types as _sets_ of possible values? Boolean \\( = \\{true, false\\} \\) and so on. Well, then tuples can be defined as the cartesian product of those sets. For example, `(String, Boolean)` becomes String \\(\times\\) Boolean. `(String, Nothing)` becomes Nothing, because \\( A \times \varnothing = \varnothing\\) for any set \\(A\\). Similarly, sum types can be defined in terms of sums (aka disjoint unions) of sets. So `Either[Byte, Boolean]` is Byte \\(\cup\\) Boolean, while `Option[Byte]` is Byte \\(\cup\\) \\(\\{\\)`None`\\(\\}\\). There is a similar, though less intuitive, definition for _quotient types_, but I’ll leave that out of this article for the sake of brevity.


_This article is based on an untitled functional programming talk given by [Greg Stromire](https://github.com/gstro), who in turn based the relevant sections on [“Introduction to Algebraic Data Types in Scala”](https://tpolecat.github.io/presentations/algebraic_types.html) by Rob Norris_


<!-- Docs to Markdown version 1.0β17 -->
