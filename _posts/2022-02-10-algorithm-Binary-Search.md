---
layout: post
title: Leet Code - Binary Search
subtitle: 코딩테스트 공부
author: weejw
categories: algorithm

tags: [Algorithm, 알고리즘, leetcode, binary search, 이진검색]
---

요즘 코테를 그냥 아예 등지고 살아서 기초부터 다시해야한다.. 뇌 용량 모지리.. <br>
leetcode는 여러모로 좋다. 문제 난이도도 적당하고( 지금 내겐 아니다 ) 무엇보다 영어공부도 동시에 할 수 있다는 점! 

## Binary Search
[https://leetcode.com/problems/binary-search/](https://leetcode.com/problems/binary-search/)

<u>EASY</u>

Given an array of integers nums which is sorted in ascending order, and an integer target, write a function to search target in nums. If target exists, then return its index. Otherwise, return -1.
You must write an algorithm with O(log n) runtime complexity.


python dictionary는 탐색 속도가 log n이므로 이를 이용해서 풀이. 너무 꼼수인가.. 😣

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        nums = {num:i for i, num in enumerate(nums)}
        if target in nums:
            return nums[target]
        return -1
    
```

++ binary search로 푸는 법은 아래와 같다.
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums) - 1
        
        while left <= right:
            mid = left + (right-left) // 2
            if nums[mid] == target:
                return mid
            if nums[mid] > target:
                right = mid - 1
            else:
                left = mid  + 1
        return -1
    
```

## First Bad Version
[https://leetcode.com/problems/first-bad-version/](https://leetcode.com/problems/first-bad-version/)

<u>EASY</u>

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have n versions [1, 2, ..., n] and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API bool isBadVersion(version) which returns whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

<br>

나는 leecode에서 문제를 풀다보면 아래와 같이 예시로 작성된 부분에서도 몇가지 코딩 스타일(?)을 얻기도 한다.<br>
함수가 시작되는 부분에 param과 return type을 같이 적어주는 부분이 굉장히 좋은 방법인 것 같다.<br>
실제로 코드가 너무 길어지는 경우 내가 짠 코드를 내가 잊는 경우가 심히 많은데 이런 경우에 용이 할 것 같다.<br>
예전에 면접 중 파이썬에서 변수의 타입을 어떻게 관리하냐는 질문을 받은 적이 있다.. 이 때 나는 무척 당황했는데 나의 얕은 지식으로는 파이썬은 변수의 타입을 굳이 명시하지 
않아도 되기때문에 편리하다- 라는 베이스로 살아왔기때문이다... 그러나 현업에 들어서면서 실제로 굉장히 긴 코드를 작성하다보니 타입을 크게 명시해주지 않는 경우엔 내가 나에게 함정을 놓는 경우가 생겨버린다... ㅠㅠ<br>

```python
# The isBadVersion API is already defined for you.
# @param version, an integer
# @return a bool
# def isBadVersion(version):

class Solution(object):
    def firstBadVersion(self, n):
        """
        :type n: int
        :rtype: int
        """
```

### 첫번째 시도

memory error가 발생했다[input: 2126753390 1702766719]. 위 문제처럼 풀려고 했는데 너무 나이브하게 풀었나보다..
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
가 아니라,, 로직을 너무 이상하게(?) 복잡하게 짰던 것 같다. 하려는 의도는 같았던 듯.... 아래와 같이 너무 쉽게 풀린다;;;

```python
# The isBadVersion API is already defined for you.
# @param version, an integer
# @return a bool
# def isBadVersion(version):

class Solution(object):
    def firstBadVersion(self, n):
        """
        :type n: int
        :rtype: int
        """
        
        left, right = 1, n
        
        while left <= right:
            mid = left + (right - left) // 2
            
            if isBadVersion(mid):
                right = mid - 1 
            
            else:
                left = mid + 1 
                
            
        return left 
```



## Search Insert Position
[https://leetcode.com/problems/search-insert-position/](https://leetcode.com/problems/search-insert-position/)

이정도면 그냥 공식처럼 외워진다..<br>
left, right를 선언하고 right가 left보다 크거나 작을 때까지 반복하면서 mid index에 있는 데이터가 target인지 확인하고 그렇지 않으면 left, right를 옮겨준다.

```python

class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        # 안에 있으면 return하고 없으면 넣고 그 index 빼
        
        left = 0
        right = len(nums) - 1
        
        
        while left <= right:
            
            mid = left+(right-left) // 2
            
            if nums[mid] == target:
                return mid
            if nums[mid] > target:
                right = mid - 1 
            else:
                left = mid +1
        
        return left
```

이렇게 해서 Day 1의 이진트리 문제를 모두 간단하게 풀어보았다. <br>
정말 코테는 하루라도 하지않으면,, 너무 빠르게 잊어버리는 것 같다.. <br><br>


