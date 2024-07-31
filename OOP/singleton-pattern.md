# 싱글톤 패턴

# 0. 싱글톤

- 클래스에 인스턴스가 하나만 있도록 하면서 이 인스턴스에 대한 전역 접근 지점을 제공하는 생성 디자인 패턴

## 1. 장점

- 클래스에 인스턴스가 하나만 있도록 함
    - 인스턴스를 반환 받을 때, `이미 생성된 인스턴스가 있다면 그 인스턴스를 반환`
- 해당 인스턴스에 대한 `전역 접근 지점을 제공`
    - 싱글턴 패턴을 사용한 클래스는 프로그램 모든 곳에서 접근 가능

## 2. 단점

- 단일 책임 원칙(SRP) 위반
    - 위 장점의 2가지 책임을 가지기 때문이라고 합니다.
- 다중 스레드 환경에서 싱글톤 패턴을 위한 특별한 처리 필요
- 테스트의 어려움 존재

## 3. 적용 사례

- 데이터베이스 연결 클래스
    - 데이터베이스 연결을 담당하는 인스턴스는 하나만 존재해야함. (일관성을 위해)

## 3.1. 다이어 그램

![Singleton.png](../images/singleton-pattern.png)

### 3.2. 예제 코드

### 3.2.1 데이터베이스 연결 관리 클래스 (싱글톤 적용)

```java
public class DatabaseConnection {    
    private static volatile DatabaseConnection instance;
    
    // 기본 생성자
    // 외부에서 new 키워드로 생성하지 못하게 private로 제한
    private DatabaseConnection() {}

		// 싱글톤 패턴을 적용하여 전역 접근 가능
		// 이중 잠금을 통해 다중 스레드에서 안전하게 단일 인스턴스 보장
    public static DatabaseConnection getInstance() {
        if (instance == null) {
            synchronized (DatabaseConnection.class) {
                if (instance == null) {
                    instance = new DatabaseConnection();
                }
            }
        }
        return instance;
    }
    
    public void connect() {
        System.out.println("데이터베이스 연결...");
    }

    public void disconnect() {
        System.out.println("데이터베이스 연결 해제...");
    }
}
```

### 3.2.2 클라이언트 코드

```java
public class Main {
    public static void main(String[] args) {
        DatabaseConnection connection1 = DatabaseConnection.getInstance();
        DatabaseConnection connection2 = DatabaseConnection.getInstance();

        System.out.println(connection1 == connection2); // 실행 결과: true

        connection1.connect();
        connection1.disconnect();
    }

```

- `DatabaseConnection` 클래스의 생성자는 private 이기 때문에 외부에서 new 키워드로 객체 생성 불가
- `getInstance` 정적 메소드를 통해 인스턴스에 접근 가능하다.
    - `volatile` 키워드를 통해 instance는 메인 메모리에 저장되어 CPU 캐시가 쓰이지 않는다. → 다중 스레드 환경에서 instance 변수의 값을 정확하게 읽고 쓸 수 있도록 보장
    - 또한, `첫번째 검사 - 동기화 블록 - 두번째 검사` 를 통해 다중 스레드 환경에서 안전하게 인스턴스를 생성한다. → 이중 검사 잠금(Double-Checked Locking)