# 5 Ways To Remove Spaces of a Python String
From simple methods to the ultimate weapon

Handling strings, especially messy strings, is the daily work of Python developers and data scientists. One of the most common symptoms of a messy string is that there are unnecessary or duplicate spaces in it.
Fortunately, no matter how messy a string is and how complicated a scenario is, Python can provide us a corresponding way to remove spaces as we like.
This article will introduce 5 different methods to remove spaces in a messy Python string based on different scenarios and requirements. After reading, you can choose the most appropriate way for your code. More importantly, you will feel the flexibility and elegance of Python again.

## 1. Remove Leading and Trailing Spaces of a Python String
The simplest case is that all the leading and trailing spaces of a string are unnecessary. For this, we can just use the `strip()` function to remove them all.
For example:

```python
quote = "   Yang is a full stack hacker.   "
clean_quote = quote.strip()
print(clean_quote)
# Yang is a full stack hacker.
```

The above code shows how easy it is to use the `strip()` function. There is one thing worth to mention: **Strings are [immutable](https://medium.com/techtofreedom/5-levels-of-understanding-the-mutability-of-python-objects-a5ed839d6c24) in Python**. The original `quote` string was not changed. We need to assign the result to a new string after removed the spaces.
In addition, there are two variants of the `strip()` function:
- `lstrip()`: only remove the leading spaces of a string
- `rstrip()`: only remove the trailing spaces of a string


## 2. Remove All Spaces of a Python String
Since removing spaces of a string is the same as replacing every space by an empty character, we can use the `replace()` function to do this job. This is a common method for cases that we need to remove all spaces of a string.

```python
name = " Ya  n   g   "
clean_name = name.replace(' ', '')
print(clean_name)
# Yang
```

## 3. Another Way To Remove All Spaces of a Python String
Python is very flexible and there is another choice for us to do the same job as the `replace()` function — the `translate()` function.

```python
name = " Ya  n   g   "
clean_name = name.translate({ord(' '): None})
print(clean_name)
# Yang
```

The above code shows the usage of this function. If it looks intuitive at the first glance, no worries at all. We just need to know the two rules of its argument:
- It must be a dictionary or a mapping table.
- If it’s a dictionary, we must use ASCII rather than characters.

The second rule is easy to follow. We can just use the built-in `ord()` function to convert a character into its corresponding ASCII.
The first rule gives us more flexibility. If we need to change many characters instead of just spaces, writing the `ord()` many times may be annoying. In this case, we can use the `maketrans()` function to generate a mapping table from the original characters at first:

```python
name = " Ya  n   g   "
map_table = name.maketrans({' ': None})
clean_name = name.translate(map_table)
print(clean_name)
# Yang
```

## 4. Remove Duplicate Spaces of a Python String
Sometimes, we need to eliminate duplicate spaces of a string instead of removing all spaces. In this scenario, we can use the following two functions together:
- `split()`: To split a string into a list based on a separator (any whitespace by default)
- `join()`: To combine all items in a list into a string using a separator

```python
quote = "   Yang   is a full   stack hacker."
new_quote = ' '.join(quote.split())
print(new_quote)
# Yang is a full stack hacker.
```

As the above example shows, the idea is clear:
1. Split all words into a list. So the duplicate spaces are avoided.
1. Combine all words again by the expected separator — `' '`.

## 5. Use Regular Expressions To Remove Spaces as You Like
The previous four ways are useful enough for many simple scenarios. However, real data may be very complicated and messy. The previous ways cannot handle all cases. (If you are a data scientist and cleaning messy data is part of your job, you will agree with me. 🙂)
Therefore, we sometimes have to use the ultimate weapon for string manipulations — the regular expression (RegEx).
If you are familiar with RegEx, all previous four examples can be handled by it easily. For more complicated cases, RegEx will sort them out as well.
For example, if we need to remove any two consecutive spaces in a string, but keep all single spaces, the previous four ways may be hard to help. But the RegEx, as the ultimate weapon, can do us a favour:

```python
import re

quote = "  Yang is a full stack ha  ck  er."
new_quote = re.sub(r"\s{2}", "", quote)
print(new_quote)
# Yang is a full stack hacker.
```

## Conclusion
Removing spaces of a Python string is a common task for developers. For some simple scenarios, some built-in functions of Python are useful enough. For more complicated cases, the RegEx is the ultimate weapon.