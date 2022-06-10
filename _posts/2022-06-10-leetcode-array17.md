---
layout: post
title:  "[leetcode] Array - 977. Squares of a Sorted Array (Easy)"
date:   2022-06-10
category: leetcode
---

## Problem 977. Squares of a Sorted Array (Easy)
[2022/04/27 [leetcode] Array - 977. Squares of a Sorted Array (Easy)](https://undol26.github.io/ros/2022/04/27/leetcode-array3.html) 에서 시간 초과로 풀지 못했던 문제이다.

그 때는 `sort()` 함수를 몰라서 직접 짜느라 엄청 오래걸렸다.

다시 풀면 다음과 같다.

```cpp
class Solution
{
public:
    std::vector<int> sortedSquares(std::vector<int>& nums)
    {
        std::vector<int> output(nums.size());
        for (int i = 0; i < nums.size(); i++) {
            output[i] = pow(nums[i], 2);
        }
        std::sort(output.begin(), output.end());
        return output;
    }
};
```

![alt text](/public/img/leetcode/leetcode-array-17.png)