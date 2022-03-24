# Java 18
- 2022년 3월에 발표된 Java 18 new features

## INDEX
- [Simple Web Server](#simple-web-server) 
- [Code Snippets in Java API Documentation](#code-snippets-in-java-api-documentation)
- [Reimplement Core Reflection with Method Handles](#reimplement-core-reflection-with-method-handles)
- [Deprecate Finalization for Removal](#deprecate-finalization-for-removal)
<br/>

### Simple Web Server
- static file만 제공하는 최소한의 web server를 command-line tool로 시작할 수 있게 제공
- CGI 또는 servlet과 유사한 기능을 사용할 수 없음
- 실 서비스가 아닌 교육 환경에서 prototyping, 임시 코딩 및 테스트 목적에 유용
  ```
  <!-- 실행 -->
  jwebserver -p 9000
  ```
<br/>

### Code Snippets in Java API Documentation
- API 문서에 예제 코드 포함을 단순화하기 위해 JavaDoc의 표준 Doclet에 @snippet 태그 도입
- 예시
  ```java
  /**
   * The following code shows how to use {@code Optional.isPresent}:
   * {@snippet file="ShowOptional.java" region="example"}
   */
  
  public class ShowOptional {
      void show(Optional<String> v) {
          // @start region="example"
          if (v.isPresent()) {
              System.out.println("v: " + v.get());
          }
          // @end
      }
  }
  ```
<br/>

### Reimplement Core Reflection with Method Handles
- java.lang.invoke method 처리 위에 java.lang.reflect.Method, Constructor 및 Field가 재구현됨
- java.lang.reflect와 java.lang.invoke API에 대해 유지관리 및 개발 비용을 줄여줌
<br/>

### Deprecate Finalization for Removal
- 기존에는 try catch의 finally 구문을 통해 resouce close 처리를 했다.
  ```java
  FileInputStream input = null;
  FileOutputStream output = null;

  try {
      input  = new FileInputStream(file1);
      output = new FileOutputStream(file2);
      ... copy bytes from input to output ...
      output.close();
      output = null;
      input.close();
      input = null;
  } finally {
      if (output != null) output.close();
      if (input != null) input.close();
  }
  ```
- finalization의 근본적인 결함
  - `예측할 수 없는 대기시간`
    - object에 연결할 수 없게 되는 순간과 close가 호출되는 순간 사이에 임의의 긴 시간이 걸릴 수 있다.
    - GC는 종료자가 호출된다는 보장을 제공하지 않는다.
  - `제약 없는 behavior`
    - finalizer code는 모든 작업을 수행할 수 있다. 특히 완료되는 object에 대한 참조를 저장할 수 있으므로 object를 부활시키고 다시 한번 연결할 수 있다.
  - `항상 활성화됨`
    - finalizer는 명시적인 등록 메커니즘이 없다.
    - finalizer가 있는 class는 필요 여부에 관계없이 class의 모든 instance에 대한 finalization을 가능하게 한다.
    - 해당 object에 더 이상 필요하지 않은 경우에도 object의 finalizer는 취소할 수 없다.
  - `지정되지 않은 threading`
    - finalizer는 지정되지 않은 thread 에서 임의의 순서로 실행된다.
    - threading이나 순서를 제어할 수 없다.
- 결함으로 인한 문제점
  - `보안 취약성`
  - `성능`
  - `신뢰할 수 없는 실행`
  - `어려운 프로그래밍 모델`
- **따라서 try-with-resources 구문, Cleaner 등 대체 기술을 사용해야 한다.**
- 향후 버전에서는 종료자를 사용하는 경우 런타임에 경고를 발생하고, 기본적으로 종료를 비활성화하고, --finalization=enabled와 같은 옵션을 제공할 예정
<br/>

## References
- https://openjdk.java.net/projects/jdk/18/spec/
- https://luvstudy.tistory.com/175
