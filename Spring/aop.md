# 1. AOP(Aspect Orient Programming)

- 횡단 관심사(Cross-cutting concern)의 분리를 허용함으로써 모듈화를 통해 코드 유지보수를 쉽게하는  프로그래밍 패러다임
    - 기존의 코드에 부가적인 동작(어드바이스, advice)을 삽입
        - 어느 코드가 포인트컷(pointcut) 사양을 통해 수정되는지 지정
    - 비지니스 로직에 핵심적이지 않는 동작을 프로그램에 추가 가능

### 1.1. 횡단 관심사(Cross Cutting Concern)

- 어플리케이션에는 주요 비즈니스 로직 이외의 처리해야할 여러 문제가 존재
- 이러한 문제는 어플리케이션의 여러 계층과 모듈에 걸쳐 분산
    - 예) 로깅, 트랜잭션 처리, 성능 모니터링, 보안등
- AOP는 어플리케이션의 횡단 관심사를 구현하고 주요 비지니스 로직과 분리
    - 느슨하게 결합된 어플리케이션을 만드는데 도움

### 1.2. AOP 적용 방법(Weaving)

- **컴파일 시점**: AOP 프레임워크가 코드가 컴파일될 때 어드바이스를 적용
- **클래스 로딩 시점**: 클래스가 JVM에 의해 로드될 때 어드바이스를 적용
- **런타임 시점**: 어드바이스가 런타임에 동적으로 적용

### 1.3. Spring AOP

- 프록시 기반으로 AOP를 제공
    - JDK 동적 프록시: 인터페이스 기반으로 프록시 생성
    - CGLIB 프록시: 클래스 기반으로 프록시 생성

### 1.4. @AspectJ support

- AOP 주요 용어:
    - 타겟(Target)
        - 핵심 기능을 담고 있는 모듈로서 부가 기능을 부여할 대상
    - 조인포인트(Joinpoint)
        - 어드바이스가 적용될 수 있는 위치
        - 타겟 객체가 구현한 인터페이스의 모든 메서드
    - 포인트컷(Pointcut)
        - 어드바이스를 적용할 타겟의 메서드를 선별하는 정규표현식
        - 포인트컷 표현식은 `execution`으로 시작, 메서드의 시그니처를 비교하는 방식
    - 애스팩트(Aspect)
        - 어드바이스 + 포인트컷
        - Spring에서는 Aspect를 Bean으로 등록해서 사용
    - 어드바이스(Advice)
        - 타겟의 특정 조인포인트에서 제공할 부가 기능
        - @Before, @After, @Around, @AfterRetruning, @AfterThrowing 등
    - 위빙(Weaving)
        - 타겟의 조인포인트에 어드바이스를 적용하는 과정
- 예제 코드
    
    ```java
    @Aspect
    @Component
    public class LoggingAspect {
    
        @Before("execution(* com.example.service.UserService.addUser(..))")
        public void logBefore(JoinPoint joinPoint) {
            System.out.println("Before executing method: " + joinPoint.getSignature().getName());
        }
    
        @After("execution(* com.example.service.UserService.addUser(..))")
        public void logAfter(JoinPoint joinPoint) {
            System.out.println("After executing method: " + joinPoint.getSignature().getName());
        }
    }
    ```
    

## 참고:

- https://docs.spring.io/spring-framework/docs/5.2.9.RELEASE/spring-framework-reference/core.html#aop