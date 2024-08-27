# 의존성(Dependency)

## 1. 의존성이란?

- 어떤 객체가 협력하기 위해 다른 객체를 필요로 할 때 두 객체 사이의 의존성이 존재
- 의존성은 실행 시점과 구현 시점에 서로 다른 의미를 가진다.
    - **컴파일타임 의존성**: 코드를 작성하는 시점에서 발생하는 의존성. 클래스 사이의 의존성
    - **런타임 의존성**: 애플리케이션이 실행되는 시점의 의존성. 객체 사이의 의존성

## 2. 예시: 상품 할인

### 2.1. 컴파일타임 의존성

- `Order`클래스는  `FixedAmountVoucher`클래스에 직접 의존 → 컴파일타임 의존성
    
    ```java
    public class Order {
      private final List<OrderItem> orderItems;
      private final FixedAmountVoucher fixedAmountVoucher;
    
      public long totalAmount() {
        Long beforeDiscount = orderItems.stream().map(v -> v.getProductPrice() * v.getQuantity())
            .reduce(0L, Long::sum);
        return fixedAmountVoucher.discount(beforeDiscount);
      }
    }
    ```
    
    ```java
    public class FixedAmountVoucher{
      private final long amount;
    
      public long discount(long beforeDiscount) {
        return beforeDiscount - amount;
      }
    }
    ```
    
- 문제점:
    - 할인 로직을 변경하고 싶다면? → `Order` 클래스의 코드를 수정해야 한다.
        - `Order`와 `FixedAmountVoucher`가 강하게 결합되어 있기 때문

### 2.2. 런타임 의존성

- `Order`는 `Voucher`인터페이스에 의존하도록 변경
    - 할인 로직을 `Voucher` 인터페이스의 구현체로 분리 가능
    
    ```java
    public class Order {
      private final List<OrderItem> orderItems;
      private final Voucher voucher;
    
      public long totalAmount() {
        Long beforeDiscount = orderItems.stream().map(v -> v.getProductPrice() * v.getQuantity())
            .reduce(0L, Long::sum);
        return voucher.discount(beforeDiscount);
      }
    }
    ```
    
    **Voucher 인터페이스와 구현체들**:
    
    ```java
    public interface Voucher {
        long discount(long beforeDiscount);
    }
    
    public class FixedAmountVoucher implements Voucher{
      private final long amount;
    
    	@Override
      public long discount(long beforeDiscount) {
        return beforeDiscount - amount;
      }
    }
    
    ---
    
    public class PercentDiscountVoucher implements Voucher {
      private final long percent;
    
      @Override
      public long discount(long beforeDiscount) {
        return beforeDiscount * (percent / 100);
      }
    }
    ```
    
- `Order` 클래스는 `Voucher` 인터페이스에만 의존하므로, 런타임에 `FixedAmountVoucher` 또는 `PercentDiscountVoucher` 주입 가능
- 할인 로직을 변경하고 싶을 경우 → `Order` 클래스를 수정할 필요가 없어진다.

### 2.3. 결합도

- 한 모듈이 다른 모듈에 얼마나 강하게 의존하는지 나타내는 지표
- 결합도가 낮을수록 모듈간의 의존성이 줄어들어 코드의 유연성과 재사용성이 높아진다.
    - 높은 결합도: 한 클래스가 특정 구현체에 직접 의존할 때 발생, 코드 수정시 다른 부분에 미치는 영향이 커지고, 유지보수가 어려워진다.
        - 예시에서 `Order` 클래스가 `FixedAmountVoucher`에 직접 의존하는 경우
    - 낮은 결합도: 클래스가 인터페이스 또는 추상 클래스에 의존하고, 실제 구현체는 런타임 시에 주입 받을 때 발생, 코드 유연성을 높이고, 수정 및 테스트가 용이해짐
        - 예시에서 `Order` 클래스가 `Voucher` 인터페이스에 의존하고 런타임에 구현체를 주입 받을 경우