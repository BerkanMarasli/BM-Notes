# Technical Interview Questions



## Leetcode BLIND-75



### Two Sum - HashMap (Leetcode 1)

> **Problem Description:**
>
> Given an array of integers, return indices of the two numbers such that they add up to a specific target.
>
> You may assume that each input would have exactly one solution, and you may not use the same element twice.
>
> **Example:**
>
> Given nums = [2, 7, 11, 15], target = 9
>
> Because nums[0] + nums[1] = 2 + 7 = 9 => return [0, 1]
>
> ---
>
> Given nums = [2, 1, 5, 3], target = 4
>
> Because nums[1] + nums[3] = 1 + 3 = 4 => return [1, 3]

<details>
    <summary>Brute force method: O(n^2)</summary>
    <br />
    <img src='./Technical_interviews.assets/leetcode1img1.png' />
</details>

<details>
    <summary>One pass method O(n)</summary>
    <br />
    <img src='./Technical_interviews.assets/leetcode1img2.png' />
    <br />
    The solution is found when the second number is selected (underlined in blue)
</details>

```python
# Python solution - O(n)
def twoSum(nums: List[int], target: int) -> List[int]:
    prevMap = {} # HashMap -> val : index
    for i, n in enumerate(nums):
        diff = target - n
        if diff in prevMap:
            return [prevMap[diff], i]
        prevMap[n] = i
```

```javascript
// Javascript solution - O(n)
function twoSum(nums, target) {
    const prevMap = {} // HashMap -> val : index
    let result
    nums.forEach((n, i) => {
        const diff = target - n
        if (diff in prevMap) {
            result = [prevMap[diff], i]
        }
        prevMap[n] = i
    })
    return result
}
```





























