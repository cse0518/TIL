___
# Java 기초

#### 참고 영상_[(생활코딩 Java)](https://www.youtube.com/playlist?list=PLuHgQVnccGMCeAy-2-llhw3nWoQKUvQck)
<br/>

## INDEX
  - [클래스, 객체, 인스턴스](#클래스-객체-인스턴스)
    - [객체와 인스턴스의 차이?](#객체와-인스턴스의-차이)
  - [클래스 멤버, 인스턴스 멤버](#클래스-멤버-인스턴스-멤버)
    - [의문점 (해결)](#의문점-해결)
  - [상속, 생성자](#상속-생성자)
  - [overriding, overloading](#overriding-overloading)
  - [접근제어자(Access Level Modifiers)](#접근제어자access-level-modifiers)
##

## 클래스, 객체, 인스턴스
- 클래스 : 설계도
  - 객체를 만들어내기 위한 설계도, 틀
- 객체 : 구현할 대상
  - 클래스에 선언된 모양 그대로 생성된 실체
- 인스턴스 : 제품
  - 설계도를 바탕으로 구현된 구체적인 실체

### 객체와 인스턴스의 차이?
- 인스턴스는 객체에 포함된다고 볼 수 있다.
- 객체 > 인스턴스
- 객체가 메모리에 할당되어 실제 사용될 때 `인스턴스`라고 한다.
- 객체는 클래스의 인스턴스이다
- 결론은, `인스턴스`라는 용어를 반드시 클래스와 객체 사이의 관계로 한정지어서 사용할 필요는 없다.
- 인스턴스는 어떤 추상적인 원본으로부터 생성된 복제본
##

## 클래스 멤버, 인스턴스 멤버
- 클래스 멤버(변수) :
  - 클래스 내부의 전역 변수
  - static, final
  - 한번 만들고 사라지지 않음
  - 클래스에 직접 접근해서 사용할 수 있음
- 인스턴스 멤버(변수) :
  - 일시적으로 생성한 변수
  - 클래스 실행이 종료되면 사라짐
- 인스턴스 메소드는 클래스 변수에 접근 할 수 **있다**.
- 클래스 메소드는 인스턴스 변수에 접근 할 수 **없다**.
- 인스턴스 메소드는 인스턴스 변수에 접근 할 수 **있다**.
<br>

<details>
<summary>구체적인 예시</summary>
<div markdown="1">

```java
class C1{
    static int static_variable = 1;
    int instance_variable = 2;

    static void static_static(){
        System.out.println(static_variable);
    }

    static void static_instance(){
        // 클래스 메소드에서는 인스턴스 변수에 접근 할 수 없다. 
        System.out.println(instance_variable);
    }
    
    void instance_static(){
        // 인스턴스 메소드에서는 클래스 변수에 접근 할 수 있다.
        System.out.println(static_variable);
    }

    void instance_instance(){        
        System.out.println(instance_variable);
    }
}

public class ClassMemberDemo {  
    public static void main(String[] args) {
        C1 c = new C1();

        // 인스턴스를 이용해서 정적 메소드에 접근 -> 성공
        // 인스턴스 메소드가 정적 변수에 접근 -> 성공
        c.static_static();

        // 인스턴스를 이용해서 정적 메소드에 접근 -> 성공
        // 정적 메소드가 인스턴스 변수에 접근 -> 실패
        c.static_instance();

        // 인스턴스를 이용해서 인스턴스 메소드에 접근 -> 성공
        // 인스턴스 메소드가 클래스 변수에 접근 -> 성공
        c.instance_static();

        // 인스턴스를 이용해서 인스턴스 메소드에 접근 -> 성공 
        // 인스턴스 메소드가 인스턴스 변수에 접근 -> 성공
        c.instance_instance();

        // 클래스를 이용해서 클래스 메소드에 접근 -> 성공
        // 클래스 메소드가 클래스 변수에 접근 -> 성공
        C1.static_static();

        // 클래스를 이용해서 클래스 메소드에 접근 -> 성공
        // 클래스 메소드가 인스턴스 변수에 접근 -> 실패
        C1.static_instance();

        // 클래스를 이용해서 인스턴스 메소드에 접근 -> 실패
        C1.instance_static();

        // 클래스를 이용해서 인스턴스 메소드에 접근 -> 실패
        C1.instance_instance();
    }
}
```

</div>
</details>
<br>

### 의문점 (해결)
- 클래스 메소드로 호출하려면 static으로 선언 필요
- 인스턴스를 생성해서 인스턴스 메소드를 호출하려면 static 선언하면 안됨
- 그러면 하나로 통일하면 안되나?
  - 무조건 인스턴스를 생성해서 사용하면 static을 다 안쓰는 것으로 통일
  - or 무조건 다 클래스 메소드로 호출하면 안되나?
- 인스턴스 변수를 사용하는지 마는지의 차이
  - static 변수 // 불변의 공통 변수를 static 변수로 생성
  - instance 변수 // 자주 바뀔수 있는 변수를 instance 변수로 생성
  - static method // instance 변수를 사용하지 않는 경우 static method로
    - static method 호출 -> static 변수만을 사용하는 경우
  - instance method // instance 변수를 사용하는 경우 instance method로
    - instance method 호출 -> instance 변수도 사용하는 경우 instance 생성 후 호출해서 사용
- // 추가 학습 필요) instance와 call by value, call by reference 연관성
##

## 상속, 생성자
- 부모 클래스의 private 접근 제한을 갖는 필드 및 메소드는 자식이 물려받을 수 없다.
- 부모와 자식 클래스가 서로 다른 패키지에 있다면, 부모의 default 접근 제한을 갖는 필드 및 메소드도 자식이 물려받을 수 없다.
  - default 접근 제한은 '같은 패키지에 있는 클래스'만 접근이 가능하게끔 하는 접근 제한자이다.
- 다중 상속 X (하나의 부모 클래스만 상속 가능)
- 자식 객체를 생성하면, 부모 객체를 먼저 생성한 후에 자식 객체가 생성된다.
- 부모의 생성자가 선언되어 있는 경우, 자식 생성자에서 super()를 통하여 호출해줘야만 한다.
##

## overriding, overloading
- overriding: 부모의 메소드를 재정의, 덮어 쓰기(type, parameter는 맞춰줘야 함)
- overloading: 같은 이름의 메소드에 다른 자료형 or parameter 개수
##

## 접근제어자(Access Level Modifiers)
- public > protected > default > private

  |접근제어자|설명|
  |:--------:|----|
  |public    |모든 클래스에서 접근 가능|
  |protected |같은 패키지 or 해당 클래스를 상속받은 외부 패키지 클래스에서 접근 가능|
  |default   |같은 패키지 내에서 접근 가능(기본)|
  |private   |해당 클래스에서만 접근 가능|

___