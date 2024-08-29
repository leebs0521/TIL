# 1. ComponentScan

- Spring이 특정 패키지 내에서 클래스를 검색하여 자동으로 Bean으로 등록하는 기능
- 설정 클래스에서 개별적으로 Bean을 등록할 필요 없이, Stereotype 어노테이션이 붙은 클래스를 자동으로 Bean으로 등록

### 1.1. 컴포넌트 스캔으로 빈 등록

- **`@ComponentScan`**
    - Stereotype 어노테이션을 통해 스캔할 클래스를 지정
        - **Stereotype**: 특정 역할을 수행하는 클래스를 나타내는 메타데이터
        - **종류**:
            - `@Repository`: 데이터 접근 계층
            - `@Component`: 일반적인 Bean
            - `@Controller`: 웹 컨트롤러
            - `@Service`: 서비스 계층
            - `@Configuration`: 설정 클래스
- **예시**:
    
    ```java
    @Configuration
    @ComponentScan // AppConfig 클래스가 위치한 패키지와 하위 패키지에서 스캔
    public class AppConfig {}
    
    @Repository
    public class MemoryVoucherRepository implements VoucherRepository {}
    
    @Service
    public class VoucherService {}
    ```
    
    - `AppConfig` 클래스가 위치한 패키지와 그 하위 패키지에서 컴포넌트 스캔
    - `@Repository`와 `@Service` 어노테이션으로 `MemoryVoucherRepository`와 `VoucherService`가 Bean으로 등록

### 1.2. @Autowired를 이용한 의존 관계 자동 주입

- **`@Autowired`**:
    - Spring에서 의존성 주입을 자동으로 처리
    - **사용 위치**:
        - **생성자 주입**: 생성자에 `@Autowired`
        - **필드 주입**: 필드에 `@Autowired`
        - **세터 메서드 주입**: 세터 메서드에 `@Autowired`
- **예시**:
    
    ```java
    // 생성자 주입
    @Service
    public class VoucherService {
        private final VoucherRepository repository;
    
        @Autowired
        public VoucherService(VoucherRepository repository) {
            this.repository = repository;
        }
    }
    
    // 필드 주입
    @Service
    public class VoucherService {
        @Autowired
        private VoucherRepository repository;
    }
    
    // 세터 메서드 주입
    @Service
    public class VoucherService {
        private VoucherRepository repository;
    
        @Autowired
        public void setRepository(VoucherRepository repository) {
            this.repository = repository;
        }
    }
    ```
    
- **주요 특징**:
    - **자동 탐색**: Spring 컨테이너에서 적절한 타입의 Bean을 자동으로 찾아서 주입
    - **타입 기반 주입**: Bean 타입에 따라 의존성을 주입
    - **기본 생성자 주입**: Spring 4.3 이상에서 생성자가 하나만 있을 경우 `@Autowired` 생략 가능
    - **선택적 주입**: `@Autowired(required = false)`로 의존성 주입을 선택적으로 수행

### 1.3. 같은 타입의 Bean이 여러 개일 경우

- **@Primary**
    - 기본적으로 주입될 Bean을 지정.
    
    ```java
    @Component
    @Primary
    public class JdbcVoucherRepository implements VoucherRepository {...}
    ```
    
- **@Qualifier**
    - 특정 Bean을 명시적으로 주입.
    
    ```java
    @Component
    @Qualifier("jdbc")
    public class JdbcVoucherRepository implements VoucherRepository {...}
    
    @Autowired
    @Qualifier("jdbc")
    private VoucherRepository jdbcVoucherRepository;
    ```
    
- **@Profile**
    - 특정 프로파일에서만 Bean을 활성화.
    
    ```java
    @Component
    @Profile("test")
    public class MemoryVoucherRepository implements VoucherRepository {...}
    ```
    

### 1.4. Bean Scope

- **singleton**: (기본값) 하나의 인스턴스만 생성.
- **prototype**: 요청할 때마다 새로운 인스턴스 생성.
- **request**: HTTP 요청당 하나의 인스턴스 생성.
- **session**: HTTP 세션당 하나의 인스턴스 생성.
- **application**: 웹 애플리케이션 당 하나의 인스턴스 생성.
- **websocket**: WebSocket 세션당 하나의 인스턴스 생성.

### 1.5. Bean LifeCycle Callbacks

- **생성 생명 주기 콜백**:
    1. `@PostConstruct` 메서드 호출
    2. `InitializingBean` 인터페이스의 `afterPropertiesSet` 메서드 호출
    3. `@Bean(initMethod = “”)`에 설정한 메서드 호출
- **소멸 생명 주기 콜백**:
    1. `@PreDestroy` 메서드 호출
    2. `DisposableBean` 인터페이스의 `destroy` 메서드 호출
    3. `@Bean(destroyMethod=””)`에 설정한 메서드 호출
- **예시**:
    
    ```java
    @Configuration
    @ComponentScan()
    public class AppConfig {
    
      @Bean(initMethod = "init", destroyMethod = "destroyMethod")
      public BeanOne beanOne() {
        return new BeanOne();
      }
    }
    
    class BeanOne implements InitializingBean, DisposableBean {
    
      @PostConstruct
      public void postConstruct() {
        System.out.println("[BeanOne] postConstruct called");
      }
    
      @Override
      public void afterPropertiesSet() throws Exception {
        System.out.println("[BeanOne] afterPropertiesSet called");
      }
    
      public void init() {
        System.out.println("[BeanOne] init called");
      }
    	
      @PreDestroy
      public void preDestroy() {
        System.out.println("[BeanOne] preDestroy called");
      }
      
      @Override
      public void destroy() throws Exception {
        System.out.println("[BeanOne] destroy called");
      }
    
      public void destroyMethod() {
        System.out.println("[BeanOne] destroyMethod called");
      }
    }
    ```
    
    ```java
    // 실행 결과
    [BeanOne] postConstruct called
    [BeanOne] afterPropertiesSet called
    [BeanOne] init called
    [BeanOne] preDestroy called
    [BeanOne] destroy called
    [BeanOne] destroyMethod called
    ```
    

## 참고:

- https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html