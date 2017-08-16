---
title: backpack problem
date: 2017-08-16 09:03:18
tags: [python, algorithm]
categories: algorithms
---
# Type 1: only size matters
Given items with size `A[i]`, an integer `m` denotes the size of a backpack. How full you can fill this backpack? Each item can only be used for once.
```python
def back_pack_1(m,A):
  dp = [[False]*(m+1) for _ in range(len(A)+1)]
  dp[0][0]=True
  for i in range(len(A)):
    for j in range(m+1):
      dp[i+1][j]=dp[i][j]
      if j>=A[i] and dp[i][j-A[i]]:
        dp[i+1][j]=True
  for i in range(m,-1,-1):
    if dp[len(A)][i]:
      return i
  return 0
# space optimized
def back_pack_1_1(m,A):
  dp = [False]*(m+1)
  tmp = [False]*(m+1)
  dp[0] = True
  for i in range(len(A)):
    for j in range(m+1):
      tmp[j]=dp[j]
      if A[i]<=j and dp[j-A[i]]:
        tmp[j] = True
    dp = tmp
    tmp = [False]*(m+1)
  for j in range(m,-1,-1):
    if dp[j]:
      return j
  return 0
```
Following the above, but item can be used for infinite times
```python
def back_pack_2(m,A):
  dp = [[False]*(m+1) for _ in range(len(A)+1)]
  dp[0][0]=True
  for i in range(len(A)):
    for j in range(m+1):
      dp[i+1][j]=dp[i][j]
      if j>=A[i] and dp[i+1][j-A[i]]:
        dp[i+1][j]=True
  for i in range(m,-1,-1):
    if dp[len(A)][i]:
      return i
  return 0

def back_pack_2_1(m,A):
  dp=[False]*(m+1)
  dp[0]=True
  for i in range(len(A)):
    for j in range(m+1):
      if j>=A[i] and dp[j-A[i]]:
        dp[j]=True
  for i in range(m,-1,-1):
    if dp[i]:
      return i
  return 0
```
---
# Type 2: size and value
Given items with size `A[i]` and value `V[i]`, and a backpack with size `m`.
What's the maximum value can you put into the backpack?
```python
def back_pack_3(m,A,V):
  dp = [[0]*(m+1) for _ in range(len(A)+1)]
  for i in range(len(A)):
    for j in range(m+1):
      dp[i+1][j]=dp[i][j]
      if j>=A[i]:
        dp[i+1][j]=max(dp[i+1][j], dp[i][j-A[i]]+V[i])
  return dp[len(A)][m]

def back_pack_3_1(m,A,V):
  dp = [0]*(m+1)
  for i in range(len(A)):
    for j in range(m,-1,-1):
      if j >= A[i]:
        dp[j] = max(dp[j], dp[j-A[i]]+V[i])
  return dp[m]
```
Following the above, but infinite times
```python
# dp[i+1][j] = max(dp[i+1][j], dp[i+1][j-A[i]]+V[i])
def back_pack_4(m,A,V):
  dp = [0]*(m+1)
  for i in range(len(A)):
    for j in range(m+1):
      if j>=A[i]:
        dp[j]=max(dp[j],dp[j-A[i]]+V[i])
  return dp[m]
```
# Type 1.1: how many ways to fill
```python
def back_pack_5(m, A):
  dp=[[0]*(m+1) for _ in range(len(A)+1)]
  dp[0][0]=1
  for i in range(len(A)):
    for j in range(m+1):
      dp[i+1][j]=dp[i][j]
      if j>=A[i]:
        dp[i+1][j]+=dp[i][j-A[i]]
  return dp[len(A)][m]
```
Follow up: each item may be chosen unlimited number of times
```python
def back_pack_6(m, A):
  dp=[[0]*(m+1) for _ in range(len(A)+1)]
  dp[0][0]=1
  for i in range(len(A)):
    for j in range(m+1):
      dp[i+1][j]=dp[i][j]
      if j>=A[i]:
        dp[i+1][j]+=dp[i+1][j-A[i]]
  return dp[len(A)][m]

def back_pack_6_1(m,A):
	dp=[0]*(m+1)
  dp[0]=1
  for i in range(len(A)):
    for j in range(m+1):
      if j>=A[i]:
        dp[j]+=dp[j-A[i]]
  return dp[m]
```
# Type 1.2: slightly different
Given an array of positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer target.
```python
# Given A = [1, 2, 4], m = 4
# # The possible combination ways are:
# [1, 1, 1, 1]
# [1, 1, 2]
# [1, 2, 1]
# [2, 1, 1]
# [2, 2]
# [4]

def back_pack_7(m, A):
  dp=[0]*(m+1)
  dp[0]=1
  for i in range(m+1):
    for j in range(len(A)):
      if i>=A[j]:
        dp[i]+=dp[i-A[j]]
  return dp[m]

```
[ref](http://love-oriented.com/pack/Index.html)
