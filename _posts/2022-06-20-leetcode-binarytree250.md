---
layout: post
title:  "[leetcode] Binary Tree - 250. Count Univalue Subtrees (Medium)"
date:   2022-06-20
category: leetcode
---

## Problem 250. Count Univalue Subtrees (Medium)
Given the `root` of a binary tree, return the number of **uni-value** subtrees.

<br>

A **uni-value subtree** means all nodes of the subtree have the same value.

<br>

A **leaf** is a node with no children.

### Example 1:
```yaml
Input: root = [5,1,5,5,5,null,5]
Output: 4
```

### Example 2:
```yaml
Input: root = []
Output: 0
```

### Example 3:
```yaml
Input: root = [5,5,5,5,5,null,5]
Output: 6
```

### Constraints:
* The number of nodes in the tree is in the range `[0, 1000]`.
* `-1000 <= Node.val <= 1000`

## My solution

문제가 이해 되질 않네... 솔루션의 문제를 보고 문제가 이해가 되었다.

이해한 바탕으로 조건을 세우면 다음과 같다.
1. 최 상단의 `root->val` 값과 같아야 하고 
2. && `child root->val` 값 `parent root->val` 값이 같아야 하고
3. && 윗단으로 갈수록, 1, 2의 조건을 만족한 녀석과 비교가 되어야 한다. (ex. 한 번 1,2번의 조건에서 벗어난 부분이 있으면, 그쪽 방향은 `uni-value subtree`가 될 수 없음.)

즉, 조건 비교 뿐만 아니고, 한 번이라도 subtree가 아니면 비교할 필요가 없어지므로, 이에 대한 체크를 해주면 될 것 같다.



```cpp

```

![alt text](/public/img/leetcode/leetcode-binarytree-11.png)