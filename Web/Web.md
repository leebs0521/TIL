# 0. WEB 이란

- 월드 와이드 웹(World Wide Web, WWW)
- 인터넷에 연결된 컴퓨터를 통해 사람들이 정보를 공유할 수 있는 전 세계적인 공간
- ≠ 인터넷
    - 웹은 이메일과 같이 인터넷 상에 동작하는 하나의 서비스
- HTTP 프로토콜, HTML 형식등을 사용하여 그림과 문자를 교환하는 전송 방식

## 1. WEB의 구성

- WEB은 다음과 같이 구성 된다:
    - HTTP: 어플리케이션 컨트롤
    - URI: 리소스 식별자
    - HTML: 하이퍼미디어 포맷
- HTTP ↔ URI: HTTP는 URI로 조작 대상을 지정
- HTML ↔ HTTP: HTML은 HTTP로 통신
- URI ↔ HTML: UTML의 링크는 URI를 이용

## 1.1. URI (Uniform Resource Identifier)

- 웹에서 자원을 식별하고 위치를 지정하는 데 사용되는 문자열
- 구성:
    - `[scheme]://[host]:[port]/[path]?[query]`
    - 예시 → http://kdt.programmers.com:8080/search?q=test&debug=true
        - **scheme**: http
        - **host**: kdt.programmers.com
        - **port**: 8080
        - **path**: search
        - **query**: q=test&debug=true
- URI에서 사용할 수 있는 문자
    - ASCII
        - 알파벳: A-Za-z
        - 숫자: 0-9
        - 기호: -.:~@!&`()

### 1.2. HTTP(HyperText Transfer Protocol)

- 웹에서 정보를 전송하는데 사용되는 프로토콜
    - 웹 브라우저와 웹 서버간의 데이터 교환을 관리하는 규칙과 표준을 정의
- 통신 과정:
    1. 클라이언트 요청
        - 사용자가 웹서버 요청
        - 웹 브라우저가 HTTP 요청을 서버 전달
            - 메서드, URL, 헤더 본문 포함
    2. 서버 응답
        - 서버는 요청을 처리하고 적절한 응답을 클라이언트에 반환
            - 상태코드, 응답헤더, 그리고 요청한 데이터 등이 포함
- 특징
    - TCP/IP 기반
    - 요청/응답형 프로토콜
    - 동기형 프로토콜
    - stateless → 각 요청은 독립적으로 처리 즉, 이전 요청의 정보를 기억하지 x (보완: 쿠키 or 세션)
- 메서드와 CRUD
    
    
    | CRUD | 의미 | 메서드 |
    | --- | --- | --- |
    | Create | 생성 | POST/PUT |
    | Read | 조회 | GET |
    | Update | 갱신 | PUT/UPDATE |
    | Delete | 삭제 | DELETE |

### 1.3. HTML(HyperText Markup Language)

- 웹 페이지를 구조화하고 콘텐츠를 정의하는 데 사용되는 마크업 언어
- 웹 브라우저가 렌더링하고 표시할 수 있도록하는 기본적인 구성 요소와 지침을 제공
- HTML 특징:
    - 구조화
    - 마크업
    - 요소와 속성
    - 계층 구조
    - 문서유형 정의