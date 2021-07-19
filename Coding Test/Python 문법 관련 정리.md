___
# Python 문법 관련 정리
<br>

## 목차
  - [List 관련 메소드](#list-관련-메소드)
  - [튜플을 사용하면 좋은 경우](#튜플을-사용하면-좋은-경우)
  - [사전 자료형(Dictionary)](#사전-자료형dictionary)
  - [집합 자료형](#집합-자료형)
  - [기본 입출력](#기본-입출력)
  - [반복문](#반복문)
  - [함수와 람다 표현식](#함수와-람다-표현식)
  - [실전에서 유용한 표준 라이브러리](#실전에서-유용한-표준-라이브러리)
___
<br>

## List 관련 메소드

| 함수명 | 사용법 | 설명 | 시간 복잡도 |
|:--------:|--------|------|-------------|
| append() | 변수명.append() | 원소를 하나 **삽일**할때 사용 | O(1) |
| sort() | 변수명.sort() | **오름차순**으로 정렬 | O(NlogN) |
| " | 변수명.sort( reverse = True ) | **내림차순**으로 정렬 | " |
| reverse() | 변수명.reverse() | 리스트 원소의 **순서를 반대로** | O(N) |
| insert() | insert(삽입할 위치 인덱스, 삽입할 값) | 특정한 위치에 **원소 삽입** | O(N) |
| count() | 변수명.count(특정 값) | 특정 값을 가지는 **데이터의 개수** | O(N) |
| remove() | 변수명.remove(특정 값) | 특정 값을 갖는 **원소**를 **하나 제거** | O(N) |
<br>

- 원소 모두 제거하기
```python
a = [1, 2, 3, 4, 5, 5, 5]
remove_set = {3, 5} # 집합 자료형

# remove_list에 포함되지 않은 값만을 저장
result = [i for i in a if i not in remove_set]
print(result)
```
```python
[1, 2, 4]
```
___
<br>

## 튜플을 사용하면 좋은 경우
- 서로 다른 성질의 데이터를 묶ㅇ어서 관리해야 할 때
  - 최단경로 알고리즘에서는 (비용, 노드 번호)의 형태로 튜플 자료형을 자주 사용함.
- 데이터의 나열을 해싱의 키 값으로 사용해야 할 때
  - 튜플은 변경이 불가능하므로 리스트와 다르게 키 값으로 사용될 수 있음.
- 리스트보다 메모리를 효율적으로 사용해야 할 때
___
<br>

## 사전 자료형(Dictionary)
```python
a = dict()
a['홍길동'] = 97
a['이순신'] = 99

print(a)

b = {
    '홍길동': 97,
    '이순신': 99
}

print(b)
print(b['이순신'])

key_list = b.keys()
print(key_list)

key_list = list(b.keys())
value_list = list(b.values())
print(key_list)
print(value_list)
```
```python
{'홍길동': 97, '이순신': 99}
{'홍길동': 97, '이순신': 99}
99
dict_keys(['홍길동', '이순신'])
['홍길동', '이순신']
[97, 99]
```
___
<br>

## 집합 자료형
- 집합 자료형 초기화
```python
# 집합 자료형 초기화 방법 1
data = set([1, 1, 2, 3, 4, 4, 5])
print(data)

# 집합 자료형 초기화 방법 2
data = {1, 1, 2, 3, 4, 4, 5}
print(data)
```
```python
{1, 2, 3, 4, 5}
{1, 2, 3, 4, 5}
```
<br>

- 합집합, 교집합, 차집합
```python
a = set([1, 2, 3, 4, 5])
b = set([3, 4, 5, 6, 7])

# 합집합
print(a | b)

# 교집합
print(a & b)

# 차집합
print(a - b)
```
```python
{1, 2, 3, 4, 5, 6, 7}
{3, 4, 5}
{1, 2}
```
<br>

- 원소 삽입, 삭제
```python
data = set([1, 2, 3])
print(data)

# 원소 삽입
data.add(4)
print(data)

# 원소 여러개 삽입
data.update([5, 6])
print(data)

# 특정 원소 삭제
data.remove(3)
print(data)
```
```python
{1, 2, 3}
{1, 2, 3, 4}
{1, 2, 3, 4, 5, 6}
{1, 2, 4, 5, 6}
```
___
<br>

## 기본 입출력
- input() 함수는 **한 줄의 문자열을 입력받는 함수**입니다.
- map() 함수는 **리스트의 모든 원소에 각각 특정한 함수를 적용할 때 사용**합니다.
- ex) 공백을 기준으로 구분된 데이터를 입력 받을 때는 다음과 같이 사용합니다.
  - list( map( int, input().split() ))
- ex) 공백을 기준으로 구분된 데이터의 개수가 많지 않다면, 단순히 다음과 같이 사용합니다.
  - a, b, c = map( int, input().split() )
<br>

- 데이터를 입력 받아 int 형으로 형변환 후 list로 저장
```python
# 데이터의 개수 입력
n = int(input())

# 각 데이터를 공백을 기준으로 구분하여 입력
data = list(map(int, input().split()))

data.sort(reverse=True)
print(data)
```
```python
5
65 90 75 34 99
[99, 90, 75, 65, 34]
```
<br>

- 빠르게 입력 받기
```python
import sys

# 문자열 입력 받기
data = sys.stdin.readline().rstrip()
print(data)
```
<br>

- f-string
```python
answer = 7
print(f"정답은 {answer}입니다.")
```
```python
정답은 7입니다.
```
___
<br>

## 반복문
```python
array = [9, 8, 7, 5, 3]

for x in array:
  print(x)
```
```python
9
8
7
5
3
```
<br>

- 연속적인 값을 차례대로 순회하는 range(시작 값, 끝 값 + 1)
```python
result = 0

# i는 1부터 9까지의 모든 값을 순회
for i in range(1, 10):
  result += i

print(result)
```
```python
45
```
<br>

- 반복문에서 코드 실행을 건너뛰는 continue
```python
scores = [90, 85, 77, 65, 97]
cheating_student_list = {2, 4}

for i in range(5):
  if i + 1 in cheating_student_list:
    continue
  if scores[i] >= 80:
    print(i + 1, "번 학생은 합격입니다.")
```
```python
1 번 학생은 합격입니다.
5 번 학생은 합격입니다.
```
___
<br>

## 함수와 람다 표현식
- global 키워드
```python
a = 0

def func():
  global a
  a += 1

for i in range(10):
  func()

print(a)
```
```python
10
```
<br>

- 여러개의 return 값
```python
def operator(a, b):
  add = a + b
  sub = a - b
  mul = a * b
  div = a / b
  return add, sub, mul, div

a, b, c, d = operator(7, 3)
print(a, b, c, d)
```
```python
10 4 21 2.333333335
```
<br>

- 람다 표현식
  - 함수가 매우 간단하거나, 한번 사용하고 말 경우에 이용
```python
print((lambda a, b: a + b)(3, 7))
```
```python
10
```
<br>

- 람다 함수를 이용한 정렬
```python
array = [('홍길동', 50), ('이순신', 32), ('아무개', 74)]

def my_key(x):
  return x[1]

print(sorted(array, key = my_key))
print(sorted(array, key = lambda x: x[1]))
```
```python
[('이순신', 32), ('홍길동', 50), ('아무개', 74)]
[('이순신', 32), ('홍길동', 50), ('아무개', 74)]
```
<br>

- 여러 개의 리스트에 적용되는 람다 표현식
```python
list1 = [1, 2, 3, 4, 5]
list2 = [6, 7, 8, 9, 10]

result = map(lambda a, b: a + b, list1, list2)
print(list(result))
```
```python
[7, 9, 11, 13, 15]
```
___
<br>

## 실전에서 유용한 표준 라이브러리
- 내장 함수 : 기본 입출력 함수부터 정렬 함수까지 기본적인 함수들을 제공
- itertools : 파이썬에서 반복되는 형태의 데이터를 처리하기 위한 유용한 기능들을 제공
  - 특히 순열과 조합 라이브러리는 코딩 테스트에서 자주 사용됨
  - 완전 탐색 문제에서 소스 코드를 간결하게 해줌
- heapq : 힙 자료구조를 제공
  - 일반적으로 우선순위 큐 기능을 구현하기 위해 사용
  - 최단 경로 알고리즘에서 많이 활용됨
- bisect : 이진 탐색 기능 제공
- collections : 덱(deque), 카운터 등의 유용한 자료구조를 포함
- math : 필수적인 수학적 기능 제공
  - 팩토리얼, 제곱근, 최대공약수(GCD), 삼각함수 관련 함수부터 파이와 같은 상수를 포함

<br>

- 내장 함수
```python
# sum()
result = sum([1, 2, 3, 4, 5])
print(result)

# min(), max()
min_result = min(7, 3, 5, 2)
max_result = max(7, 3, 5, 2)
print(min_result, max_result)

# eval()
result = eval("(3 + 5) * 7")
print(result)

# sorted()
result = sorted([9, 1, 8, 5, 4])
reverse_result = sorted([9, 1, 8, 5, 4], reverse=True)
print(result)
print(reverse_result)

# sorted() with key
array = [('홍길동', 50), ('이순신', 32), ('아무개', 74)]
result = sorted(array, key = lambda x: x[1], reverse=True)
print(result)
```
```python
15
2 7
56
[1, 4, 5, 8, 9]
[9, 8, 5, 4, 1]
[('이순신', 75), ('아무개', 50), ('홍길동', 35)]
```
<br>

- 순열과 조합
<img width="700px" src="https://user-images.githubusercontent.com/60170616/126166426-46a855f5-b8b8-449f-8c2a-430aa13e6743.JPG">
<br>

- 순열
```python
from itertools import permutations

data = ['A', 'B', 'C'] # 데이터 준비

result = list(permutations(data, 3)) # 모든 순열 구하기
print(result)
```
```python
[('A', 'B', 'C'), ('A', 'C', 'B'), ('B', 'A', 'C'), ('B', 'C', 'A'), ('C', 'A', 'B'), ('C', 'B', 'A')]
```

- 중복 순열
```python
from itertools import product

data = ['A', 'B', 'C'] # 데이터 준비

result = list(product(data, repeat = 2)) # 2개를 뽑는 모든 순열 구하기(중복 허용)
print(result)
```
```python
[('A', 'A'), ('A', 'B'), ('A', 'C'), ('B', 'A'), ('B', 'B'), ('B', 'C'),
('C', 'A'), ('C', 'B'), ('C', 'C')]
```
<br>

- 조합
```python
from itertools import combinations

data = ['A', 'B', 'C'] # 데이터 준비

result = list(combinations(data, 2)) # 2개를 뽑는 모든 조합 구하기
print(result)
```
```python
[('A', 'B'), ('A', 'C'), ('B', 'C')]
```

- 중복 조합
```python
from itertools import combinations_with_replacement

data = ['A', 'B', 'C'] # 데이터 준비

result = list(combinations_with_replacement(data, 2)) # 2개를 뽑는 모든 조합 구하기(중복 허용)
print(result)
```
```python
[('A', 'A'), ('A', 'B'), ('A', 'C'), ('B', 'B'), ('B', 'C'), ('C', 'C')]
```
<br>

- Counter
```python
from collections import Counter

counter = Counter(['red', 'blue', 'red', 'green', 'blue', 'blue'])

print(counter['blue'])
print(counter['green'])
print(dict(counter))
```
```python
3
1
{'red': 2, 'blue': 3, 'green': 1}
```
<br>

- 최대 공약수( gcd() 함수 )와 최소 공배수
```python
import math

# 최소 공배수(LCM)를 구하는 함수
def lcm(a, b):
  return a * b // math.gcd(a, b)

a = 21
b = 14

print(math.gcd(a, b)) # 최대 공약수 계산
print(lcm(a, b)) # 최소 공배수 계산
```
```python
7
42
```
___