# 1. Environment

- 스프링 프레임워크에서 `Environment`는 애플리케이션의 실행 환경을 나타냄
- 설정된 프로파일, 속성, 그리고 여러 환경 정보를 관리하고 제공
- `ApplicationContext`가 제공
    - `EnvironmentCapable` 인터페이스를 통해 접근
- `Environment`는 주로 애플리케이션의 속성(Property) 값을 관리하고, 현재 활성화된 프로파일(Profile)을 판별하며, 다양한 외부 자원을 로딩하는 데 사용

## 1.1 Properties

- 스프링 애플리케이션의 설정 정보는 주로 외부 파일에 저장되며, 이러한 파일은 `Environment`를 통해 애플리케이션에서 쉽게 접근
    - .properties, .yaml 파일
- **.properties 파일**:
    - `.properties` 파일은 키-값 쌍의 형식으로 애플리케이션의 설정 정보를 저장하는 데 사용
        - 예를 들어, 데이터베이스 연결 정보, 서버 포트 번호, 애플리케이션 모드 등을 정의
    - `@Value` 어노테이션을 사용하여 속성을 직접 주입하거나, `PropertySource` 어노테이션을 통해 특정 파일을 명시적으로 로드
    
    **예제**:
    
    - **`application.properties` 파일**:
        
        ```
        app.name=MyApplication
        app.version=1.0.0
        server.port=8080
        ```
        
    - **자바 클래스에서 `@Value` 사용 예**:
        
        ```java
        @Component
        public class AppConfig {
        
            @Value("${app.name}")
            private String appName;
        
            @Value("${server.port}")
            private int serverPort;
        
            // getters
        }
        ```
        
    - **`@PropertySource`를 통한 파일 로드 예**:
        
        ```java
        @Configuration
        @PropertySource("classpath:application.properties")
        public class AppConfig {
            // 이 설정으로 application.properties 파일을 로드
        }
        ```
        
- **.yaml 파일**:
    - YAML(YAML Ain’t Markup Language)은 데이터 표현 언어로, JSON이나 XML보다 가독성이 높고 계층 구조 표현이 용이
    - 스프링 부트는 기본적으로 `.yaml` 파일을 지원하며, 계층적 데이터를 더 쉽게 관리 가능
        - 특히 중첩된 속성이나 다수의 프로파일을 관리할 때 유용
        
        **예제**:
        
        - **`application.yml` 파일**:
            
            ```yaml
            app:
              name: MyApplication
              version: 1.0.0
            ```
            
- **스프링 부트의 자동 로딩**
    - 스프링 부트는 애플리케이션이 시작될 때 자동으로 다음과 같은 파일을 읽는다
        1. **`application.properties`** 파일
        2. **`application.yml(yaml)`** 파일

## 1.2 @ConfigurationProperties

- 스프링 부트는 외부 설정을 Bean으로 바인딩하여 관리할 수 있는 기능을 제공
    - `@ConfigurationProperties` 어노테이션을 통해 구현
- **@ConfigurationProperties**:
    - 이 어노테이션을 사용하면 특정 그룹의 속성을 자바 객체에 자동으로 바인딩
    - 예를 들어, `application.properties` 또는 `application.yaml` 파일에 정의된 특정 속성 그룹을 클래스로 매핑하여, 해당 속성을 Bean으로 등록
    - `@ConfigurationProperties`로 정의된 클래스는 일반적으로 `@EnableConfigurationProperties` 또는 자동 구성 메커니즘을 통해 활성화
- **예시**:
    
    ```java
    @ConfigurationProperties(prefix = "app")
    public class AppConfig {
        private String name;
        private String version;
    
        // getters and setters
    }
    ```
    
    - `application.properties` 파일의 `app.name`과 `app.version` 속성을 자동으로 바인딩

## 1.3 Spring Profile

- 애플리케이션 설정의 일부를 분리하여 특정 환경(예: 개발, 테스트, 운영)에서만 적용
    - 이를 통해 동일한 코드베이스에서 환경별로 다른 설정을 손쉽게 적용
- **Spring Profile**:
    - 애플리케이션이 특정 프로파일에서만 로드되도록 설정
        - 예를 들어, `@Profile("dev")` 어노테이션을 사용하여, 해당 클래스나 메서드는 개발 환경에서만 활성화
    - `application-dev.properties`나 `application-prod.yaml`과 같은 파일을 사용하여 환경별로 구분된 설정을 제공할 수 있으며, `spring.profiles.active` 속성을 통해 활성화할 프로파일을 지정
    - 프로파일은 설정 파일뿐만 아니라 자바 코드에서도 사용될 수 있어, 환경에 따라 특정 Bean이나 서비스의 동작을 다르게 설정
    - **예시**:
        
        ```java
        @Configuration
        @Profile("dev")
        public class DevConfiguration {
            // 개발 환경에 특화된 빈 정의
        }
        ```
        
        - 위의 설정은 `dev` 프로파일이 활성화된 경우에만 해당 Bean이 로드

## 1.4 리소스

- 파일 시스템, 클래스패스, URL 등을 통해 접근할 수 있는 다양한 외부 파일
    - 예를 들어, 이미지 파일, 텍스트 파일, 또는 암복호화를 위한 키 파일 등
- **Resource 인터페이스**:
    - 스프링은 외부 리소스를 관리하기 위해 `Resource` 인터페이스를 제공
        - 파일 시스템, 클래스패스, URL 등의 다양한 위치에 있는 리소스에 접근
    - `Resource` 인터페이스는 리소스를 일관되게 다룰 수 있는 메서드를 제공하여, 리소스의 위치에 관계없이 동일한 방식으로 접근 가능
    - **Resource 구현체**:
        - UrlResource: URL을 통해 리소스 읽기
        - ClassPathResource: 클래스패스에서 리소스 읽기
        - FileSystemResource: 파일 시스템에서 리소스 읽기
        - PathResource: `java.nio.file.Path`로 리소스 읽기
        - ServletContextResource: 서블릿 컨텍스트에서 리소스 읽기
        - InputStreamResource: `InputStream`으로 리소스 읽기
        - ByteArrayResource: 바이트 배열로 리소스 읽기
- **ResourceLoader 인터페이스**:
    - `ResourceLoader`는 `Resource` 객체를 로드하는 데 사용되는 인터페이스
    - `ApplicationContext`는 `ResourceLoader`를 구현하고 있으며, 이를 통해 스프링 컨테이너 내에서 리소스를 쉽게 로드
    - `getResource()` 메서드를 통해 다양한 타입의 리소스를 로드할 수 있으며, 리소스의 타입은 접두어(예: `classpath:`, `file:`, `http:`)로 결정
- **예시**:
    
    ```java
    @Resource
    private ResourceLoader resourceLoader;
    
    public void loadResource() {
        Resource resource = resourceLoader.getResource("classpath:data/file.txt");
        // 리소스 읽기 작업 수행
    }
    ```
    
    - 위 코드에서 `ResourceLoader`를 사용하여 클래스패스에 위치한 텍스트 파일을 읽을 수 있다

## 참고

- https://docs.spring.io/spring-framework/reference/core/beans/environment.html#beans-property-source-abstraction
- [Spring Framework Documentation - Resources](https://docs.spring.io/spring-framework/reference/core/resources.html#resources-implementations)