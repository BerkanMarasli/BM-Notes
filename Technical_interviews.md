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

























