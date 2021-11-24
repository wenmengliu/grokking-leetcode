#### Description

Given the `root` of a binary tree, return *the length of the **diameter** of the tree*.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

The **length** of a path between two nodes is represented by the number of edges between them.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

```markdown
Input: root = [1,2,3,4,5]
Output: 3
Explanation: 3 is the length of the path [4,2,1,3] or [5,2,1,3].
```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `-100 <= Node.val <= 100`

#### Solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        res = 0
        
        def postorder(node):
            nonlocal res
            if not node:
                return 0
            
            left = postorder(node.left) 
            right = postorder(node.right) 
            
            res = max(res, left + right)
            
            return max(left, right) + 1
        
        postorder(root)
        
        return res
```

In a tree, only root node can use `both` left and right nodes. We define:

LP(root) := max path length that passess root and at most one of its child

if root is none:

​	LP(root) = 0

else:

​    LP(root) = 1 + max(LP(root.left), LP(root.right))

for a res variable, it considers both of its Child 

res = max(res, 1 + LP(root.left) + 1 + LP(root.right))

Time Complexity: O(n) each node only visits one time. n is the number of nodes

Space Complexity: O(h) h is the height of tree. If a tree is skewed, space complexity is O(n) (worst case); best case for a perfect tree is O(logn)