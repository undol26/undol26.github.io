---
layout: post
title:  "[leetcode] Array - 26. Remove Duplicates from Sorted Array (Easy)"
date:   2022-05-08
category: leetcode
---

## Problem 26. Remove Duplicates from Sorted Array (Easy)
Given an integer array `nums` sorted in **non-decreasing order**, remove the duplicates **in-place** such that each unique element appears only **once**. The **relative order** of the elements should be kept the **same**.

<br>

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the **first part** of the array `nums`. More formally, if there are `k` elements after removing the duplicates, then the first `k` elements of `nums` should hold the final result. It does not matter what you leave beyond the first `k` elements.

<br>

Return `k` *after placing the final result in the first `k` slots of `nums`.*

<br>

Do **not** allocate extra space for another array. You must do this by **modifying the input array in-place** with $O(1)$ extra memory.

<br>

**Custom Judege:**

<br>

The judge will test your solution with the following code:

```yaml
int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct length

int k = removeDuplicates(nums); // Calls your implementation

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```
If all assertions pass, then your solution will be **accepted**.

### Example 1:
```yaml
Input: nums = [1,1,2]
Output: 2, nums = [1,2,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

### Example 2:
```yaml
Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

### Constraints:
* `1 <= nums.length <= 3 * 10^4`
* `-100 <= nums[i] <= 100`
* `nums` is sorted in **non-decreasing** order.

<br>

---
## My solution

임의의 배열 `nums`가 중복된 값을 포함하여 오름차순으로 있고, 중복되는 값을 지운 오름차순의 배열을 반환하는 문제이다. 이 때 중복된 값을 하나는 남겨둬야 한다.

특정 언어의 문제 때문에 `nums`의 배열 길이는 입력과 같지만, 최종 중복된 값을 제거한 개수를 반환해야 한다.

<br>

단순히 생각하면 이중 포문을 돌리면 되겠지만, 문제 자체가 요구하는 것이 $O(1)$ 이기 때문에 생각을 더 해보았다. 

그 결과 `nums`의 앞뒤를 비교하여, 중복된 값이면 `pass`, 이후 다시 앞뒤 비교, 중복되지 않으면 살리고. 이 과정을 반복하면 된다고 판단하였다.

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        std::vector<int> expectedNums;
        expectedNums = nums;

        int index = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (i == nums.size() - 1) {
                expectedNums[index] = nums[i];
                index++;
                break;
            }
            if (nums[i] == nums[i + 1]) {
                continue;
            }
            else {
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

결과 해석이 근데 잘 안되네.. runtime은 상위 96.32%로 매우 좋은것 같은데..
memory usage는 6.64%로 별로라는 건가? `expectedNums` vector를 하나 선언해서 그런걸까?

![alt text](/public/img/leetcode/leetcode-array-7.png)

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fundol26.github.io%2Fleetcode%2F2022%2F05%2F08%2Fleetcode-array7.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)