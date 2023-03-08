---
title: "코틀린 문법 정리"
date: 2023-03-08
toc: true
toc_sticky: true
categories: Else
---

- 변수
    - val : 불변
    - var : 가변 (변수 타입은 유지)

- 문자열
    - ${코틀린 식}
        - ${Date()}
    - $코틀린값
        - e.g. $name
    - raw string
        - e.g. """ asdf """.trimIndent()
    - 접근
        - s[0], s[1], ...
    - 동등성
        - ==, !=
        - 문자들의 순서와 길이가 같으면 같은 문자열로 간주 (인스턴스 비교 시에도 동일)
        - 코틀린에서 자바와 같은 참조 동등성을 쓰고 싶다면 '===, !==' 사용해야 한다.
- 배열
    - Array\<T>
        - e.g. IntArray, BooleanArray, ...
    - 코틀린에서 배열 타입은 다른 배열타입과 서로 하위 타입 관계가 성립하지 않는다고 간주된다.
    - 배열끼리 + 연산 사용 가능
        - e.g. intArrayOf(1, 2, 3) + intArrayOf(5, 6)
        




 
 
 
 

 
 

 
 
