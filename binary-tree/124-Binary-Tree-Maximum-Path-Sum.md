#### Description 

A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.

The **path sum** of a path is the sum of the node's values in the path.

Given the `root` of a binary tree, return *the maximum **path sum** of any **non-empty** path*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg)

```markdown
Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.
```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 3 * 104]`.
- `-1000 <= Node.val <= 1000`

#### Solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        res = float('-inf')
        
        def postorder(node):
            nonlocal res
            if not node:
                return 0
            
            l = max(0, postorder(node.left))
            r = max(0, postorder(node.right))
            
            
            res = max(res, l + r + node.val)
            
            return max(l, r) + node.val
        
        postorder(root)
        
        return res
```

similiar to 687

Time Complexity: O(n) each node only visits one time. n is the number of nodes

Space Complexity: O(h) h is the height of tree. If a tree is skewed, space complexity is O(n) (worst case); best case for a perfect tree is O(logn)