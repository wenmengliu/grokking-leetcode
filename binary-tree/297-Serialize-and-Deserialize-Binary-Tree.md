#### Description

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

**Clarification:** The input/output format is the same as [how LeetCode serializes a binary tree](https://leetcode.com/faq/#binary-tree). You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/09/15/serdeser.jpg)



```
Input: root = [1,2,3,null,null,4,5]
Output: [1,2,3,null,null,4,5]
```

#### Solution

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Codec:

    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """
        stringBuilder = []
        def preorder(node, sb):
            if not node:
                sb += ["#"]
                return
            sb.append(str(node.val))
            preorder(node.left, sb)
            preorder(node.right, sb)
        
        preorder(root, stringBuilder)
        return ",".join(stringBuilder)
        

    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: TreeNode
        """
        if not data:
            return data
        queues = deque(data.split(","))

        def preorder(queue):
            node = queue.popleft()
            # empty nodes
            if node == "#":
                return
            root = TreeNode(int(node))
            root.left = preorder(queue)
            root.right = preorder(queue)
            return root
        return preorder(queues)

# Your Codec object will be instantiated and called as such:
# ser = Codec()
# deser = Codec()
# ans = deser.deserialize(ser.serialize(root))
```

