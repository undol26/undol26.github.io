---
layout: post
title:  "[leetcode] Array - 414. Third Maximum Number (Easy)"
date:   2022-05-14
category: leetcode
---

## Problem 414. Third Maximum Number (Easy)
Given an integer array `nums`, return *the **third distinct maximum** number in this array. If the third maximum does not exist, return the maximum number.*

### Example 1:
```yaml
Input: nums = [3,2,1]
Output: 1
Explanation:
The first distinct maximum is 3.
The second distinct maximum is 2.
The third distinct maximum is 1.
```

### Example 2:
```yaml
Input: nums = [1,2]
Output: 2
Explanation:
The first distinct maximum is 2.
The second distinct maximum is 1.
The third distinct maximum does not exist, so the maximum (2) is returned instead.
```
### Example 3:
```yaml
Input: nums = [2,2,3,1]
Output: 1
Explanation:
The first distinct maximum is 3.
The second distinct maximum is 2 (both 2's are counted together since they have the same value).
The third distinct maximum is 1.
```

### Constraints:
* `1 <= nums.length <= 10^4`
* `-2^31 <= nums[i] <= 2^31 - 1`.

<br>

**Follow up**: Can you find an `O(n)` solution?

---
## My solution

임의의 배열 `nums`가 있을 때 세번째로 큰 값을 반환해라. 만약 그 값이 없다면 `max` 값을 반환하라.

* 처음 든 생각은 
    * `sort` 먼저하고
    * `distinct` value만 취급하므로, 중복된 값은 지우고
    * 개수가 3보다 크거나 같다면 두 개를 `pop`한 후, 마지막 값을 리턴하자.
    * 개수가 3보다 작다면 마지막 값을 리턴하자.

```cpp
class Solution {
public:
    int thirdMax(vector<int>& nums) {
        std::sort(nums.begin(), nums.end());
        for (std::vector<int>::iterator it = nums.begin(); it < nums.end();) {
            if (it == nums.end() - 1) {
                break;
            }
            if (*it == *(it + 1)) {
                nums.erase(it);
            }
            else {
                it++;
            }
        }

        if (nums.size() >= 3) {
            nums.pop_back();
            nums.pop_back();
        }
        return nums.at(nums.size() - 1);
    }
};
```

솔직히 이번건 아이디어 괜찮았다고. 짜면서 기대했다.

결과는 안좋아.

빠른 결과들을 보니 굳이 값을 `erase()`하고, `pop_back()`을 할 필요가 없는거다. 그 index같은 것만 체크했다가 그 index의 nums를 return하면 되는 거였다.

크게 배웠다. 무조건 값을 지우기 보다는, 그 값은 그대로 두고 index만 건드리는게 더 속도가 빠르구나.

![alt text](/public/img/leetcode/leetcode-array-414.png)

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fundol26.github.io%2Fleetcode%2F2022%2F05%2F14%2Fleetcode-array15.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)