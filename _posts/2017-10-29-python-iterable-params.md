---
title: Beware of iterable default params in Python
category: coding
date: 2017-10-29
photo-author-profile: keithmisner
photo-author: Keith Misner
header-url: https://images.unsplash.com/32/Mc8kW4x9Q3aRR3RkP5Im_IMG_4417.jpg?ixlib=rb-0.3.5&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=1900&fit=crop&s=8b9757eb8b586d7e8ee5506fc3437067
---

I got a question in this [Python assessment quiz](https://dotkomblog.com/learning/2017/10/27/the-biggest-challenge-in-self-learning/) the other day, that surprised me:

![](/img/posts/python_assess2.png)

I picked this as an answer:

``` python
[1]
[2]
[3]
```

but the right one was:

``` python
[1]
[1, 2]
[1, 2, 3]
```

So, the keyword param `items` retains its previous value, even though the function `f` is called repeatedly. This is not what I would intuitively expect from it. 

I don’t know about every other languages, but I knew that in JavaScript a similar code would work as I expected. So this:

``` js
function f (a, items=[]) {
  items.push(a)
  return items
}

console.log(f(1))
console.log(f(2))
console.log(f(3))
```

would gracefully output this:

``` js
[1]
[2]
[3]
```

It turns out, MDN explicitly calls out Python when explainig JS default params.

> The default argument gets evaluated at call time, so unlike e.g. in Python, a new object is created each time the function is called. ([Default parameters - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters))

I found the relevant section in the Python docs too, which warns about this behaviour. Strangely enough it also admits that this is not what was intended, and proposes a ‘workaround’ (which in turn appears all over the place in Python libraries):

> Default parameter values are evaluated from left to right when the function definition is executed. This means that the expression is evaluated once, when the function is defined, and that the same “pre-computed” value is used for each call. This is especially important to understand when a default parameter is a mutable object, such as a list or a dictionary: __if the function modifies the object (e.g. by appending an item to a list), the default value is in effect modified. This is generally not what was intended.__ A way around this is to use None as the default, and explicitly test for it in the body of the function ([The Python Language Reference](https://docs.python.org/3/reference/compound_stmts.html#function-definitions) - emphasis is mine)




