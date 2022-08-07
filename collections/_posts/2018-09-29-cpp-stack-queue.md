---
title: "[C++] Stack과 Queue 사용법"
categories:
  - C++
tags:
  - Language
  - C++
date: 2018-09-29
last_modified_at: 2021-11-02
toc: false
---

Cpp에서 Stack과 Queue를 사용하는 간단한 방법입니다.

``` cpp
#include <iostream>
#include <stack>
#include <queue>

using namespace std;

int main()
{
    cout << "-----------------Stack-----------------" << endl;

    cout << endl;
 
    stack<int> st;
 
    st.push(1);
    st.push(2);
    st.push(3);
 
    int count = st.size();
    for(int i=0; i < count; i++){
        cout << st.top() << endl;
        st.pop();
    }
 
    cout << endl;
    cout << endl;
 
    cout << "-----------------Queue-----------------" << endl;
 
    cout << endl;
 
    queue<int> qu;
     
    qu.push(1);
    qu.push(2);
    qu.push(3);
 
    count = qu.size();
    for(int i=0; i < count; i++){
        cout << qu.front() << endl;
        qu.pop();
    }
 
    return 0;
}
```
