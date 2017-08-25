---
title: sentence break
date: 2017-08-25 08:56:07
tags: [python, algorithm]
categories: algorithms
---
Given a long string, break it into a rray of strings. Each string length should be no larger than the given limit.
```python
def break_down(string, char_limit):
  res = []
  arr = string.split()
  pre = ''
  i = 0
  while i < len(arr):
    if len(pre) == 0:
      pre = arr[i]
      i += 1
    elif len(pre) > char_limit:
      chop_arr = chopWord(pre, char_limit)
      if len(chop_arr[-1]) == char_limit or len(chop_arr[-1]) + 1 == char_limit:
        res += chop_arr
        pre = ''
      else:
        res += chop_arr[:-1]
        pre = chop_arr[-1]
    elif len(pre) == char_limit or len(pre) + 1 == char_limit:
      res.append(pre)
      pre = ''
    else:
      if len(pre) + 1 + len(arr[i]) <= char_limit:
        pre = pre + ' ' + arr[i]
        i += 1
      else:
        res.append(pre)
        pre = ''
        # if len(arr[i]) <= char_limit:
        #     res.append(pre)
        #     pre = ''
        # else:
        #     pre = pre + ' ' + arr[i]
        #     i += 1
  if len(pre) > 0:
    res += chopWord(pre, char_limit)
  return res

def chopWord(word, limit):
  res = []
  def helper(word, limit, arr):
    if len(word) <= limit:
      arr.append(word)
      return
    else:
      arr.append(word[:limit])
      helper(word[limit:], limit, arr)
  helper(word, limit, res)
  return res

s = "Hey Ian, your Uber is arriving now!"
res = break_down(s, 3)
for i in res:
  print (i)
res = break_down(s, 20)
for i in res:
  print(i)
s = "Hey Ian Smithington-Mephisto, your Uber is on the way!"
res = break_down(s, 10)
for i in res:
  print(i)
```
