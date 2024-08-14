# Servlet and JSP

- 서블릿(Servlet)과 JSP(JavaServer Pages)는 자바 기반의 웹 어플리케이션 개발에 사용되는 기술
- 서버 측에서 동적인 웹 콘텐츠를 생성

## 1. 서블릿(Servlet)

- 기능:
    - Java 클래스로 작성, 웹 서버에서 클라이언트의 요청을 처리하고 응답을 생성하는 역할
    - 요청을 수신하고, 비지니스 로직을 처리하며, 응답을 생성
- 작성 방식:
    - Java 코드만 포함된 클래스로 작성, HTML 코드는 Java 코드 내에 직접 삽입
    - 복잡한 HTML 및 비지니스 로직을 모두 자바 코드에서 작성할 경우
        - 코드가 길어지고 유지보수가 어려울 수 있음
- 응답 생성:
    - `HttpServletResponse`  객체를 사용하여 응답 작성
    - HTML 코드의 경우 `PrintWriter` 객체를 통해 직접 HTML 출력
- 사용 예:
    - 주로 비지니스 로직을 처리하고 데이터베이스와 상호 작용하는데 적합
- 예시 코드
    
    ```jsx
    import java.io.IOException;
    import javax.servlet.ServletException;
    import javax.servlet.annotation.WebServlet;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    
    @WebServlet("/hello")
    public class HelloServlet extends HttpServlet {
        @Override
        protected void doGet(HttpServletRequest request, HttpServletResponse response)
                throws ServletException, IOException {
            response.setContentType("text/html");
            response.getWriter().println("<h1>Hello, World!</h1>");
        }
    }
    ```
    

## 2. JSP(JavaServer Pages)

- 기능:
    - JSP는 HTML과 자바 코드를 혼합하여 동적인 웹 페이지 생성하는 데 사용
    - HTML 템플릿 내에 자바 코드를 삽입하여 동적인 콘텐츠 생성
- 작성 방식:
    - .jsp 확장자를 가지며, HTML 마크업 안에 자바 코드를 포함
    - JSP 파일은 서버에 의해 서블릿으로 컴파일되며, 자바 코드는 자동으로 서블릿 클래스의 `service()` 메세드에 삽입
- 응답 생성
    - JSP는 페이지 내에서 HTML을 직접 작성할 수 있고, `<% %>` 태그를 사용하여 자바 코드 삽입 가능
- 사용 예:
    - JSP는 주로 프레젠테이션 레이어(View)를 생성하는 데 사용
    - 서버 측 로직이 복잡할 경우 서블릿과 결합하여 사용
- 예제 코드
    
    ```jsx
    <%-- JSP 페이지의 시작 --%>
    <html>
    <head>
        <title>Hello JSP</title>
    </head>
    <body>
        <h1>Hello, World!</h1>
        <%
            String name = "JSP";
            out.println("<p>Hello, " + name + "!</p>");
        %>
    </body>
    </html>
    ```
    

## 3. MVC Pattern in Servlet

- MVC:
    - Model
        - 자바 객체 또는 POJO
    - View
        - 모델이 담고 있는 객체를 시각적으로 보여주는 역할 (JSP)
    - Controller
        - 모델과 뷰를 연결하기 위한 매개체, 사용자 요청에 따라 모델의 상태를 변경해주고 그에 따른 뷰를 업데이트(Filter/Servlet)
- Servlet 라이프 사이클
    - 주로 `init()`, `service()`, `destroy()` 메서드 포함
        - `init()`: 서블릿이 최초로 로드될 때 호출되며 초기화 작업 수행
        - `service()` : 클라이언트 요청이 들어올 때마다 호출되며, 요청을 처리하고 응답을 생성
            - HTTP 요청 메서드에 따라 `doGet(), doPost(), doPut(), and etc..` 메서드 호출
        - `destroy()` : 서블릿이 서버에서 제거되거나 서버가 종료될 때 호출되며 정리 작업을 수행
- Servlet 컨테이너
    - 서블릿은 서블릿 컨테이너(예: Apache Tomcat) 또는 서블릿 엔진 필요
    - 서블릿 컨테이너는 서블릿을 로드하고, 클라이언트 요청을 서블릿에 전달하며, 서블릿의 응답을 클라이언트에 반환하는 역할