# Controller Layer Unit Test

> Controller Layer 의 역할은 요청을 받고 응답을 주며 그 과정에서 Service의 메소드를 호출한다.
> 
> < 검증할 부분 >
> - 요청을 잘 받았는지
> - Service를 잘 호출하는지
> - 응답을 잘 줬는지

<br/>

## Controller Mock Test
- Controller를 테스트 할 때는 서블릿 컨테이너를 mocking하여 실제 서블릿 컨테이너가 아닌 테스트용 모형 컨테이너를 사용한다.
- 웹상에서 요청과 응답에 대해 테스트할 수 있을 뿐만 아니라 Security, Filter까지 자동으로 테스트하며 수동으로 추가, 삭제까지 가능하다.
- 서블릿 컨테이너를 모킹한 MockMvc 객체 사용
- `@WebMvcTest` 또는 `@AutoConfigureMockMvc`+`@SpringBootTest` 사용

### @WebMvcTest
- `@Controller`, `@RestController`가 설정된 클래스들을 찾아 메모리에 생성
- `@Controller`, `@ControllerAdvice`, `@JsonComponent`와 `Filter`, `WebMvcConfigurer`,  `HandlerMethodArgumentResolver`만 로드
- `@WebMvcTest(특정 Controller.class)`를 지정해주면 특정 Controller만 로드
  ```java
  @WebMvcTest(UserController.class)
  public class ControllerTest {
  
      @Autowired
      private MockMvc mockMvc;
  
  }
  ```

### @AutoConfigureMockMvc
- `@SpringBootTest`와 함께 사용한다.
- `@SpringBootTest`에서 MockMvc를 주입받기 위해 `@AutoConfigureMockMvc` 사용
- 컨트롤러뿐만 아니라 테스트 대상이 아닌 `@Service`나 `@Repository` 등의 모든 Bean을 등록한다.
  - Unit Test 에는 부적합
  ```java
  @AutoConfigureMockMvc
  @SpringBootTest
  public class ControllerTest {
  
      @Autowired
      private MockMvc mockMvc;
  
  }
  ```

<br/>

## MockMvc, ObjectMapper
- `MockMvc`를 통해 Mock Controller에 요청을 보내고 응답을 검증할 수 있다.
  - perform()
    - HTTP 요청을 보낼 수 있다.
  - get(), post(), put(), patch(), delete(), multipart() 등
    - HTTP 메소드 설정
  - header(), contentType(), content() 등
    - request 설정
  - andExpect()
    - 응답 결과 검증
  - status()
    - 응답 상태 코드 검증
  - view()
    - 뷰 검증
  - model()
    - 모델 검증
  - andDo(print())
    - 요청, 응답 전체 메세지 확인
- `ObjectMapper`를 통해 DTO 등의 객체를 String으로 변환할 수 있다.

<br/>

- `Request` 예시
  ```java
  import org.springframework.http.HttpHeaders;
  import org.springframework.http.MediaType;
  import static org.springframework.restdocs.mockmvc.RestDocumentationRequestBuilders.get;
  import static org.springframework.restdocs.mockmvc.RestDocumentationRequestBuilders.post;
  import static org.springframework.restdocs.mockmvc.RestDocumentationRequestBuilders.put;
  import static org.springframework.restdocs.mockmvc.RestDocumentationRequestBuilders.patch;
  import static org.springframework.restdocs.mockmvc.RestDocumentationRequestBuilders.delete;
  import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.multipart;
  
  // GET method, `@QueryParam`
  ResultActions perform = mockMvc.perform(get("/api/v1/...")
                  .header(HttpHeaders.AUTHORIZATION, "token")
                  .queryParam("param", "value"));
  
  // POST method, `@RequestBody`
  ResultActions perform = mockMvc.perform(post("/api/v1/...")
                  .contentType(MediaType.APPLICATION_JSON)
                  .content(objectMapper.writeValueAsString(requestDto)));
  
  // PUT method, `@PathVariable`
  ResultActions perform = mockMvc.perform(put("/api/v1/.../{myId}", myId));
  
  // multipart, `@RequestPart`
  ResultActions perform = mockMvc.perform(multipart("/api/v1/...")
                  .file("images", "testImage".getBytes())
                  // DTO를 `@RequestPart`로 받는 경우
                  .file(new MockMultipartFile(
                          "request",
                          "request.txt",
                          "application/json",
                          objectMapper.writeValueAsBytes(requestDto)))));
  ```
- `Response` 예시
  ```java
  import org.springframework.test.web.servlet.ResultActions; // andExpect(), andDo()
  import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
  import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
  
  perform.andExpect(status().isOk()) // 200
  perform.andExpect(status().isCreated()) // 201
  perform.andExpect(status().isNotFound()) // 404
  perform.andExpect(status().isMethodNotAllowed()) // 405
  perform.andExpect(status().isInternalServerError()) //500
  perform.andExpect(status().is(int status)) // 응답 상태코드 직접 지정  ex) is(200)
  perform.andDo(print()) // 요청, 응답 전체 메세지 출력
  ```

<br/>

## Service 호출 검증
- Controller에서 Service의 메소드를 잘 호출하는지 검증이 필요하다.
- 이떄 Service를 `@MockBean` 또는 `@SpyBean` 해서 사용한다.
  - 스프링 컨텍스트에 등록
  - Mockito로 호출 검증
- `@MockBean` 예시
  ```java
  @WebMvcTest(UserController.class)
  public class ControllerTest {
  
      @MockBean // @SpyBean
      private UserService userService;
  
      @Autowired
      private MockMvc mockMvc;
  
      @Autowired
      private ObjectMapper objectMapper;
  
      ...
  }
  ```

<br/>

## Test 검증
- Service의 비즈니스 로직은 확인하지 않고, Controller에서 잘 호출하는지만 확인.
  ```java
  import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
  import org.springframework.boot.test.mock.mockito.MockBean;
  import org.springframework.test.web.servlet.MockMvc;
  import com.fasterxml.jackson.databind.ObjectMapper;
  import org.springframework.test.web.servlet.ResultActions;
  import org.springframework.http.MediaType;
  import static org.springframework.restdocs.mockmvc.RestDocumentationRequestBuilders.post;
  import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
  import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
  import static org.mockito.ArgumentMatchers.any;
  import static org.assertj.core.api.Assertions.assertThat;
  import static org.mockito.BDDMockito.given;
  import static org.mockito.Mockito.*;
  ...

  @WebMvcTest(UserController.class)
  public class ControllerTest {
  
      @MockBean
      private UserService userService;
  
      @Autowired
      private MockMvc mockMvc;
  
      @Autowired
      private ObjectMapper objectMapper;

      @Test
      @DisplayName("회원가입을 할 수 있다.")
      void joinTest() throws Exception {
          // Given
          final UserJoinResponseDto response = createUserJoinResponseDto(1L);
          given(userService.join(any(UserJoinRequestDto.class)))
                  .willReturn(createUserJoinResponseDto(1L));

          // When
          final ResultActions perform = mockMvc.perform(post("/api/v1/users")
                  .contentType(MediaType.APPLICATION_JSON)
                  .content(objectMapper.writeValueAsString(createUserJoinRequestDto())));

          // Then
          final String responseBody = perform.andExpect(status().isCreated())
                  .andDo(print())
                  .andReturn()
                  .getResponse()
                  .getContentAsString();

          assertThat(responseBody).isEqualTo(objectMapper.writeValueAsString(response));
          verify(userService, times(1))
                  .join(any(UserJoinRequestDto.class));
      }
  }
  ```

<br/>

## References
- https://elevatingcodingclub.tistory.com/61
- https://cobbybb.tistory.com/16
