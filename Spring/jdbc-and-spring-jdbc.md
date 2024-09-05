# 1. JDBC(Java Database Connectivity)

- 자바 애플리케이션이 데이터베이스와 상호작용할 수 있도록 해주는 API
- JDBC를 통해 데이터베이스에 연결하고, SQL 쿼리를 실행하며, 결과 처리

### 1.1. JDBC Architecture Model

1. **JDBC API**: 
    - 자바 애플리케이션이 데이터베이스와 상호작용할 수 있도록 하는 인터페이스 제공
    - `Connection`, `Statement`, `PreparedStatement`, `ResultSet` 등
2. **JDBC Driver Manager**: 
    - JDBC 드라이버를 관리하며, 데이터베이스 연결 설정
    - 드라이버를 로드하고, 필요한 드라이버를 찾아 `Connection` 객체 생성
3. **JDBC Driver**: 
    - 실제 데이터베이스와 통신을 담당하는 드라이버
    - 데이터베이스에 따라 다양한 드라이버가 사용

### 1.2. JDBC Driver Model

- JDBC 드라이버는 데이터베이스와의 연결을 처리, 드라이버의 종류에 따라 구현 방식이 다름
- 종류:
    - Type 1: JDBC-ODBC Bridge
    - Type 2: Native APT-Partly Java Driver
    - Type 3: Network Protocol-Fully Java Driver
    - Type 4: Thin Driver-Fully Java Driver

### 1.3. JDBC Flow

- JDBC를 사용하는 기본적인 흐름:
    1. **DriverManager를 통해 Connection 객체를 받아옴**
        - `DriverManager`를 사용하여 데이터베이스에 연결할 `Connection` 객체 획득
    2. **Connection을 통해서 Statement를 가져옴**
        - `Connection` 객체를 사용하여 `Statement` 또는 `PreparedStatement` 객체 생성
    3. **Statement를 통해서 쿼리를 실행 → ResultSet을 가져오거나 update 실행**
        - `Statement` 객체를 사용하여 SQL 쿼리를 실행. 쿼리 결과는 `ResultSet` 객체로 반환되거나, `executeUpdate()` 메서드를 통해 변경된 행의 수를 반환
    4. **Connection 종료**
        - 모든 작업이 완료되면, `Connection` 객체를 닫아 데이터베이스와의 연결 종료
            - `Statement` 및 `ResultSet` 객체도 함께 닫음

### 1.4. JDBC CURD 처리하기

- JDBC를 사용하여 데이터베이스에서 CURD 작업을 수행

### 1.4.1. 데이터베이스 연결

- JDBC를 통해 데이터베이스와 연결을 설정
    - `DriverManager`를 사용하여 `Connection` 객체를 얻음
    
    ```java
    // JDBC 드라이버 로드
    Class.forName("com.mysql.cj.jdbc.Driver");
    
    // 데이터베이스 연결
    Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydatabase", "username", "password");
    ```
    

### 1.4.2. Create (생성)

- `INSERT` SQL 쿼리를 사용
    - `Statement` 또는 `PreparedStatement` 객체를 통해 쿼리를 실행
    
    ```java
    String insertQuery = "INSERT INTO users (name, email) VALUES (?, ?)";
    PreparedStatement preparedStatement = connection.prepareStatement(insertQuery);
    preparedStatement.setString(1, "John Doe");
    preparedStatement.setString(2, "john.doe@example.com");
    int rowsAffected = preparedStatement.executeUpdate();
    ```
    
- `PreparedStatement`이 `Statement`보다 SQL 인젝션 공격에 더 안전
- `executeUpdate()`는 `INSERT`, `UPDATE`, `DELETE` 쿼리의 실행에 사용

### 1.4.3. Read (읽기)

- `SELECT` SQL 쿼리를 사용. `Statement` 또는 `PreparedStatement`를 사용하여 쿼리를 실행하고, `ResultSet`을 통해 결과를 처리
    
    ```java
    String selectQuery = "SELECT * FROM users WHERE email = ?";
    PreparedStatement preparedStatement = connection.prepareStatement(selectQuery);
    preparedStatement.setString(1, "john.doe@example.com");
    ResultSet resultSet = preparedStatement.executeQuery();
    
    while (resultSet.next()) {
        String name = resultSet.getString("name");
        String email = resultSet.getString("email");
        System.out.println("Name: " + name + ", Email: " + email);
    }
    ```
    

### 1.4.4. Update (수정)

- `UPDATE` SQL 쿼리를 사용. `PreparedStatement`를 통해 쿼리를 실행
    
    ```java
    String updateQuery = "UPDATE users SET name = ? WHERE email = ?";
    PreparedStatement preparedStatement = connection.prepareStatement(updateQuery);
    preparedStatement.setString(1, "John Smith");
    preparedStatement.setString(2, "john.doe@example.com");
    int rowsAffected = preparedStatement.executeUpdate();
    ```
    
- `executeUpdate()`는 수정된 행의 수를 반환

### 1.4.5. Delete (삭제)

- `DELETE` SQL 쿼리를 사용. `PreparedStatement`를 통해 쿼리를 실행, 삭제된 행의 수를 확인
    
    ```java
    String deleteQuery = "DELETE FROM users WHERE email = ?";
    PreparedStatement preparedStatement = connection.prepareStatement(deleteQuery);
    preparedStatement.setString(1, "john.doe@example.com");
    int rowsAffected = preparedStatement.executeUpdate();
    ```
    
- `executeUpdate()`는 삭제된 행의 수를 반환

# 2. Spring JDBC

- Spring JDBC는 자바 애플리케이션에서 데이터베이스와 상호작용할 때 발생할 수 있는 다양한 문제를 해결하고, JDBC API를 보다 쉽게 사용할 수 있도록 보조

### 2.1. DataSource

- `DataSource`는 데이터베이스 연결을 생성하고 관리하는 객체
- Spring에서는 `DataSource`를 통해 데이터베이스와의 연결을 관리
    - 다양한 데이터베이스 커넥션 풀 라이브러리와 통합할 수 있으며 DB 연결의 성능과 효율성을 높임
- **설정 예시**:
    
    ```java
    @Bean
    public DataSource dataSource() {
        return DataSourceBuilder.create()
                .url("jdbc:mysql://localhost:3306/mydatabase")
                .username("username")
                .password("password")
                .driverClassName("com.mysql.cj.jdbc.Driver")
                .build();
    }
    ```
    

### 2.2. Database Connection Pool (DBCP)

- `Database Connection Pool` (DBCP)는 데이터베이스 연결을 재사용하여 성능을 향상
- Spring은 여러 가지 커넥션 풀 구현체를 지원
- **Apache Commons DBCP** **설정 예시**:
    
    ```java
    @Bean
    public DataSource dataSource() {
        return new BasicDataSourceBuilder()
                .url("jdbc:mysql://localhost:3306/mydatabase")
                .username("username")
                .password("password")
                .driverClassName("com.mysql.cj.jdbc.Driver")
                .build();
    }
    ```
    

### 2.3. HikariCP

- HikariCP는 성능이 뛰어난 데이터베이스 커넥션 풀
    - Spring Boot의 기본 커넥션 풀, 매우 빠르고 안정적
- **설정 예시**:
    
    ```java
    @Bean
    public DataSource dataSource() {
        HikariDataSource dataSource = new HikariDataSource();
        dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/mydatabase");
        dataSource.setUsername("username");
        dataSource.setPassword("password");
        dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        return dataSource;
    }
    // 혹은 아래 -> HikariCP가 기본
    @Bean
    public DataSource dataSource() {
        return DataSourceBuilder.create()
                .url("jdbc:mysql://localhost:3306/mydatabase")
                .username("username")
                .password("password")
                .driverClassName("com.mysql.cj.jdbc.Driver")
                .build();
    }
    ```
    

### 2.4. JdbcTemplate

- Spring JDBC에서 데이터베이스 작업을 더 간편하게 수행할 수 있도록 도와주는 템플릿 클래스
    - SQL 쿼리 실행, 결과 집합 처리, 예외 처리 등을 간단하게 해준다.
- **주요 메서드**:
    - `query()`: 결과 집합을 `List`로 반환
    - `update()`: 데이터베이스 업데이트
    - `queryForObject()`: 단일 객체 반환
- **사용 예시**:
    
    ```java
    // DataSource Bean이 있어야함
    @Autowired
    private JdbcTemplate jdbcTemplate;
    
    public List<User> getUsers() {
        return jdbcTemplate.query("SELECT * FROM users", 
    		    new BeanPropertyRowMapper<>(User.class));
    }
    
    public int addUser(User user) {
        return jdbcTemplate.update(
    		    "INSERT INTO users (name, email) VALUES (?, ?)",                          
    				    user.getName(), user.getEmail());
    }
    ```
    

### 2.5. NamedParameterJdbcTemplate

- SQL 쿼리에서 이름 기반 파라미터를 사용하여 가독성을 높이고, 쿼리와 파라미터를 보다 명확하게 연결할 수 있도록 도와주는 클래스
- **주요 메서드**:
    - `query()`: 결과 집합을 `List`로 반환
    - `update()`: 데이터베이스 업데이트
    - `queryForObject()`: 단일 객체 반환
- **사용 예시**:
    
    ```java
    // DataSource Bean이 있어야함
    @Autowired
    private NamedParameterJdbcTemplate namedParameterJdbcTemplate;
    
    public List<User> getUsersByEmail(String email) {
        String sql = "SELECT * FROM users WHERE email = :email";
        Map<String, Object> params = new HashMap<>();
        params.put("email", email);
        return namedParameterJdbcTemplate.query(sql, params, new BeanPropertyRowMapper<>(User.class));
    }
    
    public int addUser(User user) {
        String sql = "INSERT INTO users (name, email) VALUES (:name, :email)";
        Map<String, Object> params = new HashMap<>();
        params.put("name", user.getName());
        params.put("email", user.getEmail());
        return namedParameterJdbcTemplate.update(sql, params);
    }
    ```
    

### 2.6. DataAccessException

- Spring Framework에서 데이터베이스와 관련된 예외를 처리하기 위해 제공하는 추상 클래스
- 데이터베이스 작업 중 발생할 수 있는 다양한 예외를 처리하기 위해 `DataAccessException`을 사용
- 특징:
    - **계층화된 예외 모델**:
        - `DataAccessException`은 다양한 구체적인 예외 클래스로 확장
        - 이러한 계층화된 구조를 통해 특정 데이터베이스나 오류 유형에 따라 적절한 예외 처리
    - **RuntimeException의 하위 클래스**:
        - `DataAccessException`은 `RuntimeException`을 상속받아 런타임 예외로 처리
        - 이는 예외 처리가 강제되지 않으며, 예외 처리를 직접 구현할 수 있음

### 2.7. Embedded Database

- 애플리케이션 내에 통합되어 배포되는 데이터베이스로, 별도의 데이터베이스 서버 없이 애플리케이션과 함께 실행
- 가볍고 설치가 용이하며, 자주 데이터베이스 서버를 설정하거나 관리하지 않아도 되는 장점
    - 주로 개발, 테스트, 또는 소규모 애플리케이션에서 사용
- **주요 장점**:
    - 설치와 설정이 간편
    - 경량화
    - 테스트와 개발에 적합
    - 비용 절감
- **주요 Embedded Database**:
    - **H2 Database**
        - Java로 작성된 오픈소스 데이터베이스로, 빠르고 간단하게 사용할 수 있으며 메모리 기반 또는 파일 기반으로 동작
        - 다양한 SQL 표준을 지원하고, 웹 기반 콘솔을 제공하여 데이터베이스를 관리
    - **설정 예시**:
        
        ```java
        @Bean
        public DataSource dataSource() {
            return new EmbeddedDatabaseBuilder()
                    .setType(EmbeddedDatabaseType.H2)
                    .addScript("schema.sql")
                    .addScript("data.sql")
                    .build();
        }
        ```
        

## 참고:

- https://www.javaguides.net/2018/10/java-jdbc-api-overview.html
- https://docs.oracle.com/javase/tutorial/jdbc/basics/sqldatasources.html
- https://www3.ntu.edu.sg/home/ehchua/programming/java/JDBC_Basic.html
- https://brownbears.tistory.com/289
- https://github.com/brettwooldridge/HikariCP
- https://www.baeldung.com/spring-jdbc-jdbctemplate
- https://docs.spring.io/spring-framework/reference/data-access/jdbc/embedded-database-support.html