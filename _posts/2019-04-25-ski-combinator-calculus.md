---
layout: post
title: What's the SKI Combinator Calculus?
subtitle: A Turing-complete, untyped computational system comprised of three combinators that predates the lambda calculus.
gh-repo: beverast/beverast.github.io
gh-badge: [star, fork, follow]
bigimg: /img/sf-icon.png
tags: [lambda calculus, formal methods, functional programming, logic]
comments: true
---

## Overview and Informal Definition

The SKI combinator calculus is a [combinatory logic](https://en.wikipedia.org/wiki/Combinatory_logic) invented by Moses Schofinkel and Haskell Curry in 1920. To quote the Wikipedia page: "**Combinatory logic** is a notion to eliminate the need for quantified variables in mathematical logic. A **combinator** is a higher-order function that uses only function application and other combinators to define a result (another function) from its arguments." SKI combinator calculus is [Turing-complete](https://en.wikipedia.org/wiki/Turing_completeness), meaning that this computational system can approximately simulate the computational aspects of any other real-world computer or programming language. S, K, and I are combinators that form a complete **S-K basis** meaning that with those three combinators any term in the lambda caculus or any other Turing-complete language can be extensionally derived. 

The operations within the SKI calculus can be encoded into a binary tree representation. Each node or leaf is a combinator, and the application of arguments is left-associative with each argument being [curried](https://en.wikipedia.org/wiki/Currying). I'll explain how the combinator logic works, but first here's a visualization of what a binary tree representation could look like to get a visual intuition: 

![S,K Combinator Derivation](https://people.cs.uchicago.edu/~odonnell/Teacher/Lectures/Formal_Organization_of_Knowledge/Examples/combinator_calculus/img30.gif)
[S, K Combinator Derivation](https://people.cs.uchicago.edu/~odonnell/Teacher/Lectures/Formal_Organization_of_Knowledge/Examples/combinator_calculus/)

## SKI Usage

First, we'll cover the definitions of combinators S, K, I then walk through some examples of [abstraction elimination](https://en.wikipedia.org/wiki/Combinatory_logic#Completeness_of_the_S-K_basis), and finally a few "incantations" including encoding True and False boolean logic.