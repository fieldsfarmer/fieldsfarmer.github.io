---
title: c++ template example
date: 2016-12-23
tags: [c++]
categories: programming languages
---
<!-- more -->
```cpp
#include<iostream>
#include<string>
using namespace std;

template<typename T>
struct node{
  T val;
  node* next;
  node(T v):val(v),next(NULL){}
};

template<typename T>
void print_list(node<T>* head){
  while(head){
    cout << head->val << " ";
    head = head->next;
  }
  cout << endl;
}

template<typename T>
node<T>* reverse(node<T>* head){
  if(!head || !(head->next)) return head;
  auto tail = head->next;
  head->next = NULL;
  auto new_head = reverse(tail);
  tail->next = head;
  return new_head;
}

// template<typename T>
// node<T>* reverse(node<T>* head){
//   node<T>* new_head = NULL;
//   node<T>* next = NULL;
//   while(head){
//     next = head->next;
//     head->next = new_head;
//     new_head = head;
//     head = next;
//   }
//   return new_head;
// }

int main(){
  auto h1 = new node<string>("A");
  h1->next = new node<string>("B");
  h1->next->next = new node<string>("C");
  print_list<string>(h1);
  auto new_head = reverse<string>(h1);
  print_list(new_head);
  auto h2 = new node<int>(1);
  h2->next = new node<int>(2);
  h2->next->next = new node<int>(3);
  print_list<int>(h2);
  auto new_head2 = reverse<int>(h2);
  print_list<int>(new_head2);
  return 0;
}
```
