# Java 17 (LTS)
- 2021년 9월에 발표된 Java 17 버전

## INDEX
- [**Java 12**](#java-12) 
- [**Java 13**](#java-13)
- [**Java 14**](#java-14)
- [**Java 15**](#java-15)
- [**Java 16**](#java-16)
- [**Java 17**](#java-17)
<br/>

## Java 12

#### Switch Expressions (Java 12 ~ 14)
- 이전 switch 구문의 경우 불필요하게 장황하고 누락된 break 문으로 우발적인 오류가 발생하기 쉬웠다.
  ```java
  switch (day) {
      case MONDAY:
      case TUESDAY:
      case WEDNESDAY:
      case THURSDAY:
      case FRIDAY:
          System.out.println("weekday");
          break;
      case SATURDAY:
      case SUNDAY:
          System.out.println("weekend");
          break;
  }
  ```
- Java 12 부터는 람다 표현식을 사용할 수 있다.
  ```java
  switch (day) {
      case MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY -> System.out.println("weekday");
      case SATURDAY, SUNDAY -> System.out.println("weekend");
  }
  ```
- 변수에 할당할 수도 있다.
  ```java
  String dayType = switch (day) {
      case MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY -> "weekday";
      case SATURDAY, SUNDAY -> "weekend";
      default "weekday";
  }
  ```

#### Default CDS 저장 장소
- 어플리케이션 간에 공유되는 CDS (Class Data Sharing)에 대한 파일이 classes.jsa로 기본 저장

#### Shenandoah (Java 12 ~ 15)
- 멈춤 시간이 적은 가비지 컬렉터 도입 (최대 10ms)

#### Unicode 11 지원
<br/>

## Java 13

#### 	Text Block 기능 도입
- String에서 여러 줄의 문자열을 한번에 입력할 수 있다.
  ```java
  // 기존
  String html = "<html>\n" +
                "    <body>\n" +
                "        <p>Hello, world</p>\n" +
                "    </body>\n" +
                "</html>\n";

  // Text Block 사용
  String html = """
                <html>
                    <body>
                        <p>Hello, world</p>
                    </body>
                </html>
                """;
  ```

#### yield 예약어 사용
- switch문에서 yield (switch문의 return) 사용
  ```java
  String dayType = switch (day) {
      case MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY -> "weekday";
      case SATURDAY, SUNDAY -> {
          System.out.println("WeekEnd!!");
          yield "weekend";
      }
      default "weekday";
  }
  ```

#### Unicode 12.1 지원
<br/>

## Java 14

#### instanceof 패턴 매칭
- instanceof 객체에 대한 캐스팅이 필요 없어졌다.
  ```java
  // 기존
  if (obj instanceof String) {
      String s = (String) obj;
      // ...
  }

  // Java 14
  if (obj instanceof String s) {
      // can use s
  } else {
      // can't use s
  }
  ```

#### NullPointerException 기능 추가
- NPE 발생 시, Exception 메세지에 어떤 변수가 null 인지 알려준다.

#### 패키징 툴인 jpackage 도구 제공
- Java 응용 프로그램을 해당 OS(Windows, Linux, Mac)에 맞게, 실행에 필요한 모든 라이브러리와 함께 패키징하는 jpackage 도구 제공

#### Record Class 도입
- 단순한 불변 데이터를 선언하기 위한 간단한 문법 제공
- Language Data를 다루는 record 클래스를 이용해서 간단하게 새로운 데이터 클래스 생성 가능
- 주요 기능
  - 해당 record 클래스는 final 클래스라서 상속할 수 없다.
  - 각 필드는 private final 필드로 정의된다.
  - 선언된 모든 다른 필드는 static 하다.
  - 모든 필드를 초기화하는 RequiredAllArgument 생성자가 생성된다.
  - 각 필드의 getter는 getXXX()가 아닌, 필드명을 딴 getter가 생성된다.  
    (ex. name(), age())
  ```java
  // record 클래스 생성
  record MyClass(int x, int y) { }
  ```
<br/>

## Java 15

#### Sealed Class 도입
- sealed 키워드를 이용하여 클래스나 인터페이스의 상속을 제한할 수 있다.
- sealed class의 목적은 모든 subclass에 대해 명확하게 알 수 있도록 하는 것
  ```java
  // 두 종류의 subclass만을 허용
  public abstract sealed class Human
        permits Man, Woman {...}
  
  public class Man extends Human {...}
  public class Woman extends Human {...}
  ```
<br/>

## Java 16

#### 	Unix 도메인 소켓 채널
- 소켓 채널 API에 유닉스 플랫폼과 Windows에서 흔히 볼 수 있는 유닉스 도메인 소켓의 모든 기능에 대한 지원을 추가

#### Windows/AArch64 지원
- Windows ARM64 지원

#### Alpine Linux 지원
- 작은 용량의 Alpine Linux 지원

#### 외부 링커 API
- JNI(Java Native Interface)의 교체로, 네이티브 코드에 접근할 수 있는 API 제공
- Java application이 Java heap 외부의 외부 메모리에 안전하고 효율적으로 액세스 할 수 있도록하는 API 제공
<br/>

## Java 17

#### Pattern Matching for switch (Preview)
- switch문 pattern matching
  ```java
  // 기존
  static String formatter(Object o) {
      String formatted = "";
      if (o instanceof Integer i) {
          formatted = String.format("int %d", i);
      } else if (o instanceof Long l) {
          formatted = String.format("long %d", l);
      } else if (o instanceof Double d) {
          formatted = String.format("double %f", d);
      } else if (o instanceof String s) {
          formatted = String.format("String %s", s);
      }

      return formatted;
  }

  // switch문 pattern matching
  static String formatter(Object o) {
      return switch (o) {
          case Integer i -> String.format("int %d", i);
          case Long l -> String.format("long %d", l);
          case Double d -> String.format("double %f", d);
          case String s -> String.format("String %s", s);
          default -> o.toString();
      };
  }
  ```
- switch문 null checking
  ```java
  // 기존
  static void test(String s) {
      // null 체크를 하지 않으면 switch문에서 NPE 발생 가능
      if (s == null) {
          System.out.println("null!");
          return;
      }
      
      switch (s) {
          case "integer", "long" -> System.out.println("Number");
          default -> System.out.println("Not Number");
      }
  }

  // switch문 null checking
  static void test(String s) {
      switch (s) {
          // null check 가능
          case null -> System.out.println("null!");
          case "integer", "long" -> System.out.println("Number");
          default -> System.out.println("Not Number");
      }
  }
  ```
<br/>

## References
- https://blogs.oracle.com/javakr/post/java8-16
- https://luvstudy.tistory.com/152
