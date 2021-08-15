___
# Collection

### 참고 영상
- [생활코딩_Java 입문 수업 Collections framework 151~159](https://www.youtube.com/playlist?list=PLuHgQVnccGMCeAy-2-llhw3nWoQKUvQck)
### 참고 자료
- [생활코딩 Collections Framework 강의 자료](https://opentutorials.org/module/516/6446)

<br/>

## INDEX
  - [Collections](#collections)
  - [ArrayList()](#arraylist)
  - [List와 Set의 차이점](#list와-set의-차이점)
  - [HashSet()](#hashset)
  - [iterator](#iterator)
  - [Map](#map)
##

## Collections
- `Collections`는 `Collection`과 `Map`으로 이뤄짐
- `Collections`의 구성
  <img width="500px" src="https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/516/2160.png">
##

## ArrayList()
```java
import java.util.ArrayList;
 
public class ArrayListDemo {
    public static void main(String[] args) {
        // String
        String[] arrayObj = new String[2];
        arrayObj[0] = "one";
        arrayObj[1] = "two";
        // arrayObj[2] = "three"; 초과

        for(int i = 0; i < arrayObj.length; i++){
            System.out.println(arrayObj[i]);
        }
        

        // ArrayList
        ArrayList al = new ArrayList();
        al.add("one"); // 인자들이 Object type으로 들어감
        al.add("two");
        al.add("three");

        for(int i = 0; i < al.size(); i++) {
            // String value = al.get(i);
            /* 불가능. String type에 Object type을 담을 수 없음.
               따라서 Generic 사용 */

            System.out.println(al.get(i));
        }

        // Generic
        ArrayList<String> al = new ArrayList<String>();
    }
}
```
##

## List와 Set의 차이점
- List는 순서가 있고 중복된 값도 저장될 수 있다.
  - get(n)으로 n번째 값 가져올 수 있음
- Set은 순서가 없고, 중복되는 값은 저장되지 않음(고유한 값)
```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.Iterator;
 
public class ListSetDemo {
    public static void main(String[] args) {
        ArrayList<String> al = new ArrayList<String>();
        al.add("one");
        al.add("two");
        al.add("two");
        al.add("three");
        al.add("three");
        Iterator ai = al.iterator();
        while(ai.hasNext()){
            System.out.println(ai.next());
            // 실행 결과: one two two three three
        }
         
        HashSet<String> hs = new HashSet<String>();
        hs.add("one");
        hs.add("two");
        hs.add("two");
        hs.add("three");
        hs.add("three");
        Iterator hi = hs.iterator();
        while(hi.hasNext()){
            System.out.println(hi.next());
            // 실행 결과: one two three
        }
    }
}
```
##

## HashSet()
- Set == 집합
```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.Iterator;
 
public class SetDemo {
    public static void main(String[] args) {
        HashSet<Integer> A = new HashSet<Integer>();
        A.add(1);
        A.add(2);
        A.add(3);
         
        HashSet<Integer> B = new HashSet<Integer>();
        B.add(3);
        B.add(4);
        B.add(5);
         
        HashSet<Integer> C = new HashSet<Integer>();
        C.add(1);
        C.add(2);
         
        System.out.println(A.containsAll(B)); // false
        System.out.println(A.containsAll(C)); // true (부분집합)
         
        A.addAll(B); // 합집합 -> A = {1, 2, 3, 4, 5}
        A.retainAll(B); // 교집합 -> A = {3}
        A.removeAll(B); // 차집합 -> A = {1, 2}
         
        Iterator hi = A.iterator();
        while(hi.hasNext()){
            System.out.println(hi.next());
        }
    }
}
```
##

## iterator
- `iterator` **interface**
- List와 Set에서 모두 사용 가능
- 메소드: hasNext(), next()
```java
import java.util.HashSet;
import java.util.Iterator;
 
public class SetDemo {
    public static void main(String[] args) {
        HashSet<Integer> A = new HashSet<Integer>();
        A.add(1);
        A.add(2);
        A.add(3);

        Iterator hi = A.iterator();
        while(hi.hasNext()){ // next 값이 있나? true or false
            System.out.println(hi.next()); // next 값으로 이동
            // {1, 2, 3} 처음 1에서 next를 하면 1이 사라지고(iterator 안에서만) 2로 이동
        }
    }
}
```
##

## Map
- key와 value로 저장
- 특정 key값을 요청하면 value가 return됨
- key 값은 중복되지 않음
- 이미 있는 key 값과 value를 저장하면 해당 key 값에 해당하는 value 덮어쓰기
- put() 은 collection에는 없고 map에만 존재
```java
import java.util.*;
 
public class MapDemo {
    public static void main(String[] args) {
        HashMap<String, Integer> a = new HashMap<String, Integer>();
        // key, value 저장
        a.put("one", 1);
        a.put("two", 2);
        a.put("three", 3);
        a.put("four", 4);

        // key값에 해당하는 value 호출
        System.out.println(a.get("one"));
        System.out.println(a.get("two"));
        System.out.println(a.get("three"));
         
        iteratorUsingForEach(a);
        iteratorUsingIterator(a);
    }


    // map에는 iterator 기능이 없어서 set을 구현
    // Entry는 (key, value)가 (String, Integer) type으로 들어있음

    static void iteratorUsingForEach(HashMap map){
        Set<Map.Entry<String, Integer>> entries = map.entrySet();

        // key, value 값들을 순서대로 출력(key값 오름정렬 순서)
        for (Map.Entry<String, Integer> entry : entries) {
            System.out.println(entry.getKey() + " : " + entry.getValue());
        }
    }

    static void iteratorUsingIterator(HashMap map){
        Set<Map.Entry<String, Integer>> entries = map.entrySet();
        Iterator<Map.Entry<String, Integer>> i = entries.iterator();
        while(i.hasNext()){
            Map.Entry<String, Integer> entry = i.next();
            System.out.println(entry.getKey()+" : "+entry.getValue());
        }
    }
}
```
##

## Collections 정렬
```java
import java.util.*;

class Computer implements Comparable{
    int serial;
    String owner;

    Computer(int serial, String owner){
        this.serial = serial;
        this.owner = owner;
    }

    public int compareTo(Object o) {
        return this.serial - ((Computer)o).serial;
    }

    public String toString(){
        return serial+" "+owner;
    }
}
 
public class CollectionsDemo {
    public static void main(String[] args) {
        List<Computer> computers = new ArrayList<Computer>();
        computers.add(new Computer(500, "egoing"));
        computers.add(new Computer(200, "leezche"));
        computers.add(new Computer(3233, "graphittie"));

        // 정렬 전 println
        Iterator i = computers.iterator();
        while(i.hasNext()){
            System.out.println(i.next());
        }

        // 정렬
        Collections.sort(computers);

        // 정렬 후 println
        i = computers.iterator();
        while(i.hasNext()){
            System.out.println(i.next());
        }
    }
}
```
___