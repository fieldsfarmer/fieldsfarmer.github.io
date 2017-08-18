---
title: generate random number with given distribution
date: 2017-08-17 20:18:44
tags: [python, algorithm]
categories: algorithms
---
Given an discrete distribution array, use it to generate its index number with the corresponding probability. For example, `[0.4, 0.6]` will produce `0` with probability `0.4` and `1` with probability `0.6`.
```python
import random

class makeSampler:
  def __init__(self, arr):
    self.dart = [arr[0]]
    for i in range(1,len(arr)):
      t = self.dart[-1]
      self.dart.append(arr[i]+t)

  def sample(self):
    a = random.random() # generate a random number within [0.0, 1.0)
    # the following using a trick similar to "first bad version"
    l,r=0,len(self.dart)-1
    while l<=r:
      m=l+(r-l)/2
      if a<=self.dart[m]:
        r=m-1
      else:
        l=m+1
    return l

X = makeSampler([1])
print X.sample()
X = makeSampler([0.5,0.5])
print X.sample()
X = makeSampler([0.1,0.2,0.3,0.4])
print X.sample()
print X.sample()
print X.sample()
print X.sample()

```
