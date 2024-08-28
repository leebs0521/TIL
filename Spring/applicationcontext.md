# 1. ApplicationContext

- IoC 컨테이너는 객체에 대한 생성과 조합이 가능하게 하는 프레임워크
- 스프링에서는 IoC 컨테이너를 ApplicationContext 인터페이스로 제공
- 실제 ApplicationContext는 BeanFactory를 상속
    - 객체에 대한 생성, 조합, 의존관계설정 등을 제어하는 IoC 기본 기능을 BeanFactory 담당
    - Bean 이란?
        - IoC 컨테이너에 의해 관리되는 객체, @Bean 어노테이션이 붙은 클래스

## 1.1. BeanFactory와 ApplicationContext

### BeanFactory

- 스프링의 가장 기본적인 IoC 컨테이너
    - 객체의 생성과 의존성 주입을 관리

### ApplicationContext

- BeanFactory를 확장한 인터페이스
    - BeanFactory가 제공하는 기본 IoC에 더하여, 스프링 어플리케이션 개발에 유용한 기능을 제공
- 추가된 기능
    - Spring의 AOP 기능과의 더 쉬운 통합
    - 메세지 리소스 처리
    - 이벤트 게시
    - WebApplicationContext 웹 어플리케이션에서 사용하는 것과 같은 애플리케이션 계층별 컨텍스트

### BeanFactory vs ApplicationContext

| 기능 | BeanFactory | ApplicationContext |
| --- | --- | --- |
| Bean 인스턴스화/와이어링 | 가능 | 가능 |
| 통합 라이프사이클 관리 | 불가능 | 가능 |
| 자동 BeanPostProcessor 등록 | 불가능 | 가능 |
| 자동 BeanFactoryPostProcessor 등록 | 불가능 | 가능 |
| 편리한 MessageSource 접근(국제화 지원) | 불가능 | 가능 |
| 내장된 ApplicationEvent 발행 메커니즘 | 불가능 | 가능 |

## 1.2. Configuration Metadata

- 스프링의 `ApplicationContext`는 실제 만들어야할 빈 정보를 Configuration Metadata(설정 메타 데이터)에서 받아옴, 메타데이터를 통해 IoC 컨테이너에 의해 관리되는 객체를 생성하고 구성
- 작성 방법:
    - **XML 기반**: `GenericXmlApplicationContext` 구현체 사용
    - **Java 파일 기반**: `AnnotationConfigApplicationContext` 구현체 사용

### 1.2.1. XML 기반

- XML 설정 파일
    
    ```xml
    <!-- applicationContext.xml -->
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
                               http://www.springframework.org/schema/beans/spring-beans.xsd">
    
        <!-- VoucherRepository 빈 정의 -->
        <bean id="voucherRepository" class="com.example.JpaVoucherRepository"/>
    
        <!-- OrderRepository 빈 정의 -->
        <bean id="orderRepository" class="com.example.JpaOrderRepository"/>
    
        <!-- VoucherService 빈 정의 -->
        <bean id="voucherService" class="com.example.VoucherService">
            <constructor-arg ref="voucherRepository"/>
        </bean>
    
        <!-- OrderService 빈 정의 -->
        <bean id="orderService" class="com.example.OrderService">
            <constructor-arg ref="voucherService"/>
            <constructor-arg ref="orderRepository"/>
        </bean>
    </beans>
    ```
    
- 클라이언트 코드
    
    ```java
    public class Main {
    
      public static void main(String[] args) {
        ApplicationContext context = new GenericXmlApplicationContext("applicationContext.xml");
    
        OrderService orderService = context.getBean(OrderService.class);
      }
    }
    ```
    

### 1.2.2. Java 파일 기반

- AppConfig 클래스
    
    ```java
    @Configuration
    public class AppConfig {
    
      @Bean
      public VoucherRepository voucherRepository() {
        return new JpaVoucherRepository(); 
      }
    
      @Bean
      public OrderRepository orderRepository() {
        return new JpaOrderRepository();
      }
    
      @Bean
      public VoucherService voucherService() {
        return new VoucherService(voucherRepository());
      }
    
      @Bean
      public OrderService orderService() {
        return new OrderService(voucherService(), orderRepository());
      }
    }
    ```
    
- 클라이언트 코드
    
    ```java
    public class Main {
    
      public static void main(String[] args) {
        ApplicationContext ac = new AnnotationConfigApplicationContext(
            AppConfig.class);
    
        OrderService orderService = ac.getBean(OrderService.class);
      }
    }
    ```
    

### 1.2.3. XML 기반 설정 vs. Java 기반 설정

### XML 기반 설정

- 장점
    - **설정과 코드 분리**: 설정을 코드와 별도로 관리.
    - **표준화된 형식**: 많은 도구에서 지원.
- 단점
    - **타입 안전성 부족**: 컴파일 타임 오류가 아닌 런타임 오류 발생.
    - **디버깅 어려움**: 문제 추적이 복잡할 수 있음.
    - **편집 어려움**: XML 파일 유지보수가 어려울 수 있음.

### Java 기반 설정

- 장점
    - **타입 안전성**: 컴파일 타임에 오류 검출.
    - **코드 재사용성**: 설정을 코드로 관리, 유연성 높음.
    - **디버깅 용이**: 설정의 흐름을 코드에서 직접 확인.
- 단점
    - **설정과 코드 혼합**: 코드와 설정이 혼합될 수 있음.
    - **초기 설정 복잡**: 설정이 복잡할 수 있으며 학습 곡선 존재.
        - 스프링 부트 사용시 해결 → 스프링부트의 자동 설정

## 참고:

- https://docs.spring.io/spring-framework/reference/core/beans/beanfactory.html
- https://docs.spring.io/spring-framework/reference/core/beans/introduction.html