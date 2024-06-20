# SOLID 5원칙
## SOLID 5원칙이란?
객체지향 설계의 5대 원칙이라 부르는데 SRP(단일 책임 원칙), OCP(개방-폐쇄 원칙), LSP(리스코프 치환 원칙), ISP(인터페이스 분리 원칙), DIP(의존 역전 원칙)을 말하고 앞자를 따서 SOILD 원칙이라고 부른다.

- [**S**ingle Responsibility Principle (단일 책임 원칙)](#1-single-responsibility-principle-단일-책임-원칙)
- [**O**pen/Closed Principle (개방 폐쇄 원칙)](#2-openclosed-principle-개방-폐쇄-원칙)
- [**L**iskov Substitution Principle (리스코프 치환 원칙)](#3-liskov-substitution-principle-리스코프-치환-원칙)
- [**I**nterface Segregation Principle (인터페이스 분리 원칙)](#4-interface-segregation-principle-인터페이스-분리-원칙)
- [**D**ependency Inversion Principle (의존 역전 원칙)](#5-dependency-inversion-principle-의존-역전-원칙)

## 1. Single Responsibility Principle (단일 책임 원칙)
 > **모든 클래스는 단일 책임을 가져야 한다. 클래스가 변경되어야 하는 이유는 오직 한 가지.**

 ### SRP 위반 예제
```java
package single_responsibility.violation;

import java.util.ArrayList;

public class Board {

    ArrayList<String> spots;

    public Board() {
        this.spots = new ArrayList<>();
        for (int i=0; i<9; i++) {
            this.spots.add(String.valueOf(i));
        }
    }

    public ArrayList<String> firstRow() {
        ArrayList<String> firstRow = new ArrayList<>();
        firstRow.add(this.spots.get(0));
        firstRow.add(this.spots.get(1));
        firstRow.add(this.spots.get(2));
        return firstRow;
    }

    public ArrayList<String> secondRow() {
        ArrayList<String> secondRow = new ArrayList<>();
        secondRow.add(this.spots.get(3));
        secondRow.add(this.spots.get(4));
        secondRow.add(this.spots.get(5));
        return secondRow;
    }

    public ArrayList<String> thirdRow() {
        ArrayList<String> thirdRow = new ArrayList<>();
        thirdRow.add(this.spots.get(6));
        thirdRow.add(this.spots.get(7));
        thirdRow.add(this.spots.get(8));
        return thirdRow;
    }

    public void display() {
        String formattedFirstRow = this.spots.get(0) + " | " + this.spots.get(1) + " | " + this.spots.get(2) + "\n" + this.spots.get(3) + " | " + this.spots.get(4) + " | " + this.spots.get(5) + "\n" + this.spots.get(6) + " | " + this.spots.get(7) + " | " + this.spots.get(8);
        System.out.print(formattedFirstRow);
    }
}
```
> Board 클래스는 여러 가지 작업을 수행 -> SRP 위반
 - `Board` 객체의 상태 관리
 - `firstRow()`, `secondRow()`, `thirdRow()` 메소드를 통해 각 행의 데이터 반환
 - `display()` 메소드를 통해 보드 출력

### Solution
``` java
package single_responsibility.solution;

import java.util.ArrayList;

public class Board {
    int size;
    ArrayList<String> spots;

    public Board(int size) {
        this.size = size;
        this.spots = new ArrayList<>();

        for(int i=0; i<size; i++) {
            for(int j=0; j<size; j++>) {
                this.spots.add(String.valueOf(size * i + j));
            }
        }
    }

    public ArrayList<String> valuesAt(ArrayList<Integer> indexes) {
        ArrayList<String> values = new ArrayList<>();

        for (int index : indexes) {
            values.add(this.spots.get(index));
        }

        return values;
    }
}
```
``` java
package single_responsibility.solution;

public class BoardPresenter {

    Board board;

    public BoardPresenter(Board board) {
        this.board = board;
    }

    public void display() {
        String formattedBoard = "";
        for (int i=0; i<this.board.size * this.board.size; i++) {
            String borderOrNewLine = "";
            if ((i+1) % board.size == 0) {
                borderOrNewLine += "\n";
            } else {
                borderOrNewLine += "|";
            }

            formattedBoard += board.spots.get(i);
            formattedBoard += borderOrNewLine;
        }

        System.out.print(formattedBoard);
    }
}
```
``` java
package single_responsibility.solution;

import java.util.ArrayList;

public class BoardShaper {
    int size;

    public BoardShaper(int size) {
        this.size = size;
    }

    public ArrayList<ArrayList<Integer>> rowIndexes() {
        ArrayList<ArrayList<Integer>> rowIndexes = new ArrayList<>();

        for (int i=0; i<this.size; i++) {
            ArrayList<Integer> row = new ArrayList<>();
            for(int j=0; j<this.size; j++) {
                row.add(i*size + j);
            }
            rowIndexes.add(row);
        }

        return rowIndexes;
    }
}

```
> SRP 준수
 - `Board` -> 보드 객체의 상태 관리 역할, 크기과 각 셀의 값을 초기화하고 주어진 인덱스의 셀의 값을 반환하는 역할
 - `BoardPresenter` -> 보드를 화면에 출력하는 역할
 - `BoardShaper` -> 보드의 구조(행,열)를 정의하는 역할

## 2. Open/Closed Principle (개방 폐쇄 원칙)
>**확장에는 열려있고 수정에 대해서는 닫혀있어야 한다.**
- 확장: 요구사항이 변경될 때는 새로운 동작을 추가하여 기능 확장이 가능해야한다.
- 수정: 기존의 코드를 수정하지 않고 기능을 추가하거나 변경 가능해야한다.

### OCP 위반 예제
```java
package open_closed.violation;

public class Greeter {
    String formality;

    public String greet() {
        if (this.formality.equals("formal")) {
            return "Good evening, sir.";
        }
        else if (this.formality.equals("casual")) {
            return "Sup bro?";
        }
        else if (this.formality.equals("intimate")) {
            return "Hello Darling!";
        }
        else {
            return "Hello.";
        }
    }

    public void setFormality(String formality) {
        this.formality = formality;
    }
}
```
> `Greeter` 클래스는 `formality` 변수 값에 따라 다양한 인사말 반환
 - 새로운 인사 방식이 추가되거나 기존 방식이 변경될 떄마다 `greet()` 메소드 수정 요구 -> 확장에 열려있지 않고 수정에 닫혀 있지 않음.

### OCP Solution
```java
package open_closed.solution;

public class Greeter {
    private Personality personality;

    public Greeter(Personality personality) {
        this.personality = personality;
    }

    public String greet(){
        return this.personality.greet();
    }
}
```
```java
package open_closed.solution;

public interface Personality {
    public String greet();
}
```
```java
package open_closed.solution;

public class CasualPersonality implements Personality{
    public String greet() {
        return "Sup bro?";
    }
}
```
```java
package open_closed.solution;

public class FormalPersonality implements Personality{
    @Override
    public String greet() {
        return "Good evening, sir.";
    }
}
```
```java
package open_closed.solution;

public class InitmatePersonality implements Personality{
    @Override
    public String greet() {
        return "Hello Darling!";
    }
}
```
> OCP 준수
 - `Greeter` -> 인사말을 생성하는 역할만 수행, 구체적인 인사말을 구현하지 않고 `Personaliy` 인터페이스를 통해 위임
 - `Personality` -> 인사말을 생성하는 메소드 `greet()`를 정의, 새로운 인사말이 필요한 경우 이 인터페이스를 구현하여 생성 가능

## 3. Liskov Substitution Principle (리스코프 치환 원칙)
> **하위 타입은 상위 타입을 대체할 수 있어야 한다.**
 - **해당 객체를 사용하는 클라이언트는 상위 타입이 하위타입으로 변경되어도, 차이를 인식하지 못한 채 상위 타입의 인터페이스를 통해 서브 클래스를 사용할 수 있어야한다.**

### LSP 위반 예제
```java
package liskov_subsitution.violation;

abstract public class Apartment {
    int squareFootage;
    int numberOfBedrooms;

    abstract void setSqureFootage(int squareFootage);
}
```
```java
package liskov_subsitution.violation;

public class PenthouseSuite extends Apartment{

    public PenthouseSuite() {
        this.numberOfBedrooms = 4;
    }

    @Override
    void setSqureFootage(int squareFootage) {
        this.squareFootage = squareFootage;
    }
}
```
```java
package liskov_subsitution.violation;

public class Studio extends Apartment{
    public Studio() {
        this.numberOfBedrooms = 0;
    }

    @Override
    void setSqureFootage(int squareFootage) {
        this.squareFootage = squareFootage;
    }
}
```
```java
package liskov_subsitution.violation;

public class UnitUpgrader {
    public UnitUpgrader(Apartment apartment) {
        apartment.squareFootage += 40;

        if (apartment.getClass() != Studio.class) {
            apartment.numberOfBedrooms += 1;
        }
    }
}
```
> `Apartment` 인터페이스가 존재하고 `PenthouseSuite`와 `Studio`가 인터페이스의 구현체로 생성
 - `UnitUpgrader` 는 내부 메소드 `UnitUpgrader(Apartment apartment)`에서 `Stdio`와 같은 특정 하위 클래스에 의존 -> LSP 위반
 - `Apartment`를 다룰 때 예상치 못한 결과를 야기할수 있음

### LSP Solution
```java
package liskov_subsitution.solution;

public class PenthouseSuite {
    int squareFootage;
    int numberOfBedrooms;

    public PenthouseSuite() {
        this.numberOfBedrooms = 4;
    }

    public void setSquareFootage(int squareFootage) {
        this.squareFootage = squareFootage;
    }
}
```
```java
package liskov_subsitution.solution;

public class Stdio {
    int squareFootage;
    int numberOfBedrooms;

    public Stdio() {
        this.numberOfBedrooms = 0;
    }

    public void setSquareFootage(int squareFootage) {
        this.squareFootage = squareFootage;
    }
}
```
```java
package liskov_subsitution.solution;

public class BedroomAdder {
    public void addBedroom(PenthouseSuite penthouseSuite) {
        penthouseSuite.numberOfBedrooms += 1;
    }
}
```
> LSP 준수
 - `BedroomAdder` -> `addBedroom(PenthouseSuite penthouseSuite)`에서 `PenthouseSuite` 클래스만 파라미터로 받으므로 하위 클래스에 의존 x

## 4. Interface Segregation Principle (인터페이스 분리 원칙)
> **목적과 관심이 각기 다른 클라이언트가 있다면 인터페이스를 통해 적절하게 분리**
 - **클라이언트의 목적과 용도에 적합한 인터페이스만을 제공**

### ISP 위반 예제
```java
package interface_segregation.violation;

public interface Bird {
    public void fly();
    public  void molt();
}
```
```java
package interface_segregation.violation;

public class Eagle implements Bird{
    String currentLocation;
    int numberOfFeathers;

    public Eagle(int initialFeatherCount) {
        this.numberOfFeathers = initialFeatherCount;
    }

    @Override
    public void fly() {
        this.currentLocation = "in the air";
    }

    @Override
    public void molt() {
        this.numberOfFeathers -= 1;
    }
}
```
```java
package interface_segregation.violation;

public class Penguin implements Bird{
    String currentLocation;
    int numberOfFeathers;

    public Penguin(int initialFeatherCount) {
        this.numberOfFeathers = initialFeatherCount;
    }
    @Override
    public void fly() {
        throw new UnsupportedOperationException();
    }

    @Override
    public void molt() {
        this.numberOfFeathers -= 1;
    }

    public void swim() {
        this.currentLocation = "in the water";
    }
}
```
> `Bird` 인터페이스는 모든 새가 날(`fly()`) 수 있고, 털을 갈아야(`molt()`)한다고 가정
 - `Penguin` 클래스는 `Bird`의 구현체 이지만 `fly()` 할 수 없다. -> ISP 위반

### ISP Solution
```java
package interface_segregation.solution;

public interface FeatheredCreature {
    public void molt();
}
```
```java
package interface_segregation.solution;

public interface FlyingCreature {
    public void fly();
}
```
```java
package interface_segregation.solution;

public interface SwimmingCreature {
    public void swim();
}
```
```java
package interface_segregation.solution;

public class Eagle implements FlyingCreature, FeatheredCreature {

    String currentLocation;
    int numberOfFeathers;

    public Eagle(int initialNumberOfFeathers) {
        this.numberOfFeathers = initialNumberOfFeathers;
    }

    @Override
    public void molt() {
        this.numberOfFeathers -= 1;
    }

    @Override
    public void fly() {
        this.currentLocation = "in the air";
    }
}
```
```java
package interface_segregation.solution;

public class Penguin implements SwimmingCreature, FeatheredCreature{

    String currentLocation;
    int numberOfFeathers;

    @Override
    public void molt() {
        this.numberOfFeathers -= 4;
    }

    @Override
    public void swim() {
        this.currentLocation = "in the wather";
    }
}
```
> ISP 준수
 - `Bird` 인터페이스를 (`FlyingCreature`, `FeatheredCreature`, `SwimmingCreature`)으로 세분화
 - `Eagle`, `Penguin` 모두 이에 맞는 인터페이스를 구현

## 5. Dependency Inversion Principle (의존 역전 원칙)
> **고수준 모듈은 저수준 모듈의 구현에 의존해서는 안되며, 저수준, 고수준 모듈 모두 추상화에 의존해야한다.**
 - 고소준 모듈: 입력과 출력에 먼 추상화된 모듈
 - 저수준 모듈: 입력과 출력에 가까운 구현 모듈

### DIP 위반 예제
```java
package dependency_inversion.violation;

public class Emailer {
    public String generateWeatherAlert(String weatherConditions) {
        String alert = "It is " + weatherConditions;
        return alert;
    }
}
```
```java
package dependency_inversion.violation;

public class Phone {
    public String generateWeatherAlert(String weatherConditions) {
        String alert = "It is " + weatherConditions;
        return alert;
    }
}
```
```java
package dependency_inversion.violation;

public class WeatherTracker {
    String currentConditions;
    Phone phone;
    Emailer emailer;

    public WeatherTracker() {
        phone = new Phone();
        emailer = new Emailer();
    }

    public void setCurrentConditions(String weatherDescription) {
        this.currentConditions = weatherDescription;

        if (weatherDescription.equals("rainy")) {
            String alert = phone.generateWeatherAlert(weatherDescription);
            System.out.println(alert);
        }

        if (weatherDescription.equals("sunny")) {
            String alert = emailer.generateWeatherAlert(weatherDescription);
            System.out.println(alert);
        }
    }
}
```
> `WeatherTracker`는 `Phone`과 `Emailer`에 직접 의존 -> DIP 위반

### DIP Solution
```java
package dependency_inversion.solution;

public interface Notifier {
    public void alertWeatherConditions(String weatherConditions);
}
```
```java
package dependency_inversion.solution;

public class EmailClient implements Notifier{
    @Override
    public void alertWeatherConditions(String weatherConditions) {
        if (weatherConditions.equals("sunny")) {
            System.out.print("It is sunny.");
        }
    }
}
```
```java
package dependency_inversion.solution;

public class MobileDevice implements Notifier{
    @Override
    public void alertWeatherConditions(String weatherConditions) {
        if (weatherConditions.equals("rainy")) {
            System.out.print("It is rainy");
        }
    }
}
```
```java
package dependency_inversion.solution;

public class WeatherTracker {
    String currentConditions;

    public void setCurrentConditions(String weatherDescriptions) {
        this.currentConditions = weatherDescriptions;
    }

    public void notify(Notifier notifier) {
        notifier.alertWeatherConditions(currentConditions);
    }
}
```
> DIP 준수
 - `WeatherTracker`는 `notify(Notifier notifier)`에서 `Notifier` 추상화에 의존 -> DIP 준수
 - `EmailClient`나 `MobileDevice`와 같은 구현체에 의존 x

## Reference
- [SOLID](https://github.com/mikeknep/SOLID) by Mike Knepper