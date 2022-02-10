# Java 8
- 2014년에 발표된 Java SE 8 버전에서는 많은 사항이 변경되고, 새롭게 추가되었다.

## INDEX
- [람다 표현식(Lambda Expression)](#람다-표현식lambda-expression) : 함수형 프로그래밍
- [스트림 API(Stream API)](#스트림-apistream-api) : 데이터의 추상화
- [java.time 패키지](#javatime-패키지) : Joda-Time을 이용한 새로운 날짜와 시간 API
- [나즈혼(Nashorn)](#나즈혼nashorn) : 자바스크립트의 새로운 엔진
- [Optional](#optional)
<br/>

### 람다 표현식(Lambda Expression)
- 메소드를 하나의 식으로 표현한 것
- 식별자 없이 실행할 수 있는 함수 표현식 -> `익명 함수`(Anonymous Function)
- 클래스를 만들고 객체를 생성하지 않아도 메소드를 사용할 수 있다.  
또한, 메소드의 매개변수로 전달될 수 있고, 메소드의 결과값으로 반환될 수 있다.
- 불필요한 코드를 줄여주고, 코드의 가독성을 높이는 목적
<br/>

### 스트림 API(Stream API)
- 많은 양의 데이터를 저장하기 위해 배열이나 컬렉션을 사용해왔고, 저장된 데이터에 접근하기 위해서 반복문이나 반복자(iterator)를 사용하여 매번 코드를 작성해야 했다.  
-> 길이가 너무 길고 가독성도 떨어지며, 코드의 재사용이 거의 불가능하다.
- 이러한 문제점을 극복하기 위해 도입된 방법이 `스트림(Stream) API`이다.
- 스트림 API는 데이터를 추상화하여 다루므로, 다양한 방식으로 저장된 데이터를 읽고 쓰기 위한 공통된 방법을 제공한다.
- 주요 기능 : `Filtering`, `Mapping`, `Sorting`, `Iterating`
<br/>

### java.time 패키지
- JDK 1.0 에서는 Date 클래스를 사용하여 날짜에 관한 처리를 수행했다.  
하지만 Date 클래스는 현재 대부분의 메소드가 사용을 권장하지 않고 있다.
- JDK 1.1 부터 새롭게 제공된 Calendar 클래스는 날짜와 시간에 대한 정보를 얻을 수는 있지만, 다음과 같은 문제점을 가지고 있다.
1. Calendar 인스턴스는 불변 객체가 아니라서 값이 수정될 수 있다.
2. 윤초(leap second)와 같은 특별한 상황을 고려하지 않는다.
3. Calendar 클래스에서는 month를 나타낼 때 1월부터 12월을 0부터 11까지로 표현해야 하는 불편함이 있다.
- 따라서 많은 Java 개발자들은 Calendar 클래스뿐만 아니라 더 나은 성능의 Joda-Time 이라는 라이브러리를 함께 사용해왔다.
- Java SE 8 버전에서는 이러한 Joda-Time 라이브러리를 발전시킨 새로운 날짜와 시간 API인 java.time 패키지를 제공한다.
- 주요 클래스 : `LocalDate`, `LocalTime`, `LocalDateTime`
<br/>

### 나즈혼(Nashorn)
- 지금까지 자바스크립트의 기본 엔진으로는 Mozzila의 리노(Rhino)가 사용되어 왔다. 하지만 시간이 지나며 Java의 최신 개선사항 등을 제대로 활용하지 못하는 등 노후화된 모습을 보여준다.
- Java SE 8 버전부터는 자바스크립트의 새로운 엔진으로 오라클의 나즈혼(Nashorn)을 도입했다. 나즈혼(Nashorn)은 기존에 사용되어 온 리노에 비해 성능과 메모리 관리 면에서 크게 개선된 스크립트 엔진이다.
<br/>

### Optional
- 가장 쉽게 직면할 수 있는 예외 중 하나가 바로 NPE(Null Pointer Exception)다. NPE를 피하기 위해서는 null 체크 로직을 추가해야 하는데, 코드가 복잡해지고 로직이 상당히 번거로워 질 수 있다.
- Java SE 8 버전부터는 `Optional<T>` 클래스를 사용해 NPE를 방지할 수 있도록 도와준다. `Optional` 은 null이 올 수 있는 값을 감싸는 Wrapper 클래스로, 참조하더라도 NPE가 발생하지 않는다.
<br/>

## References
- https://blogs.oracle.com/javakr/post/java8-16
- https://livenow14.tistory.com/81
- [java.time 패키지 상세내용 참고](http://blog.eomdev.com/java/2016/04/01/%EC%9E%90%EB%B0%948%EC%9D%98-java.time-%ED%8C%A8%ED%82%A4%EC%A7%80.html)
- [Optional 상세내용 참고](https://mangkyu.tistory.com/70)
