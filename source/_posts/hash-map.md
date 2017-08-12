---
title: hash map
date: 2016-12-31
tags: [c++]
categories: data structure
---
<escape><!-- more --></escape>
A simple implementation using separate chaining, [ref](https://medium.com/@aozturk/simple-hash-map-hash-table-implementation-in-c-931965904250#.m7j76vce2).
```cpp
#include<stdexcept>
#include<iostream>
#include<string>
using space std;

template<typename K, typename V>
struct node{
  K key; V value; node * next;
  node(const K& k, const V& v):key(k),value(v),next(NULL){}
};
unsigned long TABLE_SIZE = 101;
template<typename K, typename V, typename F>
class sc_hash_map{
public:
  sc_hash_map(unsigned long sz = TABLE_SIZE):_size(sz){
    _table = new node<K, V>*[_size];
  }
  ~sc_hash_map(){
    for(int i=0; i<_size; ++i){
      auto entry = _table[i];
      while(entry){
        auto prev = entry;
        entry = entry->next;
        delete prev;
      }
      _table[i] = NULL;
    }
    delete[] table;
  }
  const V& get(const K& k){
    auto index = _hash_func(k);
    auto entry = _table[index];
    while(entry){
      if(entry->key == k){
        return entry->value;
      }
      entry = entry->next;
    }
    raise out_of_range("invalid key");
  }
  void insert(const K& k, const V& v){
    auto index = _hash_func(k);
    auto entry = _table[index];
    while(entry && entry->key != k){
      auto prev = entry;
      entry = entry->next;
    }
    if(!entry){
      entry = new node<K,V>(k,v);
      auto tmp = _table[index];
      _table[index] = entry;
      entry->next = tmp;
    } else {
      entry->value = v;
    }
  }
  void remove(const K& k){
    auto index = _hash_func(k);
    auto entry = _table[index];
    node<K,V>* prev = NULL;
    while(entry && entry->key != k){
      prev = entry;
      entry = entry->next;
    }
    if(!entry){
      return;
    } else {
      if(_table[index] == entry){
        _table[index] = entry->next;
      } else {
        prev->next = entry->next;
      }
      delete entry;
    }
  }
private:
  node<K, V> ** _table;
  F _hash_func;
  unsigned long _size;
}
struct my_hash_func{
  unsigned long operator()(const int& k){
    return k%TABLE_SIZE;
  }
}
int main(){
  sc_hash_table<int, string, my_hash_func> mp;
  mp.insert(1, "hello");
  mp.insert(2, "world");
  mp.insert(102, "nihao");
  cout << mp.get(1) << endl;
  return 0;
}
```
---
open addressing
```cpp
#include<iostream>
#include<string>
using namespace std;

enum STATE {EMPTY, OCCUPIED, DELETED};
int TABLE_SIZE = 101;
template<typename K, typename V, typename F>
class lp_hash_map{
public:
  lp_hash_map(unsigned long sz = TABLE_SIZE):_size(sz){
    _table = new pair<K, V>[_size];
    _state = new STATE[_size];
  }
  ~lp_hash_map(){
    delete[] _table;
    delete[] _state;
  }
  int find_index(const K& k){
    auto index = _hash_func(k);
    auto start = index;
    while(_state[index] != EMPTY){
      if(_state[index] == OCCUPIED && _table[index].first == k)
        return index;
      index = (index + 1)%_size;
      if(index == start) break;
    }
    return -1;
  }
  V& get(const K& k){
    auto index = find_index(k);
    if(index < 0){
      throw out_of_range("invalid key!");
    } else {
      return _table[index].second;
    }
  }
  void insert(const K& k, const V& v){
    auto index = _hash_func(k);
    auto start = index;
    while(_state[index] == OCCUPIED && _table[index].first != k){
      index = (index + 1)%_size;
      if(index == start) throw out_of_range("full!");
    }
    _table[index] = make_pair(k, v);
    _state[index] = OCCUPIED;
  }
  void remove(const K& k){
    auto index = find_index(k);
    if(index < 0) return;
    _state[index] = DELETED;
  }
private:
  F _hash_func;
  pair<K, V>* _table;
  STATE* _state;
  unsigned long _size;
};
struct MyKeyHash {
  unsigned long operator()(const int& k){
    return k % TABLE_SIZE;
  }
};
int main(){
  lp_hash_map<int, string, MyKeyHash> mp;
  mp.insert(1,"hello");
  cout << mp.get(1) << endl;
  mp.insert(2,"world");
  mp.insert(1,"nihao");
  cout << mp.get(1) << endl;
  mp.remove(1);
  cout << mp.get(1) << endl;
  return 0;
}
```
