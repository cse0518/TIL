___
# Assert
- 개발, 테스트 단계에서 parameter가 제대로 넘어왔는지 또는 특정 메소드가 작동하는 상황을 정하는 용도로 사용
  - Assert expression;
    - booleanTypeExpression가 참이면 pass, 거짓이면 AssertionError 예외 발생
  - Assert expression1: expression2;
    - expression1이 거짓인 경우 expression2 값이 예외 메세지로 보여짐
- Assert.isTrue(expression, MessageFormat.format("exception message"))
___