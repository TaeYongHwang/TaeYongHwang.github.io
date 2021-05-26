---
title: "cpp 자주 쓰는 알고리즘"
date: 2020-11-20
toc: true
toc_sticky: true
categories: Algorithm 
---

# 순열

nCk를 구할 수도 있는데, 이 경우 v에 1원소를 k개, 0원소를 n-k개 삽입해 구할 수 있다. [0,0, ..., 1, 1, ..., 1] 형태


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

# 그래프

## 플로이드-워셜 알고리즘
- 모든 최단경로를 구하는 문제

```cpp
vector<vector<int>> board;


int findAns(int n) {
    vector<vector<int>> minFares = vector<vector<int>>(n, vector<int>(n,0));
    
    for(int i = 0 ; i < n ; i++) {
        for(int j = 0 ; j < n ; j++) {
            minFares[i][j] = board[i][j];
        }
    }
    
    
    for(int k = 0 ; k < n ; k ++) {      
        for(int i = 0 ; i < n ; i++) {
            for(int j = 0 ; j < n ; j ++) {
                if(minFares[i][k] + minFares[k][j] < minFares[i][j]) {
                    minFares[i][j] = minFares[i][k] + minFares[k][j];
                }
            }          
        }
        
    }
}

```

## 다익스트라 알고리즘
- 하나의 정점에서 다른 모든 정점으로 가는 최단경로 알고리즘
- 음의 간선 포함 X

1. 출발 노드 설정
2. 출발 노드 기준으로 각 노드 최소 비용 저장
3. 방문하지 않은 노드 중 가장 비용이 적은 노드 선택
4. 최소 비용 갱신
5. 모든 노드에 대해 3~4 반복


