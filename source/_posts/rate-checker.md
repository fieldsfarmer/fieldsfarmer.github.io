---
title: rate checker
date: 2016-11-04 17:36:07
tags: [python, algorithm]
categories: [algorithms]
---
```py
import collections
import time

class RateChecker(object):
  def __init__(self,q,n):
    self.dq = collections.deque()
    self.q = q
    self.n = n

# if in q seconds, has more than n requests, then true;
# otherwise, false
  def check(self):
    t = time.time()
    self.dq.append(t)
    if len(self.dq) < self.n:
      return False
    last = self.dq.popleft()
    return t-last < self.q

def main():
  rc = RateChecker(1,3)
  for i in range(3):
    print rc.check()
    time.sleep(0.1)
  time.sleep(1.7)
  for i in range(4):
    print rc.check()
    time.sleep(0.1)

#False False True False False True True

if __name__ == '__main__':
  main()
```
