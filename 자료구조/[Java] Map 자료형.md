# Map

### Map 자료형 입력 데이터 순서 보장

- `HashMap<>` : 입력 순서 보장 X
- `LinkedHashMap<>` : 입력 순서 보장 O
<br/>

### Map 자료형의 함수 활용

|함수명|key 값이 있는 경우|key 값이 없는 경우&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|return 값|
|------|------------------|------------------|:-------:|
|compute()          |value 값 `수정`|(key, value) `삽입`|&nbsp;(key 값 있는 경우 / 없는 경우)&nbsp;</br>value 값 / value 값|
|computeIfPresent()&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |value 값 `수정`|변화 없음|value 값 / null|
|merge()            |value 값 `수정`</br>수정할 값이 null이면 데이터 `삭제` &nbsp;&nbsp;&nbsp;&nbsp;|(key, value) `삽입`|value 값 / value 값|
|computeIfAbsent()  |value 값 `반환`|(key, value) `삽입`|value 값 / value 값|
|putIfAbsent()      |value 값 `반환`|(key, value) `삽입`|value 값 / null|

```java
// key 값이 존재하면 value 값 수정 -> value 반환
// key 값이 없으면 (key, value) 삽입
map.compute("key", (k, v) -> v);

// key 값이 존재하면 value 값 수정 -> value 반환
// key 값이 없으면 null 반환 (삽입 X)
map.computeIfPresent("key", (k, v) -> v);

// key 값이 존재하면 v2 값 수정 -> v2 반환
// 람다가 null이면 key 삭제
// key 값이 없으면 (key, v1) 새로 삽입 -> v1 반환
map.merge("key", v1, (k, v) -> v2);

// key 값이 존재하면 v1 반환
// key 값이 없으면 (key, v2) 새로 삽입 -> v2 반환
map.computeIfAbsent("key", v -> v2);

// key 값이 존재하면 v1 반환
// key 값이 없으면 (key, v2) 새로 삽입 -> null 반환
map.putIfAbsent("key", v2);
```
<br/>

### Map <--> 배열 변환

```java
Map<String, Integer> map = new HashMap<>();

// Map<String, Integer> -> String[] 변환
map.keySet().toArray(String[]::new);

// Map<String, Integer> -> int[] 변환
map.values().toArray(Integer[]::new);
```
<br/>

### Map 정렬 후 반환

```java
Map<String, Integer> map = new HashMap<>();

map.entrySet()
  .stream()

  // key 값(String) 사전순 정렬
  .sorted(Map.Entry.comparingByKey())

  // value 값(Integer) 역순 정렬
  .sorted(Map.Entry.comparingByValue(Comparator.reverseOrder()))

  // 배열 변환
  .map(Map.Entry::getKey)
  .toArray(String[]::new);
```
<br/>

### forEach()

```java
Map<String, Integer> map = new HashMap<>();
map.forEach((s, i) -> System.out.println(s + ", " + i));
```
<br/>

## Reference
- https://blog.advenoh.pe.kr/java/%EC%9E%90%EB%B0%948-HashMap-%EB%B3%B4%EB%8B%A4-%EA%B0%84%EA%B2%B0%ED%95%98%EA%B3%A0-%ED%9A%A8%EA%B3%BC%EC%A0%81%EC%9C%BC%EB%A1%9C-%EC%9E%91%EC%84%B1%ED%95%98%EA%B8%B0/
