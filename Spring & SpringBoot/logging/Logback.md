# Logback 구조
- `logback-core`
  - 다른 두 모듈(classic, access)을 위한 기반
  - `Appender`와 `Layout` 인터페이스 포함
- `logback-classic`
  - logback-core 에서 확장된 모듈
  - logback-core와 SLF4J API 라이브러리 포함
    - 포함된 라이브러리들은 해당 Artifact의 올바른 버전 사용이 필요하고, 명시적으로 선언하는 것이 좋기 때문에 logback-classic 추가시 exclude 추가
  - `Logger` 클래스 포함
- `logback-access`
  - Servlet Container와 통합되어 HTTP 액세스에 대한 로깅 기능을 제공
  - 웹 애플리케이션 레벨이 아닌 컨테이너 레벨에 설치

# Logback 주요 클래스
- `Logger`
  - 로그에 레벨 지정
    - 레벨이 지정되지 않은 경우 가까운 부모의 레벨을 상속 받는다.
  - 다른 로그를 방해하지 않고 특정 로그 비활성화 가능
- `Appender`
  - 로그 메세지를 특정 위치에 전달 및 기록
- `Layout`
  - 로그 이벤트를 formatting

# Log 패턴

- %d{yyyy-MM-dd HH:mm:ss.sss}
  - 2022-01-01 01:01:01.001
- [%d{yyyy-MM-dd HH:mm:ss}:%-4relative]
  - 2022-01-01 01:01:01:0001
  - %relative: 밀리초
- %thread (%t)
  - 실행 Thread 명
- %-5level (%p)
  - 로그 레벨
- %c
  - 로깅이 발생한 카테고리
- %C
  - 로깅이 발생한 클래스 명
  - %C{2}는 package.class 출력
- %M
  - 로깅이 발생한 메소드 명
- %L (%line)
  - 로깅이 발생한 라인
- %msg (%m)
  - 로그 메시지
- %n
  - 줄바꿈(new line)
- %%
  - % 출력
- %logger
  - 패키지 포함 클래스 정보
  - %logger{0}: 패키지를 제외한 클래스 이름만 출력
  - %logger{length}: Logger 이름을 최대 length까지 출력
- %F (%file)
  - 로깅이 발생한 프로그램 파일명 출력
- %r
  - 애플리케이션 시작 이후부터 로깅이 발생한 시점까지의 시간(ms)
- %X (%mdc)
  - %X{key 값}
  - 로그를 동적으로 변경하기 위한 패턴
- ${PID:-}
  - 프로세스 아이디

# Logback 설정
```gradle
dependencies {
    implementation 'org.slf4j:slf4j-api:1.7.31'
    implementation 'ch.qos.logback:logback-core:1.2.3'

    implementation ('ch.qos.logback:logback-classic:1.2.3'){
        exclude group: 'org.slf4j', module: 'slf4j-api'
        exclude group: 'ch.qos.logback', module: 'logback-core'
    }
}
```

## References
- [velog.io/@hanblueblue](https://velog.io/@hanblueblue/%EB%B2%88%EC%97%AD-logback)