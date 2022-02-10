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
