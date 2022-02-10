# RestDocs
- RestDocs는 RESTful 서비스를 문서화하는데 사용된다.
- 주로 자바 문서화에는 `Swagger` 또는 `RestDocs`가 사용된다.
  - `Swagger`
    - API를 테스트 해볼수 있는 화면 제공
    - 적용하기 쉬움
    - 하지만 서비스되는 코드에 어노테이션을 추가해야함
    - 실제 서비스 코드와 동기화가 안될수 있음
  - `RestDocs`
    - 실제 서비스되는 코드에 영향을 끼치지 않음
    - 테스트를 성공해야 문서화 작업이 이뤄짐
    - 하지만 Swagger보다 어렵다.

**Controller 계층**에서의 테스트 코드와 함께 RestDocs 문서화 작업을 하게된다.  
`@SpringBootTest`를 활용한 통합 테스트는 전체 컨텍스트를 로드하여 시간이 오래걸리기 때문에, 주로 `@WebMvcTest`를 활용한 단위 테스트와 함께 RestDocs를 사용한다.

<br/>

## 환경 설정
- build.gradle
  ```java
  // plugin 추가 - adoc 파일 변환, build 디렉토리에 복사
  // gradle 7 이전 버전은 org.asciidoctor.convert plugin 추가
  plugins {
      id "org.asciidoctor.jvm.convert" version "3.3.2"
  }

  // 전역변수 설정
  ext {
      snippetsDir = file('build/generated-snippets')
  }

  // asciidoctor 추가
  asciidoctor {
      dependsOn test
      inputs.dir snippetsDir
  }

  // 기존에 존재하는 docs 삭제
  asciidoctor.doFirst {
      delete file('src/main/resources/static/docs')
  }

  // bootJar 추가 - 문서 작성후 html 파일을 복사
  bootJar {
      dependsOn asciidoctor
      copy {
          from "${asciidoctor.outputDir}"
          into 'BOOT-INF/classes/static/docs'
      }
  }

  // src/main/resources/static/docs로 복사
  task copyDocument(type: Copy) {
      dependsOn asciidoctor
      from file("build/docs/asciidoc")
      into file("src/main/resources/static/docs")
  }

  // 의존성 추가
  dependencies {
      testImplementation 'org.springframework.restdocs:spring-restdocs-mockmvc'
  }
  ```

<br/>

## RestDocs 문서화
- HTTP 요청과 응답에 대한 상세 내용을 문서화한다.
- `@AutoConfigureRestDocs` 어노테이션을 붙여주고 사용한다.
- 테스트 코드 예시
  ```java
  import org.springframework.boot.test.autoconfigure.restdocs.AutoConfigureRestDocs;
  import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
  import org.springframework.test.web.servlet.MockMvc;
  import static org.springframework.restdocs.headers.HeaderDocumentation.*;
  import static org.springframework.restdocs.mockmvc.MockMvcRestDocumentation.document;
  import static org.springframework.restdocs.mockmvc.RestDocumentationRequestBuilders.*;
  import static org.springframework.restdocs.operation.preprocess.Preprocessors.*;
  import static org.springframework.restdocs.payload.PayloadDocumentation.*;
  import static org.springframework.restdocs.request.RequestDocumentation.parameterWithName;
  import static org.springframework.restdocs.request.RequestDocumentation.pathParameters;

  @AutoConfigureRestDocs
  @WebMvcTest(UserController.class)
  public abstract class ControllerTest {

      @Autowired
      protected MockMvc mockMvc;

      ...

      @Test
      void test() {
          mockMvc.perform(get("/api/v1/...")
                /* 생략 */)
                .andExpect(status().isOk())
                .andDo(print())

                /* RestDocs 문서화 시작 */
                .andDo(document("{identifier}",
                      preprocessRequest(prettyPrint()),
                      preprocessResponse(prettyPrint()),
                      
                      /* request 예시 */
                      requestHeaders(
                            headerWithName(HttpHeaders.AUTHORIZATION).description("토큰")
                      ),
                      pathParameters(
                            parameterWithName("id").description("아이디")
                      ),
                      requestFields( // requestBody
                            fieldWithPath("field1").type(JsonFieldType.STRING).description("필드 설명1"),
                            fieldWithPath("field2").type(JsonFieldType.STRING).description("필드 설명2")
                                    .optional() // optional 설정 가능
                      ),
                      requestPartBody("file"), // key 값

                      /* response 예시 */
                      responseHeaders(
                            headerWithName(HttpHeaders.LOCATION).description("location 정보")
                      ),
                      responseFields(
                            fieldWithPath("field").type(JsonFieldType.NUMBER).description("필드 설명")
                      )));
      }
  }
  ```
- 주의할 점
  - ResponseBody로 필드 하나만 응답으로 보낸다면 parsing 오류가 날 수 있다.  
    따라서 객체(Dto)로 담아서 응답을 보내줘야한다.
  <br/>
- 테스트 작성후, 테스트가 성공하면 `build/generated-snippets` 하위에 스니펫이 생성된다.
- `API 문서 만들기`
  - `src/docs/asciidoc` 위치에 adoc 파일을 만들어줘야 한다.
  - adoc 파일에 스니펫들을 첨부해서 문서로 만든다.
  - 예시
    ```adoc
    ifndef::snippets[]
    :snippets: ./build/generated-snippets
    endif::[]

    == REQUEST

    include::{snippets}/user/create/http-request.adoc[]

    == RESPONSE

    include::{snippets}/user/create/http-response.adoc[]
    ```
  - 이후에 `./gradlew build` 해주면 `resources/static/docs` 하위에 문서가 생긴다.

<br/>

## References
- https://velog.io/@max9106/Spring-Spring-rest-docs%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%AC%B8%EC%84%9C%ED%99%94
- https://techblog.woowahan.com/2597/
