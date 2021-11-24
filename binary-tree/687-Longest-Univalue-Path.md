#### Description

Given the `root` of a binary tree, return *the length of the longest path, where each node in the path has the same value*. This path may or may not pass through the root.

**The length of the path** between two nodes is represented by the number of edges between them.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/13/ex1.jpg)

```markdown
Input: root = [5,4,5,1,1,5]
Output: 2
```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `-1000 <= Node.val <= 1000`
- The depth of the tree will not exceed `1000`.

#### Solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def longestUnivaluePath(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        res = 0
        
        def postorder(node):
            nonlocal res
            if not node:
                return 0
            
            l = postorder(node.left) 
            r = postorder(node.right) 
            
            pl, pr = 0, 0
            
            if node.left and node.left.val == node.val:
                pl = l
            
            if node.right and node.right.val == node.val:
                pr = r
            
            res = max(res, pl + pr)
            
            return max(pl, pr) + 1
        
        postorder(root)
        
        return res
```

类似543

postorder recursion function just returns a length path without turning point (one direction to go down)

LP(root) := max path length that passess root and **at most one of its child** 

if root is none:

​	LP(root) = 0

else:

​    LP(root) = 1 + max(LP(root.left), LP(root.right))

we need to deal with univalue nodes to see if a non-empty left node value is the same as root value, if not the left path pl = 0 otherwise pl = l. Same for pr

res = max(1 + LP(root.left) + 1 + LP(root.right))



Time Complexity: O(n) each node only visits one time. n is the number of nodes

Space Complexity: O(h) h is the height of tree. If a tree is skewed, space complexity is O(n) (worst case); best case for a perfect tree is O(logn)