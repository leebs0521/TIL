# Spring vs Spring Framework vs Spring Boot

## 1. Spring

- 자바 어플리케이션을 위한 포괄적인 프레임워크
- 스프링 자체는 여러 하위 프로젝트로 구성되어 있으며, 각각의 프로젝트는 특정 기능 제공
    - **Spring Framework**: 스프링의 핵심으로, IoC 컨테이너, AOP, 데이터 접근, 웹 어플리케이션 개발 등을 지원
    - **Spring Data, Spring Security, Spring Cloud and etc**: 다양한 기능을 제공하는 모듈화된 프로젝트

## 2. Spring Framework

- 스프링의 핵심 구성 요소, 자바 어플리케이션 개발을 위한 기본적인 기능 제공
- 주요 기능:
    - IoC 컨테이너: 객체의 생성과 관리, 의존성 주입 담당
    - AOP: 어플리케이션의 공통 관심사를 모듈화하고 코드 중복 제거
    - 데이터 접근: JDBC와 ORM(예: Hibernate, JPA) 통합을 통해 데이터베이스 접근 간소화
    - Spring MVC: 웹 어플리케이션 개발을 위한 모델-뷰-컨트롤러 패턴 제공
    - Spring WebFlux: 비동기 및 반응형 웹 어플리케이션을 위한 기능 지원
    - 리소스 핸들링, 벨리데이션, 타입 변환: 다양한 자바 어플리케이션의 기본적인 필요 지원

## 3. Spring Boot

- 스프링 어플리케이션의 개발과 배포를 간소화하기 위해 설계된 프레임 워크
- Spring framework 위에 구축되어 있으며 다음 기능 제공:
    - **Auto Configuration**: 어플리케이션의 설정을 자동으로 구성하여 개발자의 설정 작업을 줄임
    - **SpringApplication 클래스**: 어플리케이션 시작과 실행을 간소화
    - **외부 환경 설정**: Properties, YAML, CLI args 등을 통해 환경 설정을 쉽게 관리
    - **프로파일**: 개발, 테스트, 프로덕션 등 다양한 실행 환경을 관리할 수 있는 기능 제공
    - **패키징**: 독립 실행형 JAR 파일로 패키징하여 배포 및 실행을 간편하게 제공
    - **개발자 도구**: 자동 재시작, 리로드 기능 등 개발 시 유용한 도구 제공

## 정리

- **Spring**: 다양한 하위 프로젝트로 구성된 자바 어플리케이션 개발 프레임워크
- **Spring Framework**: Spring의 핵심 구성 요소로, IoC, AOP, 데이터 접근, 웹 개발 등을 지원
- **Spring Boot**: Spring Framework의 기능을 기반으로, 어플리케이션 개발과 배포를 간소화하는 도구로 자동 구성, 실행의 용이함, 패키징 기능 등을 제공