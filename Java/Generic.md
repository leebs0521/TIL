# Generic
## 목차
### [1. Generic의 개념](#generic의-개념)
### [2. Generic의 다양한 표현](#generic의-다양한-표현)

## Generic의 개념
### 제네릭(Generic)이란?
 - 자바에서 제네릭은 데이터의 타입을 일반화하는 것을 의미
 - 클래스나 메소드에서 사용할 내부 데이터 타입을 컴파일 시에 미리 지정하는 방법

### 제네릭의 장점
 - 클래스나 메소드 내부에서 사용되는 객체의 타입 안정성을 높임
 - 반환값에 대한 타입 변환 및 타입 검사에 들어가는 비용을 아낄 수 있음

### JDK1.5 이전
 - 여러 타입을 사용하는 대부분의 클래스나 메소드에서 인수나 반환값으로 Object 타입 사용
 - 제네릭 도입(JDK1.5) 이후 제네릭을 사용하면서 컴파일 시에 미리 타입이 정해지므로, 타입 검사나, 타입 변환과 같은 번거러운 작업 생략

 #### 예제
```java
public class MyArray {

    private  Object obj;

    public Object getObj() {
        return obj;
    }

    public void setObj(Object obj) {
        this.obj = obj;
    }
}
```

```java
public class MyArrayExample {
    public static void main(String[] args) {
        MyArray myArray = new MyArray();
        myArray.setObj("hello");
        String str = (String) myArray.getObj();
    }
}
```


### 제네릭의 선언 및 생성
#### 제네릭 선언
```java
class MyArray<T> {

    T element;

    void setElement(T element) { this.element = element; }

    T getElement() { return element; }
}
```
- `T`를 타입 변수(type variable)라고 하고, 임의의 참조형 타입을 의미
- `T`가 아닌 임의의 문자 사용 가능, 여러 개의 타입 변수는 쉼표(`,`)으로 구분하여 명시
- 클래스 뿐만아니라 메소드의 매개변수나, 반환값으로 사용 가능

#### 제네릭 클래스 생성
```java
MyArray<Integer> myArr1 = new MyArray<Integer>(); // 실제 타입인 Integer 명시
```
- 컴파일시, 내부적으로 정의된 타입 변수(`T`)가 명시된 실제 타입(`Integer`)으로 변환

#### JAVA SE 7 이후
```java
MyArray<Integer> myArr2 = new MyArray<>(); // 인스턴스 생성 시 타입 생략 가능
```

### 타입 변수의 다형성
```java
import java.util.*;

class LandAnimal { 
    public void crying() { 
        System.out.println("육지동물"); 
    } 
}

class Cat extends LandAnimal { 
    public void crying() { 
        System.out.println("냐옹냐옹"); 
    } 
}

class Dog extends LandAnimal {
    public void crying() { 
        System.out.println("멍멍"); 
    } 
}

class Sparrow {
    public void crying() { 
        System.out.println("짹짹"); 
    } 
}

class AnimalList<T> {
    ArrayList<T> al = new ArrayList<T>();
    void add(T animal) { al.add(animal); }
    T get(int index) { return al.get(index); }
    boolean remove(T animal) { return al.remove(animal); }
    int size() { return al.size(); }
}

public class Generic01 {
    public static void main(String[] args) {
        AnimalList<LandAnimal> landAnimal = new AnimalList<>();
        landAnimal.add(new LandAnimal());
        landAnimal.add(new Cat());
        landAnimal.add(new Dog());
        // landAnimal.add(new Sparrow()); // 에러 발생

        for (int i = 0; i < landAnimal.size() ; i++) {
            landAnimal.get(i).crying();
        }
    }
}
```
- `Cat`, `Dog` 클래스는 `LandAnimal` 클래스를 상속받은 자식 클래스이므로 `AnimalList`에 추가 가능
- `Sparrow`클래스는 타입이 다르므로 리스트에 추가 불가능 -> 에러 발생

#### 제네릭의 제거 시기
- **컴파일 시 컴파일러에 의해 자동으로 검사되어 타입 변환**
- 코드 내의 모든 제네릭 타입 제거 -> **컴파일된 `class`파일에는 제네릭 타입 포함 x**
- 그 이유는, 제네릭을 사용하지 않는 코드와의 호환성을 위해


## Generic의 다양한 표현
### 타입 변수의 제한
- 제네릭은 `T`와 같은 타입 변수를 사용하여 타입을 제한
- `extends`키워드를 사용하여 타입 변수에 특정 타입만을 사용하도록 제한 가능
```java
class AnimalList<T extends LandAnimal> { ... } // 클래스 내부에서 LandAnimal를 상속 받은 클래스만 사용 o

...

interface WarmBlood { ... }

...

class AnimalList<T extends WarmBlood> { ... } // implements 키워드를 사용 x

...

class AnimalList<T extends LandAnimal & WarmBlood> { ... } // 상속 및 구현
```
- 클래스 내부에서 사용된 모든 타입 변수에 제한이 생김
- **(주의)** 인터페이스를 구현할 경우에도 `implements`가 아닌 `extends` 키워드 사용
- 클래스와 인터페이스를 동시에 상속받고 구현해야하는 경우 -> 엠퍼센트(`&`) 기호 사용

#### 예제
```java
import java.util.*;

class LandAnimal {
    public void crying() {
        System.out.println("육지동물");
    }
}

class Cat extends LandAnimal {
    public void crying() {
        System.out.println("냐옹냐옹");
    }
}

class Dog extends LandAnimal {
    public void crying() {
        System.out.println("멍멍");
    }
}

class Sparrow {
    public void crying() {
        System.out.println("짹짹");
    }
}

class AnimalList<T extends LandAnimal> {
    ArrayList<T> al = new ArrayList<T>();
    void add(T animal) { al.add(animal); }
    T get(int index) { return al.get(index); }
    boolean remove(T animal) { return al.remove(animal); }
    int size() { return al.size(); }
}

public class Generic02 {
    public static void main(String[] args) {
        AnimalList<LandAnimal> landAnimal = new AnimalList<>();
        landAnimal.add(new LandAnimal());
        landAnimal.add(new Cat());
        landAnimal.add(new Dog());
        // landAnimal.add(new Sparrow()); --> 오류가 발생
        for (int i = 0; i < landAnimal.size() ; i++) {
            landAnimal.get(i).crying();
        }
    }
}
```
- AnimalList 클래스의 선언부에 명시한 `'extends LandAnimal'` 구문을 생략해도 제대로 동작
- 제네릭 선언시, `AnimalList<LandAnimal> landAnimal = new AnimalList<>();`
- 코드 명확성을 위해, 위와 같이 타입 제한을 명시하는 것이 좋음.

### 제네릭 메소드
- 메소드 선언부에 타입 변수를 사용한 메소드를 의미
- 타입 변수 선언은 메소드 선언부에서 반환 타입 바로 앞에 위치
```java
public static <T> void sort( ... ) { ... }
```

#### 예제
```java
class AnimalList<T> {
    ...
    public static <T> void sort(List<T> list, Comparator<? super T> comp) {
        ...
    }
    ...
}
```
- **제네릭 클래스에서 정의된 타입변수 `T`, 제네릭 메소드에서 사용된 타입변수 `T`는 별개의 것**

### 와일드 카드 사용
- 이름에 제한을 두지 않음을 표현하는데 사용
- 자바의 제네릭에서는 `?` 기호를 사용하여 와일드카드 사용
```java
<?>           // 타입 변수에 모든 타입을 사용 o
<? extends T> // T 타입과 T 타입을 상속받는 자손 클래스 타입만을 사용 o
<? super T>   // T 타입과 T 타입이 상속받은 조상 클래스 타입만을 사용 o
```

#### 예제
```java
import java.util.*;

class LandAnimal {
    public void crying() {
        System.out.println("육지동물");
    }
}

class Cat extends LandAnimal {
    public void crying() {
        System.out.println("냐옹냐옹");
    }
}

class Dog extends LandAnimal {
    public void crying() {
        System.out.println("멍멍");
    }
}

class AnimalList<T> {
    ArrayList<T> al = new ArrayList<T>();
    public static void cryingAnimalList(AnimalList<? extends LandAnimal> al) {
        LandAnimal la = al.get(0);
        la.crying();
    }
    void add(T animal) { al.add(animal); }
    T get(int index) { return al.get(index); }
    boolean remove(T animal) { return al.remove(animal); }
    int size() { return al.size(); }
}

public class Generic03 {
    public static void main(String[] args) {
        AnimalList<Cat> catList = new AnimalList<Cat>();
        catList.add(new Cat());
        AnimalList<Dog> dogList = new AnimalList<Dog>();
        dogList.add(new Dog());
        AnimalList.cryingAnimalList(catList);
        AnimalList.cryingAnimalList(dogList);
    }
}
```


## Reference
[TCPSCHOOL.com](https://www.tcpschool.com/java/java_generic_concept)