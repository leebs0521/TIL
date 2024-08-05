# 동작 파라미터화(Behavior Parameterization)

## 0. 동작 파라미터화(Behavior Parameterization)

- 동작의 파라미터화
    - 여러가지 메소드를 메소드의 인자로 받아 내부적으로 사용하여 작업을 수행하는 기능
    - 행위의 매개변수화라고도 한다.
- 장점
    - 동적으로 다양한 동작을 메서드에 전달 가능
    - 코드의 유연성을 높이고 재사용성을 높인다.
- 구현 방법
    - 함수형 인터페이스, 람다 표현식, 익명 클래스, 스트림 API 등

## 1. 예제 - 사과 인벤토리에서 특정 조건으로 사과 필터링

## 1.1. 필터링 조건 - 사과의 색상

- filterApples 메소드 내부의 if문에 직접 명시

```java
enum Color { 
	RED, GREEN
}

public List<Apple> filterApples(List<Apple> inventory){
	List<Apple> res = new ArrayList<>();
	for (Apple apple : inventory) {
	  // 필터링 조건을 직접 명시
		if(GREEN.equals(apple.getColor()){
			res.add(apple);
		}
	}
```

- filterApples 메소드의 파라미터에 명시

```java
public List<Apple> filterApples(List<Apple> inventory, Color color){
	List<Apple> res = new ArrayList<>();
	for (Apple apple : inventory) {
	  // 필터링 조건의 인자로 받아옴
		if(color.equals(apple.getColor()){
			res.add(apple);
		}
	}
```

## 1.2. 필터링 조건이 추가된다면 - 사과의 무게추가

- filterApples 메소드의 파라미터에 명시

```java
public List<Apple> filterApples(List<Apple> inventory, Color color, int weight){
	List<Apple> res = new ArrayList<>();
	for (Apple apple : inventory) {
	  // 필터링 조건의 인자로 받아옴
		if(color.equals(apple.getColor() && apple.getWeight() >= weight){
			res.add(apple);
		}
	}
```

## 1.3. 동작의 파라미터화

- 기존 코드의 문제
    - 필터링 조건이 추가될 때마다 기존 filterApples 코드를 수정해야한다.
    - 코드의 유지보수가 어렵고, 반복적인 코드를 작성이 필요함
- 동작의 파라미터화 적용
    - 필터링을 수행하는 메서드를 파라미터로 전달 받도록 함

## 1.4.1. 필터링 메서드

```java
// 동작의 파라미터화: ApplePredicate를 사용하여 필터링 조건을 파라미터로 전달받음
public List<Apple> filterApples(List<Apple> inventory, ApplePredicate predicate) {
  List<Apple> result = new ArrayList<>();
  for (Apple apple : inventory) {
      if (predicate.test(apple)) {
          result.add(apple);
      }
  }
  return result;
}
```

## 1.4.2 함수형 인터페이스 정의

```java
@FunctionalInterface
interface ApplePredicate {
    boolean test(Apple apple);
}
```

## 1.4.3. 함수형 인터페이스 구현 클래스

```java
public class RedApplePredicate implements ApplePredicate {
    @Override
    public boolean test(Apple apple) {
        return Color.RED.equals(apple.getColor());
    }
}
```

## 1.4.4. 함수형 인터페이스 구현 클래스 사용

```java
public static void main(String[] args) {
	// 사과 인벤토리 생성
	List<Apple> inventory = List.of(
		new Apple(Color.RED),
		new Apple(Color.GREEN),
		new Apple(Color.RED),
		new Apple(Color.GREEN)
	);
	
	// 함수형 인터페이스 구현 클래스 사용
	ApplePredicate redApplePredicate = new RedApplePredicate();
	
	// 필터링 조건을 적용하여 사과 목록 필터링
	List<Apple> redApples = filterApples(inventory, redApplePredicate);
	
}

```

## 1.4.5. 익명 클래스 사용

```java
public static void main(String[] args) {
	// 사과 인벤토리 생성
	List<Apple> inventory = List.of(
		new Apple(Color.RED),
		new Apple(Color.GREEN),
		new Apple(Color.RED),
		new Apple(Color.GREEN)
	);
	
  // 익명 클래스를 사용하여 필터링 조건 정의
	ApplePredicate predicate = new ApplePredicate() {
		@Override
		public boolean test(Apple apple) {
			return Color.RED.equals(apple.getColor());
		}
	};

  // 필터링 조건을 적용하여 사과 목록 필터링
	List<Apple> redApples = filterApples(inventory, predicate);

}
```

## 1.4.6. 람다 표현식 사용

```java
public static void main(String[] args) {
	// 사과 인벤토리 생성
	List<Apple> inventory = List.of(
		new Apple(Color.RED),
		new Apple(Color.GREEN),
		new Apple(Color.RED),
		new Apple(Color.GREEN)
	);
	
	// 람다 표현식을 사용하여 필터링 조건 정의
	List<Apple> redApples = filterApples(inventory, 
							apple -> Color.RED.equals(apple.getColor()));

}
```

## 1.4.7. 스트림 API 사용

```java
public static void main(String[] args) {
	// 사과 인벤토리 생성
	List<Apple> inventory = List.of(
		new Apple(Color.RED),
		new Apple(Color.GREEN),
		new Apple(Color.RED),
		new Apple(Color.GREEN)
	);
	
	// 스트림 API를 사용하여 필터링
	List<Apple> redApples = inventory.stream()
			.filter(apple -> Color.RED.equals(apple.getColor()))
			.collect(Collectors.toList());
	
}
```

## 2. 결론

- 동작의 파라미터화를 통해 코드의 유연성과 재사용성을 크게 향상 시킬 수 있다.
- 사과 인벤토리 예제에서, 다양한 필터링 조건을 메서드의 파라미터로 전달 받아 동적으로 처리할 수 있다.


## Reference
- 모던-자바-인-액션