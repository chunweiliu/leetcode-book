# Binary Search
* [Template](#template)
* [Search in rotated sorted array](#search-in-rotated-sorted-array)
* [Search insert position](#search-insert-position)
* [Search range](#search-range)
---

### [Template](#template)
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
---

### [Search in rotated sorted array](#search-in-rotated-sorted-array)
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
---

### [Search insert position](#search-insert-position)
```python
def search_insert_position(numbers, target):
    if not numbers:
        return -1

    first, last = 0, len(numbers) - 1
    while first + 1 < last:
        mid = (first + last) / 2

        if numbers[first] <= target:
            last = mid
        else:
            first = mid

    if numbers[first] == target:
        return first
    if numbers[last] == target:
        return last

    # indice: [x, first, x, last, x]
    if target < numbers[first]:
        return first
    if target < numbers[last]:
        return last
    return last + 1
```
---

### [Search range](#search-range)

```python
def search_range(numbers, target):
    return [search_first(numbers, target), search_last(numbers, target)]

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