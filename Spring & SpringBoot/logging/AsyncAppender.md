# AsyncAppender

- 기존의 Appender를 async 하게 사용하도록 해준다.
- 장점
  - 로그를 남기는데 있어서 성능 향상(file i/o 작업 생략)
- 단점
  - 버퍼를 이용하므로 메모리 및 CPU 사용량 증가
  - 로그를 기록하는데 손실이 발생할 수 있음
    - queueSize가 작게 설정된 경우
    - 서버가 shutdown 되었을 때 버퍼에 로그가 남아있다면 손실 발생 가능
  - 큐 메모리 관리 등 관리 대상이 늘어나게 됨
- 결론
  - 대용량 트래픽을 받으며 로그를 많이 남겨야 하는 경우 성능 향상을 위해 사용할 수 있다.
  - 단점들을 고려해서 꼭 성능 향상이 필요한 경우에 사용하는게 좋을 것이다.

<br/>

## AsyncAppender 설정
```xml
<configuration debug="true">
  <conversionRule conversionWord="clr" converterClass="org.springframework.boot.logging.logback.ColorConverter" />

  <property name="LOG_PATH" value="./logs"/>
  <property name="FILE_NAME" value="logback-test.log"/>

  <appender name="RollingFile" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <file>${LOG_PATH}/${FILE_NAME}</file>
    <encoder>
      <Pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} %-5level [%thread,%X{X-B3-TraceId:-},%X{X-B3-SpanId:-},%X{X-Span-Export:-}] %logger{36} [%file:%line] - %msg ##%n</Pattern>
          %d{yyyy-MM-dd HH:mm:ss}:%-4relative %-5level [%C.%M]:%L] %n    > %msg%n
    </encoder>
    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
      <fileNamePattern>${LOG_PATH}/${FILE_NAME}.%d{yyyy-MM-dd}</fileNamePattern>
      <maxHistory>5</maxHistory>
    </rollingPolicy>
  </appender>

  <!-- AsyncAppender -->
  <appender name="file-async" class="ch.qos.logback.classic.AsyncAppender">
    <appender-ref ref="RollingFile" />
    <queueSize>4096</queueSize>
    <discardingThreshold>10</discardingThreshold>
    <includeCallerData>false</includeCallerData>
    <neverBlock>true</neverBlock>
    <maxFlushTime>3000</maxFlushTime>
  </appender>

  <root level="DEBUG">
    <!-- AsyncAppender로 수정 -->
    <appender-ref ref="file-async" />
  </root>

</configuration>
```

<br/>

## AsyncAppender 옵션

|Option|Description|
|:-----|:----------|
|queueSize|버퍼에 저장해두는 queue의 사이즈(default=256)<br/>|
|discardingThreshold|💥queue 사이즈의 80%(default)를 초과하게 되면 warn, error level의 log를 제외하고 삭제한다.<br/>queue가 몇 퍼센트 남으면 info level 이하의 log를 삭제할 지 설정(default=20)<br/>-> 0으로 세팅하면 삭제되지 않음|
|includeCallerData|로그를 남길때 현재 클래스의 몇 번째 라인에서 로그를 남기고 있는지 명시해준다.<br/>로그를 추적하는데 큰 도움이 되지만 큰 성능 차이를 보인다.|
|maxFlushTime|서버가 멈추거나 재배포될 때, 설정한 시간만큼 queue에 남아있는 log를 처리하고 종료한다.<br/>0으로 설정하면 queue의 모든 처리를 마치고 종료한다.|
|neverBlock|discardingThreshold 설정에 의해 info level 이하의 log를 한 번 삭제하고 또다시 queue가 가득차면 다른 스레드의 작업들이 blocking 상태에 빠지게 되는데, 해당 옵션을 주면 blocking 상태에 빠지지 않고 log를 삭제하며 계속 진행할 수 있게 해준다.|

<br/>

## References
- [공식문서](https://logback.qos.ch/manual/appenders.html#AsyncAppender)
- [kangwoojin.github.io](https://kangwoojin.github.io/programing/logback-async-appender/)