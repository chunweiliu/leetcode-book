# Dynamic Programming
* [Coordinates](dynamic-programming.md#coordinates)
	* [Triangle](dynamic-programming.md#triangle)
	* [Unique Paths](dynamic-programming.md#unique-paths)
	* [Climbing Stairs](dynamic-programming.md#climbing-stairs)
	* [House Robber](dynamic-programming.md#house-robber)
	* [Minimum Path Sum](dynamic-programming.md#minimum-path-sum)
	* [Jump Game](dynamic-programming.md#jump-game)
	* [Longest Increasing Subsequence](dynamic-programming.md#longest-increasing-subsequence)
* [Sequence](dynamic-programming.md#sequence)
	* [Word Break](dynamic-programming.md#word-break)
	* [Palindrome Partitioning](dynamic-programming.md#palindrome-partitioning)
* [Double Sequence](dynamic-programming.md#doulbe-sequence)
	* [Longest Common Subsequence](dynamic-programming.md#longest-common-susequence)
	* [Edit Distance](dynamic-programming.md#edit-distance)
	* [Distinct Subsequence](dynamic-programming.md#distinct-subsequence)
	* [Interleaving String](dynamic-programming.md#interleaving-string)
* [Advance Topics](dynamic-programming.md#advance-topics)
	* [Best Time to Buy and Sell Stock](dynamic-programming.md#best-time-to-buy-and-sell-stock)
	* [Maximum Subarray](dynamic-programming.md#maximum-subarray)
	* [Coin in A Line](dynamic-programming.md#coin-in-a-line)
	* [Stone Game](dynamic-programming.md#stone-game)
	* [Burst Balloons](dynamic-programming.md#burst-ballons)
	* [Scramble String](dynamic-programming.md#scramble-strings)

---

## [Coordinates](#coordinates)

### [Triangle](#triangle)

```python
def min_path_total(triangle):
    """For i > 0, j > 0:
        node[i][j] = value[i][j] + min(node[i - 1][j - 1], node[i - 1][j])
    """
	if not triangle or not triangle[-1]:
        return 0

	for i, row in enumerate(triangle):
        for j, node in enumerate(row):
	        if i == 0 and j == 0:
                continue

            if j == 0:
                triangle[i][j] += triangle[i - 1][j]
            elif j == len(row) - 1:
                triangle[i][j] += triangle[i - 1][-1]
            else:
                triangle[i][j] += min(triangle[i - 1][j - 1], triangle[i - 1][j])
            
    return min(triangle[-1])
```

---

### [Unique Paths](#unique-paths)

```python
def unique_paths(row, column):
    """For x > 0, y > 0:
        paths[i][j] = paths[i - 1][j] + paths[i][j - 1]
    """
    if row == 0 or column == 0:
        return 0

    paths = [1] * column

    for _ in range(1, row):
        for j in range(1, column):
            paths[j] += paths[j - 1]
    
    return paths[-1]
```

```python
def unique_paths(obstacles):
    """
    """
    if not obstacles or not obstacles[0]:
        return 0

    row, column = len(obstacles), len(obstacles[0])

    # Initalize the first row paths
    paths = [0] * column
    for j in range(column):
        if obstacles[0][j] == 1:
            break
        paths[j] = 1


    for i in range(1, row):
        for j in range(column):
            if obstacles[i][j] == 1:
                paths[j] = 0
            # Aware the first column
            elif j > 0:
                paths[j] += paths[j - 1]

    return paths[-1]
```

---

### [Climbing Stairs](#climbing-stairs)

```python
def climbing_stair_ways(stairs):
    """
    ways[1] = 1
    ways[2] = 2
    For i > 2:
        ways[i] = ways[i - 1] + ways[i - 2]
                  (2-step-ways) (1-steps-ways)
    """
    one_step_ways, two_step_ways = 1, 2

    for _ in range(3, n):
        two_step_ways, one_step_ways = one_step_ways + two_step_ways, two_step_ways

    if n == 1:
        return one_step_ways
    if n == 2:
        return two_step_ways
    return one_step_ways + two_step_ways
```

---

### [House Robber](#house-robber)
```python
def max_value(nums):
    """Either rob ith house or not
        f(-2) = 0
        f(-1) = 0
        f(0)  = nums[0]
        f(1)  = max(nums[0], nums[1])
        ...
        f(i)  = max(f(i - 2) + nums[i], f(i - 1))
    """
    not_robbed_last, robbed_last = 0, 0

    for num in nums:
        current = max(not_robbed_last + num, robbed_last)
        not_robbed_last, robbed_last = robbed_last, current

    return max(not_robbed_last, robbed_last)

```

