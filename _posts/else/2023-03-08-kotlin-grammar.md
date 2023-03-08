---
title: "코틀린 문법 정리"
date: 2023-03-08
toc: true
toc_sticky: true
categories: Else
---

# 변수
    - val : 불변
    - var : 가변 (변수 타입은 유지)

# 문자열
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
# 배열
    - Array\<T>
        - e.g. IntArray, BooleanArray, ...
    - 코틀린에서 배열 타입은 다른 배열타입과 서로 하위 타입 관계가 성립하지 않는다고 간주된다.
    - 배열끼리 + 연산 사용 가능
        - e.g. intArrayOf(1, 2, 3) + intArrayOf(5, 6)

# 함수
    - 자바와 달리 unreachable code가 오류가 아님
        - 컴파일러에서 경고는 표시해준다.
    - 함수 파라미터는 무조건 불변
        - 그렇기에 val, var 표시가 붙지 않는다.
        - e.g. fun increment(n: Int): Int { ... }
    - 반환 타입을 생략할 수 있는 상황이 존재
        - 자바 void에 해당하는 Unit 타입을 반환
        - 함수가 단일 식으로 구성되어 있는 경우
            ```kotlin 
            fun circleArea(radius: Double) = PI*radius*radius 
            ```
    - 위치 기반 인자
        - 함수 호출 시, 매개변수 선언 순서에 맞게 파라미터를 넘겨준다.
            ```kotlin
            retangleArea(w, h)
            ```
    - 이름 붙은 인자 (named argument)
        - 함수 호출 시, 매개변수의 이름을 파라미터에 명시함으로써 전달
            ```kotlin
            rectangleArea(width = w, height = h)
            ```    
    - 오버로딩
        - 오버로딩한 함수의 파라미터 타입이 모두 달라야 한다.
    - 디폴트 파라미터
        - 디폴트 값이 있는 파라미터를 함수 인자 목록 뒤쪽에 몰아두는 게 더 좋은 코딩스타일.
        ```kotlin
        fun readInt(radix: Int = 10) = readLine()!!.toInt(radix)
        ```
    - vararg
        ```kotlin
        fun printSorted(vararg items: Int) { ... }

        printSorted(1, 2, 3, 4);

        val numbers = intArrayOf(1, 2, 3, 4)
        printSorted(*numbers) // * : spread operator (swallow copy)

        printSorted(1, 2, *numbers, 3, 4)
        ```

# 패키지
    - 같은 패키지 안에서는 간단한 이름을 사용해 패키지 내에 있는 다른 정의를 참조할 수 있다
    - 디렉토리 구조와는 별개. (자바는 같아야 함)
        - 한 디렉토리에 있는 소스 파일들이 각각 다른 패키지에 포함될수도,
        - 한 패키지에 포함된 소수 파일들이 각각 다른 디렉토리에 들어갈 수도 있다.
    - import alias
        ```kotlin
        import foo.readInt as fooReadInt
        import bar.readInt as barReadInt

        import kotlin.math.*
        ```


# 조건문
    - if/else
        - 자바와는 다르게 if문을 식으로 사용할 수 있다.
        ```kotlin
        // 각 if/else 블록 맨 끝에 있는 식의 값이 블록 전체의 값이 된다.
        // 식으로 사용하는 경우, else문이 필수가 된다.
        fun max(a: Int, b: Int) = if (a > b) a else b
        ```
        ```kotlin
        fun someFunction(fullName: String): String {
            val i = fullName.lastIndenOf('.');
            val prefix = if (i>= 0) fullName else return "" //return문은 Nothing이라는 특별한 타입의 값으로 간주되며, Nothing 타입은 모든 코틀린 타입의 하위 타입으로 간주되고 때문에, return을 사용해도 타입 오류가 발생하지 않는다.
            return prefix;
        }
        ```
    - 범위, 진행, 연산
        - kotlin.ranges 패키지 문서 참고
        - 모든 비교 가능한(comparable) 타입에 대해 지원, 닫힌 연산
        ```kotlin
        val chars = 'a'..'h' // 'a'부터 'h'까지 모든 문자
        val twoDigits = 10..99 // 10부터 99까지의 모든 수
        val zero2one = 0.0..1.0 // 0부터 1까지의 모든 부동소수점 수

        val num = 33;
        println(num in 10..99)


        // [10, 100)
        val twoDigits = 10 until 100

        1..10 step 3 // 1, 4, 7, 10
        15 downTo 9 step 2 // 15, 13, 11, 9

        2 in intArrayOf(3, 7, 2, 1) // true
        'a' in "Hello!" // false
        ```



    



 
 
 
 

 
 

 
 
