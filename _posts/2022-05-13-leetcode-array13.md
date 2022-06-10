---
layout: post
title:  "[leetcode] Array - 1051. Height Checker (Easy)"
date:   2022-05-13
category: leetcode
---

## Problem 1051. Height Checker (Easy)
A school is trying to take an annual photo of all the students. The students are asked to stand in a single file line in **non-decreasing order** by height. Let this ordering be represented by the integer array `expected` where `expected[i]` is the expected height of the `ith` student in line.

You are given an integer array `heights` representing the **current order** that the students are standing in. Each `heights[i]` is the height of the `ith` student in line (**0-indexed**).

Return *the ***number of indices*** where* `heights[i] != expected[i]`.

### Example 1:
```yaml
Input: heights = [1,1,4,2,1,3]
Output: 3
Explanation: 
heights:  [1,1,4,2,1,3]
expected: [1,1,1,2,3,4]
Indices 2, 4, and 5 do not match.
```

### Example 2:
```yaml
Input: heights = [5,1,2,3,4]
Output: 5
Explanation:
heights:  [5,1,2,3,4]
expected: [1,2,3,4,5]
All indices do not match.
```

### Example 3:
```yaml
Input: heights = [1,2,3,4,5]
Output: 0
Explanation:
heights:  [1,2,3,4,5]
expected: [1,2,3,4,5]
All indices match.
```

### Constraints:
* `1 <= heights.length <= 100`
* `1 <= heights[i] <= 100`

---
## My solution

임의의 배열 `heights`가 있을 때 이 배열 자체와, 오름차순으로 재정렬한 배열과 element를 비교하여, 다른 개수를 반환하는 문제이다. 쉬워서 설명 생략.

```cpp
class Solution {
public:
    int heightChecker(vector<int>& heights) {
        std::vector<int> ordered_heights = heights;
        std::sort(ordered_heights.begin(), ordered_heights.end());
        int different = 0;
        for (int i = 0; i < heights.size(); i++) {
            if (heights[i] != ordered_heights[i]) {
                different++;
            }
        }

        return different;        
    }
};
```

결과는 가장 빠른 녀석과 거의 코드가 같은데 나는 더 느리게 나오네.. `if`를 거치는게 오래 걸리는게 맞나보다.

![alt text](/public/img/leetcode/leetcode-array-13.png)

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fundol26.github.io%2Fleetcode%2F2022%2F05%2F13%2Fleetcode-array13.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
