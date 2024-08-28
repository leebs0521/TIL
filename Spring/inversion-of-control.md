# 1. 제어의 역전(IoC, Inversion of Control)

- 객체의 생성 및 의존성 관리의 책임을 개발자가 아닌 외부 컨테이너 또는 프레임워크에 맡기는 방법
- 코드의 유연성과 확장성을 높일 수 있고, 특히 의존성 주입(Dependency Injection)과 함께 사용할 때 효과가 좋다.

## 1.1. 예제: 상품 관리 예제

- `OrderContext`:
    - `OrderService`, `VoucherService` 등의 객체 생성과 의존성을 관리
    
    ```java
    public class OrderContext {
    
      public VoucherRepository voucherRepository() {
        return new JpaVoucherRepository();
      }
    
      public OrderRepository orderRepository() {
        return new JpaOrderRepository();
      }
      
      public VoucherService voucherService() {
        return new VoucherService(voucherRepository());
      }
      
      public OrderService orderService() {
        return new OrderService(voucherService(), orderRepository());
      }
    }
    ```
    
- 객체 생성 및 의존성 관리
    - `OrderContext` 는 각 객체를 생성하는 역할
    - `voucherService()` 메서드는 `VoucherRepository` 를 생성하여 `VoucherService`에 주입
    - `orderService()` 메서드는 `VoucherService` 와 `OrderRepository` 를 생성하여 `OrderService`에 주입
- 제어의 역전
    - 일반적으로 객체는 스스로 필요한 의존성을 생성, 하지만 IoC에서는 이러한 제어가 외부로 넘겨진다.
        - 예시에서는 `OrderContext`
    - 제어의 역전을 통해 각 클래스는 자신의 의존성에 대해 알 필요가 없어지고, 코드 응집성이 높아진다.
- 유연성 증가
    - IoC를 사용하면 코드의 유연성 증가 → `OrderContext` 클래스에서 특정 개체의 생성 방식을 변경하려면 해당 메서드만 수정하면 된다.
        - 쉽게 다른 구현체로 교체하거나 테스트 환경에서 Mock 객체 주입 가능
            - `JpaVoucherRepository` → `InMemoryVoucherRepository`로 변경하고 싶을 경우
            - `voucherRepository()` 메서드에서 `new InMemoryVoucherRepository()`으로 변경

## 1.2. IoC 장점

- **유연성**: IoC는 객체의 생성과 의존성 관리를 외부에서 처리하므로, 코드를 더욱 유연하게 변경 가능
- **테스트** **용이성**: 의존성 주입 덕분에 Mock 객체나 다른 의존성을 쉽게 주입할 수 있어 단위 테스트 용이
- **모듈화**: 객체 간의 결합도를 낮추고, 각 모듈에서 독립적으로 동작하도록 할 수 있다.

## 1.3. 헐리우드 원칙(Hollywood Principle)

- Don't call us, we'll call you
- 이 원칙은 하위 모듈(객체)이 상위 모듈을 호출하지 않고, 상위 모듈이 필요할 때 하위 모듈을 호출하는 방식으로 제어 흐름을 관리
- **헐리우드 원칙 예시**: **OrderContext의 OrderService**
    - 하위 모듈(예: `VoucherService`, `orderRepository`)은 상위 모듈인 `OrderService`가 필요할 때 호출
    - 하위 모듈은 언제 호출될지 알 필요가 없으며, 상위 모듈이 필요할 때 호출되므로 코드의 결합도가 낮아지고 유연성이 증가

## 1.4. 스프링 프레임워크와 IoC

- 스프링 프레임워크는 IoC를 핵심 개념으로 사용하여 객체의 생성, 관리와 의존성 주입을 처리
- 스프링의 IoC 컨테이너는 `OrderContext` 와 유사한 역할 수행
    - `BeanFactory`, `ApplicationContext` 인터페이스