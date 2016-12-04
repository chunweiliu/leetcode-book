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
	* [Palindrome Partitioning II](dynamic-programming.md#palindrome-partitioning-ii)
* [Double Sequence](dynamic-programming.md#doulbe-sequence)
	* [Longest Common Subsequence](dynamic-programming.md#longest-common-susequence)
	* [Edit Distance](dynamic-programming.md#edit-distance)
	* [Distinct Subsequence](dynamic-programming.md#distinct-subsequence)
	* [Interleaving String](dynamic-programming.md#interleaving-string)
* [Advance Topics](dynamic-programming.md#advance-topics)
	* [Best Time to Buy and Sell Stock](dynamic-programming.md#best-time-to-buy-and-sell-stock)
	* [Maximum Subarray](dynamic-programming.md#maximum-subarray)
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

### [Jump Game](#jump-game)

```python
def reachable(nums):
    """[Greedy] Update the furthest place we can visit 
    """
    furthest = 0
    for i, num in enumerate(nums):
        if i > furthest:
            break

        furthest = max(furthest, i + num)

    return furthest >= len(nums) - 1
```

### [Longest Increasing Subsequence](#longest-increasing-subsequence)

```python
def longest_increasing_subsequence(nums):
    """DP O(n^2)
    If i < j and nums[i] < nums[j], then the sequence of f(i) can be 
    the sequence of f(j)'s prefix.

    f(j) = max 1 + f(i) for 0 <= i < j, nums[i] < nums[j]
    """
    if not nums:
            return 0

    lis = [1] * len(nums)

    for j, num in enumerate(nums):
        lis[j] = max(
            [1 + lis[i] 
             for i, previous_num in enumerate(nums[:j])
             if previous_num < num] or [1])

    return max(lis)
```

## [Sequence](#sequence)

### [Word Break](#word-break)
```python

def word_break(self, s, word_dict):
    """DP O(n^2)
    
    break[j] is true if break[i] is true and s[i:j] in word_dict.

    Example: 
    d = {'face', 'book'}
    s =  'facebook'
    si    01234567
    b    TFFFTFFFT
    bi   012345678

    j = 4, i = 0
    break[4] = break[0] and s[0:4] in d

    j = 8, i = 4
    break[8] = break[4] and s[4:8] in d

    Question:
        - Is the word in dictionary reuseable?
    """

    constructeds = [False] * (len(s) + 1)
    constructeds[0] = True

    for j in range(len(constructeds)):
        for i in range(j):
            if constructeds[j]:
                break

            constructeds[j] = constructeds[i] and s[i:j] in word_dict

    return constructeds[-1]

```

### [Palindrome Partitioning](#palindrome-partitioning-ii)

```python
def min_cut(s):
    """DP O(n^2)
    """
    n = len(s)

    def update_cuts(left, right):
        while 0 <= left <= right < n and s[left] == s[right]:
            # Either include a new palindrome with one additional cut or not.
            cuts[1 + right] = min(cuts[1 + right], 1 + cuts[left])
            left -= 1
            right += 1
    
    cuts = [i - 1 for i in range(n + 1)]

    for i in range(n):
        left, right = i, i
        update_cuts(left, right)
        
        left, right = i, i + 1
        update_cuts(left, right)

    return cuts[-1]

```

### [Edit Distance](#edit-distance)

```python
def edit_distance(s, t):
    """DP O(n^2)
    """
    m, n = len(s), len(t)

    if not m:
        return n
    if not n:
        return m

    table = [[0] * (1 + n) for _ in range(1 + m)]

    for i in range(1 + m):
        table[i][0] = i
    for j in range(1 + n):
        table[0][j] = j

    for i in range(1, 1 + m):
        for j in range(1, 1 + n):
            table[i][j] = min(
                table[i - 1][j] + 1,
                table[i][j - 1] + 1,
                table[i - 1][j - 1] + (s[i] != t[j]))

    return table[-1][-1]

```

### [Interleaving String](#interleaving-string)

```python
def interleaving_string(s1, s2, s3):
    """
    Either choose s1 or s2 each time for s3.

    interleaved[i][j] = if s1[:i] and s2[:j] can form s3[:i + j]
    
    interleaved[i][j] = interleaved[i - 1][j] and s1[i] == s3[i + j] or
                        interleaved[i][j - 1] and s2[j] == s3[i + j]

    Hint:
        - padding s1, s2, and, s3 for better indexing
    """
    if len(s3) != len(s1 + s2):
        return False

    s1 = '0' + s1
    s2 = '0' + s2
    s3 = '0' + s3

    m, n = len(s1), len(s2)

    interleaved = [[False] * n for _ in range(m)]

    interleaved[0][0] = True

    i = 1
    while i < m and interleaved[i - 1][0] and s1[i] == s3[i]:
        interleaved[i][0] = True
        i += 1

    j = 1
    while j < n and interleaved[0][j - 1] and s2[j] == s3[j]:
        interleaved[0][j] = True
        j += 1

    for i in range(1, m):
        for j in range(1, n):
            # For example, when i = 1 and j = 1, you are checkecking s3[2],
            # which is the current character we care about. s3[1] has to be
            # filled by either s1[1] or s2[1] (after padding).
            interleaved[i][j] = interleaved[i - 1][j] and s1[i] == s3[i + j] or \
                                interleaved[i][j - 1] and s2[j] == s3[i + j]

    return interleaved[-1][-1]

```

## [Advance Topic](#advance-topic)

### [Best Time to Buy and Sell Stock](#best-time-to-buy-and-sell-stock)

```python
def max_profit_one_time(prices):
    """Only buy and sell one time.
    """
    if not prices:
        return 0

    profit, lowest_price = 0, price[0]
    for price in prices[1:]:
        profit = max(profit, price - lowest_price)
        lowest_price = min(lowest_price, price)

    return profit
```

```python
def max_profit_unlimited(prices):
    """Buy and sell as much as you can.
    """
    if not prices:
        return 0

    return sum(sell - buy for sell, buy in zip(prices[1:], prices[:-1]) if sell > buy)
```

```python
def max_profit(prices, k):
    """Buy and sell as much as k times.

    Time: O(n)
    Space: O(k)
    """
    if not prices:
        return 0

    if k >= len(prices) / 2:
        return max_profit_unlimited(prices)

    # b, bs, bsb, bsbs, ... [2 * k][bs]
    min_value = -max(prices)
    profits = [[min_value if i % 2 == 0 else 0 for i in range(2 * k)]
               for _ in range(2)]

    current, next = 0, 1
    for price in prices:
        for state in range(2 * k):
            buy = 1 if state % 2 == 0 else -1
            profits[next][state] = max(
                profits[current][state],
                profits[current][state - 1] - buy * price)
        current, next = next, current

    # Max profit happens with a least one sale.
    return max(profits[current])
```

### [Maximum Subarray](#dynamic-programming.md#maximum-subarray)

```python
def max_subarray(nums):
    if not nums:
        return 0

    max_overall, max_included_last = nums[0], nums[0]

    for num in nums[1:]:
        max_included_last = max(max_included_last + num, num)
        max_overall = max(max_overall, max_included_last)

    return max_overall
```

### [Burst Balloons](#burst-ballons)

```python
def max_coins(nums):
    if not nums:
        return 0

    nums = [1] + nums + [1]
    max_coins = [[0] * len(nums) for _ in range(len(nums))]

    for size in range(2, len(nums)):
        # (left, right)
        for left in range(len(nums) - size):
            right = left + size
            for last in range(left + 1, right):
                max_coins[left][right] = max(
                    max_coins[left][right],
                    nums[left] * nums[last] * nums[right] + max_coins[left][last] + max_coins[last][right])

    return max_coins[0][len(nums) - 1]
```
