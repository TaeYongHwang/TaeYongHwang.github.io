---
categories: Spring
title: "Spring Bean Lifecycle"
toc: true
toc_sticky: true
date: 2022-02-27
---

# Spring Bean Lifecycle
- 스프링 빈의 라이플사이클은 **스프링 컨테이너**에 의해 관리된다.

## TODO
1. 빈의 생성
    - 스프링 컨테이너가 생성되며 진행
    1. 특정 interface를 실행하는 일반적인 빈 라이프사이클
    2. 명시적으로 지정해주는 방식
2. 빈의 삭제
    - 스프링 컨테이너가 소멸되며 진행
    1. 특정 interface를 실행하는 일반적인 빈 라이프사이클
    2. 명시적으로 지정해주는 방식
     
3. 프로토타입 빈의 경우
    - 프로토타입 빈: 객체가 할당될 때마다 생성
    - 명시적 삭제 루틴?
    