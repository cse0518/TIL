# Repository Layer Unit Test

> 📌 JPA Test만 다룬다.  
> Repository Layer 의 역할은 CRUD이다.
> 
> < 검증할 부분 >
> - CRUD가 잘 이뤄지는지
> - 예외 상황
> - 연관관계 매핑

<br/>

## @DataJpaTest
- `@DataJpaTest`는 오직 JPA 컴포넌트만을 테스트하기 위한 어노테이션이다.
  - `@SpringBootTest`는 모든 config를 전부 load 하기 때문에 Repository만을 테스트하기보다는 통합테스트에 적합하다.
- full-auto config를 해제하고, JPA 관련 config만 적용한다.
- in-memory DB를 활용한다.
- `@Transactional`이 포함되어 있고, 자동 롤백된다.
  ```java
  @Target(value=TYPE)
  @Retention(value=RUNTIME)
  @Documented
  @Inherited @BootstrapWith(value=org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTestContextBootstrapper.class)
  @ExtendWith(value=org.springframework.test.context.junit.jupiter.SpringExtension.class)
  @OverrideAutoConfiguration(enabled=false)
  @TypeExcludeFilters(value=DataJpaTypeExcludeFilter.class)
  @Transactional
  @AutoConfigureCache
  @AutoConfigureDataJpa
  @AutoConfigureTestDatabase
  @AutoConfigureTestEntityManager
  @ImportAutoConfiguration
  public @interface DataJpaTest {}
  ```

<br/>

## TestEntityManager
- EntityManager를 사용하려면 Spring과 통합테스트 환경이 필요하다.
- `TestEntityManager`는 EntityManager의 기본적인 메소드를 제공함과 동시에 테스트하기 유용하다. 
  - JPA 테스트를 하기 위한 대안
  - persist(), flush(), find() 등 제공

<br/>

## Test 검증
```java
import org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTest;
import org.springframework.boot.test.autoconfigure.orm.jpa.TestEntityManager;
import static org.assertj.core.api.Assertions.assertThat;
...

@DataJpaTest
public class UserRepositoryTest {

    @Autowired
    private UserRepository userRepository;

    @Autowired
    private TestEntityManager testEntityManager;

    @Test
    @DisplayName("유저 이름으로 유저를 조회할 수 있다.")
    void findUserByUserName_Success() {
        // Given
        final User mockUser = createMockUser();
        testEntityManager.persist(mockUser);

        // When
        final Optional<User> user = userRepository.findByUserName(mockUser.getUserName());

        // Then
        assertThat(user).isSameAs(mockUser);
    }

    @Test
    @DisplayName("존재하지 않는 유저 이름으로 유저를 조회할 수 없다.")
    void findUserByUserName_Fail() {
        // When
        final Optional<User> user = userRepository.findByUserName("invalid name");
        
        // Then
        assertThat(user).isEmpty();
    }
}
```

<br/>

## References
- https://cobbybb.tistory.com/23
