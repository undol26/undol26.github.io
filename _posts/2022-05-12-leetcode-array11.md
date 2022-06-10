---
layout: post
title:  "[leetcode] Array - 283. Move Zeroes (Easy)"
date:   2022-05-12
category: leetcode
---

## Problem 283. Move Zeroes (Easy)
Given an integer array `nums`, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

<br>

**Note** that you must do this in-place without making a copy of the array.

### Example 1:
```yaml
Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]
```

### Example 2:
```yaml
Input: nums = [0]
Output: [0]
```

### Constraints:
* `1 <= nums.length <= 10^4`
* `-2^31 <= nums[i] <= 2^31 - 1`

<br>

**Follow up**: Could you minimize the total number of operations done?

---
## My solution

임의의 배열 `nums`가 있을 때 (아니 매번 `arr` 였다가 `nums` 였다가 아오), 0인 element를 맨 오른쪽으로 밀고, 0 아닌 값들을 왼쪽으로 붙이는 문제이다. 이 때 상대적인 순서는 유지해야 한다.

array를 copy해서 하지 말라고 했기 때문에 문제가 생각보다 복잡하다. 

이번 문제의 목표는 이중 포문을 안쓰고 푸는 것이다.

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int originalNumsSize = nums.size();
        for (std::vector<int>::iterator it = nums.begin(); it < nums.end();) {
            if (*it == 0) {
                nums.erase(it);
            }
            else {
                it++;
            }
        }
        nums.resize(originalNumsSize);       
    }
};
```

결과는 이번에도 `Runtime`이 매우 느렸다. 이번에는 이중 포문을 안썼고, 나름 vector의 함수 (`erase`)를 써서 결과가 빠를 것으로 예상했는데. 무엇이 오래 걸린걸까.

![alt text](/public/img/leetcode/leetcode-array-11.png)

가장 빠른 알고리즘을 봤더니 잘 이해가 안가고, 두 번째 빠른거는 번뜩이는 아이디어긴 하네. 근데 내가 한것과 속도가 그렇게 차이가 나지 않을것 같은데.

그렇다면 가설은 `if/else` 에서 오래 걸리거나, `vector`의 `iterator`로 접근하면 오래걸리거나 일거 같다.

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fundol26.github.io%2Fleetcode%2F2022%2F05%2F12%2Fleetcode-array11.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)