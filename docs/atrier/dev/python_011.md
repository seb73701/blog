# Make Your Python Program Bug-Free: 8 Essential Tips

Although bugs are nearly inevitable in programming, senior developers can avoid lots of unnecessary bugs and write robust programs.
Why?

On the one hand, they made enough mistakes before thus they can avoid them in similar scenarios.

On the other hand, they are familiar with the programming language they are using, so they know where are the bug-prone points and how to deal with them.

This article will talk about 8 essential points to avoid bugs in Python programs based on my 8-year experience. Hope it’s useful.

## 0. Mind the Scope of Variables
The scope of Python variables is important but not intuitive for beginners. Bugs may be introduced if we don’t totally understand it.

Let’s see a simple example:

```python
score = 100
def test():
    score =0
    print(score)

test()
# 0
print(score)
# 100
```

As shown above, the statement `score=0` inside the `test()` function will not change the global variable’s value. Because Python treats the inside `score` as a local variable and the outside `score` as a global variable.
If we really want to change the global variable inside the function, add a statement with the `global` keyword:

```python
score = 100

def test():
    global score
    score = 0
    print(score)

test()
# 0
print(score)
# 0
```

In fact, if the global variables are some constant values that we will not change, the best practice is to put them together inside a class and name them by uppercase letters. When we need to use a global constant variable, just call it by this class. This way can make everything clear.

```python
class CONST:
    SCORE = 100

def test():
    print(CONST.SCORE)

```


## 1. Don’t Change the Type of a Variable if You Don’t Have To
Python is a dynamic typing language. This is convenient in most cases. However, if we abuse this feature, our program will be bug-prone.
For example, if we define an integer variable named `score` on a program:

```python
score = 100
```

The best practice is to keep its type always being an integer.
If we re-assign another type of value to this variable later, such as a list:

```python
score = [1, 5, 100]
```

Of course, it’s okay in Python to change the variable’s type at any time.
But if our program becomes larger and we need to use this variable again at some point, we may forget its latest type and handle it incorrectly. Not to mention that if another developer needs to write code based on the current program, he or she probably will be confused about the types.
Therefore, keeping the types of variables being consistent is a good habit.

## 2. Be Careful When Get Values From Dictionaries
The most intuitive way to get a value from a Python dict is:

```python
dict[key]
```

However, if the dict doesn’t have this key, a `KeyError` will be raised as the following example:

```python
test = {'A': 5, 'B': 2}
print(test['C'])
# KeyError: 'C'
```

To avoid unexpected errors, we can use the exception handling syntax of Python (the `try...except...finally` structure). However, it makes us write more code and it’s ugly more or less.
A better idea is to always access values from a dict with the `get()` function:

```python
test = {'A': 5, 'B': 2}
print(test.get('C'))
# None
```

As shown above, the `get()` function will just return `None` if a key doesn’t exist. No errors will be raised.
Another benefit is that we can define the return value as the second argument of the `get()` function:

```python
test = {'A': 5, 'B': 2}
print(test.get('C', 0))
# 0
```


## 3. Don’t Use the Eval Function in Python
If you don’t know what the `eval()` function is, congratulations and please jump to the next tip.
But if you are curious, let’s see an example of how powerful the `eval()` function is:

```python
def cal(a, b, op): return eval(f'{a} {op} {b}')
```

The above line of code define a function:
+ It receives two numbers `a`,`b` and an operator (`+` `-` `*` or `/`).
+ It returns the result of the calculation (`a+b`, `a-b`, `a*b`, or `a/b`).

It looks cool at the first glance. Cause we don’t need to write many `if-else` statements to check out which operator was received and only one line of code can solve the problem.

But, don’t use the `eval()` function unless you are 100% sure that the inputs are trusted. If the inputs are from an evil attacker, you may cry.

Let’s see how an attacker will delete all your files by the previous function:

```python
def cal(a, b, op): return eval(f'{a} {op} {b}')

cal('','',__import__('os').system('rm -rf *'))
```

Therefore, don’t use the eval function in Python to keep your system safe.

## 4. Know What Will be Returned From a Function
A function in Python will return `None` by default if you don’t specify a return value for it.

```python
def test():
    pass
print(test())
# None
```

It’s just a simple feature. But what is worth mentioning here is:
We cannot return anything except `None` in the `__init__()` function inside a class. Otherwise, a `TypeError` will be raised.

```python
class Yang:
    def __init__(self):
        return 0
y = Yang()
# TypeError: __init__() should return None, not 'int'
```


## 5. Don’t Use Mutable Objects As Default Arguments
The default arguments of a function are the places where bugs love to stay. Let’s see an interesting example:

```python
def show_my_gadgets(gadget, gadget_list=[]):
    gadget_list.append(gadget)
    print(gadget_list)


show_my_gadgets("PS7")
# ['PS7']
show_my_gadgets("iPhone 17")
# ['PS7', 'iPhone 17']
show_my_gadgets("iPad Pro 2077")
# ['PS7', 'iPhone 17', 'iPad Pro 2077']
```

The above code looks weird. We call the `show_my_gadgets()` function three times, but the `gadget_list` is always the same one.
In fact, the mistake of the above example is using a mutable object as a default argument in the function.
Since a list is a mutable object, the `gadget_list` will be bind into an empty list once when the function is defined and the same list will be used again and again in the successive function calls.
Therefore, if a default argument is a mutable object. The best practice is to define it as `None`:

```python
def show_my_gadgets(gadget, gadget_list=None):
    if gadget_list is None:
        gadget_list = []
    gadget_list.append(gadget)
    print(gadget_list)


show_my_gadgets("PS7")
# ['PS7']
show_my_gadgets("iPhone 17")
# ['iPhone 17']
show_my_gadgets("iPad Pro 2077")
# ['iPad Pro 2077']
```

## 6. Don’t Write Complex Lambda Functions

Lambda functions give us more abilities to make our programs cleaner and shorter.

For instance, the following one-line code defines a function which can receive a number “N” and return the Nth Fibonacci number:

```python
fib = lambda x: x if x<=1 else fib(x-1) + fib(x-2)
```

It looks elegant and neat. Isn’t it?
But my suggestion is not to overuse the lambda function syntax. The best practice is to use it only for simple functions.
Although the lambda functions do make our code shorter, shorter code may not always be better. Writing more lines of code can avoid accidental errors and improve the readability of our code.



## 7. Mind the Differences Between Assignment and Copy
In Python, assignments are not the same as copy. In other words, we should be careful about which is pass-by-reference and which is pass-by-value.
Compare the following two programs:

```python
A = [1, 2, 3]
B = A
B.append(4)
print(A)
# [1, 2, 3, 4]
print(B)
# [1, 2, 3, 4]
```
Description : Assign A to B

```python
import copy

A = [1, 2, 3]
# B = A.copy()
B = copy.copy(A)  # The same as `B = A.copy()`
B.append(4)

print(A)
# [1, 2, 3]
print(B)
# [1, 2, 3, 4]
```
Description : Copy A to B

As the above examples show, assign the list `A` to the list `B` is actually pass-by-reference. So when we change `B`, the A will be changed as well.
On the other hand, copy `A` to `B` will really “copy” the list `A`, and no matter how we change the list `B`, `A` will not be affected.

![](https://miro.medium.com/max/2400/0*pq9T37oKPmy24Ke1.jpeg)

In fact, we don’t need to import the “copy” module. There are 3 ways to copy a list, and the third is the most convenient one:
* `B = A.copy()`
* `B = copy.copy(A)`
* `B = A[:]`

## Conclusion

Python is easy to learn but hard to master. Devils are always in the details. Besides these 8 points, there are more and more issues worth to mention. The ultimate way to improve your Python programming skills and make your coding bug-free is to keep practising, summarising and taking notes whenever you find a new thing.

---
Source : https://medium.com/techtofreedom/make-your-python-program-bug-free-8-essential-tips-1700bceb5b8