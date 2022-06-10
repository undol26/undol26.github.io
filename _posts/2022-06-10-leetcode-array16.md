---
layout: post
title:  "[leetcode] Array - 448. Find All Numbers Disappeared in an Array (Easy)"
date:   2022-06-10
category: leetcode
---

## Problem 448. Find All Numbers Disappeared in an Array (Easy)
Given an array `nums` of `n` integers where `nums[i]` is in the range `[1, n]`, return *an array of all the integers in the range `[1, n]` that do not appear in `nums`.*

### Example 1:
```yaml
Input: nums = [4,3,2,7,8,2,3,1]
Output: [5,6]
```

### Example 2:
```yaml
Input: nums = [1,1]
Output: [2]
```

### Constraints:
* `n == nums.length`
* `1 <= n <= 10^5`.
* `1 <= nums[i] <= n`
<br>

**Follow up**: Could you do it without extra space and in `O(n)` runtime? You may assume the returned list does not count as extra space.

---
## My solution

임의의 배열 `nums`가 있을 때 `nums[i]`는 `1`부터 `n(nums.length)`까지의 값을 가진다. 이 때 `nums`에 없는 값들을 반환해라.

이 때 
1. without extra space -> 새로 벡터 같은걸 만들지 말고
2. `O(n)` -> 첫 포문 하나로 끝내라.

이중 포문을 쓰면 진짜 간단한데, `O(n)`으로 하는게 진짜 어렵네. 
```cpp
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        std::vector<int> output;
        for (int i = 0; i < nums.size(); i++) {
            output.push_back(i + 1);
        }
        std::vector<int>::iterator it = output.end() - 1;
        std::sort(nums.begin(), nums.end());
        bool isPass = false;
        for (int i = nums.size() - 1; i >= 0; i--) {
            while (nums[i] != *it) {
                if (it >= output.begin() + 1) {
                    it--;
                }
                else {
                    isPass = true;
                    break;
                }
            }
            if (isPass == false) {
                output.erase(it);
                it = output.end() - 1;
                continue;
            }

            if (i == 0 && nums[i] == *it) {
                output.erase(it);
                break;
            }

            if (nums[i] != *it) {
                it = output.end() - 1;
                isPass = false;
                continue;
            }
        }
        return output;
    }
};
```

for 문 안에 while문을 쓰면 결국 O(n)이 아니겠지? 아무리 생각해도 모르겠다. 답을 봐야겠다 ㅠㅠ

아... histogram처럼 생각하면 간단하구나..
다시 짜보자.

```cpp
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        std::vector<int> hist(nums.size());
        std::vector<int> output;

        for (int i = 0; i < nums.size(); i++) {
            hist[nums[i] - 1] += 1;
        }

        for (int i = 0; i < nums.size(); i++) {
            if (hist[i] == 0) {
                output.push_back(i + 1);
            }
        }

        return output;
    }
};
```

코드가 정말 간결해졌다. 와 저렇게 생각하면 정말 간단한데..

아니 그런데 `Could you do it without extra space` 라고 해서 새로 벡터 선언 안하고 하나만 하려고 계속 한거였는데.. 이럴거면 문제가 잘못된거 아닌가 ㅠ

![alt text](/public/img/leetcode/leetcode-array-16.png)