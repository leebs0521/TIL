# Overloading and Overriding
## 1. 오버로딩 (Overloading)
> 오버로딩(Overloading)이라는 뜻은 사전적으로 '과적하다.'라는 뜻이다. 
- 자바의 한 클래스 내에 같은 이름을 가진 메소드가 있더라도 매개변수의 개수 또는 타입이 다르면, 같은 이름을 사용해서 메소드를 정의할 수 있다.
- **메소드 이름과 매개변수는 같지만 리턴 타입만 다른 것은 오버로딩 할 수 없다.**

> 장점
- 같은 기능하는 메소드를 하나의 이름으로 사용 가능
- 메소드 이름을 절약

> 예제
```java
package overloading;

public class Overloading {

    public static void main(String[] args) {
        OverloadingMethods om = new OverloadingMethods();

        om.print();
        System.out.println(om.print(3));
        om.print("Hello!");
        System.out.println(om.print(4, 5));
    }
}

class OverloadingMethods {
    public void print() {
        System.out.println("오버로딩1");
    }

    String print(Integer a) {
        System.out.println("오버로딩2");
        return a.toString();
    }

    void print(String a) {
        System.out.println("오버로딩3");
        System.out.println(a);
    }

    String print(Integer a, Integer b) {
        System.out.println("오버로딩4");
        return a.toString() + b.toString();
    }

}
```
## 2. 오버라이딩 (Overriding)
> 부모 클래스로부터 상속받은 메소드를 자식 클래스에서 재정의하는 것
- 자식 클래스에서 상황에 맞게 변경해야하는 경우 오버라이딩이 필요하다.
- **오버라이딩하고자하는 메소드이름, 매개변수, 리턴 타입이 모두 같아야한다.**
- **부모 클래스의 메소드보다 더 좁은 범위의 접근제어자, 더 큰 범위의 예를 선언할 수 없다.**

> 장점
- 메소드 하나로 여러 객체를 다루고 객체마다 다른 기능을 사용할 수 있다.

> 예제
```java
package overriding;

class Person {
    void cry() {
        System.out.println("ㅠㅠ");
    }
}

class Child extends Person {
    @Override
    protected void cry() {
        System.out.println("응애");
    }
}

class Senior extends Person {
    @Override
    public void cry() {
        System.out.println("훌쩍");
    }
}

public class Overriding {
    public static void main(String[] args) {
        Person person = new Person();
        Child child = new Child();
        Senior senior = new Senior();

        person.cry();
        child.cry();
        senior.cry();
    }
}
```

## Reference
- [https://programmingnote.tistory.com/29](https://programmingnote.tistory.com/29)
- [https://hyoje420.tistory.com/14](https://hyoje420.tistory.com/14)