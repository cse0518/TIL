# Entity Unit Test

> < 검증할 부분 >
> - 원하는대로 Entity가 생성되는지
> - Entity를 생성할 때 예외 상황 검증
> - Entity 내부 메소드 검증
> - 연관관계 매핑이 잘 되었는지

<br/>

## @ParameterizedTest
- `@ParameterizedTest`는 하나의 테스트에 여러 개의 argument를 주입해준다.
- `@Test` 대신에 `@ParameterizedTest`를 명시한다.
- 최소 하나의 source 어노테이션을 붙여주어야 한다.
  - `@ValueSource`
  - `@NullSource`, `@EmptySource`, `@NullAndEmptySource`
  - `@CsvSource`
  - `@EnumSource`
  - `@MethodSource`

<br/>

### @ValueSource
- argument가 하나인 테스트에 사용
- short, byte, int, long ,float, double, char, boolean, String, java.lang.Class 등을 argument로 받을 수 있다.
- 예시
  ```java
  @ParameterizedTest
  @ValueSource(ints = {"1, 2, 3"})
  void valueSourceTest1(int lessThatFour) {
      assertThat(lessThatFour).isLessThan(4);
  }

  @ParameterizedTest
  @ValueSource(strings = {"one, two, three"})
  void valueSourceTest2(String numbers) {
      assertThat(numbers).isNotEmpty();
  }
  ```

### @NullSource, @EmptySource, @NullAndEmptySource
- argument로 null, empty 값 사용
- 예시
  ```java
  @ParameterizedTest
  @NullSource
  void nullSourceTest(String nullPoint) {
      assertThat(nullPoint).isNull();
  }

  @ParameterizedTest
  @EmptySource
  void emptySourceTest(String emptyValue) {
      assertThat(emptyValue).isEmpty();
  }

  @ParameterizedTest
  @NullAndEmptySource // 총 2번 테스트
  void nullAndEmptySourceTest(String nullAndEmptyValue) {
      assertThat(nullAndEmptyValue).doesNotContain("value");
  }
  ```

### CsvSource
- ','로 분리된 문자열을 argument로 전달한다.
- ' '는 empty가 들어오고 아예 비워두면 null이 들어온다.
- 예시
  ```java
  @ParameterizedTest
  @CsvSource({
      "choi, 25, true",
      "kim, 16, false",
      "park, 23, true"
  })
  void csvSourceTest(String name, int age, boolean adult) {
      // argument가 3개씩 3번 들어오게 됨
  }
  ```

### @EnumSource
- argument로 Enum 타입을 하나씩 테스트
- 예시
  ```java
  public enum Number {
      ONE, TWO, THREE;
  }
  ```
  ```java
  @ParameterizedTest
  @EnumSource
  void enumSourceTest1(Number number) {
      // ONE, TWO, THREE가 각각 들어옴
      // @EnumSource(Number.class)도 가능
  }
  
  @ParameterizedTest
  @EnumSource(names = {"ONE", "TWO"})
  void enumSourceTest2(Number number) {
      // ONE, TWO만 들어옴
  }
  
  @ParameterizedTest
  @EnumSource(mode = EXCLUDE, names = {"ONE", "TWO"})
  void enumSourceTest3(Number number) {
      // THREE만 들어옴
  }
  
  @ParameterizedTest
  @EnumSource(mode = MATCH_ALL, names = "regex")
  void enumSourceTest4(Number number) {
      // regex도 사용 가능
  }
  ```

### MethodSource
- 테스트 클래스 내의 메소드 혹은 외부 클래스의 메소드가 반환하는 값을 argument로 넘겨준다.
- 테스트 클래스 내에 있고 `@TestInstance(Lifecycle.PRE_CLASS)`를 붙인 것이 아니라면, 모두 static 메소드여야 한다.
- 예시
  ```java
  @ParameterizedTest
  @MethodSource("parametersProvider")
  void methodSourceTest(String name, int age, boolean adult) {
      // argument가 3개씩 3번 들어오게 됨
  }
  
  static Stream<Arguments> parametersProvider() {
      return Stream.of(
          arguments("choi", 25, true),
          arguments("kim", 16, false),
          arguments("park", 23, true)
      );
  }
  ```

<br/>

## References
- https://www.baeldung.com/parameterized-tests-junit-5
- https://lannstark.tistory.com/52
