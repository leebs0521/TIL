# 1. Spring Test

### 1.1. 소프트웨어 테스팅

- 소프트웨어 테스팅은 소프트웨어 제품의 품질을 보장하고 이해관계자들에게 신뢰할 수 있는 정보를 제공하는 중요한 과정

### 1.2. 테스팅 레벨

- **Unit Testing (단위 테스트)**:
    - 프로그램의 기본 단위인 모듈을 독립적으로 테스트
    - 각 모듈의 기능이 명세서대로 구현되었는지 확인
- **Integration Testing (통합 테스트)**:
    - 여러 모듈이 함께 작동할 때의 상호작용을 테스트
    - 모듈 간의 데이터 흐름과 인터페이스를 검증
- **System Testing (시스템 테스트)**:
    - 시스템 전체를 테스트하여 요구 사항에 맞게 동작하는지 확인
    - 보통 전체 애플리케이션을 실제 환경과 유사한 환경에서 테스트
- **Acceptance Testing (인수 테스트)**:
    - 최종 사용자의 요구 사항과 기대에 맞게 시스템이 동작하는지 검증
    - 일반적으로 사용자 관점에서 애플리케이션을 테스트

### 1.2. 단위 테스트

- 단위 테스트는 소프트웨어의 개별 모듈(클래스, 메소드 등)이 올바르게 작동하는지 검증
    - 주로 개발자가 작성, 빠른 피드백을 제공
- **목표**: 모듈이 예상대로 동작하는지 확인
- **도구**: JUnit
- **예시**: 특정 메소드가 주어진 입력에 대해 올바른 출력을 반환하는지 테스트

### 1.3. 테스트 더블

- 테스트 더블은 테스트할 때 실제 의존성 대신 사용하는 객체
    - **Mock**: 특정 동작을 검증할 때 사용. 예를 들어, 특정 메소드 호출 여부나 호출 횟수를 검증
    - **Stub**: 상태 검증을 위해 사용. 특정 메소드가 호출되었을 때의 상태를 검증
    - **Spy**: 실제 객체를 감싸고, 특정 메소드 호출을 추적
    - **Fake**: 실제 구현체로, 단순하지만 실제 기능을 제공하는 객체

### 1.4. 통합 테스트 (Integration Test)

- 통합 테스트는 여러 모듈이나 컴포넌트가 함께 작동할 때의 동작을 검증
- 데이터베이스와 같은 외부 시스템과의 상호작용을 포함하여 시스템의 상호작용을 테스트
    - **목표**: 모듈 간의 상호작용과 시스템의 통합성을 검증
    - **도구**: Spring TestContext Framework 등

### 1.5. JUnit

- 자바 기반의 단위 테스트 프레임워크로, 테스트 클래스와 메소드에 대한 어노테이션을 제공하여 테스트 케이스를 작성하고 실행
- **기능**:
    - 테스트 클래스의 인스턴스를 매 단위 테스트마다 새로 생성하여 테스트의 독립성을 보장
    - 다양한 애노테이션을 사용하여 테스트 라이프 사이클을 관리하고 테스트 코드를 간결하게 작성
    - `assert` 메소드를 사용하여 테스트 케이스의 결과를 검증
        - 예: `assertEquals(expected, actual)`
    - 테스트 러너를 통해 IDE에서 테스트 코드를 쉽게 실행

### 1.5.1. JUnit 4

- 단일 JAR 파일로 배포되는 모노리틱 아키텍처
- **주요 특징**: `@Test`, `@Before`, `@After` 등 기본적인 테스트 어노테이션을 제공

### 1.5.2. JUnit 5

- **구성 요소**:
    - **JUnit Platform**: JVM에서 테스트를 실행하기 위한 기반을 제공. 다양한 TestEngine을 통해 테스트를 발견하고 실행
    - **JUnit Jupiter**: JUnit 5의 주요 모듈로, 테스트와 확장 기능을 위한 새로운 API를 제공
        - `@Test`, `@BeforeEach`, `@AfterEach` 등 새로운 어노테이션
    - **JUnit Vintage**: 기존 JUnit 3 및 4로 작성된 테스트 코드를 실행할 수 있는 레이어를 제공

### 1.6. Mock Object (모의 객체)

- 테스트 중에 실제 의존성 대신 사용되는 객체로, 테스트의 특정 시나리오를 시뮬레이션하고 검증하는 데 도움
    - **Mock**: 객체의 동작을 검증. 특정 메소드 호출 여부나 호출 횟수를 검증
    - **Stub**: 객체의 상태를 검증. 특정 메소드가 호출된 후 객체의 상태를 검증

### 1.6.1. Mock Object 생성을 도와주는 테스트 프레임워크

- **Mockito**: 강력하고 유연한 모킹 프레임워크로, 메소드 호출을 검증하고 모의 객체를 쉽게 생성
- **JMock**: 메소드 호출을 검증할 수 있는 모킹 프레임워크로, 동작 검증을 위해 설계
- **EasyMock**: 간단한 API를 통해 모의 객체를 생성하고 검증하는 기능을 제공

### 1.7. Spring의 JUnit 5 지원

- Spring은 JUnit 5와 통합되어 테스트 작성 및 실행을 지원
- **단위 테스트 지원**:
    - **Mock Objects**: Spring은 Mockito와 통합되어 Mock 객체를 쉽게 사용
    - **General Testing Utilities**: Spring의 다양한 유틸리티를 통해 단위 테스트를 지원
    - **Spring MVC Testing Utilities**: `MockMvc`를 사용하여 Spring MVC 컨트롤러의 동작을 테스트
- **통합 테스트 지원**:
    - **Spring TestContext Framework**: Spring의 통합 테스트를 지원하는 프레임워크로, 애플리케이션 컨텍스트를 로드하고 의존성을 주입하여 테스트를 수행
- **MockMvc**:
    - **설명**: Spring MVC 애플리케이션의 컨트롤러를 테스트할 때 사용. 실제 서버를 실행하지 않고, HTTP 요청과 응답을 시뮬레이션하여 테스트
    - **사용 예**:
        
        ```java
        @WebMvcTest(MyController.class)
        public class MyControllerTests {
            @Autowired
            private MockMvc mockMvc;
        
            @Test
            public void testHello() throws Exception {
                mockMvc.perform(get("/hello"))
                    .andExpect(status().isOk())
                    .andExpect(content().string("Hello, GET!"));
            }
        }
        ```
        

### 1.8. 테스트시 자주 사용하는 어노테이션

1. **@SpringBootTest**
    - **용도**: 전체 애플리케이션 컨텍스트를 로드하여 통합 테스트를 수행
    - **특징**: 모든 Bean을 로드, 실제 애플리케이션 환경과 유사하게 테스트
        
        ```java
        @SpringBootTest
        public class MyServiceIntegrationTest {
            @Autowired
            private MyService myService;
        
            @Test
            public void testServiceMethod() {
                // 테스트 코드
            }
        }
        ```
        
2.  **@WebMvcTest**
    - **용도**: Spring MVC의 웹 계층을 테스트할 때 사용. 컨트롤러에 관련된 컴포넌트만 로드
    - **특징**: 서비스나 리포지토리와 같은 다른 컴포넌트는 로드하지 않는다
        
        ```java
        @WebMvcTest(MyController.class)
        public class MyControllerTest {
            @Autowired
            private MockMvc mockMvc;
        
            @Test
            public void testControllerMethod() throws Exception {
                mockMvc.perform(get("/endpoint"))
                   .andExpect(status().isOk());
            }
        }
        ```
        
3. **@DataJpaTest**
    - **용도**: JPA 컴포넌트만 로드하여 데이터베이스와 관련된 테스트를 수행
    - **특징**: 데이터베이스와 관련된 설정만 로드, Spring MVC 컨트롤러나 서비스는 로드하지 않는다
        
        ```java
        @DataJpaTest
        public class MyRepositoryTest {
            @Autowired
            private MyRepository myRepository;
        
            @Test
            public void testFindById() {
                MyEntity entity = myRepository.findById(1L).orElse(null);
                assertNotNull(entity);
            }
        }
        ```
        
4. **@MockBean**
    - **용도**: ApplicationContext에 Mock Bean을 등록하여 실제 Bean 대신 Mock Bean을 사용
    - **특징**: 특정 Bean을 Mock 객체로 교체하여 테스트
        
        ```java
        @SpringBootTest
        public class MyServiceTest {
            @MockBean
            private MyRepository myRepository;
        
            @Autowired
            private MyService myService;
        
            @Test
            public void testServiceMethod() {
                when(myRepository.findById(1L)).thenReturn(Optional.of(new MyEntity()));
                MyEntity entity = myService.getEntityById(1L);
                assertNotNull(entity);
            }
        }
        ```
        
5. **@TestConfiguration**
    - **용도**: 테스트 전용의 Spring 설정을 정의할 때 사용
    - **특징**: 테스트 환경에서만 필요한 추가적인 Bean 또는 설정 정의
        
        ```java
        @TestConfiguration
        public class MyTestConfig {
            @Bean
            public MyService myService() {
                return new MyService();
            }
        }
        ```
        
6. **@Transactional**
    - **용도**: 테스트 메소드나 클래스에 트랜잭션을 적용하여 데이터베이스 상태를 롤백
    - **특징**: 테스트 후 데이터베이스 상태를 자동으로 초기화
        
        ```java
        @Transactional
        public class MyServiceTest {
            @Autowired
            private MyService myService;
        
            @Test
            public void testTransactionalMethod() {
                // 테스트 코드
            }
        }
        ```
        
7. **@BeforeEach / @AfterEach**
    - **용도**: 각각의 테스트 메소드 실행 전후에 특정 작업을 수행
    - **특징**: JUnit 5에서 제공되며, 초기화 작업이나 정리 작업을 처리
        
        ```java
        @BeforeEach
        public void setUp() {
            // 테스트 실행 전 초기화 작업
        }
        
        @AfterEach
        public void tearDown() {
            // 테스트 실행 후 정리 작업
        }
        ```
        
8. **@TestInstance**
    - **용도**: 테스트 클래스의 인스턴스 생성을 정의
    - **특징**: 테스트 클래스 인스턴스를 매번 새로 생성하거나, 하나의 인스턴스를 재사용
        
        ```java
        @TestInstance(TestInstance.Lifecycle.PER_CLASS)
        public class MyTests {
            @Test
            public void testMethod() {
                // 테스트 코드
            }
        }
        ```
        
9. **@DirtiesContext**
    - **용도**: 테스트 실행 후 애플리케이션 컨텍스트가 변경되었음을 선언
    - **특징**: 테스트가 컨텍스트를 변경한 경우, 이후 테스트에 영향을 미치지 않도록 컨텍스트를 재로드
        
        ```java
        @Test
        @DirtiesContext
        public void testContextChange() {
            // 컨텍스트 변경이 필요한 테스트
        }
        ```
        
10. **@TestExecutionListeners**
    - **용도**: 테스트 실행 과정에서 특정 작업을 수행할 수 있도록 하는 리스너를 등록
    - **특징**: 테스트 실행 전후에 특정 리스너를 추가하여 추가적인 처리
        
        ```java
        @TestExecutionListeners(listeners = CustomTestExecutionListener.class, mergeMode = MergeMode.MERGE)
        public class MyTests {
            @Test
            public void testMethod() {
                // 테스트 코드
            }
        }
        ```
        
11. **@ContextConfiguration**
    - **용도**: 테스트에 사용할 Spring 컨텍스트의 구성을 정의
    - **특징**: XML 또는 Java Config 클래스를 통해 컨텍스트를 설정
        
        ```java
        @ContextConfiguration(classes = MyTestConfig.class)
        public class MyTests {
            @Test
            public void testMethod() {
                // 테스트 코드
            }
        }
        ```
        

## 참고:

- https://en.wikipedia.org/wiki/Software_testing#Testing_levels
- https://martinfowler.com/articles/microservice-testing/#conclusion-summary
- https://docs.spring.io/spring-framework/reference/
- https://docs.spring.io/spring-framework/reference/testing/testcontext-framework.html