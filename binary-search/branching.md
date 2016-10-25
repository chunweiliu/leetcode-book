## Branching Conditions

We can modify the braching conditions in the loop.

### Search in a rotated sorted array
```python
def search_in_rotated_sorted(numbers, target):
    """Assert the search result in the ordered side
    """
    if not numbers:
        return -1

    first, last = 0, len(numbers) - 1
    while first + 1 < last:
        mid = (first + last) / 2

        # Assert the search result in the ordered side.
        if numbers[first] < numbers[mid]:
            if numbers[first] <= target <= numbers[mid]:
                last = mid
            else:
                first = mid
        else:
            if numbers[mid] <= target <= numbers[last]:
                first = mid
            else:
                last = mid

    if numbers[first] == target:
        return first
    if numbers[last] == target:
        return last
    return -1
```