# 1. 로깅(Logging)

- 시스템을 작동할 때, 시스템의 작동 상태의 기록과 보존, 이용자의 습성 조사 및 시스템 동작의 분석 등을 하기 위해 작동중의 각종 정보를 기록해둘 필요가 있다.
- 이 기록을 만드는 것을 로깅이라고 한다. 즉 로그 시스템의 사용에 관계된 일련의 사건을 시간 경과에 따라 기록하는 것.

### 1.1. Java Logging Framework

- **java.util.logging**: JDK에 포함된 기본 로깅 API. 가볍지만 기능이 제한적
- **Apache Commons Logging**: 다양한 로깅 프레임워크와 호환이 가능하지만, 설정이 복잡할 수 있음
- **Log4j**: 높은 성능과 유연한 설정을 제공하지만, 최근 보안 이슈(Log4Shell)로 인해 주의가 필요
- **logback**: Log4j의 창시자가 만든 차세대 로깅 프레임워크로, SLF4J와 자연스럽게 통합 가능
- **SLF4J**: 다양한 로깅 프레임워크를 추상화한 인터페이스로, 로깅 구현체를 쉽게 교체 가능

### 1.2. SLF4J(Simple Logging Facade for Java)

- Logging Framework들을 추상화해 놓은 것, Facade Pattern을 이용한 Logging Framework
    - Facade Pattern: 많은 서브 시스템을 거대한 클래스로 만들어 감싸서 편리한 인터페이스 제공
- **SLF4J의 주요 이점**:
    - 로깅 프레임워크에 대한 종속성 감소: 코드를 변경하지 않고도 다양한 로깅 프레임워크로 전환이 가능
    - 통일된 인터페이스 제공: 다양한 로깅 프레임워크와의 호환성을 제공하며, 개발자가 특정 로깅 프레임워크에 의존하지 않게 함
- **바인딩 모듈**:
    - **logback-classic(logback)**: 성능이 우수하고, 다양한 설정 옵션을 제공하는 logback과의 연결 모듈
    - **slf4j-log4j12**: SLF4J와 Log4j 1.x 버전을 연결하는 모듈
    - **log4j-slf4j-impl**: SLF4J와 Log4j 2.x 버전을 연결하는 모듈
- **로그 레벨:**
    - **trace**: 가장 상세한 정보로, 메서드의 진입/퇴출, 루프의 반복 등을 추적할 때 사용
    - **debug**: 디버깅을 목적으로 중요한 변수의 값이나 상태를 기록할 때 사용
    - **info**: 시스템의 정상 동작을 알리는 일반적인 정보를 기록할 때 사용
    - **warn**: 시스템이 동작 중이지만 주의가 필요한 상태나 예외 상황을 기록할 때 사용
    - **error**: 시스템의 중요한 오류나 예외를 기록하여 관리자에게 알릴 때 사용
- **Logger의 생성 방법**:
    - `LoggerFactory.getLogger(ClassName.class)`: 클래스 이름을 기반으로 Logger 인스턴스를 생성하여 사용
        
        ```java
        public class MyClass {
        		private final Logger log = LoggerFactory.getLogger(MyClass.class);
            public void doSomething() {
                log.info("This is an info log.");
            }
        }
        ```
        
    - Lombok의 @Slf4j
        
        ```java
        @Slf4j
        public class MyClass {
            public void doSomething() {
                log.info("This is an info log.");
            }
        }
        ```
        
- **logback 설정파일 찾는 순서**:
    - `logback-test.xml`: 테스트용 설정 파일
    - `logback.groovy`: Groovy 스크립트로 작성된 설정 파일
    - `logback.xml`: 일반적으로 사용되는 XML 형식의 설정 파일
    - **기본 전략(BasicConfiguration)**: 설정 파일이 없을 경우 기본적으로 사용되는 설정
- **로그 Appender:**
    - ConsoleAppender: 로그 메세지를 콘솔에 출력
    - FileAppender: 로그 메세지를 파일에 기록
    - RollingFileAppender: 일정 크기 또는 기간이 지나면 새로운 파일로 로그를 롤링
- **PatternLayout:**
    - 로그 메시지의 형식을 정의하는 데 사용
        - `[%-5level] %d{yyyy-MM-dd HH:mm:ss} - %msg%n`는 로그 레벨, 날짜 및 시간, 메시지를 포함한 형식을 정의
    - 주요 패턴 요소:
        - `%d{}`: 로그의 시간/날짜 포맷
        - `%level`: 로그 레벨 (INFO, DEBUG 등)
        - `%msg`: 로그 메시지
        - `%logger`: 로그를 호출한 클래스의 이름
        - `%thread`: 현재 실행 중인 스레드의 이름

## 1.2.3 로그 설정 파일

- logback.xml (복잡한 설정시)
    
    ```xml
    <configuration>
    	
        <!-- 콘솔에 로그 출력 -->
        <appender name="console" class="ch.qos.logback.core.ConsoleAppender">
            <encoder>
                <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
            </encoder>
        </appender>
    
        <!-- 롤링 파일에 로그 저장 (시간 기반) -->
        <appender name="timeBasedRollingFileAppender" class="ch.qos.logback.core.rolling.RollingFileAppender">
            <file>logs/current.log</file> <!-- 현재 로그 파일 경로 -->
            
            <!-- 롤링 정책 정의 (시간 기반 롤링) -->
            <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
                <fileNamePattern>logs/app-time-rolling.%d{yyyy-MM-dd}.log</fileNamePattern> <!-- 일별 롤링 -->
                <maxHistory>30</maxHistory> <!-- 30일 동안의 로그 파일 보관 -->
            </rollingPolicy>
            
            <encoder>
                <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
            </encoder>
        </appender>
    
        <!-- 루트 로거 설정 -->
        <root level="info">
            <appender-ref ref="console" />
            <appender-ref ref="timeBasedRollingFileAppender" />
        </root>
    
    </configuration>
    ```
    
- Spring 설정 파일로도 가능 (간단한 설정만)
    - .yaml, .properties
- **우선순위**: `logback.xml` > `application.yml` 또는 `application.properties`

## 참고:

- https://logback.qos.ch/manual/configuration.html
- https://logback.qos.ch/manual/layouts.html
- https://docs.spring.io/spring-boot/reference/features/logging.html