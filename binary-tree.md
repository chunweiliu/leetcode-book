#Binary Tree
* [Binary Tree Preorder Traversal](binary-tree.md#binary-tree-preorder-traversal)
* [Binary Tree Inorder Traversal](binary-tree.md#binary-tree-inorder-traversal)
* [Binary Tree Postorder Traversal](binary-tree.md#binary-tree-postorder-traversal)
* [Binary Tree Level Order Traversal](binary-tree.md#binary-tree-level-order-traversal)
* [Maximum Depth of Binary Tree](binary-tree.md#maximum-depth-of-binary-tree)
* [Minimum Depth of Binary Tree](binary-tree.md#minimum-depth-of-binary-tree)
* [Balanced Binary Tree](binary-tree.md#balanced-binary-tree)
* [Binary Tree Maximum Path Sum](binary-tree.md#binary-tree-maximum-path-sum)
* [Lowest Common Ancestor of a Binary Tree](binary-tree.md#lowest-common-ancestor-of-a-binary-tree)

---

### [Binary Tree Preorder Traversal](#binary-tree-preorder-traversal)

```python
def preorder(root):
    """Depth-first search.

    Get the node value when we see the node in the first time.
    """
	values = []

    stack, current = [], root
    while stack or current:
        if current:
            values.append(current.val)
            stack.append(current)
            current = current.left
        else:
            current = stack.pop()
            current = current.right

    return values
```

```python
def preorder(root):
	"""Breath-first search.
	"""
	values = []

	if not root:
		return []

	stack = [root]
	while stack:
		current = stack.pop()
		values.append(current.val)
		if current.right:
			stack.append(current.right)
		if current.left:
			stack.append(current.left)

	return values
```

---

### [Binary Tree Inorder Traversal](#binary-tree-inorder-traversal)

```python
def inorder(root):
	"""Depth-first search.

	Get the node value when we traverse back from the left subtree.
	"""
	values = []

    stack, current = [], root
    while stack or current:
        if current:
            stack.append(current)
            current = current.left
        else:
            current = stack.pop()
            values.append(current.val)
            current = current.right

    return values
```

---

### [Binary Tree Postorder Traversal](#binary-tree-postorder-traversal)

```python
def postorder(root):
	"""Depth-first search.

	Get the node value when we traverse back from the right subtree.
	"""
	values = []

	previous = None
	stack, current = [], root
	while stack or current:
		if current:
			stack.append(current)
			current = current.left
		else:
			# Need to preserve the node when we traverse the right subtree.
			peek = stack[-1]
			if not peek.right or peek.right == previous:
				values.append(peek.val)
				previous = stack.pop()
			else:
				current = peek.right
	return values
```

```python
def postorder(root):
	"""Breadth-first search.

	Use two stacks to reverse the visit order.
	"""
	values = []

	stack2 = []

	stack1 = [root] if root else []  # Aware edge case.
	while stack1:
		current = stack1.pop()
		stack2.append(current)

		if current.left:
			stack1.append(current.left)
		if current.right:
			stack1.append(current.right)

	while stack2:
		values.append(stack2.pop().val)

	return values
```

---

### [Binary Tree Level Order Traversal](#binary-tree-level-order-traversal)
```python
def level_order(root):
	level_values = []

    level = [root] if root else []
    while level:
        level_values.append([node.val for node in level])

        next_level = []
        for node in level:
            if node.left:
                next_level.append(node.left)
            if node.right:
                next_level.append(node.right)
        level = next_level

    return level_values
```

---

### [Maximum Depth of Binary Tree](#maximum-depth-of-binary-tree)

```python
def max_depth(root):
	"""DFS.

	The *depth* of a node is the number of edges from the node to the tree's root node. A root node will have a depth of 0.

	The *height* of a node is the number of edges on the longest path from the node to a leaf. A leaf node will have a height of 0.
	"""

	if not root:
		return 0
	return 1 + max(max_depth(root.left), max_depth(root.right))
```

### [Minimum Depth of Binary Tree](#minimum-depth-of-binary-tree)
```python
def min_depth(root):
	"""BFS
	"""

	if not root:
		return 0

	level, depth = [root], 1
	while level:

		next_level = []
		for node in level:
			if not node.left and not node.right:
				return depth

			if node.left:
				next_level.append(node.left)
			if node.right:
				next_level.append(node.right)

		depth += 1
		level = next_level
```

---

### [Balanced Binary Tree](#balanced-binary-tree)

```python
def is_balanced_tree(root):
    def height_or_unbalanced(root):
        """DFS.

        Either left or right tree is unbalanced, then we
        shouldn't waste time on calling the subroutine.
        """
        if not root:
          return 0

        left_height = height_or_unbalanced(root.left)
        if left_height == -1:
            return -1

        right_height = height_or_unbalanced(root.right)
        if right_height == -1:
            return -1

        if abs(left_height - right_height) > 1:
            return -1

        return 1 + max(left_height, right_height)

    return height_or_unbalanced(root) != -1
```

---

### [Binary Tree Maximum Path Sum](#binary-tree-maximum-path-sum)


```python
def max_path_sum(root):
	"""Each path in this tree is either 

	1) starting from a root to a leaf, or 
	2) across a root from one subtree to another. 

	We exam each node as a root in this tree.
	"""

	def max_path_to_root(root):
		if not root:
            return 0

        left_max_value = max(0, max_path_to_root(root.left))
        right_max_value = max(0, max_path_to_root(root.right))

        # The value won't pass upon.
        #      root
        #    /      \
        # left      right
        max_path_value = max(max_path_value,
                             left_max_value + root.val + right_max_value)
        
        # The value can be passed to its partent.
        # parent
        #       \
        #      root
        #    /
        # left
        return root.val + max(left_max_value, right_max_value)

    max_path_value = None
    max_path_to_root(root)
    return max_path_value
```

---

### [Lowest Common Ancestor of a Binary Tree](#lowest-common-ancestor-of-a-binary-tree)
```python
def common_ancestor(root, p, q):
	"""A common ancestor is a node that have p and q in different subtree.
	"""

	if root in (None, p, q):
		return root

	left, right = (common_ancestor(child, p, q)
	               for child in (root.left, root.right))

	return root if left and right else left or right
```

```python
def common_ancestor(root):
	"""A bad implementation.

	Calling node_found_at is O(n), we call it for every node. 
	The algorithm becomes O(n ** 2).
	"""

	def node_found_at(root, node):
        if not root:
            return False

        if root == node:
            return True

        return node_found_at(root.left) or node_found_at(root.right)

    if not root:
        return root

    if node_found_at(root.left, p) and node_found_at(root.left, q):
        return self.lowestCommonAncestor(root.left, p, q)

    if node_found_at(root.right, p) and node_found_at(root.right, q):
        return self.lowestCommonAncestor(root.right, p, q)

    return root
```
