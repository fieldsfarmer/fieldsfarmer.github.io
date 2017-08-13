---
title: thread safe stack
date: 2017-01-01
tags: [c++]
categories: data structure
---
<!-- more -->
```cpp
template<typename T>
class my_stack{
public:
  void push(const T& item){
    lock_guard<mutex> g(_m);
    _s.push(item);
  }
  void pop(){
    lock_guard<mutex> g(_m);
    _s.pop();
  }
  T top() {
    lock_guard<mutex> g(_m);
    return _s.top();
  }
  bool empty() {
    lock_guard<mutex> g(_m);
    return _s.empty();
  }
private:
  stack<T> _s;
  mutex _m;
};
```
