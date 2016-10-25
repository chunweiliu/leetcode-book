## Return Conditions

We can modifiy the return conditions in the end.

### Search for insert position
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