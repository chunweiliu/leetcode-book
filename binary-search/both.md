## Branching and Return Conditions

Sometime we have to modify both branching and return conditions.

### Search the first target in a list with duplicated numbers
```python
def search_first(numbers, target):
    """Keep update last to find the first target
    """
    if not numbers:
        return -1

    first, last = 0, len(numbers) - 1
    while first + 1 < last:
        mid = (first + last) / 2

        # Keep update last to find the first target.
        if numbers[mid] < target:
            first = mid
        else:
            last = mid

    if numbers[first] == target:
        return first
    if numbers[last] == target:
        return last
    return -1
```

### Search the last target in a list with duplicated numbers
```python
def search_last(numbers, target):
    """Keep update first to find the last target
    """
    if not numbers:
        return -1

    first, last = 0, len(numbers) - 1
    while first + 1 < last:
        mid = (first + last) / 2

        # Keep update first to find the last target
        if numbers[mid] <= target:
            first = mid
        else:
            last = mid

    # Return the last one first if it matches.
    if numbers[last] == target:
        return last
    if numbers[first] == target:
        return first
    return -1
```