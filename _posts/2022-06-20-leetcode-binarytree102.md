---
layout: post
title:  "[leetcode] Binary Tree - 101. Symmetric Tree (Easy)"
date:   2022-06-20
category: leetcode
---

## Problem 101. Symmetric Tree (Easy)
Given the `root` of a binary tree, *check whether it is a mirror of itself* (i.e., symmetric around its center).

### Example 1:
```yaml
Input: root = [1,2,2,3,4,4,3]
Output: true
```

### Example 2:
```yaml
Input: root = [1,2,2,null,3,null,3]
Output: false
```

### Constraints:
* The number of nodes in the tree is in the range `[0, 1000]`.
* `-100 <= Node.val <= 100`

## My solution

임의의 `binary tree`가 주어질 때, `tree`가 대칭 인지/아닌지를 구해라. 문제가 재밌다.

아직 tree구조는 명확하게 감이 오진 않는다. 어렴풋이 감이 왔지만 제대로 구현이 되지 않아 답을 봤다. 답을 보니, 내가 생각한 것이 어렴풋한 것도 아니라는걸 깨달았다. 

이쪽은 알고리즘 공부가 많이 필요하겠다. 아예 생각의 구조가 좀 다르네. 다시 말하면 `recursive`한 알고리즘에 내가 좀 약하구나..

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
    bool isSymmetric(TreeNode *root)
    {
        if (root == NULL) return true;  // Tree is empty

        return isSymmetricTest(root->left, root->right);
    }

    bool isSymmetricTest(TreeNode *p, TreeNode *q)
    {
        if (p == NULL && q == NULL)  // left & right node is NULL
            return true;

        else if (p == NULL || q == NULL)  // one of them is Not NULL
            return false;

        else if (p->val != q->val)
            return false;

        return isSymmetricTest(p->left, q->right) &&
               isSymmetricTest(p->right,
                               q->left);  // comparing left subtree's left child with right
                                          // subtree's right child --AND-- comparing left subtree's
                                          // right child with right subtree's left child
    }
};
```

![alt text](/public/img/leetcode/leetcode-binarytree-101.png)