---
layout: post
title: [leetcode] Binary Search
subtitle: [1/1000]
author: weejw
categories: algorithm
banner:
  loop: true
  volume: 0.8
  start_at: 8.5
  image: https://upload.wikimedia.org/wikipedia/commons/thumb/3/39/Kubernetes_logo_without_workmark.svg/1200px-Kubernetes_logo_without_workmark.svg.png
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
tags: leetcode algorithm 알고리즘 BinarySearch
sidebar: []
---

# Binary Search
https://leetcode.com/problems/binary-search/

<u>EASY</u>

Given an array of integers nums which is sorted in ascending order, and an integer target, write a function to search target in nums. If target exists, then return its index. Otherwise, return -1.
You must write an algorithm with O(log n) runtime complexity.


python dictionary는 탐색 속도가 log n이므로 이를 이용해서 풀이.

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        nums = {num:i for i, num in enumerate(nums)}
        if target in nums:
            return nums[target]
        return -1
    
```



# First Bad Version
https://leetcode.com/problems/first-bad-version/

<u>EASY</u>

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have n versions [1, 2, ..., n] and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API bool isBadVersion(version) which returns whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.


### 첫번째 시도

memory error가 발생했다. 위 문제처럼 풀려고 했는데 너무 나이브하게 풀었나보다..
```python
class Solution:
    def firstBadVersion(self, n: int) -> int:
        n = list(range(1, n+1))
        n_len = len(n)
        n_center_pivot = n_len // 2
        n_center_version = n[n_center_pivot]
        
        while True:
            n_len = len(n)
            n_center_pivot = n_len // 2
            
            
            if n_len == 1:
                return n[0] if isBadVersion(n[0]) else n_center_version
            
            elif n_len ==2:
                if isBadVersion(n[0]):
                    return n[0]
                elif isBadVersion(n[1]):
                    return n[1]
                else:
                    return n_center_version
            
            
            
            n_center_version = n[n_center_pivot]   
            
            if isBadVersion(n_center_version):
                n = n[:n_center_pivot]
                
            else:
                n = n[n_center_pivot+1:]
                 
```
