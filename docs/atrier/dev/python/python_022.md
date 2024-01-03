# 10 Python Hacks in 30 Seconds
Let’s dive in…


## 1. Checking All Element The Same in A List

```python
def all_equal(lst):
 """
 It returns True if all the elements in the list are equal, and False otherwise

 :param lst: The list to check
 :return: True or False
 """
 return lst[1:] == lst[:-1]

all_equal([1, 2, 3, 4, 5, 6]) # False
all_equal([1, 1, 1, 1]) # True
```

## 2. Checking All Element Unique in A List

```python
def all_unique(lst):
 """
 If the length of the list is the same as the length of the set of the list, then all the elements in
 the list are unique.
 
 :param lst: a list of values
 :return: True or False
 """
 return len(lst) == len(set(lst))

x = [1,2,3,4,5,6]
y = [1,2,2,3,4,5]
all_unique(x) # True
all_unique(y) # False
```

## 3. Separate One List into Two List Based on Filter

```python
def bifurcate(lst, filter):
    """
    It takes a list and a filter, and returns a list of two lists, the first containing all the elements
    of the original list that match the filter, and the second containing all the elements that don't

    :param lst: The list to be bifurcated
    :param filter: a list of boolean values
    :return: A list of two lists. The first list contains all the elements of the original list that
    correspond to a True value in the filter list. The second list contains all the elements of the
    original list that correspond to a False value in the filter list.
    """
    return [
        [x for i, x in enumerate(lst) if filter[i] == True],
        [x for i, x in enumerate(lst) if filter[i] == False]
    ]
```

## 4. Find Elements in One List but Not in Another

```python
def difference(a, b):
    """
    It returns a list of all elements in a that are not in b

    :param a: the list of all the words in the corpus
    :param b: the list of words to be removed from the vocabulary
    :return: A list of items in a that are not in b.
    """
    _b = set(b)
    return [item for item in a if item not in _b]

difference([1, 2, 3], [1, 2, 4]) # [3]
```

## 5. Flatten Nested Lists

```python
def flatten(lst):
    """
    For each element in the list, for each element in that element, add that element to the new list.

    :param lst: the list to be flattened
    :return: A list of all the elements in the list of lists.
    """
    return [x for y in lst for x in y]

flatten([[1,2,3,4],[5,6,7,8]]) # [1, 2, 3, 4, 5, 6, 7, 8]
```

## 6. Convert A Number into List of Digits

```python
def digitize(n):
    """
    It converts an integer into a list of its digits
    
    :param n: The number to be digitized
    :return: A list of the digits of the number n.
    """
    return list(map(int, str(n)))

digitize(123) # [1, 2, 3]
```

## 7. Shuffle List

```python
from random import randint
from copy import deepcopy

def shuffle(lst):
    """
    We create a copy of the list, then we iterate through the list, and for each element, we swap it
    with a random element in the list

    :param lst: the list to be shuffled
    :return: A list of the same length as the input list, but with the elements in a random order.
    """
    temp_lst = deepcopy(lst)
    m = len(temp_lst)
    while (m):
        m -= 1
        i = randint(0, m)
        temp_lst[m], temp_lst[i] = temp_lst[i], temp_lst[m]
    return temp_lst

foo = [1,2,3]
shuffle(foo) # [2,3,1] , foo = [1,2,3]
```

## 8. Return The Number if in Range or The Closest Boundary

```python
def clamp_number(num, a, b):
    """
    Return the number if it's between a and b, otherwise return the closest of a or b.

    :param num: the number to be clamped
    :param a: the minimum value of the range
    :param b: the number of bits in the binary representation of the number
    :return: the maximum of the minimum of the number and the maximum of a and b, and the minimum of a
    and b.
    """
    return max(min(num, max(a, b)), min(a, b))

clamp_number(2, 3, 5) # 3
clamp_number(1, -1, -5) # -1
```

## 9. Find String Byte Size

```python
def byte_size(string):
    """
    It returns the length of the string in bytes
    
    :param string: The string to get the length of
    :return: The length of the string in bytes.
    """
    return len(string.encode('utf-8'))

byte_size(' ') # 4
byte_size('Hello World') # 11
```

## 10. Greatest Common Divisor

```python
def gcd(numbers):
    """
    It takes a list of numbers and returns the greatest common divisor of all the numbers in the list
    
    :param numbers: a list of numbers
    :return: The greatest common divisor of the numbers in the list.
    """
    return reduce(math.gcd, numbers)

gcd([8,36,28]) # 4
```

## Conclusion:
Above are the 10 python hacks and hope it peeks your interest in some of python fundamentals. If more comes up, I am happy to add more. Feel free to share your ones and let’s learn from each other!

Source : https://medium.com/@caopengau/10-python-hacks-in-30s-1b86b0e91ff2