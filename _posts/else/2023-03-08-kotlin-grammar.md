---
title: "코틀린 문법 정리"
date: 2023-03-08
toc: true
toc_sticky: true
categories: Else
---

# 코틀린 기초 
## 변수
- val : 불변
- var : 가변 (변수 타입은 유지)

## 문자열
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
## 배열
- Array\<T>
    - e.g. IntArray, BooleanArray, ...
- 코틀린에서 배열 타입은 다른 배열타입과 서로 하위 타입 관계가 성립하지 않는다고 간주된다.
- 배열끼리 + 연산 사용 가능
    - e.g. intArrayOf(1, 2, 3) + intArrayOf(5, 6)

## 함수
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

## 패키지
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


## 조건문
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

- when
    ```kotlin
    fun hexDigit(n: Int): Char {
        when {
            n in 0..9 -> return '0' + n
            n in 10..15 -> return 'A' + n - 10
            else -> return '?'
        }

        when (n) { //n에 대해서만 수행
            in 0 until 9 -> n
            in 10 until 12 -> n +2
            else -> n+1
        }
    }
    ``` 
    - 다른 언어의 switch문 같은 느낌
        - fall-through가 없기 떄문에, break를 사용해서 탈출하는 게 아님
    - if문처럼 식으로 사용 가능

## 루프
- while, do-while
   - 다른 언어들과 동일
- for
    - 특정 컨테이너를 for 루프에 사용하기 위해서는 컨테이너가 iterator() 함수를 지원하기만 하면 된다. 
    
    ```kotlin
    val a = IntArray(10) {it*it}
    var sum = 0
    
    for(x in a) {
        sum += x
    }

    for(i in 0..a.lastIndex) { //0, 1, 2, ...
        ...      
    }

    for(i in a.indices step 2) { //0, 2, 4, ...
        ...
    }
    ``` 

- tail recursive(꼬리 재귀 함수)에 대한 최적화된 컴파일    
    - 함수에 `tailrec` 키워드를 붙이면 된다.
        - 컴파일러가 재귀 함수를 비재귀적인 코드로 자동 변환해준다.
        - 이런 변환을 적용하기 위해서, 함수가 재귀 호출 다음에 아무 동작도 수행하면 안 된다.

## 예외 처리
- 기본적으로 자바와 같다
    - `new` 키워드가 없기 때문에, `NumberFormatException("...")`와 같이 호출한다.
- try문도 식으로 간주된다. (해당 식의 값은 try, catch블록의 값임)
    ```kotlin
    fun readInt(default: Int) = try {
        readLine()!!.toInt()
    } catch (e: Exception) {
        default
    }
    ``` 
- 검사 예외, 비검사 예외를 구분하지 않는다. 

## 클래스
- default는 public이며 internal, private으로 설정 가능
- 멤버 가시성
    - public (default)
    - internal
        - 멤버를 멤버가 속한 클래스가 포함된 컴파일 모듈 내부에서만 볼 수 있다.
    - protected
        - 멤버가 속한 클래스와 하위 클래스 안에서
    - private


### 주생성자
```kotlin
//클래스 헤더의 파라미터 목록을 주생성자 선언이라고 부름
class Person(firstName: String, familyName: String) {
  val fullName = "$firstName $familyName"
  
  init { //초기화 블록
    println("Created new Person instance: $fullName")
  }
}


// 본문을 생략할 수도 있다.
class Person(val firstName: String, val familyName: String = "")
```
- 주생성자 실행 순서
    - 프로퍼티 초기화와 초기화 블록이 등장하는 순서대로 구성
    - 초기화(init) 블록 존재
        - 주생성자가 호출될 때마다 실행된다.
        - 여러 개의 init 블록 존재 가능
        - return문은 불가
- 주생성자 파라미터를 프로퍼티 초기화나 init 블록 밖에서 사용할 수 없다.
- 생성자 파라미터 앞에 `val`, `var` 키워드를 덧붙이면, 자동으로 해당 생성자 파라미터로 초기화되는 프로퍼티를 정의한다.

### 부생성자
- constructor 키워드 사용
```kotlin
class Person {
  val firstName: String
  val familyName: String

  
  constructor(fullName: String) {
    val names = fullName.split(" ")
    if (names.size != 2) {
      throw IllegalArgumentException("Invalid name: $fullName")
    }
    firstName = names[0]
    familyName = names[1]
  }

  constructor(firstName: String, familyName: String): 
    this("$firstName $familyName") //생성자 위임호출
}
```
- 클래스에 주생성자를 선언하지 않은 경우, 모든 부생성자는 자신의 본문을 실행하기 전에 프로퍼티 초기화와 init 블록을 실행
    
### Nested class
```kotlin
class Person24(val firstName: String, val familyName: String) {
  inner class Possession(val description: String) {
    fun getOwner() = this@Person24 //외부 클래스의 인스턴스를 가리켜야 할 때
  }
}
```
- 내포된 클래스에 `inner`를 붙이면 외부 클래스의 현재 인스턴스에 접근할 수 있다.
    - java에서는 static을 붙여야 정적 클래스가 되지만, kotlin에서는 기본이 정적 클래스이며,
    - 외부 클래스와 연결되기를 원하는 경우 `inner`를 붙여야 한다. (java에서는 안 붙이는 것과 동일)
- `inner`를 붙이지 않는다면, 바깥쪽 클래스는 nest class의 비공개 멤버에 접근할 수 없다.   


## Null
 - 코틀린 타입 시스템을 null 허용/불허 참조 타입을 확실히 구분해준다.
    - 기본적으로 not null 타입.
    - NPE를 컴파일 시점으로 앞당김
- Null을 허용하기 위해서는 타입 뒤에 물음표(?)를 붙여야 함
    - 런타임에는 null과 not null을 구분하지 않기에 어떤 부가 비용도 들지 않는다.
- `x == null` 등을 통해 null 체크 가능 (smart cast)
    - `!!(not-null assertion)` 사용도 가능하지만, 이는 KotlinNPE를 발생시킬 수도 있다.
    - `?.` 를 통해 null인 경우는 null을 반환하고, not null인 경우만 작업 수행일 시킬 수도 있다.
    - `?: ` 를 통해 null을 대신할 default 값을 지정할 수 있다.
        - `name ?: return "Unknown"` 같이 제어 흐름을 깨는 코드를 넣어줄 수도 있다. 
 
 
 

 
 

 
 
