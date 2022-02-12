# Java 11 (LTS)
- 2018년 9월에 발표된 Java 11 버전

## INDEX
- [**Java 9**](#java-9) 
- [**Java 10**](#java-10)
- [**Java 11**](#java-11)
<br/>

## Java 9

#### Java Platform Module System(Jigsaw) 추가
- 유연한 런타임 이미지를 만들기 위해 Java 플랫폼을 모듈화하여 필요한 모듈만으로 경량화된 이미지를 만들 수 있게 됨
- 기존에는 JRE 일부분만 배포가 불가능했지만 Jigsaw를 통하여 원하는 모듈만 모아 런타임 환경 이미지를 만들 수 있게 됨

#### Process API Updates
- 운영체제의 프로세스 제어 및 관리를 위한 API의 기능 향상

#### JShell 추가
- JavaScript(Node), Python 같은 인터프리터 언어의 shell 환경처럼 바로 코드를 작성하고 결과를 확인할 수 있는 REPL(Read-Eval-Print-Loop) 도구 제공

#### Compact String
- 자바의 기본 char형은 UTF-16 기반의 2Byte를 차지하는데 개선된 String은 문자열에 따라 Latin-1(1Byte)와 UTF-16(2Byte)으로 나누어졌다. 영어만 있는 문자열의 경우 1Byte의 영역을 차지해 기존보다 메모리 공간을 절약할 수 있게 되었다.

#### HTML5 Javadoc / Javadoc Search
- 이전 버전까진 HTML 4.01 형식으로 JavaDoc을 생성하였으나 이제 HTML5 방식의 마크업으로 Javadoc을 생성할 수 있다.

#### UTF-8 Property Files
- properties file에 대한 기본적인 파일 인코딩 기존 ISO-8859-1 에서 UTF-8 로 변경

#### Multi-Release JAR Files
- JAR 파일 형식을 확장하여 여러 버전의 클래스 파일을 하나의 JAR 안에 공존시킬 수 있다. 
- 동일한 클래스를 Java 버전 별로 동작할 수 있게 구성하여 jar 안에 넣을 수 있다.

#### More Concurrency Updates (Reactive Stream)
- 프로그램의 동시성 및 병렬 처리 지원을 위한 라이브러리의 지원이 늘어남
- 개선된 Completable Future 와 Reactive Stream 도입

#### Unicode 8.0
- Unicode 6.1을 지원했던 java 8 에 비해 10,555자, 29개의 스크립트 및 42개 블록의 유니코드 8.0을 지원

#### Convenience Factory Methods for Collections
- 불변 Collection을 만들 수 있는 메서드 추가

#### SHA-3 Hash Algorithms
- SHA-1, SHA-2 표준 해시 함수를 대체하는 진보된 암호화 해시 함수를 구현하여 제공

#### Deprecate the Applet API
- 더 이상 브라우저에서 사용하지 않는 Applet API에 대한 지원 종료

#### Implement Selected ECMAScript 6 Features in Nashorn
- JVM의 공식 JavaScript 엔진인 Nashorn이 ECMAScript 6 버전의 많은 새로운 기능 중 일부를 구현하여 제공

#### Private Interface Method
- Interface에 private method, private static method 제공

#### try-with-resource 향상
- Java 7 에서 등장했던 try-with-resources의 불편사항 개선

#### Improve Diamond Operator
- 익명 클래스에 Diamond Operator 사용 가능
```java
MyHandler<Integer> intHandler = new MyHandler<>(10) {
    @Override
    public void handle() {
        // ...
    }
};
```

#### Stream Improvements
- Java 9의 Stream은 `iterate()`, `takeWhile()`/`dropWhile()`, `ofNullable()` 과 같은 새로운 추가 메서드를 제공하여 비동기 프로그래밍에 대한 몇 가지 유용한 개선 사항을 제공한다.

#### Optional class Stream
- Optional class에 `or()`, `ifPresentOrElse()`, `stream()` 메서드가 추가되어 활용도가 높아졌다.
<br/>

## Java 10

#### Local-variable type inference
- var를 사용하여 생성할 변수의 타입을 추론할 수 있는 기능 추가
- var를 통해 지역변수에 대해서는 type을 명시해주지 않아도 됨

#### Application Class-Data Sharing
- Class-Data Sharing(CDS)는 JVM 기동시의 성능을 향상하거나, 여러 대의 JVM이 하나의 물리 장비 또는 가상 장비에서 돌아가는 경우, 자원에 미치는 영향을 줄이기 위해 개발된 기능이다.
- CDS는 JVM에서 공통으로 사용하는 클래스들을 공유하는 저장소에 위치시키고 이를 공유해서 사용하는 방식으로 제공된다

#### Additional Unicode Language-Tag Extensions
- java.util.Locale 클래스의 향상된 버전으로 Java 9에서 제공되는 BCP 47 Language tag 에 몇 가지를 더 추가함.
  - cu (currency type)
  - fw (first day of week)
  - rg (region override)
  - tz (time zone)

#### Time-Based Release versioning
- Java SE Platform과 JDK의 버전 명시에 대한 기준이 생겼다. 새로운 버전 모델인 "Six-month release model"을 적용하기 위한 버전 표기법으로 등장하였다.
- `$FEATURE`, `$INTERIM`, `$UPDATE`, `$PATCH`

#### Parallel Full GC for G1
- Full GC를 병렬로 만들어, G1의 worst-case latency를 개선
- 이전까지는 단일 스레드로 GC를 수행하였다면, jdk 10부터는 병렬로 Mark-Sweep-Compact를 수행

#### New APIs
- 73개의 표준 클래스 라이브러리 API 추가
  - java.io.Reader  
    long transferTo(Writer);  
    모든 문자를 읽고 주어진 순서대로 지정된 writer에 문자를 쓴다.
  - Optional.orElseThrow()  
    Optional이 값을 보유하면 반환, 그렇지 않으면 NoSuchElementException 발생

#### APIs for Creating Unmodifiable Collections
- Unmodifiable Collections을 쉽게 만들 수 있는 몇가지 새로운 API 추가
  - `List.copyOf`, `Set.copyOf`, `Map.copyOf`
- Stream의 요소를 Unmodifiable Collections에 수집 가능
  - Collector toUnmodifiableList();
  - Collector toUnmodifiableSet();
  - Collector toUnmodifiableMap(Function, Function);
  - Collector toUnmodifiableMap(Function, Function, BinaryOperator);
<br/>

## Java 11

#### Local-Variable Syntax for Lambda Parameters
- jdk 10 버전에서 지역변수로써 사용되어 타입을 추론할 수 있는 var가 새로운 feature로 추가되었었는데, jdk 11 버전에서는 var를 람다 표현식에 쓰는 경우 전달되는 parameter들의 타입을 추론할 수 있는 feature가 추가되었다.

#### HTTP Client (Standard)
- jdk 9 버전에서 추가되고 jdk 10 버전에서 업데이트된 java.incubator.http 패키지가 java.net.http 패키지로 표준화되었다.
  - Non-Blocking Request and Response 지원
  - BackPressure 지원
  - HTTP/2 지원
  - Factory Method 형태로 지원
  - WebSocket 지원

#### ZGC: A Scalable Low-Latency Garbage Collector (Experimental)
- jdk 11에서 새로 등장한 가비지 컬렉터
- 특징
  - GC 일시 중지 시간은 10ms를 초과하지 않는다.
  - 작은 크기(수백 MB) ~ 매우 큰 크기(수 TB) 범위의 힙을 처리한다.
  - G1에 비해 애플리케이션 처리량이 15% 이상 감소하지 않는다.
  - 향후 GC 최적화를 위한 기반 마련
  - 처음에는 Linux / x64을 지원 (향후 추가 플랫폼 지원 가능)

#### Flight Recorder
- Oracle Java의 상용 addon이었던 JFR(Java Flight Recorder)를 오픈소스로 개발하였다. JFR은 실행 중인 Java 애플리케이션의 진단 및 프로파일링 데이터를 수집하는 도구다. 주로 실행 중인 Thread, GC Cycles, Locks, Sockets, 메모리 사용량 등에 대한 데이터를 수집한다.

#### Launch Single-File Source-Code Programs
- Single file 프로그램의 Main 메서드 실행 시 javac를 하지 않고도 바로 실행할 수 있도록 편의성 향상
  ```
  // 기존
  $ javac TestClass.java
  $ java TestClass
  - 실행 -
  
  // Java 11
  $ java TestClass.java
  - 실행 -
  ```

#### Deprecate the Nashorn javascript Engine
- 빠른 변화에 맞추어 대응하기가 어렵다고 판단되어 Javascript 엔진은 Deprecate 되었다.

#### Collection 인터페이스에 새로운 메소드 추가
- Collection의 `toArray()` 메소드를 오버 로딩하는 메소드가 추가되었고, 원하는 타입의 배열을 선택하여 반환할 수 있게 됨.
  ```java
  List list = Arrays.asList("test1", "test2", "test3"); 
  String[] stringArray = list.toArray(String[]::new); 
  ```

#### Predicate 인터페이스에 새로운 메소드 추가
- Predicate 인터페이스에 부정을 나타내는 `not()` 메소드 추가
  ```java
  List<String> list = Arrays.asList("test1", "test2", " ", "test3"); 
  List notBlank = list.stream() 
      .filter(Predicate.not(String::isBlank))
      .collect(Collectors.toList());
  assertThat(notBlank).containsExactly("test1", "test2", "test3");
  ```
<br/>

## References
- https://blogs.oracle.com/javakr/post/java8-16
- https://daddyprogrammer.org/post/10411/jdk-roadmap-change-jdk9-11/
- https://steady-coding.tistory.com/598
