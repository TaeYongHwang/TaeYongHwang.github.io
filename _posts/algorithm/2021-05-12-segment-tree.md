---
title: "segment tree"
date: 2021-05-12
toc: true
toc_sticky: true
categories: Algorithm 
---

# Segment Tree
- 배열의 값이 지속적으로 바뀌는 상황에서 부분합을 구하는 방법
- __full binary tree__ 구조 사용
- 변경 시마다 O(logN) 만큼 걸림

> 부모 노드의 값은 양쪽 자식 노드의 합
> 배열의 요소는 리프 노드에 존재


## 부분합 구하기
- 노드가 담당하는 구간이 [start, end]
- 합을 구하는 구간이 [left, right]

1. [left, right] 와 [start, end] 가 겹치지 않는 경우
    - return 0; 로 탐색 종료

2. [left, right] 가 [start, end] 를 완전히 포함하는 경우
    - 해당 노드값을 리턴
    
3. [start, end] 가 [left, right] 를 완전히 포함하는 경우
4. [left, right] 와 [start, end] 가 겹쳐져 잇는 경우 (1,2,3제외)
    - 3,4의 경우 왼쪽, 오른쪽 자식으로 들어가야 함
    
## 수 변경
- 노드가 담당하는 구간이 [start, end]
- diff : 변경값 - 원래값

1. [start, end] 에 idx 가 포함되는 경우
    - 해당 값을 diff만큼 증가

2. [start, end] 에 idx 가 포함되지 않는 경우
    - 탐색 종료


## 코드
```cpp
#include <vector>
#include <cmath>

using namespace std;

class SegTree {
    public:
        vector<long long> tree;
        int height;
        int leafStart;

    // SegTree tree(a.size());
    SegTree(int n) {
        height = ceil(log2(n));
        tree = vector<long long>(1 << (height + 1));
    }

    // Ex) init(a, 1, 0, a.size() -1);
    long long init(vector<long long> &a, int node, int start, int end) {
        long long& ret = tree[node];

        if(start == end) return ret = a[start];

        int mid = (start + end) / 2;

        return ret = init(a, node * 2, start, mid) + init(a, node * 2 + 1, mid+1, end);
    }

    // Ex) sum(1, 0, a.size()-1, left, right) , [left, right] 
    long long sum(int node, int start, int end, int left, int right) {
        if(right < start || left  > end) {
            return 0;
        }

        if(left <= start && end <= right) {
            return tree[node];
        }

        int mid = (start + end) /2;

        return  sum(node*2, start, mid, left, right) + sum(node*2 +1, mid + 1, end, left, right);
    }

    // Ex) update(1, 0, a.size()-1, idx, diff), idx : a에 접근 가능한 idx, diff : 현재 a[idx] - 이전 a[idx]
    void update(int node,  int start, int end, int idx, long long diff) {
        if(idx < start || idx > end) return;

        tree[node] = tree[node] + diff;
        if (start != end) {
            int mid = (start + end) / 2;
            update(node*2, start, mid, idx, diff);
            update(node*2 +1, mid+1, end, idx, diff);
        }
    }
};

```
