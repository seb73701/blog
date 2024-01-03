# 5 Common Anti-Patterns in Python

Avoid using list comprehensions when input is large and more

> “It’s not enough for code to work.” — Robert C. Martin

Python is one of the most popular languages in 2021.
The simple syntax makes it a popular choice for newbie programmers.
Because of the dynamic nature and flexibility, Python developers are sometimes prone to writing faulty, inefficient code.
This article will introduce readers to common anti-patterns found in Python and propose fixes that will lead to better developer practices.
## 1. Using map() and filter() On Iterable Objects
The inbuilt `map` and `filter` helps us transform iterable objects in Python via the principles of [functional programming](https://www.geeksforgeeks.org/functional-programming-paradigm).
Both methods accept a function and an iterable as arguments and return a corresponding object.
This object can be transformed into a list by passing it as an argument into the inbuilt `list` constructor in Python.
We often use `lambda` functions as arguments to the `map` , `filter` functions:

```python
my_list = [1, 2, 3, 4 ,5, 6, 7, 8, 9, 10]

# Multiply each element by 2
print(list(map(lambda x: x * 2, my_list))) 
# [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]

# Filters out even numbers
print(list(filter(lambda x: x % 2 == 0, my_list))) 
# [2, 4, 6, 8, 10]
```

The code looks quite clunky and unclear. A better way to achieve the same outcome is to use **[list comprehensions](https://www.w3schools.com/python/python_lists_comprehension.asp)**:

```python
my_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Same as map
print([x * 2 for x in my_list])
# [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]

# Same as filter
print([x for x in my_list if x % 2 == 0])
# [2, 4, 6, 8, 10]
```

The lack of lambda functions makes list comprehensions much more readable and concise.

## 2. Using List Comprehensions When Input Is Large
List comprehensions are awesome for writing clear, concise code.
However, a list comprehension always creates a list of each value in the iterable. This can lead to memory problems when the input is very large; it can cause your machine to crash.
**[Generator expressions](https://www.geeksforgeeks.org/generator-expressions/)** provide a more efficient way to deal with large input sequences by combining the best of both worlds of list comprehensions and generators.
To create them, all you have to do is replace the `[]` brackets in a list comprehension with the `()` brackets.

![](https://miro.medium.com/max/2400/1*P25IfcBcUmeDwXohGySP9A.png)

Instead of composing an entirely new list, generator expressions create an iterator.
This **reduces the speed of creation** and **optimizes memory allocation**. You can access every subsequent element of the generator expression by using the `next` function or by looping over it.

```python
my_list = [1, 2, 3, 4 ,5, 6, 7, 8, 9, 10]

my_gen_expr = (x * 2 for x in my_list)

print(next(my_gen_expr))
print(next(my_gen_expr))
# >>
# 2
# 4

for x in my_gen_expr:
  print(x)
# >>
# 6
# 8
# 10
# 12
# 14
# 16
# 18
# 20
```

**Note:**
Generator expressions are stateful, so be careful when trying to reuse them. You may need to recreate the iterator if you plan on using it more than once.

## 3. The Case for Not Using range()
The `range` function is useful for iterating over integers.

```python
for i in range(10):
    print(i)
```

When iterating over data structures that are iterable like lists, you can rely purely on the for loop syntax in order to access each item. Here’s the code:

```python
my_list = [2, 4, 6, 8, 10]

for item in my_list:
    print(item)

# Output:
# 2
# 4
# 6
# 8
# 10
```

However, when wanting to access the indices and the elements, we can use the `range` method on the length of the list in order to do this, as follows:

```python
my_list = [2, 4, 6, 8, 10]

for i in range(len(my_list)):
    print("index: ", i, "value: ", my_list[i])
    
# Output:
# index: 0 value: 2
# index: 1 value: 4
# index: 2 value: 6
# index: 3 value: 8
# index: 4 value: 10
```

The code looks unreadable because you have to call `len` on the list and then wrap the output using `range`. To make it more Pythonic, we must improve code readability.
A better way to do this is to call the `enumerate` function on the list object. This will create a generator that yields the indices and the values of the list items.

```python
my_list = [2, 4, 6, 8, 10]

for i, v in enumerate(my_list):
    print("index: ", i, "value: ", v)
    
# Output:
# index: 0 value: 2
# index: 1 value: 4
# index: 2 value: 6
# index: 3 value: 8
# index: 4 value: 10
```

Doesn’t this look much cleaner?

## 4. The Problem With Missing Dictionary Keys
The ability of dictionaries to have fast access, assignments, inserts, and deletes makes it a very popular data structure.
Newbie developers often run into issues when **trying to access a key that does not exist in the dictionary**.

```python
crypto_price = {
  "Bitcoin": 64000,
  "Ethereum": 2300,
  "Dogecoin": 0.12
}

crypto_price["XRP"]
```

![](https://miro.medium.com/max/2400/1*LJWoV9J2rleWVbLK2wwaYg.png)


One way to handle this would be to either check whether the key exists in the dictionary or not, and here’s the code to help you:

```python
key = "XRP"

if key not in crypto_price:
    crypto_price[key] = 1.2
    
print(crypto_price[key])
```

Another way to accomplish this is to use the try/except block, as follows:

```python
key = "XRP"

try:
    xrp = crypto_price[key]
except KeyError:
    xrp = 1.2
    
crypto_price[key] = xrp
```

The code above does accomplish our goal but we can improve it further by using the dictionary method `get`.
Instead of accessing a key of a dictionary using the square brackets `[]` , you can use the `get` method in order to get the value of the respective key.
Additionally, if the key does not exist, the `get` method returns `None` which is favorable instead of throwing a `KeyError` . You can also pass an argument to the `get` method to get a default value should there be an absent key instead of `None` .

```python
key = "XRP"

if crypto_price.get("XRP") is None:
  crypto_price["XRP"] = 1.2
```

```python
ada = crypto_price.get("ADA", 0)

# Prints 0
print(ada)
```


## 5. Lazy Keyword and Positional Arguments Design
Python functions have the ability to accept both **positional** and **keyword** arguments.
> A positional argument is a name that is not followed by an equal sign (=) and default value.
> A keyword argument is followed by an equal sign and an expression that gives its default value.
Because of this design, python functions are extremely flexible to create and reuse.
However, bad design choices when defining functions can lead to bugs in the code that are hard to fix.
Let’s take an example of a function that computes [compound interest](https://www.investopedia.com/terms/c/compoundinterest.asp):

```python
# Compound Interest calculator compounded annually/monthly
def calculate_compound_interest(principal, rate, time_in_years,
                                compounded_monthly, to_string):
  t = 1
  if compounded_monthly:
    t = 12
  amt = principal * (1 + rate/(t * 100)) ** (time_in_years * t)
  if to_string:
    return f"${amt - principal:.2f}"

  return amt - principal
   
  
  calculate_compound_interest(100, 5, 2, False, False)
  # 10.25
  
```

One issue that arises when calling the function is that the two boolean arguments, `compounded_monthly` and `to_string` at the end can easily be confused for one another. This could lead to issues that are difficult to track.
We could improve readability by changing the function definition like so:

```python
# Compound Interest calculator compounded annually/monthly
def calculate_compound_interest(principal, rate, time_in_years,
                                compounded_monthly=False, to_string=False):
```

By specifying the two boolean arguments as keyword arguments, the function caller can explicitly specify the boolean values they want to set which will override the default value.

```python
calculate_compound_interest(100, 5, 2, compounded_monthly=True)
# 10.49413355583269

calculate_compound_interest(100, 5, 2, to_string=True)
# '$10.25'
```

However, the main reason this will still cause problems is that the keyword arguments are optional and hence, nothing enforces the caller to use these as keyword arguments.
Therefore, the function can still be called using the old method:

```python
calculate_compound_interest(100, 5, 2, False, False)
```

A fix for this issue is enforcing the boolean arguments to be keywords only when defining the function:

```python
# Compound Interest calculator compounded annually/monthly
def calculate_compound_interest(principal, rate, time_in_years, *, # Changed
                                compounded_monthly=False, to_string=False):
```

We see that the `*` symbol indicates the end of positional arguments and the beginning of keyword-only arguments.
Now calling it like this:

```python
calculate_compound_interest(100, 5, 2, False, False)
```

will lead to an error:
```python
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-32-faf75d2ad121> in <module>
----> 1 print(calculate_compound_interest(1000, 5, 2, False, False))
TypeError: calculate_compound_interest() takes 3 positional arguments but 5 were given
```

However, the keyword arguments and their default behavior will still be intact, as follows:

```python
calculate_compound_interest(100, 5, 2, compounded_monthly=True)
# 10.49413355583269

calculate_compound_interest(100, 5, 2, to_string=True)
# '$10.25'
```

However, there still remains one problem.
Let’s say that the caller decides to use a mix of positional and keywords for the first three required arguments (`principal`, `rate`, `time_in_years`).
In the event that the function argument names are changed for these three parameters, we will see the Python interpreter complain. It’ll say something like the following:

```python
# Compound Interest calculator compounded annually/monthly
def calculate_compound_interest(p, r, t_in_y, *, # Changed
                                compounded_monthly=False, to_string=False):
```

```python
calculate_compound_interest(principal=1000, rate=5, time_in_years=2)

calculate_compound_interest(1000, 5, time_in_years=2)
```


```python
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-36-42e7ec842cd5> in <module>
----> 1 calculate_compound_interest(principal=1000, rate=5, time_in_years=2)
TypeError: calculate_compound_interest() got an unexpected keyword argument 'principal'
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-37-1bc57c40980f> in <module>
----> 1 calculate_compound_interest(1000, 5, time_in_years=2)
TypeError: calculate_compound_interest() got an unexpected keyword argument 'time_in_years'
```

Because we did not account for the caller to use the positional parameters explicitly, our code breaks.
A solution to this was introduced in Python 3.8 where we can redefine the function using the `/` parameter which indicates where the positional-only arguments end. Here’s the code:

```python
# Compound Interest calculator compounded annually/monthly
def calculate_compound_interest(p, r, t_in_y, /, *, # Changed
                                compounded_monthly=False, to_string=False):
```

Now calling the function like so yields proper results:

```python
calculate_compound_interest(100, 5, 2, compounded_monthly=True)
# 10.49413355583269

calculate_compound_interest(100, 5, 2, to_string=True)
# '$10.25'
```

However, if it is called like this:

```python
calculate_compound_interest(p=1000, r=5, t_in_y=2)
```

then the appropriate error is shown:

```python
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-21-883e876a7e8b> in <module>
----> 1 calculate_compound_interest(p=1000, r=5, t_in_y=2)
      2
TypeError: calculate_compound_interest() got some positional-only arguments passed as keyword arguments: 'p, r, t_in_y'
```

Source : https://betterprogramming.pub/5-common-anti-patterns-in-python-a9d6443fabe4