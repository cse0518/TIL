___

> ## ✨ Issue
> - enum을 잘 활용한 다른분의 코드를 보고 감명 받아 자세히 찾아봤습니다.
> - enum을 단순히 열겨형 클래스로만 사용하기보다, values()를 호출하여 그 값들에 stream()을 활용한다면 유용하게 쓰일 것 같다.
> - 마지막에 `Supplier`를 활용한 방법으로 `Factory 패턴`을 사용하는 걸 잘 활용해야겠다!!
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
- 두가지 상수 옵션
```java
public enum Season {
    SPRING(1, "봄"),
    SUMMER(2, "여름"),
    AUTUMN(3, "가을"),
    WINTER(4, "겨울");
    
    final private int index;
    final private String name;

    private Season(int index, Stirng name) {
        this.index = index;
        this.name = name;
    }

    public String getIndex() {
        return index;
    }
    
    public String getName() {
        return name;
    }
}

public class SeasonCheck {
    public static void main(String[] args) {
        for(Season season : Season.values()) {
            System.out.println(
                season.getIndex +
                "번 " +
                season.getName() +
                "입니다."
            );
            // 1번 봄입니다.
            // 2번 여름입니다.
            // 3번 가을입니다.
            // 4번 겨울입니다.
        }
    }
}
```
- `Supplier`를 활용
```java
public enum Season {
    SPRING(1, "봄",   () -> "따뜻함"),
    SUMMER(2, "여름", () -> "더움"  ),
    AUTUMN(3, "가을", () -> "시원함"),
    WINTER(4, "겨울", () -> "추움"  );
    
    final private int index;
    final private String name;
    final private Supplier<String> weather;
    
    private Season(int index, Stirng name, Supplier<String> weather) {
        this.index = index;
        this.name = name;
        this.weather = weather;
    }

    public int getIndex() {
        return index;
    }
    
    public String getName() {
        return name;
    }

    public String getWeather() {
      return weather.get();
    }
}

public class SeasonCheck {
    public static void main(String[] args) {
        for(Season season : Season.values()) {
            System.out.println(
                season.getIndex +
                "번 " +
                season.getName() +
                "의 날씨는 : " +
                season.getWeather
            );
            // 1번 봄의 날씨는 : 따뜻함
            // 2번 여름의 날씨는 : 더움
            // 3번 가을의 날씨는 : 시원함
            // 4번 겨울의 날씨는 : 추움
        }
    }
}
```
- `Enum`과 `Supplier`를 활용한 `Factory Pattern`
```java
public enum Season {
    SPRING("봄", Warm::new), // 각각의 날씨 클래스가 있다고 가정
    SUMMER("여름", Hot::new),
    AUTUMN("가을", Cool::new),
    WINTER("겨울", Cold::new);
    
    final private String name;
    final private Supplier<Weather> supplier;
    
    private Season(Stirng name, Supplier<Weather> supplier) {
        this.name = name;
        this.weather = supplier;
    }
    
    public Weather getWeather(String userInput) {
      // userInput을 Season type으로 변환
      Season parsingUserInput = Arrays.stream(Season.values()) // Season Enum 클래스의 값들 중에
                .filter(season -> season.name.equals(userInput)) // user가 입력한 것이 존재하는지 필터링
                .findFirst() // 필터링 된 것은 하나일 것이고 그 Season type을 return
                .orElseThrow(() -> new IllegalArgumentException("존재하지 않는 계절"));

      // userInput에 해당하는 Weather 클래스를 return
      return parsingUserInput.supplier.get();
    }
}

public class SeasonExecute {
    public static void main(String[] args) {
        // input에 대한 Weather 클래스에 구현되어있는 execute 메소드 실행
        season.getWeather(input).execute();
    }
}
```
___