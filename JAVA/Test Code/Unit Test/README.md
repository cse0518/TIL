# Unit Test

> 단위 테스트의 핵심은 `관심사의 분리`이다.  
> 각각의 계층에서 수행해야 하는 역할을 정확히 인지하고, 해당 **역할이 잘 수행되는지만 확인**하는 것이다.
> 
> 예를 들어, Controller Layer 테스트에서 Service Layer의 로직을 확인할 필요는 없다. Controller Test는 Service의 메소드를 호출하는지 여부만 확인하면 된다. Service Layer Test에서 로직이 잘 검증되었다면 Controller Layer에서는 당연히 잘 작동될 것이고, Service의 메소드를 잘 호출하기만 하면 되는것이다.
> 
> 이렇게 모든 각각의 기능을 검증하려면 수많은 테스트 코드가 나오게된다. 그만큼 전체 테스트 코드를 돌려보는데 많은 시간이 소요되므로, 각각의 테스트에 필요한 최소한의 환경만 구성하여 테스트를 가볍게 만들어야 한다.  

<br/>

## 각 계층의 단위 테스트
- [Entity Unit Test](Entity%20Unit%20Test.md)
  - `@ParameterizedTest`
    - @ValueSource
    - @NullSource, @EmptySource, @NullAndEmptySource
    - @CsvSource
    - @EnumSource
    - @MethodSource
- [Repository Layer Unit Test](Repository%20Layer%20Unit%20Test.md)
  - `@DataJpaTest`
  - TestEntityManager
  - Test 검증
- [Service Layer Unit Test](Service%20Layer%20Unit%20Test.md)
  - Test Double
  - Mockito
    - `@Mock`
    - `@Spy`
    - `@InjectMocks`
  - Test 검증
- [Controller Layer Unit Test](Controller%20Layer%20Unit%20Test.md)
  - `@WebMvcTest`
  - MockMvc, ObjectMapper
  - `@MockBean`, `@SpyBean`
  - Test 검증

<br/>

## 추가 참고 자료
- [Mockito와 BDDMockito](Mockito와%20BDDMockito.md)
  - Mockito와 BDDMockito 비교
  - `Mockito` 메소드
    - when().thenReturn()
    - doReturn().when()
    - verify()
  - `BDDMockito` 메소드
    - given().willReturn()
    - then().should()
    - verify()
  - `Mockito`의 문제
    - org.mockito.exceptions.misusing.PotentialStubbingProblem
    - Strict Mode, Lenient Mode
- [RestDocs](RestDocs.md)
  - 기본 환경 설정
  - `@AutoConfigureRestDocs`
  - 문서화
