---
title: "cpp 자주 쓰는 알고리즘"
date: 2020-11-20
toc: true
toc_sticky: true
categories: Algorithm 
---
# 조합

```cpp
void combination(string src, string crs, int depth) {
    if (crs.size() == depth) {
        /** 필요 로직 **/
    }
    else for (int i = 0; i < src.size(); i++)
        combination(src.substr(i+1), crs+src[i], depth);
}
```

# 문자열 분할
```cpp
// delimiter 기준 분할
#include <string>
#include <vector>
#include <sstream>
using namespace std;

vector<string> split(string str, char delimiter) {
    vector<string> answer;
    stringstream ss(str);
    string temp;
 
    while(getline(ss, temp, delimiter)) {
        /** 추가 로직 구현 **/
        answer.push_back(temp);
    }
 
    return answer;
}
```



