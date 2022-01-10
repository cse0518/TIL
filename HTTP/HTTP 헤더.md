# HTTP 헤더
- HTTP 전송에 대한 부가정보
- 2014년 이후 RFC7230~7235 등장

<br/>

## 주요 HTTP 헤더

### Content 관련 헤더
- `Content Representation`
  - `Content-Type`: 데이터 형식(미디어 타입, 인코딩)
  - `Content-Encoding`: 데이터 압축 방식
  - `Content-Language`: 데이터 자연 언어
  - `Content-Length`: 데이터 길이
- `Content Negotiation`
  - `Accept`: 클라이언트가 원하는 미디어 타입
  - `Accept-Charset`: 클라이언트가 원하는 문자 인코딩
  - `Accept-Encoding`: 클라이언트가 원하는 압축 인코딩
  - `Accept-Language`: 클라이언트가 원하는 자연 언어
- 우선순위 지정
  - 구체적일수록 높은 우선순위
    - ex) text/*, text/plain, text/plain;format=flowed, */*
  - 수치(0~1로 표현)가 클수록 높은 우선순위
    - ex) ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
  <br/>

### 정보 제공 헤더
- `Referer`
  - 현재 요청된 페이지의 이전 페이지 주소
  - 유입 경로 분석 가능
  - Request에서 사용
- `User-Agent`
  - 클라이언트의 애플리케이션 정보(웹 브라우저 정보 등)
  - 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
  - Request에서 사용
- `Host`
  - **필수**로 들어가는 HTTP 헤더
  - Request에서 사용
  - 하나의 서버가 여러 도메인을 처리해야 할 때 필요
  - 하나의 IP 주소에 여러 도메인이 적용되어 있을 때 필요
- `Location`
  - 3xx 응답 결과에 Location 헤더가 있으면 자동 리다이랙션
- `Allow`
  - 405(Method Not Allowed) 응답에 포함해야함
  - ex) Allow: GET, HEAD, PUT
- `Authorization`
  - 클라이언트 인증 정보를 서버에 전달
- `WWW-Authenticate`
  - 리소스 접근시 필요한 인증 방법 정의
  - 401(Unauthorized) 응답과 함께 사용
  <br/>

### 쿠키 관련 헤더
- `Cache-Control`
  - 요청과 응답 모두에서의 캐싱 메커니즘을 명시
  - ex) cache-control: max-age=60
    - 캐시 60초 유효
- `Pragma`
  - 캐싱 매커니즘 명시
  - HTTP 1.0 하위 호환
- 검증 헤더
  - 캐시 데이터와 서버 데이터가 같은지 검증하는 데이터
  - `Last-Modified`
    - 데이터가 마지막으로 수정된 시간
    - 캐시가 시간 초과 되었을 때 이전 데이터 수정이 없다면, HTTP 바디를 포함하지 않는 응답을 주고 다시 캐시 사용
    - 304 Not Modified 응답과 함께 사용
    - `If-Modified-Since`, `If-Unmodified-Since` (조건부 요청 헤더)
  - `ETag(Entity Tag)`
    - 캐시용 데이터에 임의의 tag 설정(Hash 키)
    - ETag가 같으면 캐시 사용, 다르면 다시 받음
    - `If-Match`, `If-None-Match` (조건부 요청 헤더)
<br/>

## 캐시 무효화 응답 설정
캐싱을 하면 안되는 데이터에 대한 상세 설정
- Cache-Control
  - no-cache
    - 데이터는 캐시해도 되지만, origin 서버에 검증하고 사용
  - no-store
    - 데이터 캐싱 X
  - must-revalidate
    - 캐시 만료후 origin 서버에 검증
    - 검증 실패시 504 에러
- Pragma
  - no-cache
    - 데이터는 캐시해도 되지만, origin 서버에 검증하고 사용

<br/>

## HTTP 추가적인 학습
- https://tools.ietf.org/html/rfc7230