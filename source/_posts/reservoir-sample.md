---
title: reservoir sample
date: 2016-10-31
tags: [python, algorithm]
categories: algorithms
---
<!-- more -->
[Algorithm R](https://en.wikipedia.org/wiki/Reservoir_sampling)
```py
import random

def reservoir_sample(arr, k):
  res = []
  t = []
  for i in range(k):
    t.append(arr[i]);
  res.append(t)
  for j in range(k,len(arr)):
    t = list(t)
    i = random.randint(0,j)
    if i<k:
      t[i]=arr[j]
    res.append(t)
  return res
arr = [1,2,3,4,5,6,7]
print(reservoir_sample(arr, 2))
```
