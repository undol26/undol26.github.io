---
layout: post
title:  "[leetcode] Array - 3. Squares of a Sorted Array"
date:   2022-04-27
category: leetcode
---

## Problem 3. Squares of a Sorted Array
Given an integer array `nums` sorted in **non-decreasing** order, return *an array* of **the squares of each number** *sorted in non-decreasing order*.

### Example 1:
```yaml
Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
Explanation: After squaring, the array becomes [16,1,0,9,100].
After sorting, it becomes [0,1,9,16,100].
```


### Example 2:
```yaml
Input: nums = [-7,-3,2,3,11]
Output: [4,9,9,49,121]
```

### Constraints:
* `1 <= nums.length <= 10^4`
* `-104 <= nums[i] <= 10^4`
* `nums` is sorted in **non-decreasing** order.

<br>

---
## My solution

입력된 배열의 원소를 제곱한 배열에서, 이를 오름차순으로 정렬하는 문제이다.

처음 생각은
1. 각 자리수를 `index 0`부터 `index n-1`로 정한다.
2. `index 0`에 대하여
    1. `index k`과 원소 값을 비교하여
    2. num(index_0) > num(index_k) 이면
      * num(index_k)을 `index 0`의 자리로 옮기고, 나머지 값은 오른쪽으로 한칸씩 이동
    3. num(inxex_0) > num(index_k+1)과 비교

       ...

    4. num(index_0) > num (index n-1)까지 `2.2`의 과정 반복
      * `index 0`을 `index 1`로 증가
3. 이를 `index n-1`까지 반복

<figure>
	<img src="/public/img/leetcode/leetcode-array-3.PNG" alt="" width="30%" height="30%"> 
</figure>

하지만 구현하다보니 `vector`에서 값을 임의의 원소에 넣고, 나머지를 한쪽으로 옮기는 게 복잡해서, 그냥 특정 `index`로 옮기지 않고, 맨 앞자리로 옮기는 것으로 변경하였다.

```cpp
class Solution
{
public:
    std::vector<int> sortedSquares(std::vector<int>& nums)
    {
        std::vector<int> squared_num(nums.size());

        for (int i = 0; i < nums.size(); i++) {
            squared_num[i] = nums[i] * nums[i];
        }

        int temp;
        int index1 = 0;
        int index2 = index1 + 1;
        while (true) {
            try {
                if (index1 == nums.size() || index2 == nums.size()) {
                    throw index1;
                }
            }
            catch (int& e) {
                std::cout << "index1: " << e << std::endl;
                break;
            }

            if (squared_num[index1] > squared_num[index2]) {
                temp = squared_num[index2];
                squared_num[index2] = squared_num[index1];
                squared_num[index1] = temp;
            }
            else {
                index2++;
                if (index2 == nums.size()) {
                    index1++;
                    if (index1 == nums.size() - 1) {
                        break;
                    }
                    index2 = index1 + 1;
                }
            }
        }

        for (int i = 0; i < squared_num.size(); i++) {
            std::cout << squared_num[i] << ", " << std::endl;
        }

        return squared_num;
    }
};
```