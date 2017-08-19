---
title: max points within angle
date: 2017-08-19 17:23:45
tags: [python, algorithm]
categories: [algorithms]
---
Given a 30 degree angle on the origin point, find the maximum points within the angle area.
```python
import math
# p is an array of tuples
def maxPointsWithIn30Degree(p):
  l = len(p)
  res = 0
  threshold = math.cos(30*math.pi/180)
  for i in range(l):
    posi_count = 1
    neg_count = 1
    for j in range(l):
      if j == i: continue
      if cos(p[i], p[j]) > threshold:
        # check p[j] is on the positive side or negative side
        if check(p[i], p[j]) >= 0: posi_count += 1
        if check(p[i], p[j]) <= 0: neg_count += 1
    res = max(res, max(posi_count, neg_count))
  return res

def checkSide(a, b):
  return a[0]*b[0] - a[1]*b[1]

def cos(a, b):
  return (a(0)*b(0) + a(1)*b(1))/(
    math.sqrt(a(0)**2 + a(1)**2)*math.sqrt(b(0)**2 + b(1)**2))
```
