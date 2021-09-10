___
# 📌 API vs Library vs Framework
- [참고 영상 (티버의 API vs Library vs Framework)](https://www.youtube.com/watch?v=We8JKbNQeLo)

## API
#### Application Programming Interface
- 응용 프로그램에서 운영체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 `Interface`
- 특징
  - 구현과 독립적으로 **사양만 정의**되어 있다.
  - API에 따라 접근 권한이 필요할 수 있다.
<br/>

### [구글 API (https://console.cloud.google.com/)](https://console.cloud.google.com/)
![image](https://user-images.githubusercontent.com/60170616/132798058-24309c37-2647-4a67-a670-e468e21dea14.png)
<br/>

![image](https://user-images.githubusercontent.com/60170616/132798553-2522771e-9b58-45f3-8f75-c5b3937b16ce.png)

<br/>

## Library
- 응용 프로그램 개발을 위해 필요한 기능(함수)을 모아 놓은 소프트웨어
- 특징
  - 독립성을 가진다.
  - **응용 프로그램이 능동적으로 라이브러리를 사용한다.**
  - Apache Commons, Lombok, Guava, jQuery 등
- #### 평균을 계산하는 라이브러리 사용 예시
```java
// 문자열을 입력받는다.
String scores = "마르코:95, 스펜서:90, 곰튀김:85, 해리:80";

// 평균을 계산하는 라이브러리 사용
double average = library.average(scores);

return average;
```
<br/>

## Framework
- 응용 프로그램이나 소프트웨어의 솔루션 개발을 수월하게 하기 위해 제공된 스프트웨어 환경
- 특징
  - 상호협력하는 `Class`와 `Interface`의 집합이다.
  - **응용 프로그램이 수동적으로 프레임워크에 의해 사용된다.**
  - Spring Framework, Junit, Ruby on Rails 등
<br/>

## ✨ 요약
- Library와 API의 차이점은 `구현 로직의 유무`이다.
- Library와 Framework의 차이점은 `응용 프로그램의 흐름 주도권을 누가 가지고 있느냐`이다.
<br/>

___