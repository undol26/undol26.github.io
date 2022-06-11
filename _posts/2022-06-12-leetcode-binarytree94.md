---
layout: post
title:  "[leetcode] Binary Tree - 94. Binary Tree Inorder Traversal (Easy)"
date:   2022-06-12
category: leetcode
---

## Problem 94. Binary Tree Inorder Traversal (Easy)
Given the `root` of a binary tree, return *the inorder traversal of its nodes' values.*

### Example 1:
```yaml
Input: root = [1,null,2,3]
Output: [1,3,2]
```

### Example 2:
```yaml
Input: root = []
Output: []
```

### Example 3:
```yaml
Input: root = [1]
Output: [1]
```

### Constraints:
* The number of nodes in the tree is in the range `[0, 100]`.
* `-100 <= Node.val <= 100`
<br>

Follow up: Recursive solution is trivial, could you do it iteratively?

## My solution

임의의 `binary tree`가 주어질 때, `inorder traversal` 로 반환해라.

[2022/06/11 [leetcode] Binary Tree - 144. Binary Tree Preorder Traversal (Easy)](https://undol26.github.io/ros/2022/06/11/leetcode-binarytree144.html) 문제에서 `push` 순서만 바꾸면 된다.

`recursive` 한 방법으로 풀었다.

```cpp
class Solution {
public:
    void recursiveInorderTraversal(TreeNode *root, std::vector<int> &output)
    {
        if (root) {
            recursiveInorderTraversal(root->left, output);
            output.push_back(root->val);
            recursiveInorderTraversal(root->right, output);
            return;
        }
    }
    std::vector<int> inorderTraversal(TreeNode *root)
    {
        std::vector<int> output;
        recursiveInorderTraversal(root, output);
        return output;
    }
};
```

![alt text](/public/img/leetcode/leetcode-binarytree-94.png)
