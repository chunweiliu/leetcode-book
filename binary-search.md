# Binary Search
* [Template](binary-search.md#template)
* [Search in rotated sorted array](binary-search.md#search-in-rotated-sorted-array)
* [Search insert position](binary-search.md#search-insert-position)
* [Search range](binary-search.md#search-range)
* [Sqrt(x)](binary-search.md#sqrt-x)
* [Search a 2D Matrix](binary-search.md#search-a-2d-matrix)
* [Find Peak Element](binary-search.md#find-peak-element)
* [Find Minimum in Rotated Sorted Array](binary-search.md#find-minimum-in-rotated-sorted-array)
* [Median of Two Sorted Arrays](binary-search.md#median-of-two-sorted-arrays)
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
Assume there is no duplicates in the array.
```python
def search_in_rotated_sorted(nums, target):
    """Assert the search result in the ordered side
    O(log n)
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

Otherwise, we have to handle the duplicates.
```python
def search_in_rotated_sorted(nums, target):
    if not nums:
        return False

    first, last = 0, len(nums) - 1
    while first + 1 < last:
        mid = (first + last) / 2

        if nums[mid] == target:
            return True

        # [0, 1, 1, 1, 1]
        # [1, 1, 1, 0, 1]
        if nums[mid] == nums[last]:
            last -= 1
        elif nums[mid] < nums[last]:
            if nums[mid] <= target <= nums[last]:
                first = mid
            else:
                last = mid
        else:
            if nums[first] <= target <= nums[mid]:
                last = mid
            else:
                first = mid

    if nums[first] == target or nums[last] == target:
        return True
    return False
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

### [Sqrt(x)](#sqrt-x)
```python
def my_sqrt(self, x):
    """Find a number r such that r ** 2 <= x and (r+1) ** 2 > x
    """

    def square(x):
        return x * x

    first, last = 0, x
    while first + 1 < last:
        mid = (first + last) / 2

        if square(mid) == x:
            return mid

        if square(mid) < x:
            first = mid
        else:
            last = mid

    # For x = 0
    if square(first) <= x and square(last) > x:
        return first
    return last
```
---

### [Search a 2D Matrix](#search-a-2d-matrix)

If the elements of the matrix are strictly increasing in order, we can apply binary search to them.
```python
def search_matrix(matrix, target):
    """Convert row and column to 1D index and search
    Time: O(log nm)
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

Otherwise, we still can eliminate a row or column each time.
```python
def search_matrix(matrix, target):
    """Eliminate a row or column each time
    Time: O(m + n)
    """
    cow, column = len(matrix), len(matrix[0])

    r, c = 0, column - 1
    while r < row and c >= 0:
        if matrix[r][c] == target:
            return True

        if matrix[r][c] < target:
            r += 1
        else:
            c -= 1
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

Assume no duplicates existed in the array.
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
        # The following statement is sufficient 
        # due to the array is ascending.
        # if nums[mid] > nums[last]:
        if nums[first] < nums[mid] > nums[last]:
            first = mid
        else:
            last = mid

    return min(nums[first], nums[last])
```

Otherwise, we need to handle it and the worst running time would be O(n).
```python
def find_minimum(nums):
    """Handle the duplicates
    Time: O(n)
        [0, 1, 1, 1, 1]
        [1, 1, 1, 0, 1]
    """
    if not nums:
        return nums

    first, last = 0, len(nums) - 1
    while first + 1 < last:
        mid = (first + last) / 2

        if nums[mid] == nums[last]:
            last -= 1
        elif nums[mid] < nums[last]:
            last = mid
        else:
            first = mid

    return min(nums[first], nums[last])
```
---

### [Median of Two Sorted Arrays](#median-of-two-sorted-arrays)
```python
def find_median(nums1, nums2):
    """For a total 10 elements, median is the 5th one. We compare the 2nd
    element at nums1 and the 3rd element at num2, and eleminate the smaller
    part. That is, we will remove either 2 elments or 3 elements.

    In the next iteration, we look for either the 3rd or 2nd larger element.
    """
    total = len(nums1) + len(nums2)
    if total % 2 == 1:
        return self.find_kth_element(nums1, nums2, total / 2 + 1)
    return 0.5 * (self.find_kth_element(nums1, nums2, total / 2) +
                  self.find_kth_element(nums1, nums2, total / 2 + 1))

def find_kth_element(self, nums1, nums2, k):
    # Assert |nums1| > |nums2|
    if len(nums1) < len(nums2):
        return self.find_kth_element(nums2, nums1, k)

    if not nums2:
        return nums1[k - 1]

    if k == 1:
        return min(nums1[0], nums2[0])

    m = k / 2
    n = min(k - m, len(nums2))
    if nums1[m - 1] < nums2[n - 1]:
        return self.find_kth_element(nums1[m:], nums2, k - m)
    return self.find_kth_element(nums1, nums2[n:], k - n)
```