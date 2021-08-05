___
# JAVA에 대해 알게된 내용을 정리합니다.
- 최대한 간결하게 기록합니다.
- 깊게 다루고 싶은 주제는 [GitHub.io](https://cse0518.github.io/) 에 정리합니다.
___

## JAVA 개발환경
- JDK : 자바 개발 키트
- JVM -> 자바가 실행되기 위해 필요
  - 자바 실행 환경 : **JRE**
- JRE + 개발툴 -> **JDK (자바 개발 환경)**
  - java + javac(자바 컴파일러)
- vsc에서 **java extension pack** 설치
  - compile과 run을 간편하게!
  - debug 기능 지원(break point 이용)
##

## Build Tool
- 자동 빌드 툴
  - Ant -> Maven -> **Gradle**
- Gradle
  - gradle init : 프로젝트 생성(application - Java - no - groovy - JUnit Jupiter)
  - tree 입력 : 그래틀 프로젝트를 트리 형태로 보여줌
  - gradle build, gradle run
  - gradle tasks : task 목록 확인
    - build.gradle 파일에 세팅이 되어 있다.
    - plugins 에 application으로 설정을 해서 이 tasks를 사용할 수 있다.
    - vsc에서 **gradle tasks**를 설치하면 쉽게 파악 가능.
##

## IDE : 통합 개발 환경
- Eclipse -> **IntelliJ**
- 편리한 단축키([IntelliJ Cheat Sheet](https://t1.daumcdn.net/cfile/tistory/999B733A5E8E015318))
  |단축키|설명|
  |:----:|:--:|
  |ALT+Enter|오류 수정 or 코드 개선|
  |ALT+1</br>ESC|폴더창으로 커서 이동</br>원위치|
  |Ctrl+N|새 파일 생성|
  |Shift+Shift|파일 검색|
  |Ctrl+W</br>Shift+Ctrl+W|블록 지정</br>크게 & 작게|
  |Ctrl+Alt+L|코드 리포맷팅|
  |Shift+Ctrl+Alt+T|리팩토링 메뉴|
  |Shift+Ctrl+A|액션(명렁어) 검색|
  |Ctrl+E|최근 실행파일 확인 및 검색 가능|
##

## Coding Convention(규칙)
- 클래스명은 대문자로 시작
  - class MyClass {}
- 메소드나 변수명은 소문자로 시작
  - int myVariable = 0;
  - void goHome() {}
- 들여쓰기 space, tab 섞어쓰지 말기
##

## Reference
- Java에는 포인터 대신 reference라는 개념이 있다.
- primitive 값 8개 뺴고는 다 reference 값이다.
  - boolean, byte, int, short, long, float, double, char
  - array는 reference로 취급

<details>
<summary>예시</summary>
<div markdown="1">
<br>

- call by value
```java
public class Test {

    // call by value
    public static void main(String[] args) {
        Test t = new Test();

        int a = 100;
        t.multi(a);

        System.out.println(a);
    }

    // 새로운 변수 a 생성
    private void multi(int a) {
        a *= 2;
    }
}
```
```java
100
```
<br>

- call by reference
```java
class Int {
    int a = 100;
}

public class Test {

    // call by reference
    public static void main(String[] args) {
        Test t = new Test();

        // Int라는 오브젝트를 만들고, a라는 변수 만듬
        // a(1) 변수 안의 값은 Int를 가리키는 reference
        Int a = new Int();
        t.multi(a);

        System.out.println(a.a);
    }
    
    // 새로운 변수 a(2) 생성
    // a(1)이 reference 값을 전달했기 때문에 a(2)도 같은 reference를 참조
    private void multi(Int a) {
        a.a *= 2;
    }
}
```
```java
200
```

</div>
</details>

- call by value & call by reference 추가 학습 필요
##

## Constant Pool
- String을 특볋하게 취급(constant pool을 이용)
- 새로운 String을 만들었을 때 constant pool을 확인 -> 같은 문자열이 있을 경우 같은 reference 참조
- 자주 바뀔 문자열은 StringBuffer를 사용하는게 효율적
- String, StringBuffer, StringBuilder [[블로그 글 포스팅]](https://cse0518.github.io/StringBuffer&StringBuilder/)
##

## Object
- 모든 객체의 최상위 객체
- 모든 객체에는 Object의 메소드를 호출할 수 있음
- ex) toString(), equals(), hashCode()
___
