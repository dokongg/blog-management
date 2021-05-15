---
layout:     post
title:      "[Java] Lambda"
date:       2021-05-13
categories: Java
---

1. **Lambda**   
1.1. Lambda란   
1.2. Lambda의 장단점   
1.3. Lambda의 기본식

2. **타겟 타입(Target Type)**

3. **함수형 인터페이스(functional Interface)**   
3.1. 함수형 인터페이스란   
3.2. 내장 함수형 인터페이스

4. **메서드 레퍼런스(Method Reference**   
4.1. 메서드 레퍼런스란   
4.2. 메서드 레퍼런스의 종류

5. **활용예시**

6. **Stream**   
6.1. Stream이란   
6.2. Stream의 특징   
6.3. 병렬처리   

- - -


#### **1. Lambda**
##### **1.1. Lambda란?**
(1) Lambda의 정의
- 메서드를 하나의 식(Expression)으로 표현한 것
- 익명 메서드(익명 함수, Anonymous functions) 생성 문
- 메서드 자체로 혼자 선언하여 쓰일 수 없다.
  + 무조건 Class 구성멤버로서 선언되어야 한다.
- Lambda식을 통해 생성되는 것은 메서드 자체가 아닌 실행문(메서드)를 가진 객체이다.
  + 일반적인 객체가 아닌 인터페이스를 구현한 익명구현객체를 생성한다.


(2) Lambda의 특징
- 익명 함수이므로, 이름을 가질 필요가 없다
- 두 개 이상의 입력이 있는 함수는 최종적으로 1개의 입력만 받는 람다 대수로 단순화 될 수 있다(커링, Curring).

> **익명 함수(Anonymous functions)**
> - 이름이 없는 함수
- 일급객체(First Class citizen)이라는 특징을 지님
  + 일급객체
    + 일반적으로 다른 객체들에 적용 가능한 연산을 모두 지원하는 객체를 말함
    + 함수를 값으로도 사용할 수 있고, 파라미터로 전달 및 변수에 대입하기 등의 연산이 가능

(3) interface와 익명 클래스
```java
interface Calculator {
  int cal(int n);
}
```
- Calculator라는 인터페이스는 cal()이라는 추상메서드를 가지고 있다.
- Calculator를 객체화하는 방법
  + implements한 class를 생성
  + 익명 클래스로 생성

> **인터페이스를 익명 클래스 방식으로 인스턴스화**
- 인터페이스는 인터페이스 자체로 구현체(객체)를 만들 수 없지만, 인터페이스의 추상 메서드를 생성과 동시에 중괄호 {}로 감싼 곳에 `@Override`하여 구현할 수 있다.
- cal() 메서드를 가진 Calculator 인터페이스를 익명클래스 방식으로 인스턴스화

```java
class Driver {
  public static void main(String[] args) {
    int n = 2;

    // 생성과 동시에 구현할 메서드를 @Override함
    Calculator cal = new Calculator() {
      @Override
      int calc(int n) {
        return n + 1;
      }
    }
  }
}
```


##### **1.2. Lambda의 장단점**
(1) Lambda의 장점
- 코드의 간결성
  + 불필요한 반복문을 삭제할 수 있으며, 복잡한 식을 단순하게 표현할 수 있다.
- 지연연산 수행
  + 지연연산을 수행함으로써 불필요한 연산을 최소화할 수 있다.
- 병렬처리 가능
  + 멀티 스레드를 활용하여 병렬처리를 사용할 수 있다.

> **지연연산(지연실행, Lazy Evaluation)**
- 코드를 바로 실행하지 않고 코드 실행이 필요할 때 실행하도록 하는 것
- 불필요한 연산을 피할 수 있다.
  + 별도의 스레드에서 코드를 실행
  + 코드를 여러 번 실행
  + 적절한 시점에 실행
  + 어떤 일(이벤트)이 발생했을 때 실행

(2) Lambda의 단점
- 호출이 까다롭다.
- Lambda의 stream을 사용했을 때, 단순 for문 혹은 while문을 사용하면 성능이 떨어진다.
- 불필요하게 과도하게 사용하면 오히려 가독성이 떨어진다.

##### **1.3. Lambda의 기본식**
`(매개변수) -> { 실행문 }`

- 매개변수로 화살표(->) 함수 몸체로 이용하여 사용할 수 있다.
- 매개변수의 타입을 자동으로 인식하기 때문에 변수 타입을 삭제할 수 있다.
- 매개변수가 하나 일 때 ()를 생략할 수 있다.
- 함수 몸체가 단일 실행문이면 중괄호 {}를 생략할 수 있다.
- 함수 몸체가 return문으로만 구성되어 있는 경우 중괄호 {}를 생략할 수 없다.

```java
class Driver {
  public static void main(String[] args) {
    int n = 2;

    // 1. (매개변수) -> { 구현로직 }
    Calculator cal1 = (int n) -> {
      return n + 1;
    };

    // 2. 매개변수의 타입을 자동으로 인식하기 때문에 변수 타입을 삭제할 수 있다.
    Calculator cal2 = (n) -> {
      return n + 1;
    };

    // 3. 매개변수가 하나 일 때 ()를 생략할 수 있다.
    // (두 개 이상 혹은 없을 때에는 필요)
    Calculator cal3 = n -> {
      return n + 1;
    };

    // 4. 로직이 한 줄 안에 끝나는 경우 {}과 return을 제거할 수 있다.
    Calculator cal4 = n - n + 1;
  }
}
```

- 람다식의 매개변수는 final 키워드가 붙지 않더라도 불변하는 상수로 취급한다.

```java
class Driver {
  public static void main(String[] args) {
    int n = 2;

    Calculator cal = n -> ++n;  // ERROR! 매개변수를 변경시키려 했기 때문
  }
}
```

#### **2. 타겟 타입(Target type)**
- 람다식이 대입되는 인터페이스   
  `인터페이스(타겟 타입) 변수 = 람다식;`
- 익명 구현 객체를 만들 때 사용할 인터페이스
- 람다식에서 생성되는 익명구현객체는 기반이 되는 interface의 타입을 갖는다.
  + 람다식의 타겟 타입은 Calculator이다.

#### **3. 함수형 인터페이스(functional interface)**
##### **3.1. 함수형 인터페이스란**
- 하나의 추상 메서드만 선언된 인터페이스
  + 컴파일러는 람다식을 해석하여 자동으로 익명구현객체로 만든다.   
    이 때, 람다식의 타겟 타입이 될 인터페이스는 컴파일러가 해당 람다식이 타겟 타입의 어떤 메서드를 구현한 것인지 알 수 없기 때문에 2개 이상의 추상 메서드를 가지면 안된다.
- 모든 인터페이스가 람다식의 타겟 타입이 될 수 있는 것은 아니다.
  + 람다식은 하나의 메서드를 정의하기 때문에, 나의 추상 메서드가 선언된 인터페이스만 타겟 타입이 될 수 있다.
- `@FunctionalInterface`
  + 하나의 추상 메서드만을 가지는 지 컴파일러가 체크하도록 한다.
    + 두 개 이상의 추상 메서드가 선언되어 있으면 컴파일 오류 발생

```java
@FunctionalInterface
interface Calculator {
  int calc(int n);
  int sum(int a, int b);  // ERROR!
}
```

##### **3.2. 내장 함수형 인터페이스**
- 함수형 인터페이스는 개발자가 직접 만들어 사용할 수도 있지만, 자바에서 제공하는 표준 인터페이스를 사용해 람다식으로 구현할 수도 있다.
  + 아래 코드의 IntBinaryOperator는 BinaryOperator 종류의 표준 함수형 인터페이스이다.
  + IntBinaryOperator는 두 개의 정수를 매개변수로 받아 정수값을 리턴해주는 추상메서드 applyAsInt()를 갖고 있다.
    + 람다식은 두 개의 정수를 매개변수로 받아 둘 중 큰 값을 반환해준다.
    + 기존의 Math 클래스의 max()를 사용해도 같은 결과를 얻을 수 있다.

```java
IntBinaryOperator op = (int x, int y) -> {
  if ( x > y ) {
    return x;
  }
  return y;
}
```


```java
IntBinaryOperator op1 = (int x, int y) -> {
  return Math.max(x, y);
}
IntBinaryOperator op2 = (x, y) -> Math.max(x, y);
```

(1) 기본 함수형 인터페이스
- 매개변수의 반환값 유무에 따라 존재   

Function과 Predicate의 차이는 반환 값이 boolean이라는 것만 다르고 Function과 동일   
(T: 데이터 타입, R: 리턴 타입)

|함수형 인터페이스|메서드|매개변수|반환값|
|----|----|----|----|
|java.lang.Runnable|void run()|X|X|
|Supplier<T>|T get()|X|O|
|Consumer<T>|void accept(T t)|O|O|
|Function<T, R>|void accept(T t)|O|O|
|Predicate<T>|boolean test(T t)|O|O|

```java
// Supplier
Supplier<String> supplier = () -> "String";
System.out.println(supplier.get()); // String;

// Consumer
Consumer<Integer> consumer = (num) -> System.out.println(num + 1);
consumer.accept(1); // 2

// Function
Function<Integer, Integer> function = (num) -> num + 1;
int result1 = function.apply(1);
System.out.println(result1);  // 2

// Predicate
Predicate<Integer> predicate = (num) -> num > 0;
boolean result2 = predicate.test(1);
System.out.println(result2);  // true
```

(2) 매개변수가 두 개인 함수형 인터페이스

(T/U: 데이터 타입, R: 리턴 타입)   

|함수형 인터페이스|메서드|매개변수|반환값|
|---|---|---|---|
|BiConsumer<T, U>|void accept(T t, U u)|O|O|
|BiFunction<T, U, R>|R apply(T t, U u)|O|O|
|BiPredicate<T, U>|boolean test(T t, U u)|O|O|

```java
// BiConsumer
BiConsumer<Integer, Integer> biConsumer = (num1, num2) -> System.out.println(num1 + num2);
biConsumer.accept(1, 2);  // 3

// BiFunction
BiFunction<Integer, Integer, Integer> biFunction = (num1, num2) -> num1 + num2;
int result1 = biFunction.apply(1, 2);
System.out.println(result1);  // 3

// BiPredicate
BiPredicate<Integer, Integer> biPredicate = (num1, num2) -> num1 > num2;
boolean result2 = biPredicate.test(1, 2);
System.out.println(result2);  // false

```

(3) 매개변수 타입과 반환 타입이 일치하는 함수형 인터페이스

(T/U: 데이터 타입, R: 리턴 타입)

|함수형 인터페이스|메서드|매개변수|반환값|
|---|---|---|---|
|UnaryOperator<T>|T apply(T t)|O|O|
|BinaryOperator<T>|T apply(T t, T t)|O|O|

```java
// UnaryOperator
UnaryOperator<Integer> unaryOperator = (num) -> num;
int result1 = unaryOperator.apply(1);
System.out.println(result1);  // 1;

// BinaryOperator
BinaryOperator<Integer> binaryOperator = (num1, num2) -> num1 + num2;
int result2 = binaryOperator.apply(1, 2);
System.out.println(result2);  // 3
```


#### **4. 메서드 레퍼런스(Method Reference)**
##### **4.1. 메서드 레퍼런스란**
- Lambda의 표현식을 더 간단하게 표현하는 방법
  + 람다 표현식이 단 하나의 메서드를 호출하는 경우, 해당 람다 표현식에서 불필요한 매개변수를 제거하고 사용 할 수 있도록 해준다.
- :: 기호를 사용하여 표현한다.   
  `클래스 이름::메서드 이름`   
  `참조변수 이름::메서드 이름`
- 이미 구현되어 있는 메서드를 참조해 매개변수의 정보와 리턴 타입을 알아내어 불필요한 매개변수를 제거하는 것이 목적이다.

```java
IntBinaryOperator op1 = (x, y) -> Math.max(x, y);
IntBinaryOperator op2 = Math::max;
```

#### **4.2. 메서드 레퍼런스의 종류**
- 정적 메서드, 인스턴스 메서드 참조
- 매개변수의 메서드 참조
- 생성자 참조   

(1) 정적 메서드, 인스턴스 메서드 참조

```java
IntBinaryOperator op = Math::max;
```

- max()가 Math 클래스의 정적 메서드이기 때문에, 정적(static) 메서드 참조방식이 된다.
- 인스턴스 메서드인 경우에는 인스턴스를 생성한 후에 `인스턴스::메서드명`으로 사용할 수 있다.

```java
class MyMath {
  public int myMax(int x, int y) {
    return x > y ? x : y;
  }
}

class Driver {
  public static void main(String[] args) {
    MyMath myMath = new MyMath();
    IntBinaryOperator op = myMath::myMax;
  }
}
```

(2) 매개변수의 메서드 참조   
`매개변수 타입::메서드`
- 매개변수로 받은 인자의 메서드를 참조하는 방식
- ToIntBiFunction은 Function 인터페이스 중 하나로서, 두 개의 매개변수를 받아 람다식의 로직을 사용하여 applyAsInt()를 사용했을 때 int 타입을 반환해주는 함수형 인터페이스이다.

```java
class Driver {
  public static void main(String[] args) {
    Integer x = 3;
    Integer y = 7;

    ToIntBiFunction<Integer, Integer> func1 = (x, y) -> x.compareTo(y);

    ToIntBiFunction<Integer, Integer> func2 = Integer::compareTo;
  }
}
```

(3) 생성자 참조   
`클래스명::new`
- 생성자 또한 일종의 메서드이기 때문에 메서드 레퍼런스로 사용할 수 있다.

```java
class Person {
  private String name;
  private Integer age;

  public Person(String name) {
    this.name = name;
    this.age = 0;
  }

  public Person(String name, Integer age) {
    this.name = name;
    this.age = age;
  }

  @Override
  public String toString() {
    return "[" + this.name + ":" + this.age + "]";
  }
}

class Driver {
  public static void main(String[] args) {
    Function<String, Person> func1 = Person::new;
    // 넘어오는 매개변수의 개수로 어떤 생성자를 호출할 지 찾아줌
    BiFunction<String, Integer, Person> func2 = Person::new;

    func1.apply("Dom");
    func2.apply("Dom, 28");
  }
}
```

#### **5. 활용예시**
- 자바에서 변수의 역할을 할 수 있는 것은 Primitive 타입(int, long, boolean 등)과 Object 타입이다.
  + 람다식을 만들 수 있는 타겟 타입도 변수가 될 수 있으므로, 람다식과 활용하면 메소드도 매개변수처럼 사용할 수 있다.
  + 메소드를 구현한 함수적 인터페이스를 변수로 사용하는 것
- 함수적 인터페이스 `Calculator` 와 정수 n을 매개변수로 받아 calc()의 실행 결과를 출력해주는 printCalc()라는 메소드가 있다.

```java
public static void printCalc(int n, Calculator cal) {
  System.out.println(cal.calc(n));
}

@FunctionalInterface
interface Calculator {
  int calc(int n);
}

class Driver {
  public static void main(String[] args) {
    int n = 2;
    printCalc(n, v -> v + 1); // 3
    printCalc(n, v -> v - 1); // 1
    printCalc(n, v -> v * 2); // 4
  }

  public static void printCalc(int n, Calculator cal) {
    System.out.println(cal.calc(n));
  }
}
```


#### **6. Stream**
##### **6.1. Stream이란**
- 컬렉션(Collection)의 요소를 하나씩 참조하여 람다식으로 처리할 수 있게 해주는 일종의 반복자

> **Java의 Collection과 Iterator**   
>
![Alt Text](https://www.javatpoint.com/images/java-collection-hierarchy.png)
>   
>
- 자바의 `Collection` 인터페이스는 Set, List, Queue 인터페이스 처럼 데이터를 저장하는 자료구조들의 상위에 있는 인터페이스이다.
  + 최상위에는 iterator() 메소드를 갖고 있는 Iterable 인터페이스가 있다.
- Collection을 implements한 모두 자료 구조들은 iterator()로 반복자를 만들어 반복문을 돌릴 수 있다.
  + iterator()는 Iterator<T>를 반환한다.
  + hasNext() 메소드와 next() 메소드를 갖고 있다.
    + hasNext(): 자료 안에 자료가 있는지 없는지 확인
    + next(): 자료구조에 저장되어 있는 자료를 하나씩 리턴

```java
class Driver {
  public static void main(String[] args) {
    List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6);

    Stream<Integer> stream = list.stream();

    stream.forEach((Integer i) -> {
      System.out.println(i);
    });

    list.steam().forEach(System.out::println);
  }

}
```

##### **6.2. Stream의 특징**


```java
class Parser {
  public static List<Integer> strToIntList(List<String> strList) {
    return strList.stream()
            .map(Integer::parseInt)
            .collect(Collectors.toList());
  }
}
class Driver {
  public static void main(String[] args) {
    List<String> strList = Arrays.asList(("1,2,3,4,5,6").split(","));
    List<Integer> intList = Parser.strToIntList(strList);

    int sum = intList.stream().mapToInt(Integer::intValue).sum();
  }
}
```

(1) 람다식을 사용할 수 있다.
- 표준 함수형 인터페이스가 모두 포함된다.

```java
List<String> strList = Arrays.asList(("1,2,3,4,5,6").split(","));

List<Integer> intList1 = Parser.strToIntList(strList);
List<Integer> intList2 = strList.stream().map(Integer::parseInt).collect(Collectors.toList());
```
- 설명
  + Collection은 `.stream()`을 사용하여 `Stream`타입의 객체로 바꿔줄 수 있다.
  + `map()`은 Function<T, R> 함수적 인터페이스(람다식)를 매개변수로 받아 Stream의 데이터를 하나씩 람다식으로 처리하여 다시 Stream 객체에 담는다.
  + `collect()`는 스트림의 데이터를 모아 새로운 객체를 만들어 리턴한다. 상단의 코드에선 `Collectors.toList()`를 사용해서 `List` 객체를 만들어 리턴한다.

- 객체 스트림(Stream<T>) 이 외에도 기본 타입에 특화된 기본 타입 스트림 라이브러리를 갖고 있다.

|리턴타입|메서드(매개변수)|소스|
|---|---|---|
|Stream<T>|java.util.Collection.stream()|컬렉션|
|Stream<br/>IntStream<br/>LongStream<br/>DoubleStream|Arrays.stream(T[]), Stream.of(T[])<br/>Arrays.stream(int[]), IntStream.of(int[])<br/>Arrays.stream(long[]), LongStream.of(long[])<br/>Arrays.stream(double[]), DoubleStream.of(double[])|배열|
|IntStream|IntStream.range(long, long)<br/>LongStream.rangeClosed(int, int)|int 범위|
|LongStream|LongStream.range(long, long)<br/>LongStream.rangeClosed(long, long)|long 범위|
|Stream<Path>|Files.find(Path, int, BiPredicate, FileVisitOption)<br/>Files.list(Path)|디렉토리|
|Stream<String>|Files.lines(Path, Charset)<br/>BufferedReader.lines()|파일|
|DoubleStream<br/>IntStream<br/>LongStream|Random.doubles(...)<br/>Random.ints()<br/>Random.longs()|랜덤 수|


```java
int sum = intList.stream().mapToInt(Integer::intValue).sum();

```
- 설명
   IntStream에 스트림의 모든 값을 더해주는 `sum()`이 있기 떄문에, `intList`를 IntStream으로 바꾸어 사용해야 한다.
   + `IntStream.of()`로 직접 생성과 초기화하여 `IntStream`으로 만드는 방법도 있지만, `mapToInt()`로 기본 스트림을 IntStream으로 바꿔줄 수 있다.


(2) 중간/단말연산을 갖고 있다.
```java
class Driver {
  public static void main(String[] args) {
    List<String> strList = Arrays.asList(("1,2,3,4,5,6").split(","));

    int sum = strList.stream().mapToInt(Integer::parseInt).sum();
  }
}
```

- 중간연산
  + 계속 스트림을 반환하며 연산을 이어서 할 수 있게 한다.
  + `Stream` 객체를 다시 가공해서 `Stream` 객체로 만드는 연산을 한다.
  + 중간 연산자들은 연속해서 사용할 수 있고, 중간 연산 이후에는 다른 스트림이 반환된다.
    + 원본인 `strList`의 `Collection` 값들은 바뀌지 않는다.

|연산|반환값|연산 인수|함수 디스크립터|
|---|---|---|---|
|filter|Stream<T>|Predicator<T>|T -> boolean|
|map|Stream<T>|Function<T, R>|T -> R|
|limit|Stream<T>|||
|sorted|Stream<T>|Comparator<T>|(T, T) -> int|
|distinct|Stream<T>|||

```java
List<String> strList = Arrays.asList("3", "1", "4", "2", "5", "5");

strList.stream()  // 문자열 스트림 생성
  .map(Integer::parseInt) // 문자열 스트림을 정수형 스트림으로 반환 => {3, 1, 4, 2, 5, 5}
  .sorted() // 정렬 => {1, 2, 3, 4, 5, 5}
  .distinct() // 중복제거 => {1, 2, 3, 4, 5}
  .limit(3) // 개수를 3개로 제한 => {1, 2, 3}
  .collect(Collectors.toList()); // List로 변환
```

> **설명**
>
- map
  + 각각의 엘리먼트를 변경하여 새로운 컨텐츠를 생성하는 기능
  + 입력 컬렉션을 출력 컬렉션으로 매핑하거나 변경   
>
>```java
List<String> names = Arrays.asList("A", "B", "C");
>
// java7
for ( string name : names ) {
  System.out.println(name.toUpperCase());
}
// java8
names.stream()
    .map(name -> name.toUpperCase() )
    .forEach(name -> System.out.println(name));
> ```
>
- filter
  + 엘리먼트 선택
  + 컬렉션을 조건에 의해 선택
  + boolean 결과를 리턴하는 람다표현식이 필요하다.
>
> ```java
List<String> names = Arrays.asList("A", "B", "C");
>
// java7
List<String> list = new ArrayList<>():
for ( String name : names ) {
  if ( names.startsWith("S") ) {
    list.add(name);
  }
}
>
// java8
list = names.stream()
            .filter( name -> name.startsWitch("S")
            .collect(Collectors.toList()));
>```
>
- reduce()
  + 엘리먼트를 비교하고 컬렉션에서 하나의 값으로 연산한다.
>
> ```java
List<String> names = Arrays.asList("A", "B", "C");
>
// java7
String str = "";
for ( String name : names ) {
  if ( ("honne".length() < names.length())
      && ( str.length() <= name.length() ) ) {
      str = name;
    }
}
>
//java8
str = names.stream()
            .reduce( "hoone", (name1, name2) -> {
              name1.length() >= nam2.length() ? name1 : name2 );
            }
> ```


> **map과 flatMap**   
- map은 스트림의 스트림을 반환하는 반면, flatMap은 스트림을 반환한다.
  + 스트림의 형태가 배열이거나 또는 입력된 값을 또 다시 스트림의 형태로 반환하고자 할 때는 flatMap이 유용하다.
>
(1) Map
  - 단일 스트림 안의 요소를 원하는 특정 형태로 변환할 수 있다.   
>
(2) FlatMap
  - 스트림의 형태가 배열과 같을 때, 모든 원소를 단일 원소 스트림으로 반환할 수 있다.
  - 여러 개의 스트림을 한 개의 스트림으로 합쳐준다.
  - 복잡한 스트림을 간단한 스트림으로 변경하는 데 사용할 수 있다.
  - `flatMap`의 결과로 단일 원소 스트림을 반환하기 떄문에 `filter` 메서드를 바로 체이닝하여 사용할 수 있다.
  - 초기에 생성된 스트림이 배열인 경우에 매우 유용하다.
>
> ```java
String[][] arrays = new String[][] { {"a1", "a2"}
                                  , {"b1", "b2"}
                                  , {"c1", "c2", "c3"} };
>
Set<String> arraysWithFlatMap
  = Arrays.stream(arrays)
          .flatMap( innerArray -> Arrays.stream(innerArray) )
          .filter( v -> v.length() > 3 )
          .collect(Collectors.toSet());
> ```
>
> (3) Map과 flatMap의 비교 예시(1)
>
> ```java
String[][] arrays = new String[][] { {"a1", "a2"}
                                  , {"b1", "b2"}
                                  , {"c1", "c2", "c3"} };
>
Set<String> arraysWithFlatMap
  = Arrays.stream(arrays)
          .flatMap( innerArray -> Arrays.stream(innerArray) )
          .filter( v -> v.length() > 3 )
          .collect(Collectors.toSet());
>
Set<String> arraysWithMap
  = Arrays.stream(arrays)
          .map( innerArray -> Arrays.stream(innerArray)
                                    .filter( name -> name.length() > 3 )
                                    .collect(Collector.toSet()) )
          .collect(HashSet::new, Set::addAll(), Set::addAll);
> ```
>
(4) Map과 flatMap의 비교 예시(2)
>
>```java
String[][] namesArray = new String[][] {
  {"kim", "taeng"}, {"mad", "play"} };
>
Arrays.stream(namesArray)
      .flatMatp( inner -> Arrays.strem(inner) )
      .filter( name -> name.equals("taeng") )
      .forEach( System.out::println );
>
Arrays.stream(namesArray)
      .map( inner -> Arrays.stream(inner) )
      .forEach( names -> names.filter( name -> name.equals("taeng"))
                              .forEach(System.out::println));
> ```
- `flatMap`은 결과를 가지고 바로 `forEach` 메서드를 체이닝하여 모든 요소를 출력할 수 있다.
- `map`은 단일 요소로 리턴되기 때문에 `map`의 결과를 가지고 `forEach` 메서드로 loop를 진행한 후, 그 내부에서 다시 한 번 `forEach` 메서드를 체이닝하여 사용해야 한다.
>
(5) Map과 flatMap의 비교 예시(3)
>
>```java
String[][] namesArray = new String[][] {
  {"kim", "taeng"}, {"mad", "play"} };
>
Arrays.stream(namesArray)
      .flatMap( inner -> Arrays.stream(inner) )
      .forEach( System.out::println );
>
Arrays.stream(namesArray)
      .map( inner -> Arrays.stream(inner) )
      .forEach( names -> names.forEach( System.out::println ));
> ```

> **`collect`**   
- reduce() 메서드와 동일하게 값을 하나로 모으는 형태
- 여러 편의 메서드를 제공한다.
>
>```java
List<String> names = Arrays.asList("A", "B", "c");
>
// java7
for ( int i = 0; i < names.size() - 1; i ++ ) {
  System.out.println( names.get(i).toUpperCase() + ", " );
}
if ( names.size() > 0 ) {
  System.out.println( names.get(names.size() - 1 ).toUpperCase() );
}
>
// java8
String name = names.stream()
                  .map(String::toUpperCase)
                  .collect(Collectors.joining(", "));
> ```
>
> ```java
> <R> R collect(Supplier<R> supplier
            , BiConsumer<R, ? super T accumulator
            , BiConsumer<R, R> combiner);
>```
>
- `supplier`: 새로운 결과 컨테이너를 생성
- `accumulator`: 결과에 추가 요소를 통합하기 위한 역할
- `combiner`: 계산 결과를 결합하는 역할
>
(2) `Collector`를 직접 정의하는 경우
- `Collector.of` 메서드를 이용
  + `combiner`의 형태는 `BinaryOperator`이다.
>
> ```java
Set<String> arraysWithMap = Arrays.stream(arrays)
          .map( innerArray -> Arrays.stream(innerArray)
                                    .filter( name -> name.length() > 3 )
                                    .collect(Collector.toSet()) )
          .collect( Collector.of(HashSet::new, Set::addAll, (oldValue, newValue) -> oldValue) );
> ```

- 단말연산
  + 스트림을 종료시키고 결과를 반환한다.
  + 스트림을 받아 결과값을 만들고 더 이상 스트림을 반환하지 않는다(스트림을 닫는다).
    + 만약 연산을 계속 이어 하고 싶다면, 스트림을 다시 만들어 작업을 이어가야 한다.

|연산|설명|
|---|---|
|forEach|스트림에 각 요소를 람다를 통해 특정 작업을 실행한다.|
|count|스트림의 요소 개수를 반환한다(long).|
|collect|스트림을 컬렉션 형태로 반환한다.|

```java
Stream.of("3", "1", "4", "2", "5", "5")
      .map(Integer::parseInt)
      .sorted()
      .distinct()
      .limit(3)
      .collect(Collectors.toList())
      .stream()
      .filter( x -> x > 1 )
      .forEach(System.out::println);
```

- 연산의 순서
  + 스트림은 지연된(lazy) 연산을 수행한다.
  + 단말 연산이 없으면 연산을 실행하지 않고, 단말 연산이 수행되기 전에는 중간연산은 실행되지 않는다.
    + 결과가 필요하기 전까지 실행되지 않는다.

```java
Stream.of("3", "1", "4", "2", "5", "5")
      .map( x -> {
        System.out.println("map: " + x);
        return Integer.parseInt(x);
      })
      .filter( x -> {
        System.out.println("filter: " + x);
        return x > 1;
      })
```

(단말 연산이 없기 때문에 아래 코드는 아무 것도 출력되지 않는다.)


```java
Stream.of("3", "1", "4", "2", "5", "5")
      .map( x -> {
        System.out.println("map: " + x);
        return Integer.parseInt(x);
      })
      .filter( x -> {
        System.out.println("filter: " + x);
        return x > 1;
      })
      .forEach( x -> {
        System.out.println("forEach: " + x);
      })
```

(아래와 같은 결과로 출력된다.)   
```js
map : 3   
filter : 3   
forEach : 3   
map : 1   
filter : 1   
map : 4   
filter : 4   
forEach : 4   
map : 2   
filter : 2   
forEach : 2   
map : 5   
filter : 5   
forEach : 5   
map : 5   
filter : 5   
forEach : 5
```

##### **6.3. 병렬처리**
- 스트림은 `parallel()`을 활용해 연산을 병렬로 처리할 수 있다.
- 스트림 안에 데이터가 매우 많은 경우, 병렬 스트림과 연산의 순서를 활용하여 더 빠르게 연산할 수 있다.

- 코드 1
  + 결과값은 각 인자가 순서대로 연산들을 통과한다.
```java
Stream.of("3", "1", "4", "2", "5", "5")
      .map( x -> {
        System.out.println("map: " + x);
        return Integer.parseInt(x);
      })
      .filter( x -> {
        System.out.println("filter: " + x);
        return x > 1;
      })
      .forEach( x -> {
        System.out.println("forEach: " + x);
      })
```
결과
```js
map : 3   
filter : 3   
forEach : 3   
map : 1   
filter : 1   
map : 4   
filter : 4   
forEach : 4   
map : 2   
filter : 2   
forEach : 2   
map : 5   
filter : 5   
forEach : 5   
map : 5   
filter : 5   
forEach : 5
```

- 코드 2
  + `parallel()`로 스트림을 병렬스트림으로 바꾼 뒤 연산을 수행하면, 순서없이 병렬로 처리된다.
```java
Stream.of("3", "1", "4", "2", "5", "5")
      .parallel()
      .map( x -> {
        System.out.println("map: " + x);
        return Integer.parseInt(x);
      })
      .filter( x -> {
        System.out.println("filter: " + x);
        return x > 1;
      })
      .forEach( x -> {
        System.out.println("forEach: " + x);
      })
```
결과
```js
filter : 2
map : 2
forEach : 2
filter : 5
filter : 5
filter : 4
filter : 1
map : 1
filter : 3
map : 3
forEach : 1
map : 4
forEach : 3
forEach : 4
```

출처
- <https://sehun-kim.github.io/sehun/java-lambda-stream/>
- <https://khj93.tistory.com/entry/JAVA-람다식Rambda란-무엇이고-사용법>
- <https://nekisse.tistory.com/15>
- <http://tcpschool.com/java/java_lambda_reference>
- <https://inma.tistory.com/151>
- <https://sehoonoverflow.tistory.com/26>
- <https://madplay.github.io/post/difference-between-map-and-flatmap-methods-in-java>
