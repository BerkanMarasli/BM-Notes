

# Technical Interview Questions



## Leetcode BLIND-75



Two Sum - HashMap (Leetcode 1)

Sliding Window: Best Time to Buy and Sell Stock (Leetcode 121)

Product of Array Except Self (Leetcode 238)

Maximum subarray (Leetcode 53)

Maximum Product Subarray (Leetcode 152)



### Two Sum - HashMap (Leetcode 1)

> ---
>
> **Problem Description:**
>
> Given an array of integers, return indices of the two numbers such that they add up to a specific target.
>
> You may assume that each input would have exactly one solution, and you may not use the same element twice.
>
> ---
>
> **Example:**
>
> Given nums = [2, 7, 11, 15], target = 9
>
> Because nums[0] + nums[1] = 2 + 7 = 9 => return [0, 1]
>
> **Example 2:**
>
> Given nums = [2, 1, 5, 3], target = 4
>
> Because nums[1] + nums[3] = 1 + 3 = 4 => return [1, 3]
>
> ---

<details>
    <summary>Brute force solution: O(n^2)</summary>
    <br />
    <img src='./Technical_interviews.assets/leetcode1img1.png' />
</details>

<details>
    <summary>One pass solution O(n)</summary>
    <br />
    The solution is found when the second number in the answer is selected (underlined in blue)
    <br />
    <br />
    <img src='./Technical_interviews.assets/leetcode1img2.png' />
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



### Sliding Window: Best Time to Buy and Sell Stock (Leetcode 121)

> ---
>
> **Problem Description:**
>
> Say you have an array for which the ith element is the price of a given stock on day i.
>
> If you were only permitted to complete at most one transaction (i.e. buy one and sell one share of the stock), design an algorithm to find the maximum profit.
>
> Note that you cannot sell a stock before you buy one.
>
> ---
>
> **Example:**
>
> Input: [7, 1, 5, 3, 6, 4]
>
> Output: 5
>
> Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6 - 1 = 5. Not 7 - 1 = 6, as selling price needs to be larger than buying price.
>
> ---

<details>
    <summary>Solution: memory O(1) and time O(n)</summary>
    <br />
    1. Initialise two points: leftP = buy at day 1 (index 0), rightP = sell at day 2 (index 1), and maxProfit = -1000
    <br />
    2. Calculate rightP - leftP as profit
    <br />
    3. If profit > maxProfit -> update maxProfit
    <br />
    4. If profit less than 0 (indicating decrease in price) -> increment leftP and rightP by one
    <br />
    5. Else (increase in price) -> increment rightP only
    <br />
    <br />
    <img src='./Technical_interviews.assets/leetcode121img1.png' />
</details>

```python
# Python solution - O(n)
def maxProfit(prices: List[int]) -> int:
    leftP, rightP = 0, 1 # left=buy, right=sell
    maxProfit = 0
    while rightP < len(prices):
        # isProfitable?
        if prices[leftP] < prices[rightP]:
            profit = prices[rightP] - prices[leftP]
            maxProfit = max(maxProfit, profit)
        else:
            leftP += rightP # as minimum is found, move leftP to rightP as you want leftP is be as small as possible
        rightP += 1
    return maxProfit
```

```javascript
// Javascript solution - O(n)
function maxProfit(prices) {
    let leftP = 0
    let rightP = 1
    let maxProfit = 0
    while (rightP < prices.length) {
        if (prices[leftP] < prices[rightP]) {
            const profit = prices[rightP] - prices[leftP]
            maxProfit = Math.max(maxProfit, profit)
        } else {
            leftP = rightP
        }
        rightP += 1
    }
    return maxProfit
}
```



### Product of Array Except Self (Leetcode 238)

>---
>
>**Problem Description:**
>
>Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.
>
>The product of any prefix of `nums` is guaranteed to fit in a 32-bit integer.
>
>You must write an algorithm that runs in O(n) time and without using the division operation.
>
>---
>
>**Example:**
>
>Input: nums = [1, 2, 3, 4]
>
>Output: [24, 12, 8, 6]
>
>---
>
>**Example 2:**
>
>Input: nums = [-1, 1, 0, -3, 3]
>
>Output: [0, 0, 9, 0, 0]
>
>---

<details>
    <summary>Solution: memory O(1) and time O(n)</summary>
    <br />
    Note: if the / operator was allowed -> take product of all integers and forEach divide the sum by the integer.
    <br />
    1. Calculate array of prefix -> sum of all integers before index (inclusive) -> O(n)
    <br />
    2. Calculate array of postfix -? sum of all integers after index (inclusive) -> O(n)
    <br />
    3. For every value in input array, insert the product of the prefix and postfix -> memory O(n^2) and time O(n)
    <br />
    Note: to avoid memory O(n^2), directly insert product of prefix and postfix to output array by first doing a loop (start to end) for prefixes and then a loop (end to start) for postfixes
    <br />
    Note: the solution is O(1) memory as the output array does not contribute. If otherwise, memory O(n)
    <br />
    <br />
    <img src='./Technical_interviews.assets/leetcode238img1.png' />
</details>

```python
# Python solution - O(n)
def productExceptSelf(nums: List[int]) -> List[int]:
    result = [1] * (len(nums)) # Does not contribute to space complexity in context of problem
    prefix = 1
    for i in range(len(nums)):
        result[i] = prefix
        prefix *= nums[i]
    postfix = 1
    for i in range(len(nums) - 1, -1, -1): # loop from end to start
        result[i] *= postfix
        postfix = *= nums[i]
    return result
```

```javascript
// Javascript solution - O(n)
function productExceptSelf(nums) {
    const result = new Array(nums.length)
    let prefix = 1
    for (let i = 0; i < nums.length; i++) {
        result[i] = prefix
        prefix = prefix * nums[i]
    }
    let postfix = 1
    for (let i = nums.length-1; i >= 0; i--) {
        result[i] = result[i] * postfix
        postfix = postfix * nums[i]
    }
    return result
}
```



### Maximum Subarray (Leetcode 53)

>---
>
>**Problem Description:**
>
>Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.
>
>---
>
>**Example:**
>
>Input: nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
>
>Output: 6
>
>Explanation: [4, -1, 2, 1] has the largest sum = 6
>
>---

<details>
    <summary>Cubic solution: O(n^3)</summary>
    <br />
    <img src="./Technical_interviews.assets/leetcode53img1.png" />
</details>

<details>
    <summary>Quadratic solution: O(n^2)</summary>
    <br />
    Decrease from O(n^3) to O(n^2) by storing the sum of the integers and just adding the current integer to the sum.
    <br />
    <br />
    <img src="./Technical_interviews.assets/leetcode53img2.png" />
</details>

<details>
    <summary>Linear solution: O(n)</summary>
    <br />
    A negative starting sum does not contribute to the maximum sum positively -> can be ignored.
    <br />
    <br />
    <img src="./Technical_interviews.assets/leetcode53img3.png" />
</details>

```python
# Python solution - O(n)
def maxSubArray(nums: List[int]) -> int:
    maxSum = nums[0]
    currentSum = 0
    for number in nums:
        if currentSum < 0:
            currentSum = 0
        currentSum += number
        maxSum = max(maxSum, currentSum)
    return maxSum
```

```javascript
// Javascript solution - O(n)
function maxSubArray(nums) {
    let maxSum = nums[0]
    let currentSum = 0
    nums.forEach(number => {
        if (currentSum < 0) {
            currentSum = 0
        }
        currentSum += number
        maxSum = Math.max(maxSum, currentSum)
    })
    return maxSum
}
```



## Maximum Product Subarray (Leetcode 152)

>---
>
>**Problem Description:**
>
>Given an integer array `nums`, find the contiguous subarray within an array (containing at least one number) which has the largest product.
>
>---
>
>**Example:**
>
>Input: nums = [2, 3, -2, 4]
>
>Output: 6
>
>Explanation: [2, 3] has the largest product = 6
>
>---

Dynamic Programming Question

<details>
    <summary>Brute force solution: O(n^2)</summary>
    <br />
    <img src='./Technical_interviews.assets/leetcode152img1.png' />
</details>

<details>
    <summary>Solution: memory O(1) and time O(n)</summary>
    <br />
    Note: If all integers are positive, product is always increasing as you increase index.
    <br />
    Note: If all integers are negative, value is always increasing but sign is alternating. There could be a subarray that yields the maximum product without including all integers in its calculation.
    <br />
    1. The idea is to keep track of both the maximum product subarray and the minimum product subarray.
    <br />
    2. For each integer, store the maximum and minimum value using the previous maximum and minimum value.
    <br />
    Edge case: 0 value -> when integer is equal to 0, replace maximum and minimum value with 1.
    <br />
    <br />
    <img src='./Technical_interviews.assets/leetcode152img2.png' />
</details>

```python
# Python solution - O(n)
def maxProduct(nums: List[int]) -> int:
    result = max(nums)
    currentMax = 1
    currentMin = 1
    for number in nums:
        if number == 0:
            currentMax = 1
            currentMin = 1
            continue
        temp = number * currentMax # To avoid using updated currentMax in calculating currentMin
        currentMax = max(number * currentMax, number * currentMin, number)
        currentMin = min(temp, number * currentMin, number)
        result = max(result, currentMax, currentMin)
    return result
```

```javascript
// Javascript solution - O(n)
function maxProduct(nums) {
    let result = Math.max(...nums)
    let currentMax = 1
    let currentMin = 1
    for (let number of nums) {
        if (number === 0) {
            currentMax = 1
            currentMin = 1
            continue
        }
        const temp = number * currentMax
        currentMax = Math.max(number * currentMax, number * currentMin, number)
        currentMin = Math.min(temp, number * currentMin, number)
        result = Math.max(result, currentMax, currentMin)
    }
    return result
}
```



















