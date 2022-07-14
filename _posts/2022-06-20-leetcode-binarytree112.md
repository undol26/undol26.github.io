---
layout: post
title:  "[leetcode] Binary Tree - 112.  Path Sum (Easy)"
date:   2022-06-20
category: leetcode
---

## Problem 112.  Path Sum (Easy)
Given the `root` of a binary tree and an integer `targetSum`, return `true` if the tree has a **root-to-leaf** path such that adding up all the values along the path equals `targetSum`.

<br>

A **leaf** is a node with no children.

### Example 1:
```yaml
Input: root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
Output: true
Explanation: The root-to-leaf path with the target sum is shown.
```

### Example 2:
```yaml
Input: root = [1,2,3], targetSum = 5
Output: false
Explanation: There two root-to-leaf paths in the tree:
(1 --> 2): The sum is 3.
(1 --> 3): The sum is 4.
There is no root-to-leaf path with sum = 5.
```

### Example 3:
```yaml
Input: root = [], targetSum = 0
Output: false
Explanation: Since the tree is empty, there are no root-to-leaf paths.
```

### Constraints:
* The number of nodes in the tree is in the range `[0, 5000]`.
* `-1000 <= Node.val <= 1000`
* `-1000 <= targetSum <= 1000`

## My solution

임의의 `binary tree`가 주어질 때, `tree`의 합이 주어진 값과 같은지 비교해라.

역시 완벽하게 풀리지는 않아서 약간의 도움을 받았다.

큰 방향성은 제대로 보는데 마무리가 안되네..



```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if (root == nullptr) {
            return false;
        }
        if (root->left == nullptr && root->right == nullptr && root->val - targetSum == 0) {
            return true;
        }

        bool left = hasPathSum(root->left, targetSum - root->val);
        bool right = hasPathSum(root->right, targetSum - root->val);

        return left || right;
    }
};
```

![alt text](/public/img/leetcode/leetcode-binarytree-112.png)