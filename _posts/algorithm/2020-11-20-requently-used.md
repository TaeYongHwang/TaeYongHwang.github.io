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



