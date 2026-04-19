# Homework 1 – Efficient Data Manipulation Using NumPy

## Question
Implement a function that takes a 1D NumPy array of integers and returns an array containing elements that are multiples of 5 and leave a remainder of 1 when divided by 2.

---

## Solution

```python
import numpy as np

def homework(a):
    idx1 = (a % 5 == 0)
    idx2 = (a % 2 != 0)
    mask = idx1 & idx2
    my_result = a[mask]
    return my_result
```

## Explanation

The solution uses NumPy boolean indexing to filter elements.

First, we select numbers divisible by 5
Then, we select odd numbers (remainder of 1 when divided by 2)
Finally, we combine both conditions using a logical AND operation

## Example
```
a = np.array([1, 5, 10, 3, 4, 25, 30])
print(homework(a))
```


## Output:
```
[ 5 25 ]
