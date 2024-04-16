---
title: Church Numbers
date: 2023-08-31 23:41:12
tags: [Math]
categories: [Python]
mathjax: true
typora-root-url: ..
---

Church Numbers are the union of `natural numbers ` which are represented by `lambda functions`.

### Math

Think that we need to use function to represent numbers, what method should we take?

The way to describe numbers by church numbers is that:

$0:x$

$1:f(x)$

and so on

So we can use codes to represent them easily by just call the function we need for the numbers times.

<!--more-->

### Code

First we need two functions in order to identify the other natural numbers:

```python
def zero(f):
    return lambda x: x

def successor(n):
    return lambda f: lambda x: f(n(f)(x))
```

It is not hard to find that:

```python
one = successor(zero)
# f(zero(f)(x)) -> f(x)
```

As you can see here, if we pass `zero` or `one` this church numbers into the `successor()`, we will finally get the number after them.

And also, there is a more interesting conclusion here, is that:

```
zero(f)(x) -> x
```

It means that `n(f)(x) == x`.

So we can simply describe all the natural numbers by lambda functions.

```python
def one(f):
    return lambda x: f(x)

def two(f):
    return lambda x: f(f(x))
```

Then how can we transform them to int?

By the code before, we will find that if we do like `two()`, it actually do twice of the passing parameter `f`.

So if we got a function that increase each time when we call it, and initialize it parameter to 0, it will be fine!

```python
def church_to_int(n):
    return n(lambda x: x + 1)(0)
```

We can just only use simple rules like this below to accomplish  the goal:

```python
def add_church(m, n):
    return lambda f: lambda x: n(f)(x) + m(f)(x)

def mul_church(m, n):
    return lambda f: lambda x: m(f)(x) * n(f)(x)

def pow_church(m, n):
    return lambda f: lambda x: m(f)(x) ** n(f)(x)
```

But here is a more advanced version:

```python
def add_church(m, n):
    return lambda f: lambda x: m(f)( n(f)(x) )

def mul_church(m, n):
    return lambda f: m(n(f))

def pow_church(m, n):
    return n(m)
```

Myself just finished the basic version and this version above is quite ingenious. 

Here is the link: [Church numbers](https://zhuanlan.zhihu.com/p/267917164).
