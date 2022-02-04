# Mockito와 BDDMockito
- `Mockito`는 Mock 객체를 편하게 사용하도록 지원해주는 테스트 프레임워크이다.
- Mock 객체의 메소드들은 행위를 지정해주게 되는데, 이때 `Mockito` 또는 `BDDMockito`의 메소드를 사용하게 된다.

<br/>

- 테스트 코드를 작성할때는 관습적으로 given, when, then 구조로 작성하게 되는데, `Mockito`의 메소드를 사용하면 given에 when() 메소드가 나오게 된다. ~뭔가 살짝 불편하고 이상한 형태를 볼 수 있다.~
  ```java
  import org.mockito.junit.jupiter.MockitoExtension;
  import static org.mockito.ArgumentMatchers.anyLong;
  import static org.mockito.Mockito.when;
  import static org.mockito.Mockito.verify;
  import static org.mockito.Mockito.times;
  ...

  @ExtendWith(MockitoExtension.class)
  public class UserServiceTest {
      ...

      @Test
      @DisplayName("ID로 유저를 조회할 수 있다.")
      void findUserByUserName_Success() {
          // Given
          final User mockUser = createMockUser();
          when(userRepository.findById(anyLong())
                  .thenReturn(Optional.of(mockUser));

          // When
          userService.findUser(1L);

          // Then
          verify(userRepository, times(1))
                  .findByUserName("유저 이름");
      }
  }
  ```
  <br/>
- 이를 해결하기 위해 `BDDMockito`가 등장했다.
- `BDDMockito`는 BDD를 사용하여 테스트코드를 작성할 때, 시나리오에 맞게 테스트 코드가 읽힐 수 있도록 도와주는 (이름을 변경한) 프레임워크이다.
  ```java
  import org.mockito.junit.jupiter.MockitoExtension;
  import static org.mockito.ArgumentMatchers.anyLong;
  import static org.mockito.BDDMockito.given;
  import static org.mockito.BDDMockito.then;

  @ExtendWith(MockitoExtension.class)
  public class UserServiceTest {
      ...

      @Test
      @DisplayName("ID로 유저를 조회할 수 있다.")
      void findUserByUserName_Success() {
          // Given
          final User mockUser = createMockUser();
          given(userRepository.findById(anyLong()))
                  .willReturn(Optional.of(mockUser));

          // When
          userService.findUser(1L);

          // Then
          then(userRepository)
                  .should()
                  .findById(anyLong());
      }
  }
  ```

<br/>

## Mockito, BDDMockito 메소드 정리
```java
// Mockito
import static org.mockito.Mockito.when;
import static org.mockito.Mockito.doReturn;
import static org.mockito.Mockito.verify;
import static org.mockito.Mockito.times;

when(인스턴스.메소드(arg))
        .thenReturn(결과값);

doReturn(결과값)
        .when(인스턴스)
        .메소드(arg);

verify(인스턴스, times(횟수))
        .메소드(arg);


// BDDMockito
import static org.mockito.BDDMockito.given;
import static org.mockito.BDDMockito.then;
import static org.mockito.BDDMockito.verify;
import static org.mockito.BDDMockito.times;

given(인스턴스.메소드(arg))
        .willReturn(결과값);

then(인스턴스)
        .should()
        .메소드(arg);

verify(인스턴스, times(횟수))
        .메소드(arg);
```

<br/>

## 결론
두 방법 다 좋은 방법이지만 `BDDMockito`를 사용하는 것을 추천한다. 왜냐하면 `Mockito`의 메소드는 `org.mockito.exceptions.misusing.PotentialStubbingProblem` mismatch 문제가 발생하는 경우가 있다.

`Mockito`는 stub을 strict하게 확인해서 여러개 정의되거나 불필요한 stub이 있으면 test fail로 처리한다. 또한 동작 방식이 두 가지로 나뉘는데 **'given().will()'** 과 **'when().then()'** 은 `Strict Mode`로, **'will().given()'** 과 **'doReturn().when()'** 은 `Lenient Mode`로 동작한다. 이 동작 방식이 mismatch되면 test가 실패하고 아래의 에러 문구가 뜨게 된다.

```java
org.mockito.exceptions.misusing.PotentialStubbingProblem: 
Strict stubbing argument mismatch. Please check.
 
Typically, stubbing argument mismatch indicates user mistake when writing tests.
Mockito fails early so that you can debug potential problem easily.
However, there are legit scenarios when this exception generates false negative signal:
  - stubbing the same method multiple times using 'given().will()' or 'when().then()' API
    Please use 'will().given()' or 'doReturn().when()' API for stubbing.
  - stubbed method is intentionally invoked with different arguments by code under test
    Please use default or 'silent' JUnit Rule (equivalent of Strictness.LENIENT).
For more information see javadoc for PotentialStubbingProblem class.
```

이때 해결하려면 when을 한 번만 호출하던가, 아예 **lenient stubbing**을 사용하면 되지만 느슨한 stubbing을 사용하게 되면 의도치 않은 버그를 찾아내기 어려울 것이다.
```java
@MockitoSettings(strictness = Strictness.LENIENT)
```

`Mockito`는 이렇게 고려해야하는 부분이 있지만, `BDDMockito`의 **'given().willReturn()'** 을 사용하면 문제되지 않는다. BDD도 실천하면서 에러도 안나는 `BDDMockito`를 즐겨쓰도록 하자!

<br/>

## References
- https://velog.io/@lxxjn0/Mockito%EC%99%80-BDDMockito%EB%8A%94-%EB%AD%90%EA%B0%80-%EB%8B%A4%EB%A5%BC%EA%B9%8C
- https://velog.io/@gkdud583/Mockito-org.mockito.exceptions.misusing.PotentialStubbingProblem-Strict-stubbing-argument-mismatch
- https://www.freeism.co.kr/wp/archives/2243#fnref-2243-6
