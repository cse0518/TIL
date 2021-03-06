# Repository Layer Unit Test

> ๐ JPA Test๋ง ๋ค๋ฃฌ๋ค.  
> Repository Layer ์ ์ญํ ์ CRUD์ด๋ค.
> 
> < ๊ฒ์ฆํ  ๋ถ๋ถ >
> - CRUD๊ฐ ์ ์ด๋ค์ง๋์ง
> - ์์ธ ์ํฉ
> - ์ฐ๊ด๊ด๊ณ ๋งคํ

<br/>

## @DataJpaTest
- `@DataJpaTest`๋ ์ค์ง JPA ์ปดํฌ๋ํธ๋ง์ ํ์คํธํ๊ธฐ ์ํ ์ด๋ธํ์ด์์ด๋ค.
  - `@SpringBootTest`๋ ๋ชจ๋  config๋ฅผ ์ ๋ถ load ํ๊ธฐ ๋๋ฌธ์ Repository๋ง์ ํ์คํธํ๊ธฐ๋ณด๋ค๋ ํตํฉํ์คํธ์ ์ ํฉํ๋ค.
- full-auto config๋ฅผ ํด์ ํ๊ณ , JPA ๊ด๋ จ config๋ง ์ ์ฉํ๋ค.
- in-memory DB๋ฅผ ํ์ฉํ๋ค.
- `@Transactional`์ด ํฌํจ๋์ด ์๊ณ , ์๋ ๋กค๋ฐฑ๋๋ค.
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
- EntityManager๋ฅผ ์ฌ์ฉํ๋ ค๋ฉด Spring๊ณผ ํตํฉํ์คํธ ํ๊ฒฝ์ด ํ์ํ๋ค.
- `TestEntityManager`๋ EntityManager์ ๊ธฐ๋ณธ์ ์ธ ๋ฉ์๋๋ฅผ ์ ๊ณตํจ๊ณผ ๋์์ ํ์คํธํ๊ธฐ ์ ์ฉํ๋ค. 
  - JPA ํ์คํธ๋ฅผ ํ๊ธฐ ์ํ ๋์
  - persist(), flush(), find() ๋ฑ ์ ๊ณต

<br/>

## Test ๊ฒ์ฆ
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
    @DisplayName("์ ์  ์ด๋ฆ์ผ๋ก ์ ์ ๋ฅผ ์กฐํํ  ์ ์๋ค.")
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
    @DisplayName("์กด์ฌํ์ง ์๋ ์ ์  ์ด๋ฆ์ผ๋ก ์ ์ ๋ฅผ ์กฐํํ  ์ ์๋ค.")
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
