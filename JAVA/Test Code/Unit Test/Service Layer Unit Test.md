# Service Layer Unit Test

> Service Layer 의 역할은 비즈니스 로직을 수행한다.
> 
> < 검증할 부분 >
> - Repository를 잘 호출하는지
> - 비즈니스 로직이 의도대로 동작되는지

<br/>

## Test Double
- 테스트를 진행하기 어려운 경우, 대신 테스트를 진행할 수 있도록 만들어주는 객체
- DB의 상태에 따라 다른 결과가 발생할 수 있기 때문에 가짜 테스트 더블 객체를 사용한다.
- **Test Double** 종류
  - `Dummy`
    - 객체는 전달되지만 사용되지 않는 객체
    - 인스턴스화 된 객체가 필요하지만 기능은 필요하지 않은 경우 사용
  - `Fake`
    - 복잡한 로직이나 객체 내부 동작을 단순화하여 구현한 객체
  - `Stub`
    - Dummy 객체가 실제로 동작하는 것처럼 보이게 만들어놓은 객체
    - 테스트에서 호출된 요청에 대해 미리 준비해둔 결과 제공
  - `Spy`
    - Stub의 역할을 가지면서 호출된 내용에 대해 약간의 정보를 기록하는 객체
  - `Mock`
    - 호출에 대한 기대를 명세하고 내용에 따라 동작하도록 프로그래밍 된 객체

<br/>

## Mockito
- Service 객체를 생성할 때 의존성이 있는 객체들을 필요로 하게 되는데, 이를 가짜 Mock 객체로 대체해준다.
- 적용 예시
  - UserService 인스턴스를 만들기 위해서는 UserRepository, PostRepository 인스턴스가 필요하다.
  ```java
  // Service 예시
  @Service
  public class UserService {
      private final UserRepository userRepository;
      private final PostRepository postRepository;

      public UserService(UserRepository userRepository, PostRepository postRepository) {
          this.userRepository = userRepository;
          this.postRepository = postRepository;
      }

      ...
  }
  ```
  ```java
  // Service Test 예시
  public class UserServiceTest {

      private UserService userService;

      @BeforeEach
      void setUp() {
          // 인스턴스 생성
          UserRepository userRepository = new UserRepository();
          PostRepository postRepository = new PostRepository();

          // 직접 의존관계 주입을 해줘야하는 불편함
          userService = new UserService(userRepository, postRepository);
      }
  }
  ```
  - Service Layer Test에서는 Repository가 필요하지 않은데, 실제 Repository 인스턴스를 만들어서 주입하는것은 비효율적이다.
  ```java
  import org.mockito.junit.jupiter.MockitoExtension;
  import org.mockito.Mockito;
  ...

  // Mockito 적용
  @ExtendWith(MockitoExtension.class)
  public class UserServiceTest {

      private UserService userService;

      @BeforeEach
      void setUp() {
          // Mockito를 활용하여 가짜 Mock 객체 생성
          UserRepository userRepository = Mockito.mock(UserRepository.class);
          PostRepository postRepository = Mockito.mock(PostRepository.class);

          // 하지만 직접 의존관계 주입을 해줘야하는 불편함
          userService = new UserService(userRepository, postRepository);
      }
  }
  ```
- Mockito annotation 적용
  - `@Mock`
    - 객체 Mocking
  - `@Spy`
    - 일부 메소드는 stub하고, 나머지는 실제 기능을 사용하는 객체 Mocking
  - `@InjectMocks`
    - Mock 객체로 의존관계 주입
    ```java
    import org.mockito.junit.jupiter.MockitoExtension;
    import org.mockito.Mock;
    import org.mockito.Spy;
    import org.mockito.InjectMocks;
    ...

    // Mockito annotation 적용
    @ExtendWith(MockitoExtension.class)
    public class UserServiceTest {

        @Mock
        private UserRepository userRepository;

        @Spy
        private PostRepository postRepository;

        @InjectMocks
        private UserService userService;

    }
    ```

<br/>

## Test 검증
- Repository의 동작 수행은 확인하지 않는다.  
  잘 호출하는지만 확인.
  - Repository 동작은 Repository Test에서 확인
  ```java
  import org.mockito.junit.jupiter.MockitoExtension;
  import org.mockito.Mock;
  import org.mockito.InjectMocks;

  import static org.assertj.core.api.Assertions.assertThatThrownBy;
  import static org.mockito.ArgumentMatchers.anyString;
  import static org.mockito.BDDMockito.given;
  import static org.mockito.Mockito.verify;
  import static org.mockito.Mockito.times;
  ...

  @ExtendWith(MockitoExtension.class)
  public class UserServiceTest {

      @Mock
      private UserRepository userRepository;

      @Mock
      private PostRepository postRepository;

      @InjectMocks
      private UserService userService;

      @Test
      @DisplayName("유저 이름으로 유저를 조회할 수 있다.")
      void findUserByUserName_Success() {
          // Given
          final User mockUser = createMockUser();

          /* userRepository의 findByUserName 메소드 행위를 지정해줌 */
          given(userRepository.findByUserName(anyString()))
                  .willReturn(Optional.of(mockUser));

          // When
          userService.findUserByUserName("유저 이름");

          // Then
          /* 메소드가 1번 실행됐는지 검증 */
          verify(userRepository, times(1))
                  .findByUserName("유저 이름");
      }

      @Test
      @DisplayName("존재하지 않는 유저 이름으로 유저를 조회시 예외가 발생한다.")
      void findUserByUserName_Fail() {
          // given
          given(userRepository.findByUserName(anyString()))
                  .willReturn(Optional.empty());

          // When
          assertThatThrownBy(() -> userService.findUserByUserName("존재하지 않는 유저 이름"))
                  .isInstanceOf(IllegalArgumentException.class)
                  .hasMessage("에러 메세지");

          // Then
          verify(userRepository, times(1))
                  .findByUserName("존재하지 않는 유저 이름");
      }
  }
  ```

<br/>

## References
- https://tecoble.techcourse.co.kr/post/2020-09-19-what-is-test-double/
- https://cobbybb.tistory.com/16
