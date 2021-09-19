___

> ## ✨ Issue
> - enum을 잘 활용한 다른분의 코드를 보고 감명 받아 자세히 찾아봤습니다.
> - enum을 단순히 열겨형 클래스로만 사용하기보다, values()를 호출하여 그 값들에 stream()을 활용한다면 유용하게 쓰일 것 같습니다.
<br/>

# Enum
- 열거형 클래스
- 서로 연관된 상수들의 집합
<br/>

### enum 장점
- 코드가 단순해진다 (가독성)
- 인스턴스 생성과 상속 방지, 상수값의 타입안정성 보장
- enum class를 사용해 새로운 상수들의 타입을 정의함으로 정의한 타입 이외의 타입을 가진 데이터 값을 컴파일시 체크한다.
- 키워드 enum을 사용하기 때문에 구현의 의도가 열거임을 분명하게 알 수 있다.
<br/>

### 기본적인 사용 방식
```java
// enum class -> Gender라는 새로운 상수 type 정의
enum Gender {
    MALE,FEMALE;
}

public class EnumExample {
    // (기존) 상수 정의
    public static final String MALE = "MALE";
    public static final String FEMALE = "FEMALE";

    public static void main (String[] args) {
        // 컴파일시 에러가 나지 않음
        String gender1;
        gender1 = EnumExample.MALE;
        gender1 = EnumExample.FEMALE;
        gender1 = "boy";
        
        // 컴파일 시 의도하지 않는 상수 값 체크 가능
        Gender gender2;
        gender2 = Gender.MALE;
        gender2 = Gender.FEMALE;
        gender2 = "boy"; // error
    }
}
```
<br/>

### enum method
- name()  
  - enum 객체의 문자열 리턴
  ```java
  // 열거 객체의 문자열 return
  Season season = Season.SPRING;
  String name = season.name(); // "SPRING"
  ```
- ordinal()
  - enum 객체가 몇 번째인지 리턴
  ```java
  public enum Season {
      SPRING, // 0
      SUMMER, // 1
      AUTUMN, // 2
      WINTER // 3
  }

  Season season = Season.AUTUMN;
  int ordinal = season.ordinal(); // 2
  ```
- compareTo()
  - 매개값으로 주어진 열거 객체를 기준으로 전/후로 몇 번째 위치하는지 비교
  ```java
  Season season1 = Season.SPRING; // 0
  Season season2 = Season.WINTER; // 3
  int result1 = season1.compareTo(season2); // -3
  int result2 = season2.compareTo(season1); // 3
  ```
- valueOf()
  - 매개값으로 주어지는 문자열과 동일한 문자열을 가지는 enum 객체 리턴  
    외부로부터 문자열을 받아 enum 객체로 변환할 때 유용
  ```java
  Season season = Season.valueOf("SPRING")
  ```
- values()
  - enum 타입의 모든 enum 객체들을 배열로 만들어 리턴
  ```java
  Season[] seasons = Season.values();

  for(Season season : seasons) {
      System.out.println(season);
  }
  ```
<br/>

### enum 상수를 다른 값과 연결
- enum 상수 각각이 enum 객체이므로 생성자를 사용해서 다음과 같이 다른 값을 할당할 수 있다.
```java
public enum Season {
    SPRING("봄"),
    SUMMER("여름"),
    AUTUMN("가을"),
    WINTER("겨울");
    
    final private String name;

    private Season(Stirng name) {
        this.name = name;
    }
    
    public String getName() {
        return name;
    }
}

public class SeasonCheck {
    public static void main(String[] args) {
        for(Season season : Season.values()) {
            System.out.println(season.getName());
            // 봄
            // 여름
            // 가을
            // 겨울
        }
    }
}
```
___