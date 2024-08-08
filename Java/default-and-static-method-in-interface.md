# 자바 인터페이스의 디폴트 메서드와 정적 메서드

## 0. 디폴트 메서드와 정적 메서드

- 인터페이스는 원래 추상 메서드만 사용 가능 했었음
- 자바 8부터 지원
- 인터페이스를 더 유용하고 강력하게 사용 가능

## 1. 디폴트 메서드 (Default Method)

### **1.1. 정의**

- 디폴트 메서드는 `default` 키워드를 사용하여 인터페이스에서 직접 구현을 제공하는 메서드
- 자바 7 이전에는 인터페이스에 메서드를 추가할 때, 이를 구현하는 모든 클래스에서 새로운 메서드를 구현이 강제
- 이는 코드의 유지보수성을 떨어뜨리고, 기존 구현체를 수정해야 하는 문제 발생

### **1.2. 필요성**

- **기존 코드 호환성**: 새로운 메서드를 추가할 때 기존의 구현체에 영향을 미치지 않도록 하기 위함.
- **코드 재사용**: 여러 클래스에서 공통된 기능을 제공하기 위해.
- **유연성 제공**: 인터페이스의 기본 구현을 제공하여 기존 클래스를 수정하지 않고도 새로운 기능을 추가할 수 있게 함.

### 1.3. 예시

### 1.3.1. 공통 기본 기능 제공

- 로깅 기능

```java
public interface Loggable {
	  // 추상 메서드: 로그 기록
    void log(String message);

    // 디폴트 메서드: 로그 메시지 포맷
    default void logError(String error) {
        log("ERROR: " + error);
    }

    default void logInfo(String info) {
        log("INFO: " + info);
    }
}

public class FileLogger implements Loggable {
    @Override
    public void log(String message) {
        // 파일에 로그를 기록하는 구현
        System.out.println("Logging to file: " + message);
    }
}

public class ConsoleLogger implements Loggable {
    @Override
    public void log(String message) {
        // 콘솔에 로그를 기록하는 구현
        System.out.println("Logging to console: " + message);
    }
}
```

- `logError()`, `logInfo()` 에 대한 코드 중복 제거
- `log()` : 구현 클래스에 원하는 방식으로 구현 가능

### 1.3.2 다중 인터페이스 상속 시 충돌 문제

- 여러 인터페이스에서 동일한 디폴트 메서드를 정의하는 경우
    - 구현 클래스에서 `오버라이딩`하여 모호성 해결

```java
public interface InterfaceA {
    default void display() {
        System.out.println("Display: InterfaceA");
    }
}

public interface InterfaceB {
    default void display() {
        System.out.println("Display: InterfaceB");
    }
}

public class ImplementingClass implements InterfaceA, InterfaceB {
    @Override
    public void display() {
        System.out.println("Display: ImplementingClass");
    }
}
```

### 1.4. 디폴트 메서드 정리

- 자바 인터페이스 기능을 확장하면서도 기존의 구현체와 호환성 유지 가능
    - 사용중인 인터페이스에 `default 메서드`가 추가되어도 구현 클래스에 문제가 발생하지 않는다.
- 공통 기능(메서드) 제공, 다중 인터페이스 상속에서 충돌 해결 가능

## 2. 정적 메서드 (Static Method)

### 2.1. 정의

- `static` 키워드를 사용하여 인터페이스에 정의된 메서드
- 인스턴스 없이 호출 → 유틸리티 메서드나 공통적인 기능을 제공할 때 유용
- 구현 클래스에서 `Overriding` 불가

### **2.2. 필요성**:

- **유틸리티 메서드 제공**: 인터페이스에서 공통적인 기능을 제공하기 위해.

### 2.3. 예시

**상황**: 컬렉션을 다루는 유틸리티 메서드를 제공하고자 할 때.

```java
import java.util.List;
import java.util.Collections;
import java.util.ArrayList;

public interface CollectionUtils {

		static void staticMethod(){
			System.out.println("CollectionUtils: method");
		}
		
    // 정적 메서드: 리스트를 역순으로 반환
    static <T> List<T> reverseList(List<T> list) {
        Collections.reverse(list);
        return list;
    }

    // 정적 메서드: 리스트의 최대값 찾기
    static <T extends Comparable<T>> T findMax(List<T> list) {
        return Collections.max(list);
    }
}
```

- 사실 `Collections` 클래스에 이미 정의된 정적 메소드를 호출 하는 것.
    - 유틸 인터페이스의 용도로만 사용
- 구현 클래스에서 `Overriding` 불가
    
    ```java
    import java.util.List;
    import java.util.Collections;
    import java.util.ArrayList;
    
    public class CollectionUtilsImpl implements CollectionUtils {
    		// -> 가능
    		// CollectionUtilsImpl.staticMethod() 호출
    		static void staticMethod(){
    			System.out.println("CollectionUtilsImpl: method"); 
    		}
    			
    			// -> 컴파일 에러
    //		@Overriding
    //		static void staticMethod(){
    //			System.out.println("CollectionUtilsImpl: method"); 
    //		}
    }
    ```
    

### 2.4. 정적 메서드 정리

- 인터페이스에서 정적 메서드는 인스턴스와 무관하게 호출, 유틸리티 메서드나 공통 기능을 제공하는 데 유용
- 구현 클래스에서는 정적 메서드를 오버라이드할 수 없다.

## **3. 인터페이스와 추상 클래스 비교**

- **인터페이스**:
    - 다중 상속 지원 (여러 인터페이스를 구현할 수 있음)
    - 모든 메서드는 기본적으로 추상적이지만, 디폴트 메서드와 정적 메서드를 통해 구현 가능
    - 상태를 가질 수 없으며, 필드는 상수로만 정의할 수 있음
    - 일반적으로 메서드의 시그니처만 제공하고, 클래스가 그 구현을 제공함
- **추상 클래스**:
    - 단일 상속만 지원 (하나의 추상 클래스만 상속 가능)
    - 인스턴스 필드와 생성자 지원
    - 추상 메서드와 구체적인 메서드(구현된 메서드)를 모두 포함할 수 있음
    - 상태를 가질 수 있으며, 자식 클래스에서 직접 상속받아 사용할 수 있음
  
## Reference

- https://www.digitalocean.com/community/tutorials/java-8-interface-changes-static-method-default-method
- https://javatechonline.com/static-methods-in-interface/
- https://docs.oracle.com/javase/specs/jls/se8/html/jls-9.html#jls-9.4
- [https://devbksheen.tistory.com/entry/디폴트-메서드default-method란](https://devbksheen.tistory.com/entry/%EB%94%94%ED%8F%B4%ED%8A%B8-%EB%A9%94%EC%84%9C%EB%93%9Cdefault-method%EB%9E%80)