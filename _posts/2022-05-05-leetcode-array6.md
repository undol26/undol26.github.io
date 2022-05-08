---
layout: post
title:  "[leetcode] Array - 6. Remove Element"
date:   2022-05-05
category: leetcode
---

## Problem 6. Remove Element
Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` **in-place**. The relative order of the elements may be changed.

<br>
Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the **first part** of the array `nums`. More formally, if there are `k` elements after removing the duplicates, then the first `k` elements of `nums` should hold the final result. It does not matter what you leave beyond the first `k` elements.

<br>
Return `k` *after placing the final result in the first* `k` *slots of* `nums`.

<br>
Do not allocate extra space for another array. You must do this by modifying the input array in-place with O(1) extra memory.

**Custom Judege:**
The judge will test your solution with the following code:

```yaml
int[] nums = [...]; // Input array
int val = ...; // Value to remove
int[] expectedNums = [...]; // The expected answer with correct length.
                            // It is sorted with no values equaling val.

int k = removeElement(nums, val); // Calls your implementation

assert k == expectedNums.length;
sort(nums, 0, k); // Sort the first k elements of nums
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
```
If all assertions pass, then your solution will be **accepted**.

### Example 1:
```yaml
Input: nums = [3,2,2,3], val = 3
Output: 2, nums = [2,2,_,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 2.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

### Example 2:
```yaml
Input: nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5, nums = [0,1,4,0,3,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.
Note that the five elements can be returned in any order.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

### Constraints:
* `0 <= nums.length <= 100`
* `0 <= nums[i] <= 50`
* `0 <= val <= 100`

<br>

---
## My solution 1

임의의 배열 `nums`가 있고, 특정 `val`을 입력으로 줄 때, 이 `val`과 같은 값들은 제거하고, 그 값들을 앞으로 채워넣는 문제이다. 

<br>

나는 `nums`와 똑같은 배열 `expectedNums`을 만들고, `nums`가 `val`과 같지 않은 값을 `expectedNums`에 넣어주는 것으로 문제를 풀었다.

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        std::vector<int> expectedNums;
        expectedNums = nums;

        int index = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] != val) {
                expectedNums[index] = nums[i];
                index++;
            }
        }
        nums = expectedNums;

        return index;
    }
};
```

결과는 이렇다.

![alt text](/public/img/leetcode/leetcode-array-6.png)

아니 그런데 문제 해석이 제대로 안돼서 기껏 푼 것들이 정답으로 채택이 되지 않는 경우가 종종있네 ㅠ

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fundol26.github.io%2Fleetcode%2F2022%2F05%2F05%2Fleetcode-array6.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)