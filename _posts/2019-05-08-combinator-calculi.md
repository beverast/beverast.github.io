---
layout: post
title: Combinator Calculi
subtitle: The DNA of Computation
gh-repo: beverast//beverast.github.io
gh-badge: [star, fork, follow]
bigimg:
tags: [functional programming, combinators, logic, python, lambda calculus, open source]
comments: true
---

# **Combinator Calculi: The DNA of Computation**

## **Environment Considerations**
------------------

### Python execution strategy and data model
* Arguments of functions are, essentially, call-by-value.
 + An argument consists of a pointer to an object, and in Python everything is an object.
* Python offers lazy evaluation of boolean expressions 

Docs:
* [Execution model](https://docs.python.org/3/reference/executionmodel.html)
* [Data model](https://docs.python.org/3/reference/datamodel.html)
* [Boolean operations](https://docs.python.org/3.7/reference/expressions.html#boolean-operations)

### Python `lambda` functions
* They're [anonymous](https://en.wikipedia.org/wiki/Anonymous_function) and [higher-order](https://en.wikipedia.org/wiki/Higher-order_function), but not necessarily [pure](https://en.wikipedia.org/wiki/Pure_function) (e.g. side-effects are allowed)
 + This means lambda functions behave like normal Python functions, although they're slighty more restricted.
 + Restrictions: lambdas can't contain (1) statements or (2) annotations
* They can reference variables from their containing scope

Docs:
* [lambda expressions for control flow](https://docs.python.org/3/tutorial/controlflow.html#lambda-expressions)
* [lambda expression language specification](https://docs.python.org/3.7/reference/expressions.html#lambda)

## **Combinators**
-------------------------

A **combinator** is a function that:
1. Takes functions as arguments
2. Returns a function 
3. Has no quantified variables
4. Uses only function application and previously defined combinators

[Combinatory Logic on Wikipedia](https://en.wikipedia.org/wiki/Combinatory_logic)

A combinator calculus, therefore, is a [formal system](https://en.wikipedia.org/wiki/Formal_system) of computation where the rules are defined by the combinators, and all possible computations can be expressed, exemplifying [Turing completeness](https://en.wikipedia.org/wiki/Turing_completeness). Wikipedia says a combinator calculus "may be perceived as a reduced version of the [untyped](https://en.wikipedia.org/wiki/Programming_language#Type_system) [lambda calculus](https://en.wikipedia.org/wiki/Lambda_calculus)." And yes, the incubator/seed accelerator Y Combinator is indeed named after *the* Y combinator!

**So, can we write combinators in Python?** Yes! Let's see some examples of those properties below.

A combinator takes functions as arguments and returns a function:


```python
# Let's define a simple function to pass to this combinator
def func():
    return 1

# ID takes a function and returns said function object
ID = lambda x: x


ID(func), ID(func).__name__
```




    (<function __main__.func()>, 'func')



Here's a semantic variation of the `lambda` argument: return the _called_ function.


```python
call_function = lambda x: x()

call_function(func)
```




    1



A combinator has no quantified variables, and uses only function application and other combinators:


```python
# This combinator has two parameters x and y, and returns x always.
CONST = lambda x: lambda y: x
CONST (ID) (5)
```




    <function __main__.<lambda>(x)>



Here's an example of **currying**, or when a function is applied to a partial number of arguments.


```python
CONST_5 = CONST (5)
CONST_5 (8), CONST_5 (ID)
```




    (5, 5)



Now, let's return `ID` from `CONST` then supply an argument to `ID`.


```python
CONST (ID) (5) (sum)
```




    <function sum(iterable, start=0, /)>



Here's the same operation, but with parentheses to show left-associativity of the derivation.

...If arguments weren't delimited by parens this line of code would have a beautiful functional syntax: `(((CONST ID) 5) sum)`


```python
(((CONST (ID)) (5)) (sum))
```




    <function sum(iterable, start=0, /)>



## **Getting Your Feet Wet with the SKI Combinator Calculus**
-------------------------

S, K, and I form what is known as a **complete basis**: with these three combinators any computation in the lambda calculus can be represented (which is related to Turing completeness and the [Church-Turing Thesis](https://en.wikipedia.org/wiki/Church–Turing_thesis)). There's a [proof](https://en.wikipedia.org/wiki/Combinatory_logic#Completeness_of_the_S-K_basis) describing a transformation **T[]** that takes any lambda term, and converts it into an equivalent expression of combinators as well. 

Enough with the fun facts 😏 Let's define these combinators with our handy `lambda` function and see what interesting things we can construct.


```python
# I is the 'Identity' operator which we've already seen: Ix = x
I = lambda x: x

# K is the 'Constant' operator: Kxy = x
K = lambda x: lambda y: x

# S is the 'Substitution' operator: Sxyz = xz(yz)
S = lambda x: lambda y: lambda z: x (z) (y (z))
```


```python
I (K)
```




    <function __main__.<lambda>(x)>



Here's a more lengthy *linear sequence*  of terms with the steps of the derivation written out below.

**K K K S K S**

**K S K S**

**S S**


```python
K (K) (K) (S) (K) (S) (K) (K) (sum)
```




    <function sum(iterable, start=0, /)>



Here I'd like to point out one of the more popular publications about combinators, ["To Mock a Mockingbird"](https://en.wikipedia.org/wiki/To_Mock_a_Mockingbird) by Raymond Smullyan. This book introduces various combinators and aspects of combinatory logic, and in it each combinator is described as a different species of bird in a forest. So, while S, K, and I are combinators, so are the Kestrel, the Mockingbird, the Kite, and the ... Idiot bird. Below I'll define some of these other combinators to add to our toolbelt.  


```python
# "Idiot Bird": returns the argument
# I x = x
# Already defined: I = lambda x: x

# "Kestrel": discards argument y
# K xy = x
# Already defined: K = lambda x: lambda y: x

# "Kite": discards argument x
# KI xy = y
KI = lambda x: lambda y: y

# "Starling": distributes argument z and performs function application
# S fgx = (f x)(g x)
S = lambda x: lambda y: lambda z: x (z) (y (z))

# "Mockingbird": self-application of argument
# M x = x (x)
M = lambda x: x (x)

# "Cardinal": swaps the argument order
# C xyz = x (z) (y)
C = lambda x: lambda y: lambda z: x (y) (z)
```

If you want to explore all the species of birds in the forest here's a link: [Combinator Birds](http://www.angelfire.com/tx4/cus/combinator/birds.html)

Below is a derivation that shows the I combinator to be "syntactic sugar" for the SKI calculus. This means that a complete basis can actually be formed from only S and K- known as the **S-K basis**. 

**S K K x**

**K x (K x)**

**x**


```python
S (K) (K) (sum), I (sum)
```




    (<function sum(iterable, start=0, /)>, <function sum(iterable, start=0, /)>)



I'm going to present one more idea: Boolean operators via _derived combinators_. I've taken the liberty of adding an `inspect` method to the True and False objects so we can get slightly more meaningful output from combinator reductions going forward.


```python
# Boolean values
T = K
T.inspect = lambda: 'T/K'

F = KI
F.inspect = lambda: 'F/KI'

# Boolean operators
AND = lambda x: lambda y: x (y) (x)
OR = lambda x: lambda y: x (x) (y)
NOT = lambda x: x (F) (T)

# Boolean Equality
BEQ = lambda x: lambda y: x (y) (NOT (y))
```


```python
AND (T) (F), AND (T) (F).inspect()
```




    (<function __main__.<lambda>(x)>, 'F/KI')




```python
OR (T) (NOT (T)).inspect()
```




    'T/K'




```python
BEQ (K) (I (K)).inspect()
```




    'T/K'



## **Next Steps**
---------------------

Now the ball's in your court! There's a rich history in combinator calculi, and the research ties into so many different fields of mathematics and computer science. A personally fascinating area of research is [graph reduction](https://en.wikipedia.org/wiki/Graph_reduction). Here's a great video on the topics of combinators and graph reduction: ["An Introduction to Combinator Compilers and Graph Reduction Machines" by David Graunke](https://www.youtube.com/watch?v=GawiQQCn3bk). If you want to test your Python and combinatory logic skills I'd suggest implementing an encoding, such as [Church Numerals](https://en.wikipedia.org/wiki/Church_encoding). 

I hope that I've managed to cultivate a sense of curiosity about these fundamental obejcts of computation. Below I've provided a list of some of the more popular research publications in combinatory logic and some related disciplines. There's a wealth of inspiration and open questions in these papers and if you find something interesting or have a burning question feel free to send me an email! 

**Combinators**

1. [Über die Bausteine der mathematischen Logik](https://link.springer.com/article/10.1007%2FBF01448013?LI=true)
2. [Grundlagen der Kombinatorischen Logik](https://www.jstor.org/stable/2370716?seq=1#page_scan_tab_contents)
3. [Another algorithm for bracket abstraction](https://www.cambridge.org/core/journals/journal-of-symbolic-logic/article/another-algorithm-for-bracket-abstraction/E307B9FC7178599CE1BEAF0B3388A983)
4. [Compact bracket abstraction in combinatory logic](https://www.cambridge.org/core/journals/journal-of-symbolic-logic/article/compact-bracket-abstraction-in-combinatory-logic/F93DDAFAB726E4412423A23B32AC629E)

**Supercombinators**

1. [Supercombinators: A New Implementation Method for Applicative Languages](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.472.4622&rep=rep1&type=pdf) PDF
2. [A Compiler for Lazy ML](https://dl.acm.org/citation.cfm?id=802038)
3. [Implementing lazy functional languages on stock hardware: the Spineless Tagless G-machine](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.53.3729&rep=rep1&type=pdf) PDF
4. [Faster Laziness Using Dynamic Pointer Tagging](https://www.researchgate.net/profile/Simon_Peyton_Jones/publication/221241145_Faster_laziness_using_dynamic_pointer_tagging/links/00463518a0cf57aacc000000/Faster-laziness-using-dynamic-pointer-tagging.pdf) PDF
5. [Implementing Type Theory in Higher Order Constraint Logic Programming](https://hal.inria.fr/hal-01410567/document) PDF

**Abstraction Elimination Algorithms**

1. [Explicit Substitutions](https://hal.inria.fr/inria-00075382/document) PDF
2. [A journey through calculi of explicit substitutions](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.38.8716&rep=rep1&type=pdf) PDF

**Evaluation by Collection**

1. [Why Functional Programming Matters](https://watermark.silverchair.com/320098.pdf?token=AQECAHi208BE49Ooan9kkhW_Ercy7Dm3ZL_9Cf3qfKAc485ysgAAAj8wggI7BgkqhkiG9w0BBwagggIsMIICKAIBADCCAiEGCSqGSIb3DQEHATAeBglghkgBZQMEAS4wEQQMhLdZXkekPbIXtKyrAgEQgIIB8mVELGwQc40fR4C3nBv1Fcj9H_glZOiIS59z-WFvKzZdsbvGug3x7h3-U9pZtXklhu1wCpbumbyqQPRo2HO7d6bcQaYk3rYxYtxWkeaVG1u4deloX_vtj3HY8bK1CMeHfJO9ZH0bR639fZdW4vY9ffYpL3_HFROQxTb7TVfcHY05YTG81GsVdxgLz9Sq3VHuVE7Za8bWKeAKT5MkJbTkUTbb9GKy9F_LthVGnhWFstNEVUQL7aIYufUuMumpZ8yLNlgpZcrpxRdHwUKL3G5mfhy5Rts8y45nr6PSvgqtxiX46GE9NKJg0KzM_Az6rShowjCM19uDAYGAGQsXvyVVvA7sDt0G4QtfeePzfQthlzzdBjAU8NK7vGFWMHCN_uQX5g9of8m9Ct1OipKfkTOQInK79WZRXSvzORSI_m1tybZnRgCl5UeBxr0Qll9vsfkpi_AuI7cOHqBkue1kRyOdl-m1LCh-4ehONefoPZNtBqjL2z9WGs-u2N3iwgXsiaPayPIeESmB2TelK8bZxrdwXiDgq5LdsS0y2tFvLYWce1ATPuRxJ9woO4H0ouHEAw8s0uKBpqXbBgWKZdMhvkgEQ0sCDaj8z5tjYfUQ6SGt-QyTWV0AfEkKr7VrBtZPHKj1-D3SidX8coOTyCAduHHBu1JNlg) PDF
2. [Fixing some Space Leaks with a Garbage Collector](https://www.semanticscholar.org/paper/Fixing-some-Space-Leaks-with-a-Garbage-Collector-Wadler/7d3168ef9ab7c4bf86b9d00b80660a494e119f65)
3. [A Combinator-based Compiler for a Functional Language](http://haskell.cs.yale.edu/wp-content/uploads/2011/03/CombComp-POPL84.pdf) PDF
4. [Some practical methods for rapid combinator reduction](https://dl.acm.org/citation.cfm?id=802032)
5. [Associative Concurrent Evaluation of Logic Programs](https://pdf.sciencedirectassets.com/271869/1-s2.0-S0743106600X00908/1-s2.0-0743106684900268/main.pdf?x-amz-security-token=AgoJb3JpZ2luX2VjEFQaCXVzLWVhc3QtMSJHMEUCIQCdraDP0VSbdo0m1ezokFIzmoCqCa0C8h8s%2Fgz3Ass7LgIgWrMgBInRbzwhFEheub7UpoaAm5SmjrMT73zXREM1yLMq2gMITBACGgwwNTkwMDM1NDY4NjUiDMT0Mn8as2l6JJi9ISq3A8VWM5TVGAvjHGoneiQxN9sEzLrbKejNefy9TwYRTtHMLyAuE4Y9QIdrFOsLMSIjni6l95q9C7OrzsuYHdlJbfGtdHYqSPswGh6G62iwcEg64NKNXNT2uIsOkqdvo1%2BXyN7pLuF%2BY7j6Re3XkLBlr02kDU7M61NVyy9bvKA6tM66lCQ5rjuX0NH9UQ2%2F1bk1AJWq3jQD3qtKgPcH4%2BecHGrCnkMN3l2KjfClAqWivMOEJvIpXKwnqJg%2BIHRboc9wW4Bf2rCrFnz9fk1Rimi8bQpGs5N2ow2OTXx2h3%2F73Y4%2Fo1S%2FWZ%2FrKmCCgkn29z2f5t86omxkF34jSzCs%2BbyMm9CpnWZ3yelR8IqOUn8eqyK31JwX3UrKnYdcmEV1kcezvMfF1TQfjXLpJqjLMsQkBGX%2BcbODIVQgsZCUSgm8skTJD6VMgbyzcyt0L8gao5USSVcg2sDnOgzzIutqRSreuyH7edENcWGCClxZ3sjuhDysO%2FwRWKYh6TysnMcZGL%2Bxkuzd5b6tndpV6IpiqhAS7w42ChGXuKf8gKYa1CLX6%2FpEtXOSnwVu4pIFW5k%2BGUtOr4TEe%2FhFc7gwj9jM5gU6tAGcsz4U74gxSbMvS5xb2bsbFOl%2FriBjd2XwNzT5kUwKIrvbkMzTt5e5TJv%2Brg4z6Pjh1PT4mvgcoj0KjNU1QMBFYWmt2nOqhWntRIljeORuxTA0TjGEHAg7ZOgAPNyuSNBMZoapUGZ%2FkHyqb93sKAo5WIZfGYhGpg%2Fd%2FEzQHtEdRX%2BHaj%2Bw3Nhf6HD9e3tYYdXAOq1pIQjFBVI55dE%2FP2hW0vZw0nlWAFeOrb1zV7EyHGKCosY%3D&AWSAccessKeyId=ASIAQ3PHCVTYVIRCWIEW&Expires=1557349965&Signature=q99RNDzfqh5YlVpc8cJ8ve%2BBuMU%3D&hash=8e3b3809558d8dbf66d16f905fbab3165aaf3ce5297aa66831ae9b2ae190b841&host=68042c943591013ac2b2430a89b270f6af2c76d8dfd086a07176afe7c76c2c61&pii=0743106684900268&tid=spdf-2f4f94ad-9705-4ea7-ba6f-36faf6c93008&sid=d4b0c53994aa5348833a10e92207a58ac262gxrqa&type=client) PDF

**Optionally Acylic Heaps**

1. [Maximal Laziness](https://pdf.sciencedirectassets.com/272990/1-s2.0-S1571066109X00381/1-s2.0-S157106610900396X/main.pdf?x-amz-security-token=AgoJb3JpZ2luX2VjEFUaCXVzLWVhc3QtMSJHMEUCIQCa1vdqWyIf2e7mOjP7LWlRwIfky8TE0wc7jubCIwHlrwIgehijkGjIpJrWNSjyuScnPxS80Gjur2LAgfiLP5zE71Uq2gMIThACGgwwNTkwMDM1NDY4NjUiDBlpLClQeSy9mMdI2iq3A0wsrL4AedtPuyq5O6ZUMTggEj9m9lqhPocw8tI4SqwufNDMtWpkLp8lP3yM8RUnAOE3H1s6kmEkmXAH%2BR3WDyF2C0b6gNWUNVbyQm5ZxAPvJGZtqMeeLlT8hW4aVSLblexAPQWGox47IPUxCPiQ2Acemp82jcivKEJIl8FhrZrZLT2uOux7U%2FLvMJ%2FDfHflAAORcyo4KuGVm1Unqqmu0BKw%2Bn2IuUgQmREjQIWnur63%2FKiYQy4KfBpX8jKXTBuz8qhQqSxzzytf6E5Xkm2GhuroLdU%2BFpP2w6AdirXx%2BoglbceE1lSERnyu02vnfvE7GCa2vxxJScRIZZcRd8cNpXCODuH12N9c9j636pcK0YuKcL%2BOTC9tEUInXJin2ecSh%2FyBUlVRxslAz1QGogJ7bOJCEQW40LXVZPsnAWSojKaam9YzQcLgG0NMFa5jUt4jMmmfeAaSc15wiuyr6raqjbB7Uy691j%2F8RSj2Xe4FscEsOxzLmHWDvuHn5O%2Byu1o1ZntdZVMuXaxkihtsuUDsHcuR3gvWlScRLhecnnpL5VeV1ynviY6B62mAah%2FMEjm84QXWQiXvwdIw0fjM5gU6tAEGMCfeV9Z4iofFatuGB9G7QqHeHiGFcv%2FmdXTcKRTPFVjdur4sHlIdnnHY5fK8OPNkPOJnDXbtepTXEJBe9%2BH8C%2F9a1nFNRFcwU7108RmiBTwREQYWKM51ASPRzQll2H1Qz55fQbtgd7Ca0N7UEheAClPGGdedsBe3%2Frh8efnqTZyeipDmzWcb3UkRp2wczQvvfObh%2FvKs75KPbGQa%2BAwBgMOeXfsiucJ%2BrtsWxV61up3PP9g%3D&AWSAccessKeyId=ASIAQ3PHCVTY2T7NYQDO&Expires=1557350076&Signature=dlXKzqMn42aR7du7rUq02g8WL0E%3D&hash=1cf9acb597110da86b66be2e6e5cc921c240764d9afdcd494e1812db69eb4944&host=68042c943591013ac2b2430a89b270f6af2c76d8dfd086a07176afe7c76c2c61&pii=S157106610900396X&tid=spdf-c44be4b1-c7d3-4215-b440-dbf3c8dbc618&sid=d4b0c53994aa5348833a10e92207a58ac262gxrqa&type=client) PDF
2. [Cache Behavior of Combinator Graph Reduction](http://users.ece.cmu.edu/~koopman/tigre/toplas_92.pdf) PDF
3. [GRIP - a high-performance architecture for parallel graph reduction](https://link.springer.com/content/pdf/10.1007/3-540-18317-5_7.pdf)
