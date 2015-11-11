---
layout: post
title:  A little bit of curry
date:   2015-09-10 20:30:00
categories: plt functional-programming
excerpt: Curry is not only a tasty dish; it is also a technique in Computer Science to make functions delicious.
---

Curry is not only a tasty dish; it is also a technique in Computer Science to make functions delicious.

## Tastes of Curry

The wikipedia article on [currying](https://en.wikipedia.org/wiki/Currying) might be a tedious read, but in essence, currying is just transforming a function that take multiple arguments into a sequence of functions that take one single argument.

Take a look at the simple example of adding two numbers. If we think of `add` as a function, it usually takes two arguments.

Let's have some curry coffee :) Fire up the CoffeeScript REPL:

``` coffee
coffee> add = (a, b) -> a + b
[Function]
```

Now we have a function as good as the `+` operator! Try it out:

``` coffee
coffee> add(1,2)
3
```

So far so good! But if we are allowed only to use **exactly one** argument for *any* function definition, can we define a function (or composition of functions) to add two numbers?

Yes we can! :)

The insight is that we can define a function that takes one argument and returns a function that takes another argument. The inner function now has access to both of the arguments, and it will return what our original function does: adding two numbers.

``` coffee
------> add_curried = (a) ->
.......   (b) -> a + b
[Function]
```

Unsurprisingly, this new function is called curried function (whereas the original, multi-argument one is uncurried function); and the transformation from multi-argument function to single-argument function is called currying.

To use our freshly minted function `add_curried` to calculate a sum, it will involve two function applications. The first time we apply the first addend to `add_curried` and it returns a function (which will take the second addend); the second time we apply the second addend to that function, it returns the result.

``` coffee
coffee> add_curried(1)
[Function]
coffee> add_curried(1)(2)
3
```

## But why?

Single argument for functions looks like a constraint/limitation, but in fact it frees you from the tight coupling of function arguments. For a function with multiple arguments, you have to supply the function with all arguments it needs in order to call it. For a curried function, you can applied only parts of the arguments it needs, which is called partial application, and pass the result around so that we can take the result for other purposes, or as building blocks for function composition.

The first application `add_curried(1)` fits the definition of partial application. It turns out that we can use this function to increment all elements in a list by 1! Just throw our curried function in:

``` coffee
coffee> [1,2,3].map(add_curried(1))
[ 2, 3, 4 ]
```

And the syntax for calling a curried function doesn't have to be verbose. In fact, in Haskell, all functions are considered curried: That is, all functions in Haskell take just single argument. (λ-calculus supports single argument only too, which is not a coincidence.) Since Haskell function application takes the highest precedence (over operators), writing function application is even simpler.

``` haskell
Prelude> (+) 1 2
3

-- you can replace any occurrence of add with add' or the other way around
add a b = a + b
add' a = \b -> a + b

-- add'' takes a tuple
-- add'' (1,2)
add'' = \(a, b) -> a + b
```

Further more, knowing that functions only takes one argument allows you to define function composition easily. You just pipe the result of a previous function application to the next function.

```haskell
(.)                     :: (b->c) -> (a->b) -> (a->c)
f . g                   = \ x -> f (g x)
```

## Make Curry

We can even define a function to make curry functions for us. Check out the Curry machine that works for functions with an arity of 2 (that takes 2 arguments):

``` coffee
# curry takes a function with two arguments
curry2 = (f) ->
  (x) ->
    (y) ->
      f(x, y)
# curry2(add_uncorried)(1)(2) === 3

# uncurry2 takes a curried function
uncurry2 = (f) ->
  (x, y) ->
    f(x)(y)

#uncurry2(add_curried)(1, 2) === 3
```

``` coffee
coffee> [1,2,3].map(curry2(add_uncorried)(1));
[ 2, 3, 4 ]
```
How about more arguments than 2? If you have a Friday afternoon to spare, challenge yourself with this puzzle: https://github.com/frantic/friday. (Hint: use bind)

Curry for the win!
<img src="{{ site.url }}/images/curry.jpg" alt="Steph Curry knows how to curry" style="width: 100%;"/>

## Additional Readings

- Currying, Chapter 3.1, Programming Languages and Lambda Calculi
- [Currying in Javascript](http://slides.com/gsklee/functional-programming-in-5-minutes#/)
- [Currying in Haskell](https://wiki.haskell.org/Currying)
