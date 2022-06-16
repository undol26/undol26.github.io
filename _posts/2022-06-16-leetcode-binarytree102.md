---
layout: post
title:  "[leetcode] Binary Tree - 102. Binary Tree Postorder Traversal (Medium)"
date:   2022-06-16
category: leetcode
---

## Problem 102. Binary Tree Postorder Traversal (Medium)
Given the `root` of a binary tree, return *the level order traversal of its nodes' values.* (i.e., from left to right, level by level).

### Example 1:
```yaml
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
```

### Example 2:
```yaml
Input: root = [1]
Output: [[1]]
```

### Example 3:
```yaml
Input: root = []
Output: []
```

### Constraints:
* The number of nodes in the tree is in the range `[0, 2000]`.
* `-1000 <= Node.val <= 1000`

## My solution

임의의 `binary tree`가 주어질 때, `level order traversal` 로 반환해라.

제대로 알고리즘이 떠오르지 않았다. 생각한 방법도 틀렸었고. 답을 봤다.  ㅠㅠ 

```cpp
class Solution {
public:
    std::vector<std::vector<int>> levelOrder(TreeNode *root)
    {
        std::vector<std::vector<int>> answer;
        if (!root) return answer;  // if root is NULL then return
        std::queue<TreeNode *> q;  // for storing nodes
        q.push(root);              // push root initially to the queue
        while (!q.empty())         // while queue is not empty go and follow few steps
        {
            int size = q.size();  // storing queue size for while loop
            std::vector<int> v;   // for storing nodes at the same level
            while (size--) {
                TreeNode *temp = q.front();  // store front node of queue  and pop it from queue
                q.pop();
                v.push_back(temp->val);  // push it to v
                if (temp->left)
                    q.push(
                        temp->left);  // if left subtree exist for temp then store it into the queue
                if (temp->right)
                    q.push(temp->right);  // if right subtree exist for temp then store it into the
                                          // queue
            }
            answer.push_back(
                v);  // push v into answer, as v consist of all the nodes of current level
        }
        return answer;  // return the answer
    }
};
```

![alt text](/public/img/leetcode/leetcode-binarytree-102.png)

여담.

datastructure를 처음 공부하다보니 어렵다. 아니 어렵다기보단, 이런 게 있어? 하는 느낌이다. 특히 이번에는 `queue`를 쓰는 것에서 놀랐다. 대충 개념만 알고있지, 실제로 저렇게 `tree` 구조를 `queue`에 넣을 수 있다는 것은 몰랐으니까. 

얼마전 동료랑 얘기하다가 혼자서 너무 붙잡지 말고, 고민하다 안되면 답을 보고 넘어가라는 조언을 받았다. 생각해보니, 전에 공부하다가 막히면 짜증나서 한동안 쳐다도 안보다가 다시 보면 풀려서, 리프레시 기간이 필요한가? 생각했는데, 이런 방식이면 대체 언제 끝날지 모를 정도로 시간이 오래 걸리겠다.

딱 30분만 고민하고 잘 안떠오르면 넘어가자. 지금의 목적은 문제를 푸는 것도 있지만, 자료구조를 공부하는 것이니까.