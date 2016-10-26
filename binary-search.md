# Binary Search
* [Template](#template)
* [Search in rotated sorted array](#search-in-rotated-sorted-array)
* [Search insert position](#search-insert-position)
* [Search range](#search-range)
* [Search a 2D Matrix](#search-a-2d-matrix)
* [Find Peak Element](#find-peak-element)
* [Find Minimum in Rotated Sorted Array](#find-minimum-in-rotated-sorted-array)
---

### [Template](#template)
```python
def binary_search(nums, target):
    """Find the first or last positon of the target in the number list
    """
    if not nums:
        return -1

    # Name first and last to imply that they are candidates.
    first, last = 0, len(nums) - 1

    # Loop until the first and last nums intersect or being side by side.
    while first + 1 < last:
        mid = (first + last) / 2
        if nums[mid] <= target:
            first = mid
        else:
            last = mid

    # We either have one or two nums here.
    if nums[first] == target:
        return first
    if nums[last] == target:
        return last
    return -1
```
---

### [Search in rotated sorted array](#search-in-rotated-sorted-array)
```python
def search_in_rotated_sorted(nums, target):
    """Assert the search result in the ordered side
    """
    if not nums:
        return -1

    first, last = 0, len(nums) - 1
    while first + 1 < last:
        mid = (first + last) / 2

        # Assert the search result in the ordered side.
        if nums[first] < nums[mid]:
            if nums[first] <= target <= nums[mid]:
                last = mid
            else:
                first = mid
        else:
            if nums[mid] <= target <= nums[last]:
                first = mid
            else:
                last = mid

    if nums[first] == target:
        return first
    if nums[last] == target:
        return last
    return -1
```
---

### [Search insert position](#search-insert-position)
```python
def search_insert_position(nums, target):
    if not nums:
        return -1

    first, last = 0, len(nums) - 1
    while first + 1 < last:
        mid = (first + last) / 2

        if nums[first] <= target:
            last = mid
        else:
            first = mid

    if nums[first] == target:
        return first
    if nums[last] == target:
        return last

    # indice: [x, first, x, last, x]
    if target < nums[first]:
        return first
    if target < nums[last]:
        return last
    return last + 1
```
---

### [Search range](#search-range)

```python
def search_range(nums, target):
    return [search_first(nums, target), search_last(nums, target)]

def search_first(nums, target):
    """Keep update last to find the first target
    """
    if not nums:
        return -1

    first, last = 0, len(nums) - 1
    while first + 1 < last:
        mid = (first + last) / 2

        # Keep update last to find the first target.
        if nums[mid] < target:
            first = mid
        else:
            last = mid

    if nums[first] == target:
        return first
    if nums[last] == target:
        return last
    return -1

def search_last(nums, target):
    """Keep update first to find the last target
    """
    if not nums:
        return -1

    first, last = 0, len(nums) - 1
    while first + 1 < last:
        mid = (first + last) / 2

        # Keep update first to find the last target
        if nums[mid] <= target:
            first = mid
        else:
            last = mid

    # Return the last one first if it matches.
    if nums[last] == target:
        return last
    if nums[first] == target:
        return first
    return -1
```
---

### [Search a 2D Matrix](#search-a-2d-matrix)
```python
def search_matrix(matrix, target):
    """Convert row and column to 1D index and search
    """
    def row_and_column_of(index, matrix):
        column_length = len(matrix[0])
        row = index / column_length
        column = index % column_length
        return row, column

    first, last = 0, len(matrix) * len(matrix[0]) - 1
    while first + 1 < last:
        mid = (first + last) / 2

        r, c = row_and_column_of(mid, matrix)
        if matrix[r][c] == target:
            return True

        if matrix[r][c] < target:
            first = mid
        else:
            last = mid

    r, c = row_and_column_of(first, matrix)
    if matrix[r][c] == target:
        return True
    r, c = row_and_column_of(last, matrix)
    if matrix[r][c] == target:
        return True
    return False
```
---

### [Find Peak Element](#find-peak-element)
```python
def find_peak_element(nums):
    """The peak is always on the greater side 
    """
    if not nums:
        return nums

    first, last = 0, len(nums) - 1
    while first + 1 < last:
        mid = (first + last) / 2

        if nums[mid - 1] < nums[mid] > nums[mid + 1]:
            return mid

        if nums[mid] < nums[mid + 1]:
            first = mid
        else:
            last = mid

    if nums[first] < nums[last]:
        return last
    return first
```
---

### [Find Minimum in Rotated Sorted Array](#find-minimum-in-rotated-sorted-array)
```python
def find_minimum(nums):
    """Find the minimum by visualizing the rotated sorted array
    """
    if not nums:
        return nums

    first, last = 0, len(nums) - 1
    while first + 1 < last:
        mid = (first + last) / 2

        #            9
        #          8   <--- top middle
        #        7         
        #      6
        #    5
        #  4
        # -0-1-2-3-4-5-6-7-8-9- [index] in array  
        #                    3
        #                  2
        #                1   <--- bottom middle
        #              0
        if nums[first] < nums[mid] > nums[last]:
            first = mid
        else:
            last = mid

    return min(nums[first], nums[last])
```