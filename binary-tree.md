#Binary Tree
* [Binary Tree Preorder Traversal](binary-tree.md#binary-tree-preorder-traversal)
  * [Binary Tree Inorder Traversal](binary-tree.md#binary-tree-inorder-traversal)
  * [Binary Tree Postorder Traversal](binary-tree.md#binary-tree-postorder-traversal)
  * [Binary Tree Level Order Traversal](binary-tree.md#binary-tree-level-order-traversal)
  * [Minimum Depth of Binary Tree](binary-tree.md#minimum-depth-of-binary-tree)
  * [Balanced Binary Tree](binary-tree.md#balanced-binary-tree)
  * [Binary Tree Maximum Path Sum](binary-tree.md#binary-tree-maximum-path-sum)
  * [Lowest Common Ancestor of a Binary Tree](binary-tree.md#lowest-common-ancestor-of-a-binary-tree)
---

### [Binary Tree Preorder Traversal](#binary-tree-preorder-traversal)
Depth-first search: get the node value when we see the node in the first time.
```python
def preorder(root):
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

Breadth-first search
```python
def preorder(root):
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
Depth-first search: get the node value when we traverse back from the left subtree.
```python
def inorder(root):
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
Depth-first search: get the node value when we traverse back from the right subtree.
```python
def postorder(root):
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

Breadth-first search: use two stacks to reverse the visit order.
```python
def postorder(root):
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
def levelorder(root):

```
---

### [Minimum Depth of Binary Tree](#minimum-depth-of-binary-tree)
```python
def min_depth(root):

```
---

### [Balanced Binary Tree](#balanced-binary-tree)
```python
def is_balanced_tree(root):

```
---

### [Binary Tree Maximum Path Sum](#binary-tree-maximum-path-sum)
```python
def max_path_sum(root):

```
---

### [Lowest Common Ancestor of a Binary Tree](#lowest-common-ancestor-of-a-binary-tree)
```python
def common_ancestor(root):

```
---
