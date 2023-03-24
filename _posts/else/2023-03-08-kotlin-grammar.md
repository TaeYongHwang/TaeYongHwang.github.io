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

## 리스트
- 기본적인 List의 경우, immutable
- 추가, 삭제 등의 처리가 필요하다면 MutableList를 사용해야 한다.

```kotlin
val listOfVehicleNames: MutableList<String> = mutableListOf()
```

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
 
 
## 프로퍼티
- 코틀린 프로퍼티는 일반 변수를 넘어서, 프로퍼티 값을 읽거나 쓰는 법을 제어할 수 있는 훨씬 더 다양한 기능 제공
- 늦은 초기화
    - `lateinit` 키워드 사용
        - 해당 키워드가 붙은 프로퍼티는 값을 읽으려고 시도할 때 프로그램이 프로퍼티가 초기화됐는지 검사해서, 초기화되지 않은 경우에 `UninitializedPropertyAccessException`을 던진다.
    
    ```kotlin
    class Content2 {
        lateinit var text: String //lateinit은 암시적인 !! 연산자와 비슷한 역할
  
        fun loadFile(file: File) {
            text = file.readText()
        }

        val text2 = File("data.txt").readText() //프로그램이 시작될 때 바로 파일을 읽는다.

        val text3 by lazy { //처음 읽을 때까지 계산을 미뤄둠, lazy 프로퍼티는 thread-safe
          File("data.txt").readText()
        }
    }
    ```
- 접근자
    - get,set 둘 다 field 키워드를 통해 뒷받침하는 필드를 사용하지 않는 경우에만 계산하고, 그렇지 않는 경우는 뒷받침하는 필드가 생성된다.
    
    ```kotlin
    class Person26(val firstName: String, val familyName: String) {
        //fullName, fullName2는 프로퍼티를 읽을 때마다 다시 계산된다. 
        //이는 뒷받침하는 필드(backing field)가 없기 때문이다.
        val fullName: String
          get(): String {
            return "$firstName $familyName"
          }

        val fullName2
          get() = "$firstName $familyName"


        // fullName3에 대해 뒷받침하는 필드가 생기지 않는다.
        var fullName3: String
          get(): String = "$firstName $familyName"
          set(value) {
            val names = value.split(" ")
            firstName = names[0]
            familyName = names[1]
          }

    }


    class Person28(val firstName: String, val familyName: String, age: Int) {
        val age: Int = age
          get(): Int {
            println("Accessing age")
            return field //뒷받침하는 필드에 접근하는 경우에는 field 키워드를 사용할 수 있다.
        }
    }

    class Person31(name: String) {
        var lastChanged: Date? = null
          private set // Person 클래스 밖에서는 변경할 수 없다, 일반적인 접근자면 키워드만으로도 가능
  
        var name: String = name
          set(value) {
            lastChanged = Date()
            field = value
          }	
    }
    ``` 

## 객체
- 싱글톤
    - class 대신 ojbect 키워드 사용

    ```kotlin
    // object 키워드는 클래스를 정의하는 동시에 클래스의 인스턴스를 정의하는 것(타입임과 동시에 값임)
    // thread-safe 하다
    object Application {
        ...
    }
    ```

- 동반 객체 (companion object)
    - 동반 객체가 들어있는 외부 클래스의 이름을 통해 접근할 수 있게 된다.    

    ```kotlin
    class Application3 private constructor(val name: String) {
        companion object Factory { // 이름 생략 가능, compainon object { ... } 형식으로
            fun create(args: Array<String>): Application3? {
              val name = args.firstOrNull() ?: return null
              return Application3(name)
            }
        }
    }

    fun main28(args: Array<String>) {
      //companion을 붙이지 않으면, Application3.Factory.create(args) 로 접근해야 함  
      val app = Application3.create(args) ?: return
      println("Application started: ${app.name}")
    }
    ```

- 객체 식
    - 자바 익명 클래스처럼 사용 가능    

    ```kotlin
    fun main29() {
      fun midPoint(xRange: IntRange, yRange: IntRange) = object {
        val x = (xRange.first + xRange.last)/2
        val y = (yRange.first + yRange.last)/2
      }
  
      val midPoint = midPoint(1..5, 2..6)
  
      println("${midPoint.x}, ${midPoint.y}") // (3, 4)
    }

    ```


# 함수형 프로그래밍

## 고차 함수

``` kotlin
val squares = IntArray(5) { n -> n*n } //0, 1, 4, 9, 16 

// op는 함수를 인자로 받을 수 있다.
fun aggregate(numbers: IntArray, op: (Int, Int) -> Int): Int {
  var result = numbers.firstOrNull()
    ?: throw IllegalArgumentException("Empty array")
    
  for (i in 1..numbers.lastIndex) result = op(result, numbers[i])
  
  return result
}

fun sum(numbers: IntArray) =
  aggregate(numbers, { result, op -> result + op }) //함수를 인자로 넘겨준다.
  
fun max(numbers: IntArray) =
  aggregate(numbers, { result, op -> if (op > result) op else result })

```
 
## 람다

```kotlin 
fun check2(s: String, condition: (Int, Char) -> Boolean): Boolean {
  for (i in s.indices) {
    if (!condition(i, s[i])) return false
  }
  return true
}

fun main11() {
  // 사용하지 않는 람다 파라미터를 '_' 로 지정할 수 있다.  
  println(check2("Hello") { _, c ->c.isLetter() })              // true
  println(check2("Hello") { i, c ->i == 0 || c.isLowerCase() }) // true
}
```

## 익명 함수
- fun 키워드 다음에 바로 파라미터 목록이 온다.

```kotlin
fun sum4(numbers: IntArray) =
  aggregate(numbers, fun(result, op) = result + op)
```
 
## 호출 가능 참조
- callable reference
- `::` 사용
- 다른 패키지에 들어 있는 함수의 callable reference를 만들려면 먼저 함수를 import해야 한다.
    - 함수 이름을 간단한 형태로만 써야 하기 때문에

```kotlin
fun check3(s: String, condition: (Char) -> Boolean): Boolean {
  for (c in s) {
    if (!condition(c)) return false
  }
  
  return true
}

fun isCapitalLetter(c: Char) = c.isUpperCase() && c.isLetter()

fun main13() {
  println(check3("Hello") { c -> isCapitalLetter(c) }) // false
  // 또는 
  println(check3("Hello") { isCapitalLetter(it) }) // false
}

fun main14() {
  // ::isCapitalLetter을 넘겨줄 수 있다.
  println(check("Hello", ::isCapitalLetter)) // false
}
```


```kotlin
class Person(val firstName: String, val familyName: String)

fun main16() {
  val createPerson= ::Person //클래스 생성자에 대한 callable reference
  createPerson("John", "Doe")
}
```

## 인라인 함수
- 함수값, 고차 함수를 사용하면 함수가 객체로 표현되기 때문에 부가 비용이 발생한다. 
    - 함숫값을 사용할 때 발생하는 런타임 비용을 줄일 수 있는 해법
- `inline`이 붙은 코틀린 함수는 가능하면 항상 인라인이 되며, 불가능한 경우 컴파일 오류로 간주된다.
    - `inline`이 붙은 함수의 파라미터로 전달되는 함숫값도 인라인되기 때문에, 인라인하지 않기를 원하는 경우에 `noinline`을 붙일 수 있다.
    - `crossinline`이라는 것을 붙여 람다 안에서 비지역 return을 사용하지 못하게 막을 수도 있다.


```kotlin
inline fun forEach3(a: IntArray, noinline action: ((Int) -> Unit)?) {
  if (action == null) return
  for (n in a) action(n)
}

```    

# 확장
- 코틀린은 마치 멤버인 것처럼 쓸 수 있는 함수나 프로퍼티를 클래스 밖에서 선언할 수 있게 해주는 '확장' 이라는 기능을 제공
- 확장을 사용하면 기존 클래스를 변경하지 않아도 새로운 기능으로 기존 클래스를 확장할 수 있기 때문에, OCP를 지원할 수 있다.

## 확장 함수

```kotlin
// 함수를 정의할 때, 수신 객체의 클래스 이름을 먼저 표시해야 한다.
// 수신 객체가 속한 클래스의 비공개 멤버에는 접근할 수 없다.
fun String.truncate(maxLength: Int): String {
  return if (length <= maxLength) this else substring(0, maxLength)
}

fun main27() {
  println("Hello".truncate(10)) // Hello
  println("Hello".truncate(3))  // Hel
}
```

## 확장 프로퍼티

```kotlin
// IntRange에 대해 leftHalf라는 확장 프로퍼티를 정의
val IntRange.leftHalf: IntRange
  get() = start..(start + endInclusive)/2

fun main29() {
  println((1..3).leftHalf) // 1..2
  println((3..6).leftHalf) // 3..4
}

```

## 동반 확장
- 동반 객체에 대한 확장 함수를 정의할 수도 있다.
    - 동반 객체가 존재하는 경우에만

```kotlin
fun IntRange.Companion.singletonRange(n: Int) = n..n

fun main33() {
  println(IntRange.singletonRange(5))           // 5..5
  println(IntRange.Companion.singletonRange(3)) // 3..3
}
```

## 수신 객체 지정 함수 타입
- functional type with receiver
- 도메인 특화 언어(DSL) 같은 API를 구축할 때 강력한 도구를 제공한다.

```kotlin
// 'Int.'을 추가해서 수신 객체의 타입을 정의함
// 이렇게 정의한 경우, 전달되는 람다는 암시적으로 수신 객체를 갖고, this를 사용해 객체에 접근할 수 있다.
fun aggregate2(numbers: IntArray, op: Int.(Int) -> Int): Int {
  var result = numbers.firstOrNull()
      ?: throw IllegalArgumentException("Empty array")
  
  for (i in 1..numbers.lastIndex) result = result.op(numbers[i])
  
  return result
}

fun sum6(numbers: IntArray) = aggregate2(numbers) { op -> this + op }
```


## 수신 객체가 있는 호출 가능 참조
- 수신 객체 지정 함수 타입을 써서 파라미터를 전달해주는 경우, callable reference를 사용해서 값을 넘겨줄 수 있다.

```kotlin
fun aggregate4(numbers: IntArray, op: Int.(Int) -> Int): Int {
  var result = numbers.firstOrNull()
      ?: throw IllegalArgumentException("Empty array")
      
  for (i in 1..numbers.lastIndex) result = result.op(numbers[i])
  
  return result
}

fun max(a: Int, b: Int) = if (a > b) a else b

fun main36() {
  println(aggregate4(intArrayOf(1, 2, 3, 4), ::max)) // callable reference
}
```

## 영역 함수
- 지역 변수를 명시적으로 선언하지 않고, 식의 값이 들어있는 암시적인 영역을 정의해서 코드를 단순화할 수 있는 경우가 있다.
- run, let, with, apply, also 



# 특별한 클래스

## Enum
- 미리 정의된 상수들로 이루어진 제한된 집합을 표현하는 특별한 클래스
  - kotlin.Enum의 하위 클래스임
- 멤버, 확장 함수, 프로퍼티 붙일 수 있다.

```kotlin
enum class WeekDay {
  MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

fun WeekDay.isWorkDay() =
  this == WeekDay.SATURDAY || this == WeekDay.SUNDAY

fun main1() {
  println(WeekDay.MONDAY.isWorkDay())   // false
  println(WeekDay.SATURDAY.isWorkDay()) // true
}
```

```kotlin
enum class RainbowColor(val isCold: Boolean) { //생성자 추가 가능
  RED(false), ORANGE(false), YELLOW(false),
  GREEN(true), BLUE(true), INDIGO(true), VIOLET(true);
  
  val isWarm get() = !isCold
}
```

## 데이터 클래스
- 데이터를 저장하기 위한 목적으로 주로 쓰이는 클래스를 선언하는 유용한 기능을 제공
- 컴파일러가 동등성을 비교하거나 String 변환 등의 기본 연산에 대한 구현을 자동으로 생성해준다.
  - 정확히는 '주생성자'에 정의된 프로퍼티의 equals(), hashCode(), toString()등을 자동 생성해준다.
- 구조 분해 선언(destructing declaration) 활용 가능
- 코틀린 표준 라이브러리에서 Pair, Triple 데이터 클래스를 제공한다.

```kotlin
// data class로 선언하면 refernce가 아닌 '값'을 비교한다.
// 프로퍼티 값의 비교도 equals() 메서드를 통한다.
data class Person2(val firstName: String,
                   val familyName: String,
                   val age: Int)

fun main11() {
  val person1 = Person2("John", "Doe", 25)
  val person2 = Person2("John", "Doe", 25)
  val person3 = person1

  println(person1 == person2) // true
  println(person1 == person3) // true

  // 구조 분해 선언 가능
  // 주생성자의 각 프로퍼티 위치에 따라 매핑되는 위치가 결정된다. 
  val (firstName, familyName, age) = person1;
}
```

```kotlin
fun main17() {
  // 코틀린 표준 라이브러리에서 Pair, Triple 데이터 클래스를 제공한다.
  val pair = Pair(1, "two")
  
  println(pair.first + 1)    // 2
  println("${pair.second}!") // two!
  
  val triple = Triple("one", 2, false)
  
  println("${triple.first}!") // one!
  println(triple.second - 1)  // 1
  println(!triple.third)      // true
}

main17()
```

## 인라인 클래스(값 클래스)
- 일반적인 원시 타입의 값과 마찬가지로 부가 비용 없이 쓸 수 있기에 **값 클래스**라고 부르기도 한다.
- 인라인 클래스 객체를 사용하는 위치에 인라인 클래스 대신 인라인 클래스에 들어있는 값이 들어간다.

```kotlin
@JvmInline //JVM 백엔드를 사용하는 경우 @JvnInline을 명시적으로 붙여줘야 한다.
// 인라인 클래스의 주생성자에는 불변 프로퍼티를 하나만 선언해야 한다.
value class Dollar(val amount: Int) { 
  fun add(d: Dollar) = Dollar(amount + d.amount)
  val isDebt get() = amount < 0
}

fun main22() {
  println(Dollar(15).add(Dollar(20)).amount) // 35
  println(Dollar(-100).isDebt) // true
}

main22()
```


# 컬렉션
- 컬렉션을 조작하는 모든 연산이 인라인 함수이기 때문에, 함수 호출이나 람다 호출에 따른 부가 비용이 들지 않는다.
- +, - 연산자 지원

```kotlin
val list5 = arrayListOf(1, 2, 3)

// +, - 연산자
list5 += 4
list5 -= 3
list5 += setOf(5, 6)
list5 -= listOf(1, 2)

// 원소 접근
println(listOf(1, 2, 3).first { it > 2 })      // 31
println(listOf(1, 2, 3).lastOrNull { it < 0 }) // null
println(arrayOf("a", "b", "c").elementAtOrElse(1) { "???" }) // b

// 조건 검사
println(listOf(1, 2, 3, 4).all { it < 10 }) // true
println(listOf(1, 2, 3, 4).none { it > 5 }) // true

// 집계
println(listOf(1, 2, 3, 4).count()) // 4
println(listOf(1, 2, 3, 4).count { it < 0 }) // 0
println(arrayOf("1", "2", "3").sumOf { it.toInt() }) // 6
println(listOf(1, 2, 3).joinToString()) // 1, 2, 3
println(intArrayOf(1, 2, 3, 4, 5).reduce { acc, n -> acc * n })  // 120

println(
  intArrayOf(1, 2, 3, 4).fold("") { acc, n -> acc + ('a' + n - 1) }
) // abcd (reduce + 누적값의 초깃값을 정정)

// 걸러내기
// List: [green, blue]
println(setOf("red", "green", "blue", "green").filter { it.length> 3 })

val allStrings = ArrayList<String>()
// green, blue 추가됨
listOf("red", "green", "blue").filterTo(allStrings) { it.length> 3 }

val (evens2, odds2) = listOf(1, 2, 3, 4, 5).partition { it % 2 == 0 }
println(evens2) // [2, 4]
println(odds2) // [1, 3, 5]

// 변환
println(setOf("red", "green", "blue").map { it.length }) // [3, 5, 4] 

println(setOf("abc", "def", "ghi").flatMap { it.asIterable() }) // [a, b, c, d, e, f, g, h, i]

println(
  listOf(listOf(1, 2), setOf(3, 4), listOf(5)).flatten()
) // [1, 2, 3, 4, 5]

println(
  listOf("red", "green", "blue").associateWith { it.length }
) // {red=3, green=5, blue=4}

println(
  listOf("red", "green", "blue").associateBy { it.length }
) // {3=red, 5=green, 4=blue}

// 추출
println(Array(6) { it*it*it }.slice(2..4)) // [8, 27, 64]

println(List(6) { it*it }.take(2))         // [0, 1]

println(List(6) { it*it }.drop(2))         // [4, 9, 16, 25]

println(List(10) { it*it }.chunked(3)) // [[0, 1, 4], [9, 16, 25], [36, 49, 64], [81]]

println(List(6) { it*it }.windowed(3)) // [[0, 1, 4], [1, 4, 9], [4, 9, 16], [9, 16, 25]] 슬라이딩 윈도우

println(list20.zipWithNext()) // [(0, 1), (1, 4), (4, 9), (9, 16), (16, 25)]

// 순서
println(intArrayOf(5, 8, 1, 4, 2).sorted()) // [1, 2, 4, 5, 8]

val array = intArrayOf(4, 0, 8, 9, 2).apply { sort() } // 원본 컬렉션을 변경하는 제자리 정렬(in place)을 수행 

println(listOf(1, 2, 3, 4, 5).shuffled()) //임의의 순서로 재배치

```


## 컬렉션 타입
- 기본적으로 배열, 이터러블(iterable), 시퀀스(sequence), 맵(map)으로 구분

### Iterable
- 원소를 순회할 수 있는 iterator()라는 메서드를 제공
  - 코틀린 for 루프에서는 해당 메서드를 통해 모든 이터러블 객체를 활용할 수 있다.  
- 불변 / 가변(MutableIterable) 컬렉션을 구분한다.   
  - 불변 컬렉션의 경우 covariance하다.
  - e.g. T가 U의 하위 타입인 경우 Iterable\<T>도 Iterable\<U>의 하위 타입이다.

### Sequence

``` kotlin
// to 연산자를 통해 key, value를 쉽게 넣어줄 수 있다.
println(
  mapOf(1 to "One", 2 to "Two").asSequence().iterator().next()
)                                                        

// 무한 시퀀스(단, 값 오버플로가 발생해서 음수와 양수를 왔다갔다 함): 1, 2, 4, 8,...
val powers = generateSequence(1) { it*2 }

// 유한 시퀀스: 10, 8, 6, 4, 2, 0
val evens = generateSequence(10) { if (it >= 2) it - 2 else null }


// SequenceScope가 수신 객체 타임인 확장 람다를 받는 sequence() 함수를 통해 빌더 구현
// 각 부분에 속한 원소에 접근하는 경우에만 yield(), yieldAll() 이 호출된다.
val numbers3 = sequence {
  yield(0)
  yieldAll(listOf(1, 2, 3))
  yieldAll(intArrayOf(4, 5, 6).iterator())
  yieldAll(generateSequence(10) { if (it < 50) it*3 else null })
}
```

- 동일하게 iterator() 메서드 제공
  - 시퀀스는 지연 연산을 가정하기 때문에 iterator() 의 의도가 Iterable과는 다름
- 시퀀스 구현은 내부적이므로 외부에서 직접 사용할 수 없다.
- 자바의 스트림과 비슷하다.
  - asSequence()를 코틀린에서 확장 함수로 제공하며, 이 함수는 자바 스트림을 감사써 코틀린 시퀀스로 사용하게 해준다.

## Comparable
- 클래스가 Comparable을 사용하면 자동으로 \<, > 등의 연산을 쓸 수 있고, 컬렉션의 순서 연산에도 쓰인다.
- compareTo() 구현은 equals() 구현과 의미가 일치해야 한다.

```kotlin
class Person(
  val firstName: String,
  val familyName: String,
  val age: Int
) : Comparable<Person> {
  val fullName get() = "$firstName $familyName"
  override fun compareTo(other: Person) = fullName.compareTo(other.fullName)
}
```

## Comparator

```kotlin
val AGE_COMPARATOR = Comparator<Person>{ p1, p2 ->
  p1.age.compareTo(p2.age)
}

val AGE_COMPARATOR2 = compareBy<Person>{ it.age }
val REVERSE_AGE_COMPARATOR = compareByDescending<Person>{ it.age }
```

## 컬렉션 생성

```kotlin
val list3 = ArrayList<String>()

val set1 = HashSet<Int>()

val map = java.util.TreeMap<Int, String>()

val emptyList = emptyList<String>()

val singletonSet = setOf("abc")

val mutableList = mutableListOf("abc")

val sortedSet = sortedSetOf(8, 5, 7, 1, 4)

val numbers = MutableList(5) { it*2 }
```


# 파일과 I/O 스트림

- 코틀린 표준 라이브러리는 JDK에 이미 있는 I/O 관련 클래스를 더 쉽게 사용할 수 있도록 해주는 확장 함수와 확장 프로퍼티를 제공한다.

```kotlin
import java.io.*

fun main() {
  FileWriter("data.txt").use { it.write("One\nTwo\nThree") }
  
  // One
  FileReader("data.txt").buffered().use { println(it.readLine()) }
  
  // One Two Three
  // use 메서드는 인자로 전달받은 람다를 실행하고 나서 마지막에 자원을 적절히 정리해준 후 람다의 결과를 돌려준다. (Java의 try-with-resources 와 같은 역할)
  FileReader("data.txt").use { println(it.readText().replace('\n', ' ')) }
  
  // [One, Two, Three]
  println(FileReader("data.txt").readLines())
}
```