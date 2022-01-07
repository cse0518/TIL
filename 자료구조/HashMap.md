# HashMap
- `HashTable`, `HashMap`, `ConcurrentHashMap`의 차이를 알고 사용하자.

<br/>

## HashTable
```java
public class Hashtable<K,V>
        extends Dictionary<K,V>
        implements Map<K,V>, Cloneable, java.io.Serializable {

    public synchronized int size() { }

    @SuppressWarnings("unchecked")
    public synchronized V get(Object key) { }

    public synchronized V put(K key, V value) { }

}
```
- 각각의 메소드 전체를 synchronized 하게 설정했다. 따라서 메소드 전체가 임계구역으로 설정된다.
- 하지만 객체마다 Lock을 하나씩 가지고 있기 떄문에 동시에 여러 작업을 해야할 때 병목현상이 발생할 수 밖에 없다.
- Thread-safe 하다는 장점이 있지만 느리다.
- 잘 사용되지 않는다.

<br/>

## HashMap
```java
public class HashMap<K,V>
        extends AbstractMap<K,V>
        implements Map<K,V>, Cloneable, Serializable {

    public V get(Object key) {}
    public V put(K key, V value) {}

}
```
- Map 인터페이스를 구현한 클래스 중에서 성능이 제일 좋다.
- 멀티 스레드 환경에서 사용할 수 없다. (Synchronized, Thread-safe 하지 않아서)

<br/>

## ConcurrentHashMap
```java
public class ConcurrentHashMap<K,V>
        extends AbstractMap<K,V>
        implements ConcurrentMap<K,V>, Serializable {

    public V get(Object key) {}

    public boolean containsKey(Object key) { }

    public V put(K key, V value) {
        return putVal(key, value, false);
    }

    final V putVal(K key, V value, boolean onlyIfAbsent) {
        if (key == null || value == null) throw new NullPointerException();
        int hash = spread(key.hashCode());
        int binCount = 0;
        for (Node<K,V>[] tab = table;;) {
            Node<K,V> f; int n, i, fh;
            if (tab == null || (n = tab.length) == 0)
                tab = initTable();
            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
                if (casTabAt(tab, i, null,
                             new Node<K,V>(hash, key, value, null)))
                    break;
            }
            else if ((fh = f.hash) == MOVED)
                tab = helpTransfer(tab, f);
            else {
                V oldVal = null;
                synchronized (f) {
                    if (tabAt(tab, i) == f) {
                        if (fh >= 0) {
                            binCount = 1;
                            for (Node<K,V> e = f;; ++binCount) {
                                K ek;
                                // 새로운 노드로 교체
                                if (e.hash == hash &&
                                    ((ek = e.key) == key ||
                                     (ek != null && key.equals(ek)))) {
                                    oldVal = e.val;
                                    if (!onlyIfAbsent)
                                        e.val = value;
                                    break;
                                }
                                Node<K,V> pred = e;
                                // Separate Chaining에 추가
                                if ((e = e.next) == null) {
                                    pred.next = new Node<K,V>(hash, key,
                                                              value, null);
                                    break;
                                }
                            }
                        }
                        // Tree에 추가
                        else if (f instanceof TreeBin) {
                            Node<K,V> p;
                            binCount = 2;
                            if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,
                                                           value)) != null) {
                                oldVal = p.val;
                                if (!onlyIfAbsent)
                                    p.val = value;
                            }
                        }
                    }
                }
                if (binCount != 0) {
                    if (binCount >= TREEIFY_THRESHOLD)
                        treeifyBin(tab, i);
                    if (oldVal != null)
                        return oldVal;
                    break;
                }
            }
        }
        addCount(1L, binCount);
        return null;
    }
}
```
- synchronized 키워드가 메소드 전체에 있지 않다.  
  put() 메소드 중간에 존재.
- 인덱스가 비어 있어서 빈 해시 버킷에 노드를 삽입하는 경우, lock을 사용하지 않고 Compare and Swap을 이용하여 새로운 노드를 해시 버킷에 삽입.(원자성)
- 이미 노드가 존재하는 경우, synchronized 를 이용해 하나의 스레드만 접근할 수 있도록 제어한다.  
  **서로 다른 스레드가 같은 해시 버킷에 접근할 때만 해당 블록이 잠기게 된다.**

- 생성자에서는 초기 해시테이블 사이즈를 결정한다. (default_capacity = 16)  
  첫 노드가 삽입될 때 해시 테이블이 생성된다.  
  해시테이블 사이즈는 **75% 이상** 채워질 때 **2배**로 증가시킨다.
- 여러 스레드에서 동시에 쓰는 작업을 할 수 있는 경우엔 ConcurrentHashMap이 적합.

<br/>

### 가변 배열 리사이징
- `HashMap`에서의 리사이징은 단순히 resize() 메소드를 통해 새로운 배열을 만들어 copy 하는 방식이었다.
- `ConcurrentHashMap`에서의 리사이징은 기존 배열(table)을 새로운 배열로 버킷을 하나씩 전송하는 방식이다. 이 과정에서 다른 스레드가 버킷 전송에 참여할 수 있다. 전송이 끝나면 크기가 2배인 새로운 배열이 된다.

<br/>

## Reference
- [https://pplenty.tistory.com/17](https://pplenty.tistory.com/17)