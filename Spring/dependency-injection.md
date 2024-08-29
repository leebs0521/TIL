# 1. DI(Dependency Injection)

- Dependency Injection은 객체가 자신이 의존하는 다른 객체들을 외부에서 주입 받는 방식
- DI는 제어의 역전(IoC, Inversion of Control)을 이루는 핵심 개념중 하나로, 객체가 스스로 의존성을 관리하지 않도록 한다.
- 결과적으로 코드가 깔끔해지고, 객체간의 결합도가 낮아져 재사용성과 테스트 용이성이 높아짐

## 1.1. 의존성 주입의 세가지 방식:

1. **생성자 기반 의존성 주입 (Constructor-based Dependency Injection)**:
    - 생성자를 통해 객체가 필요로 하는 의존성을 주입 → Spring 팀에서 권장하는 방식
    - **장점**:
        - 모든 의존성이 생성 시점에 주입되므로 객체가 불변(immutable) 상태로 유지
        - 필수 의존성을 강제할 수 있어, 객체가 완전한 상태로 생성
    - **사용 예**:
        
        ```java
        public class MyService {
            private final Dependency dependency;
        
            public MyService(Dependency dependency) {
                this.dependency = dependency;
            }
        }
        ```
        
2. **세터 기반 의존성 주입 (Setter-based Dependency Injection)**:
    - 세터 메서드를 통해 객체가 필요로 하는 의존성을 주입
    - **장점**:
        - 선택적 의존성을 주입하기에 적합하며, 나중에 의존성을 변경하거나 추가
        - 순환 의존성을 해결하는 데 도움
    - **사용 예**:
        
        ```java
        public class MyService {
            private Dependency dependency;
        
            public void setDependency(Dependency dependency) {
                this.dependency = dependency;
            }
        }
        ```
        
3. **필드 기반 의존성 주입 (Field-based Dependency Injection)**:
    - 필드에 직접 의존성을 주입. Spring에서는 보통 `@Autowired` 애노테이션을 사용
    - **장점**:
        - 코드가 간결하며, 필드에 바로 주입되기 때문에 작성이 쉽다.
    - **단점**:
        - 필드 주입은 의존성 주입이 클래스 내부에 숨겨져 있어, 테스트나 리팩토링 시 불리
        - 또한, 의존성을 변경하거나 주입 방식을 제어하기 어려움
    - **사용 예**:
        
        ```java
        public class MyService {
            @Autowired
            private Dependency dependency;
        }
        ```
        

## 1.2. Dependency Resolution Process

- Spring Framework에서 의존성 주입은 ApplicationContext 생성 및 초기화 과정에서 이루어진다.
- 이 과정은 다음과 같은 단계로 구성:
    1. **애플리케이션 컨텍스트 생성 및 초기화**
        - Spring은 XML, 자바 코드, 또는 애노테이션을 사용해 작성된 Configuration Metadata 기반으로 Application Context를  생성하고 초기화
        - Metadata는 애플리케이션에서 사용되는 모든 Bean을 정의
    2. **Bean의 의존성 정의**
        - 각 Bean은 생성 시 필요한 의존성을 속성(property), 생성자 인자(constructor argument), 또는 정적 팩토리 메서드의 인자 형태로 표현
        - Spring은 Bean을 실제로 생성할 때 이러한 의존성을 주입
    3. **의존성 주입**
        - 각 속성이나 생성자 인자는 실제로 설정할 값(value) 또는 컨테이너 내의 다른 Bean에 대한 참조(reference)로 정의
        - 이러한 참조는 Bean이 생성될 때 주입되어 Bean 간의 의존성이 해결
    4. **타입 변환**
        - 각 속성이나 생성자 인자에 대한 값이 지정된 형식에서 해당 속성이나 생성자 인자의 실제 타입으로 변환
        - 기본적으로 Spring은 문자열로 제공된 값을 int, long, String, boolean 등과 같은 기본 내장 타입으로 자동 변환

## 1.3. Circular Dependencies

- 주로 생성자 주입을 사용하는 경우 해결할수 없는 Circular dependencies가 발생할 수 있다.
    - Spring IoC 컨테이너는 `BeanCurrentlyInCreationException` 에러 발생 시킨다.
- Circular dependencies 발생 예: A → B의 인스턴스, B → A의 인스턴스 필요
    
    ```java
    @Configuration
    class CircularConfig{
    
      @Bean
      public A a(B b) {
        return new A(b);
      }
    
      @Bean
      public B b(A a) {
        return new B(a);
      }
    }
    ```
    
- 해결 방안:
    - 세터 의존성 주입 사용
    - @Lazy 어노테이션 사용: 실제로 빈을 필요로 할 때까지 초기화를 지연

## 참고:

- https://docs.spring.io/spring-framework/reference/core/beans/dependencies/factory-collaborators.html#beans-dependency-resolution