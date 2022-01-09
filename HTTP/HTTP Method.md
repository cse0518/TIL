# HTTP Method

## 주요 Method
- `GET`
  - 리소스 조회
  - 서버에 전달하고 싶은 데이터는 쿼리 파라미터로 전달
  - **메세지 바디**로 데이터를 전달할 수는 있지만, 지원하지 않는 경우가 많아서 **사용하지 않음**.
- `POST`
  - 요청 데이터 처리(주로 리소스 등록)
  - 메세지 바디를 통해 서버로 데이터 전달
  - 프로세스를 처리하는 경우에도 사용
  - 다른 메소드로 처리하기 애매한 경우 주로 POST 사용
- `PUT`
  - 리소스 대체, 없으면 생성(덮어쓰기)
  - 클라이언트가 리소스의 위치를 알고 URI를 지정할 수 있는 경우 사용
- `PATCH`
  - 리소스 부분 수정
- `DELETE`
  - 리소스 삭제

<br/>

### POST와 PUT의 차이점
- `POST` method는 클라이언트가 리소스의 **location을 모르는 상황**에서 서버에게 리소스 생성을 요청한다.
  - ex) member의 id를 모르는 상황(아직 생성되지 않음)  
    -> POST 요청으로 member 리소스 생성 요청  
    -> 서버는 member 생성 후 location 제공(members/{id})
- `PUT` method는 클라이언트가 리소스의 **location을 알고 있는 상황**에서 서버에게 리소스 대체를 요청한다.
  - ex) member의 id를 알고 있는 상황, 또는 특정 id의 member 생성  
    -> PUT 요청으로 member 리소스 수정 요청  
    -> 서버는 해당 id의 member 정보 수정, 없다면 생성

<br/>

## HTTP Method 속성
![HTTP Method 속성](https://user-images.githubusercontent.com/60170616/152749046-09f0cd7e-1db7-4ce3-b2dd-1caca99bedc3.png)
- `안전`
  - 호출해도 리소스가 변경되지 않음
- `멱등`
  - 같은 요청을 여러번 호출해도 한번 호출한 것과 결과가 같음
  - 서버에서 정상 응답이 오지 않았을 때, 같은 요청을 다시 해도 되는가?  
    멱등한가?
- `캐시가능`
  - 응답 결과 리소스를 캐시해서 사용해도 되는가?
  - 주로 GET, HEAD method만 캐시로 사용
    - POST, PATCH는 본문 내용까지 캐시 키로 고려해야 하는데, 구현이 쉽지 않음

<br/>

## URI 설계

- `Best Practices`
  - https://restfulapi.net/resource-naming/