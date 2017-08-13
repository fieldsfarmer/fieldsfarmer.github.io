---
title: c++11 thread notes
date: 2016-12-17
tags: [c++]
categories: programming languages
---
<!-- more -->
Normally you run the code by:
```
g++ filename -std=c++11 -pthread
```
---
function with reference parameter
```c
#include<thread>
#include<iostream>
#include<string>
#include<functional> //ref
using namespace std;
int main(){
  string s = "Hello!";
  cout << "Main before: " << s << endl;
  thread t([](string& s){cout << s << endl; s = "Ni hao!";}, ref(s));
  t.join();
  cout << "Main after: " << s << endl;
  return 0;
}

```
---
A situation of deadlock:
``` cpp
#include<mutex>
#include<iostream>
#include<thread>
#include<string>
using namespace std;
mutex mu1;
mutex mu2;
void shared_print_1(string s, int v){
  lock_guard<mutex> g1(mu1);
  lock_guard<mutex> g2(mu2);
  cout << s << v << endl;
}
void shared_print_2(string s, int v){
  lock_guard<mutex> g2(mu2);
  lock_guard<mutex> g1(mu1);
  cout << s << v << endl;
}
void f(int n){
  for(auto i=0; i<n; ++i){
    shared_print_2("From thread ", i);
  }
}

int main(){
  thread t(f, 10);
  for(auto i = 10; i>=0; i--){
    shared_print_1("From main ", i);
  }
  t.join();
  return 0;
}

```
To prevent it, either you should make the locks in `shared_print_1` and  `shared_print_2` have the same order; or use [`std::lock`](http://en.cppreference.com/w/cpp/thread/lock)
``` cpp
void shared_print_1(string s, int v){
  lock(mu1, mu2);
  lock_guard<mutex> g1(mu1, adopt_lock);
  lock_guard<mutex> g2(mu2, adopt_lock);
  cout << s << v << endl;
}
void shared_print_2(string s, int v){
  lock(mu2, mu1);
  lock_guard<mutex> g2(mu2, adopt_lock);
  lock_guard<mutex> g1(mu1, adopt_lock);
  cout << s << v << endl;
}
```
Some practices in avoiding deadlock:
- prefer locking single mutex
- avoid locking a mutex and then call a user provided function
- use `std::lock` to lock more than one mutex
- lock the mutex in the same order
---
An efficient way to open file using [call_once](http://en.cppreference.com/w/cpp/thread/call_once). Note the following also uses lazy initialization as the file only needed to be open when `shared_print` is called.
``` cpp
class LogFile{
  mutex _mu; ofstream _f; once_flag _flag;
public:
  LogFile(){}
  void shared_print(string s, int v){
    call_once(_flag, [&](){_f.open("filename");});
    lock_guard<mutex> locker(_mu);
    _f << s << v << endl;
  }
}
```
---
**conditional variable**: to synchronize the execution of threads</pre>
``` cpp
#include<iostream>
#include<condition_variable>
#include<mutex>
#include<deque>
#include<thread>
using namespace std;

deque<int> q;
mutex mu;
condition_variable cv;

void producer(){
  int count = 10;
  while(count > 0){
    unique_lock<mutex> lock(mu);
    q.push_front(count);
    cout << "produce data: " << count << endl;
    lock.unlock();
    cv.notify_one();
    count--;
    this_thread::sleep_for(chrono::seconds(1));
  }
}
void consumer(){
  int data = 0;
  while(data != 1){
    unique_lock<mutex> lock(mu);
    cv.wait(lock, [](){return !q.empty();}); // prevent spurious wake
    data = q.back();
    q.pop_back();
    cout << "consume data: " << data << endl;
    lock.unlock();
  }
}
int main(){
  thread t1(producer);
  thread t2(consumer);
  t1.join();
  t2.join();
}
```
