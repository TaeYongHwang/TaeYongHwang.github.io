---
categories: Spring
title: "Spring DI(Dependency Injection)"
toc: true
toc_sticky: true
date: 2022-02-18
---

# 의존 주입에서 '의존'
- 한 클래스가 다른 클래스의 메서드를 실행하는 것
- 클래스 내부에서 직접 의존 객체를 생성하는 것이 유지보수 관점에서 문제점을 유발할 수 있다.

# DI
- 객체를 직접 생성하는 대신 의존 객체를 전달받는 방식 사용
    - e.g. 생성자를 통해 의존 객체를 전달받는다.
    - 변경에 대해 유연함을 제공할 수 있다.
    
## Ojbect Assembler
- 객체를 생성하고 의존 객체를 주입해주는 클래스
- 스프링이 DI를 지원하는 Assembler
