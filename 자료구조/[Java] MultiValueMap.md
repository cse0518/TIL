# MultiValueMap
- 하나의 key에 여러 values를 받을 수 있는 Map 자료구조
  - HTTP header, HTTP 쿼리 파라미터 등 하나의 key에 여러 value를 받을 때 사용
  ```java
  MultiValueMap<String, String> map = new LinkedMultiValueMap();
  map.add("key", "value1");
  map.add("key", "value2");
  
  // 배열로 반환
  List<String> keyValues = map.get("key");
  ```
- 일반 Map은 하나의 key에 하나의 value만을 가질 수 있다.
  ```java
  HashMap<String, String> map = new HashMap<>();
  map.add("key", "value1");
  map.add("key", "value2");
  
  // keyValue = "value2"
  String keyValue = map.get("key");
  ```
