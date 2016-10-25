# Binary Search

Here is a basic binary search template, we can apply it to many problems.
```python
def binary_search(numbers, target):
    """Find the first or last positon of the target in the number list
    """
    if not numbers:
        return -1

    # Name first and last to imply that they are candidates.
    first, last = 0, len(numbers) - 1

    # Loop until the first and last numbers intersect or being side by side.
    while first + 1 < last:
        mid = (first + last) / 2
        if numbers[mid] <= target:
            first = mid
        else:
            last = mid

    # We either have one or two numbers here.
    if numbers[first] == target:
        return first
    if numbers[last] == target:
        return last
    return -1
```
