# Maven and Gradle

## 0. Build Tool

- `Maven`과 `Gradle`은 Java와 기타 JVM 언어에서 사용되는 필드 도구
- 다음과 같은 작업 관리:
    - **의존성 관리**: 필요한 라이브러리 다운로드 및 관리
    - **컴파일**: 소스 코드를 바이트 코드로 컴파일
    - **테스트**: 단위 테스트 및 기타 테스트 실행
    - **패키징**: 컴파일된 코드를 JAR, WAR, ZIP 등의 아티팩트로 패키징
    - **배포**: 패키징된 아티팩트를 레포지토리나 서버로 배포

## 1. Maven

- XML 설정을 사용하여 프로젝트를 관리
- pom.xml 파일로 작성할 수 있다.
    - POM = project Object model

### 1.1 Maven 사용 이유?

- archetypes 라는 프로젝트 템플릿을 제공하여 같은 설정을 반복하지 않게 한다.
- 사용하는 외부 라이브러리인 dependency 관리
- 플러그인과 외부 라이브러리를 분리하여 관리
- dependency → 로컬 레포지토리 or 공개 레포지토리

### 1.2. Maven coordinates

- Maven 프로젝트를 식별하는데 사용
    - groupId - 주로 회사나 단체명, ex) org.programmers
    - artifactId - 주로 프로젝트 명, ex) spring-test
    - version - 프로젝트 버전, ex) 1.0-SNAPSHOT

### 1.3. Multiple Module

- Maven은 multiple module 지원 → 하나의 프로젝트에 여러 프로젝트 관리 가능
    
    ```xml
    <modules>
    <module>service-a</module>
    <module>service-b</module>
    </modules>
    ```
    

### 1.4. build lifecycle

- 빌드 프로세스를 관리하기 위해 정의된 라이프사이클을 사용:
    - **validate**: 프로젝트의 구조와 설정을 검증
    - **complie**: 소스 코드를 컴파일
    - **test**: 테스트 실행
    - **package**: 컴파일된 코드를 패키징
    - **verify**: 추가 검증 수행
    - **install**: 패키지를 로컬 레포지토리에 설치
    - **deploy**: 패키지를 원격 레포지토리에 배포

### 1.5. Maven CLI

```bash
# mvn <lifecycle>
mvn complie

mvn clean package
```

### 1.6. 의존 범위(Dependency Scope)

- 의존성의 가시성 정의:
    - **compile**: 기본 범위로, 모든 클래스패스에서 사용 가능
    - **provided**: 컴파일 시에만 사용되고 패키징된 아티팩트에는 포함 x
    - **runtime**: 런타임 시에만 사용
    - **test**: 테스트 시에만 사용
    - **system**: provided와 유사하지만 명시적 경로 필요

## 2. Gradle

- Groovy 또는 Kotlin DSL을 사용하여 빌드 스크립트를 작성하는 유연한 빌드 도구
- build.gradle 파일로 작성

### 2.1 Artifacts Coordinates

- Maven Coordinates와 같은 개념
- 설정 유연성이 더 크다.

### 2.2. Project & Task

- Gradle Build는 하나 이상의 프로젝트 지원
    - Maven의 Multiple Module과 비슷
- 하나의 프로젝트는 하나 이상의 Task로 구성
    - Task: 클래스를 컴파일하거나 Jar를 생성하거나 하는 Build를 위한 작업
        - 일반적으로 Task는 plugins에서 제공하는 Task 사용

### 2.3. Plugin

- Gradle에서 실제 Task와 주요한 기능을 추가하게하는 것
- 하나의 프로젝트에 여러 플러그인 추가 가능