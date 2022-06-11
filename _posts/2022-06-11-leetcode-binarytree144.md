---
layout: post
title:  "[leetcode] Array - 144. Binary Tree Preorder Traversal (Easy)"
date:   2022-06-11
category: leetcode
---

## Problem 144. Binary Tree Preorder Traversal (Easy)
Given the `root` of a binary tree, return *the preorder traversal of its nodes' values.*

### Example 1:
```yaml
Input: root = [1,null,2,3]
Output: [1,2,3]
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

임의의 binary tree가 주어질 때, `preorder traversal` 로 반환해라.
`recursive` 한 방법으로 풀었다.

```cpp
// Definition for a binary tree node.
struct TreeNode
{
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode() : val(0), left(nullptr), right(nullptr)
    {
    }
    TreeNode(int x) : val(x), left(nullptr), right(nullptr)
    {
    }
    TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right)
    {
    }
};
class Solution
{
public:
    void printVec(std::vector<int> &output)
    {
        std::cout << "output: {";
        for (int i = 0; i < output.size(); i++) {
            if (i == output.size() - 1) {
                std::cout << output[i] << "}" << std::endl << std::endl;
            }
            else {
                std::cout << output[i] << ", ";
            }
        }
    }
    void recursivePreorderTraversal(TreeNode *root, std::vector<int> &output)
    {
        if (root) {
            output.push_back(root->val);
            if (root->left != nullptr) {
                recursivePreorderTraversal(root->left, output);
            }
            if (root->right != nullptr) {
                recursivePreorderTraversal(root->right, output);
            }
            return;
        }
    }
    std::vector<int> preorderTraversal(TreeNode *root)
    {
        std::vector<int> output;
        recursivePreorderTraversal(root, output);

        printVec(output);
        return output;
    }
};

int main()
{
    struct TreeNode *root1 = new TreeNode;
    struct TreeNode *root2 = new TreeNode;
    struct TreeNode *root3 = new TreeNode;

    root1->val = 1;
    root2->val = 2;
    root3->val = 3;

    root1->right = root2;
    root1->left = nullptr;
    root2->left = root3;
    root2->right = nullptr;

    Solution sol;
    sol.preorderTraversal(root1);

    return 0;
}
```

![alt text](/public/img/leetcode/leetcode-binarytree-144.png)
