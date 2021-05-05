---
title: "cpp 자주 쓰는 알고리즘"
date: 2020-11-20
toc: true
toc_sticky: true
categories: Algorithm 
---

# 순열
- nCk를 구할 수도 있는데, 이 경우 v에 1원소를 k개, 0원소를 n-k개 삽입해 구할 수 있다.
``` cpp
    vector<int> v;

	do{
        //추가 로직 구현

	}while(next_permutation(v.begin(),v.end()));
```


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

# 문자열 관련

## 문자열 분할 
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

## 문자 변환, 확인

```cpp
#include <cctype>

// 문자 넣을 시, 아스키코드 기반 변환해서 들어간다.
// 다른 모든 문자는 그대로 반환
int tolower(int c);
int toupper(int c);

int isalpha (int c);

```


