# String Interning in Python: A Hidden Gem That Makes Your Code Faster
Level up the speed of string comparisons

The string interning is a very helpful mechanism in Python to make string comparisons much faster. Unfortunately, how it works and how to use it properly are not very intuitive for beginners. If we don’t understand it, sometimes we could be confused about some unexpected results.
For example, the results of the following string comparisons may surprise you:

```python
>>> a = "Yang"
>>> b = "Yang"
>>> a is b
True
>>> c = "Yang Zhou"
>>> d = "Yang Zhou"
>>> c is d
False
```

As we knew, the `is` operator is used to compare whether two variables refer to the same object or not. Therefore, the results of the above code seem weird. **Why `a` and `b` refer to the same object but `c` and `d` are not?**

Simply put, it’s caused by the rules of the string interning mechanism in Python.

This article will talk about this mechanism, the rules behind it, and more importantly, the suggestions to use it properly.

## What Is the String Interning in Python?
The string interning in Python is a mechanism of storing only one copy of a string value in the memory. If there are a few string variables whose values are the same, they will be interned by Python implicitly and refer to the same object in the memory.
This mechanism introduces two advantages:
1. Save the space in the memory
1. Save the time when comparing strings whose values are the same

The first one is obvious, since storing only one copy needs less space than storing all of them.
The second one is because if two strings refer to the same object, they are definitely equal to each other and there is no need to compare the characters of them one by one.
However, nothing is perfect. Interning strings and maintaining the pool of interned strings also need time. If a string is very long and it will never be compared with others, the extra time to intern it may not necessary.
Therefore, in order to trade off the cost and profits, Python doesn’t intern all strings. There are some rules to determine whether a string will be interned or not.
Unfortunately, these rules make the things become complex. Because they may be different between the versions and the implementations of Python.
For the previous example, I use CPython, which is the most common implementation of Python, and the version of it is 3.8.5. In this version, any strings that include spaces will not be interned. So, the string “Yang” was interned but the string “Yang Zhou” was not.

## Don’t Rely on the Implicit String Interning
As we discussed, the rules of the implicit string interning could be different. As far as I am concerned, relying on the rules may result in unexpected bugs. Not to mention that remembering the rules adds an unnecessary burden to our developers. Because rules could change so fast, but we need to ensure the stability and robustness of our programs.
For example, up until the version of Python 3.7, the [peephole optimization](https://github.com/python/cpython/blob/0f21fe6155227d11dc02bd3ef3b061de4ecea445/Python/peephole.c) is used for string interning and all strings longer than 20 characters will not be interned. However, the algorithm was changed to the [AST optimizer](https://github.com/python/cpython/blob/3.7/Python/ast_opt.c) then, and the length is equal to 4096 rather than 20.
We can test it by simple code as follows:

```python
# version: Python 3.8.5
>>> a = 'Y'*4096
>>> b = 'Y'*4096
>>> a is b
True
>>> c = 'Y'*4097
>>> d = 'Y'*4097
>>> c is d
False
```

In practice, we prefer to focus on the logic of our projects instead of some implicit rules of the language. Therefore, my suggestion is that we shouldn’t implement some logic in our programs which is strongly relying on the implicit string interning mechanism.

## Interning Strings Explicitly To Speed Up Your Code
Python, as an elegant programming language, always opens a door for us. As to string interning, there is a built-in function that we can use to intern a string explicitly. It is the `intern()` function in the `sys` module.
Let’s see how it works by a simple example:

```python
>>> import sys
>>> c = sys.intern('Y'*4097)
>>> d = sys.intern('Y'*4097)
>>> c is d
True
```

As shown above, with the help of the `intern()` function, we can force Python to intern the strings no matter what the implicit rules are.
In practice, if we need to compare some long strings and the same value may appear many times, using the `intern()` function explicitly is a recommended way to speed up the string comparisons.
On the one hand, it can take advantage of the string interning mechanism in Python. On the other hand, it will not let us fall into the trap of complex rules.

## Don’t Use the “Is” Operator To Compare Values
One last thing worth to mention is that we should definitely use the `==` operator instead of the `is` to compare values of strings in practise.
Because the purposes of the two operators are different:
- The `is` operator is only for comparing identities (memory locations in CPython) of objects. **If the identities are different, it returns `False`**. It will never compare values.
- The `==` operator is for comparing values of objects. It compares identities of the objects firstly, since if they are the same, there is no need to compare values anymore. If the identities are different, it compares values then.
In this article, I used the `is` operator only for explaining how the interning mechanism affects the memory locations of strings.
In practice, we should use the `==` operator to compare strings. If we need to speed up the comparison, intern the strings explicitly.

## Conclusion
The string interning mechanism is a hidden gem in Python. We may don’t feel it for the first time, since Python will intern some strings implicitly. But if we totally understand it and use it explicitly and properly, it will be of great value to improve the speed of string comparisons.

---

Source : https://medium.com/techtofreedom/string-interning-in-python-a-hidden-gem-that-makes-your-code-faster-9be71c7a5f3e