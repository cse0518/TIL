# Map 정렬
- TreeMap을 이용한 정렬
  - `key` 값 정렬
  ```java
  final HashMap<Integer, String> map = new HashMap<>();
  
  map.put(3, "value1");
  map.put(1, "value2");
  map.put(2, "value4");
  map.put(5, "value3");
  map.put(4, "value5");
  
  // new TreeMap 생성, 오름차순 정렬
  final Map<Integer, String> naturalOrderMap = new TreeMap<>(Comparator.naturalOrder());
  // 내림차순 -> reverseOrder()
  
  // key 값으로 정렬
  naturalOrderMap.putAll(map);
  ```
  - `value` 값 정렬
  ```java
  final HashMap<Integer, String> map = new HashMap<>();
  
  map.put(3, "value1");
  map.put(1, "value2");
  map.put(2, "value4");
  map.put(5, "value3");
  map.put(4, "value5");
  
  final List<Map.Entry<Integer, String>> entryList = new ArrayList<>(map.entrySet());

  // value 값으로 정렬
  entryList.sort(Map.Entry.comparingByValue());
  // 내림차순
  entryList.sort((o1, o2) -> o2.getValue().compareTo(o1.getValue()));
  ```
<br/>

## Reference
- https://jino-dev-diary.tistory.com/entry/Java-Map-%EC%82%AC%EC%9A%A9%EB%B2%951-Map-%EC%A0%95%EB%A0%AC%EA%B3%BC-MultiValueMap
