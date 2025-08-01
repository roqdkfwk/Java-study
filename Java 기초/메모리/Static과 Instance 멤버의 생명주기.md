# Static & Instance 멤버

## 📋 목차
1. [메모리에 로드되는 시점](#메모리에-로드되는-시점)
2. [메모리에 저장되는 위치](#메모리에-저장되는-위치)
3. [메모리에서 지워지는 시점](#메모리에서-지워지는-시점)
4. [데이터 공유 범위](#데이터-공유-범위)
5. [스택과 힙 메모리 영역](#스택과-힙-메모리-영역)
6. [실습 예제](#실습-예제)
7. [요약 정리](#요약-정리)
8. 추가 내용
   - 클래스 메타 데이터의 종류는?
   - [(일반)가비지 컬렉터가 실제로 해당 객체에 대한 데이터를 제거하는 시점은?](#gc-timing)
   - [팩토리 메서드 패턴?](#factory-method)

---

## 메모리에 로드되는 시점

### 🔹 Static 메서드/필드
- **클래스 로딩 시점**에 메모리에 로드
- JVM이 클래스를 처음 로드할 때, 해당 클래스의 static 필드와 메서드는 메서드 영역(Method Area)에 로드
- 클래스가 최초로 사용될 때 초기화되며, 그 이후에는 모든 인스턴스가 공유

```java
public class MathUtils {
    // 클래스 로딩 시점에 메모리에 로드됨
    public static final double PI = 3.14159;
    public static int calculateCount = 0;
    
    // 클래스 로딩 시점에 메모리에 로드됨
    public static int add(int a, int b) {
        calculateCount++; // 호출될 때마다 카운트 증가
        return a + b;
    }
}

// 사용 예시
public class Main {
    public static void main(String[] args) {
        // 객체 생성 없이 바로 사용 가능 (이미 메모리에 로드됨)
        int result = MathUtils.add(5, 3);
        System.out.println("결과: " + result);
        System.out.println("계산 횟수: " + MathUtils.calculateCount);
    }
}
```

### 🔹 일반(Instance) 메서드/필드
- **객체 생성 시점**에 메모리에 로드
- 일반 필드는 객체가 생성될 때 힙 메모리에 할당
- 일반 메서드는 해당 객체가 메모리에 존재하는 한 실행될 준비를 한다.

```java
public class Person {
    // 객체 생성 시마다 힙 메모리에 할당
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;  // 각 객체마다 고유한 메모리 공간에 저장
        this.age = age;
    }
    
    // 객체가 존재하는 동안 호출 가능
    public void introduce() {
        System.out.println("안녕하세요, 저는 " + name + "이고 " + age + "살입니다.");
    }
}

// 사용 예시
public class Main {
    public static void main(String[] args) {
        // 객체 생성 시점에 name, age 필드가 힙 메모리에 할당
        Person person1 = new Person("김철수", 25);
        Person person2 = new Person("이영희", 30);
        
        person1.introduce(); // 각자 고유한 데이터로 실행
        person2.introduce();
    }
}
```

---

## 메모리에 저장되는 위치

### 🔹 Static 메서드/필드
- **메서드 영역(Method Area)** 에 저장
- 클래스 로딩 시 클래스에 관련된 정보와 함께 로드
- 모든 인스턴스가 공유하기 때문에 클래스 자체의 메모리 공간에서만 한 번 저장

### 🔹 일반 메서드/필드
- **힙(Heap) 메모리**에 저장
- 각 객체마다 별도의 메모리 공간을 가진다.
- 메서드 자체는 메서드 영역에 저장되어, 객체가 존재하는 동안 메서드에 대한 참조가 유지

```java
public class Car {
    // 메서드 영역에 저장 (모든 Car 객체가 공유)
    public static String brand = "현대";
    public static int totalCars = 0;
    
    // 힙 메모리에 저장 (각 객체마다 고유)
    private String model;
    private String color;
    
    public Car(String model, String color) {
        this.model = model;  // 각 객체의 힙 메모리에 저장
        this.color = color;
        totalCars++;         // 메서드 영역의 static 변수 증가
    }
    
    // 메서드 영역에 저장
    public static void showBrand() {
        System.out.println("브랜드: " + brand);
    }
    
    // 메서드 영역에 저장되지만, 힙의 객체 데이터를 사용
    public void showInfo() {
        System.out.println("모델: " + model + ", 색상: " + color);
    }
}
```

---

## 메모리에서 지워지는 시점

### 🔹 Static 메서드/필드
- **JVM이 종료될 때까지** 메모리에 존재
- 클래스가 로드될 때 메모리에 올라가고, 프로그램이 종료될 때까지 해당 메모리 공간에 남아 있다.
- **가비지 컬렉터(GC)에 의해 수거되지 않는다.**

```java
public class Counter {
    public static int count = 0;
    
    public static void increment() {
        count++; // 프로그램 종료까지 계속 유지됨
    }
}

public class Main {
    public static void main(String[] args) {
        Counter.increment(); // count = 1
        Counter.increment(); // count = 2
        
        // 메서드가 끝나도 static 변수는 유지됨
        System.out.println("카운트: " + Counter.count); // 2 출력
        
        // 다른 메서드에서도 동일한 값 유지
        otherMethod();
    }
    
    public static void otherMethod() {
        System.out.println("다른 메서드에서 카운트: " + Counter.count); // 여전히 2
    }
}
```

### 🔹 일반 메서드/필드
- **객체가 더 이상 참조되지 않으면** 메모리에서 해제
- 객체가 null로 설정되거나, 객체의 참조가 모두 사라지면 해당 객체는 **가비지 컬렉션(GC)의 대상**이 된다.
<a id="gc-timing"></a>
- 가비지 컬렉터가 실제로 해당 객체에 대한 데이터를 제거하는 시점은?

```java
public class Student {
    private String name;
    private int studentId;
    
    public Student(String name, int studentId) {
        this.name = name;
        this.studentId = studentId;
        System.out.println(name + " 학생 객체가 생성되었습니다.");
    }
    
    // 소멸자 역할 (GC가 호출할 때)
    @Override
    protected void finalize() throws Throwable {
        System.out.println(name + " 학생 객체가 메모리에서 해제됩니다.");
        super.finalize();
    }
}

public class Main {
    public static void main(String[] args) {
        Student student1 = new Student("김학생", 1001);
        Student student2 = new Student("이학생", 1002);
        
        // 참조를 null로 설정하면 GC의 대상이 됨
        student1 = null;
        
        // 강제로 GC 실행 (실제로는 JVM이 결정)
        System.gc();
        
        // student2는 여전히 참조되고 있으므로 메모리에 유지됨
        System.out.println("프로그램 계속 실행...");
        
        // 메서드가 끝나면 student2도 참조가 사라져 GC 대상이 됨
    }
}
```

---

## 데이터 공유 범위

### 📊 Static vs Instance 멤버 비교표

| 구분 | Static 멤버 (정적 멤버) | Instance 멤버 (일반 멤버) |
|------|------------------------|---------------------------|
| **소속** | 클래스에 소속 | 객체(인스턴스)에 소속 |
| **메모리 영역** | 메서드 영역(Method Area) | 힙(Heap) |
| **생성 시점** | 클래스가 로딩될 때 단 한 번 생성 | 객체가 생성될 때마다(new) 생성 |
| **데이터 공유** | 해당 클래스의 모든 객체가 공유 | 객체별로 고유한 공간을 가짐 (공유 안 됨) |
| **접근 방법** | 클래스명.멤버명 (객체 생성 없이 접근 가능) | 참조변수명.멤버명 (반드시 객체를 생성해야 함) |
| **멤버 간 접근** | static 메서드 내에서는 static 멤버만 직접 접근 가능 | 인스턴스 메서드 내에서는 static 멤버와 인스턴스 멤버 모두 접근 가능 |

### 🔄 데이터 공유 예제

```java
public class BankAccount {
    // static: 모든 계좌가 공유하는 데이터
    public static double interestRate = 0.03; // 이자율
    public static int totalAccounts = 0;       // 총 계좌 수
    
    // instance: 각 계좌마다 고유한 데이터
    private String accountNumber;
    private String ownerName;
    private double balance;
    
    public BankAccount(String accountNumber, String ownerName, double initialBalance) {
        this.accountNumber = accountNumber;
        this.ownerName = ownerName;
        this.balance = initialBalance;
        totalAccounts++; // 계좌 생성 시마다 총 계좌 수 증가
    }
    
    // static 메서드: 객체 없이도 호출 가능
    public static void setInterestRate(double newRate) {
        interestRate = newRate;
        System.out.println("모든 계좌의 이자율이 " + (newRate * 100) + "%로 변경되었습니다.");
    }
    
    // instance 메서드: 개별 계좌의 데이터 조작
    public void addInterest() {
        double interest = balance * interestRate;
        balance += interest;
        System.out.println(ownerName + "님의 계좌에 이자 " + interest + "원이 추가되었습니다.");
    }
    
    public void showAccountInfo() {
        System.out.println("계좌번호: " + accountNumber + 
                         ", 소유자: " + ownerName + 
                         ", 잔액: " + balance + "원");
    }
    
    // static 메서드에서는 instance 멤버에 직접 접근 불가
    public static void showTotalInfo() {
        System.out.println("총 계좌 수: " + totalAccounts);
        System.out.println("현재 이자율: " + (interestRate * 100) + "%");
        // System.out.println("잔액: " + balance); // 컴파일 에러!
    }
}

public class BankTest {
    public static void main(String[] args) {
        // 계좌 생성 전에도 static 멤버 접근 가능
        BankAccount.showTotalInfo();
        
        System.out.println("\n=== 계좌 생성 ===");
        BankAccount account1 = new BankAccount("1001", "김철수", 100000);
        BankAccount account2 = new BankAccount("1002", "이영희", 200000);
        
        System.out.println("\n=== 개별 계좌 정보 ===");
        account1.showAccountInfo();
        account2.showAccountInfo();
        
        System.out.println("\n=== 이자율 변경 (static 데이터 공유) ===");
        BankAccount.setInterestRate(0.05); // 모든 계좌에 적용
        
        System.out.println("\n=== 이자 적용 ===");
        account1.addInterest(); // 개별 계좌에 적용
        account2.addInterest();
        
        System.out.println("\n=== 최종 정보 ===");
        account1.showAccountInfo();
        account2.showAccountInfo();
        BankAccount.showTotalInfo();
    }
}
```

**실행 결과:**
```
총 계좌 수: 0
현재 이자율: 3.0%

=== 계좌 생성 ===

=== 개별 계좌 정보 ===
계좌번호: 1001, 소유자: 김철수, 잔액: 100000.0원
계좌번호: 1002, 소유자: 이영희, 잔액: 200000.0원

=== 이자율 변경 (static 데이터 공유) ===
모든 계좌의 이자율이 5.0%로 변경되었습니다.

=== 이자 적용 ===
김철수님의 계좌에 이자 5000.0원이 추가되었습니다.
이영희님의 계좌에 이자 10000.0원이 추가되었습니다.

=== 최종 정보 ===
계좌번호: 1001, 소유자: 김철수, 잔액: 105000.0원
계좌번호: 1002, 소유자: 이영희, 잔액: 210000.0원
총 계좌 수: 2
현재 이자율: 5.0%
```

---

## 스택과 힙 메모리 영역

### 📊 메모리 영역별 특징 비교표

| 구분 | 스택(Stack) 메모리 | 힙(Heap) 메모리 | 메서드 영역(Method Area) |
|------|-------------------|-----------------|-------------------------|
| **용도** | 메서드 호출 시 사용하는 지역 변수와 메서드 호출 스택 | 객체가 생성될 때 사용되는 메모리 공간 | 클래스 정보, static 멤버, 메서드 바이트코드 저장 |
| **저장 데이터** | • 지역 변수<br>• 메서드 매개변수<br>• 메서드 호출 정보 | • 객체 인스턴스<br>• 인스턴스 변수<br>• 배열 | • 클래스 메타데이터<br>• static 변수<br>• 메서드 바이트코드<br>• 상수 풀 |
| **메모리 할당** | 각 메서드 호출 시 스택에 메모리 공간 할당 | 객체는 힙에 저장됨 | 프로그램 시작 시 로드 |
| **메모리 해제** | 메서드 호출이 끝나면 메모리에서 자동으로 제거 | 객체가 더 이상 참조되지 않으면 GC에 의해 메모리에서 제거 | 종료 시까지 유지 |
| **구조 특징** | LIFO(Last In First Out) 구조 | 동적 메모리 할당 | 모든 스레드가 공유 |
| **스레드 관계** | 각 스레드마다 독립적인 스택 영역 | 모든 스레드가 공유 | 모든 스레드가 공유 |
| **크기** | 상대적으로 작음 (보통 1MB 이하) | 상대적으로 큼 (힙 크기 설정 가능) | 상대적으로 작음 |
| **접근 속도** | 빠름 | 보통 | 보통 |

### 🔄 메모리 영역별 생명주기 비교

| 메모리 영역 | 생성 시점 | 소멸 시점 | 관리 주체 |
|------------|-----------|-----------|----------|
| **Stack** | 메서드 호출 시 | 메서드 종료 시 | JVM 자동 관리 |
| **Heap** | 객체 생성 시(`new`) | GC가 수거할 때 | 가비지 컬렉터 |
| **Method Area** | 클래스 로딩 시 | JVM 종료 시 | JVM 자동 관리 |

```java
public class MemoryExample {
    // 메서드 영역에 저장
    public static int staticVar = 100;
    
    // 힙 메모리에 저장 (객체 생성 시)
    private int instanceVar;
    
    public void methodExample(int parameter) { // parameter: 스택
        int localVar = 10;          // localVar: 스택
        String localString = "Hello"; // localString 참조: 스택, "Hello" 객체: 힙
        
        MemoryExample obj = new MemoryExample(); // obj 참조: 스택, 객체: 힙
        
        // 메서드 종료 시 localVar, localString, obj 참조는 스택에서 제거
        // 하지만 힙의 객체들은 GC에 의해 나중에 제거
    }
}
```

---

## 실습 예제

### 🎯 게임 캐릭터 관리 시스템

```java
public class GameCharacter {
    // static: 모든 캐릭터가 공유하는 데이터
    public static final int MAX_LEVEL = 100;
    public static int totalCharacters = 0;
    public static String gameVersion = "v1.0";
    
    // instance: 각 캐릭터마다 고유한 데이터
    private String name;
    private int level;
    private int hp;
    private int exp;
    
    // 생성자
    public GameCharacter(String name) {
        this.name = name;
        this.level = 1;
        this.hp = 100;
        this.exp = 0;
        totalCharacters++;
        System.out.println("🎮 " + name + " 캐릭터가 생성되었습니다!");
    }
    
    // static 메서드: 게임 전체 정보
    public static void showGameInfo() {
        System.out.println("=== 게임 정보 ===");
        System.out.println("게임 버전: " + gameVersion);
        System.out.println("최대 레벨: " + MAX_LEVEL);
        System.out.println("총 캐릭터 수: " + totalCharacters);
    }
    
    // static 메서드: 게임 버전 업데이트 (모든 캐릭터에 적용)
    public static void updateGameVersion(String newVersion) {
        gameVersion = newVersion;
        System.out.println("🔄 게임이 " + newVersion + "으로 업데이트되었습니다!");
    }
    
    // instance 메서드: 개별 캐릭터 행동
    public void gainExp(int expAmount) {
        this.exp += expAmount;
        System.out.println("✨ " + name + "이(가) " + expAmount + " 경험치를 획득했습니다!");
        
        // 레벨업 체크
        if (this.exp >= 100 && this.level < MAX_LEVEL) {
            levelUp();
        }
    }
    
    private void levelUp() {
        this.level++;
        this.exp = 0;
        this.hp += 20;
        System.out.println("🎉 " + name + "이(가) 레벨업했습니다! (Lv." + level + ")");
    }
    
    public void showCharacterInfo() {
        System.out.println("--- " + name + " 정보 ---");
        System.out.println("레벨: " + level + "/" + MAX_LEVEL);
        System.out.println("체력: " + hp);
        System.out.println("경험치: " + exp + "/100");
        System.out.println("게임 버전: " + gameVersion);
    }
    
    // 소멸 시뮬레이션
    public void deleteCharacter() {
        totalCharacters--;
        System.out.println("💀 " + name + " 캐릭터가 삭제되었습니다.");
    }
}

public class GameTest {
    public static void main(String[] args) {
        System.out.println("🎯 게임 캐릭터 관리 시스템 시작!\n");
        
        // 캐릭터 생성 전 게임 정보 확인
        GameCharacter.showGameInfo();
        
        System.out.println("\n=== 캐릭터 생성 ===");
        GameCharacter warrior = new GameCharacter("전사");
        GameCharacter mage = new GameCharacter("마법사");
        GameCharacter archer = new GameCharacter("궁수");
        
        System.out.println("\n=== 캐릭터들의 모험 ===");
        warrior.gainExp(50);
        mage.gainExp(120);    // 레벨업 발생
        archer.gainExp(80);
        
        System.out.println("\n=== 각 캐릭터 정보 확인 ===");
        warrior.showCharacterInfo();
        System.out.println();
        mage.showCharacterInfo();
        System.out.println();
        archer.showCharacterInfo();
        
        System.out.println("\n=== 게임 업데이트 (static 데이터 변경) ===");
        GameCharacter.updateGameVersion("v2.0");
        
        System.out.println("\n=== 업데이트 후 캐릭터 정보 ===");
        warrior.showCharacterInfo(); // 모든 캐릭터의 게임 버전이 v2.0으로 변경됨
        
        System.out.println("\n=== 캐릭터 삭제 ===");
        archer.deleteCharacter();
        
        System.out.println("\n=== 최종 게임 정보 ===");
        GameCharacter.showGameInfo();
        
        // GC 시뮬레이션 (archer 참조 제거)
        archer = null;
        System.gc();
        System.out.println("🗑️ 가비지 컬렉션 실행 완료");
    }
}
```

---

## 요약 정리

### 🔑 핵심 포인트

| 구분 | Static 멤버 (정적 멤버) | Instance 멤버 (인스턴스 멤버) |
|------|------------------------|------------------------------|
| **생명주기** | 클래스 로딩 → JVM 종료 | 객체 생성 → 참조 제거 후 GC |
| **메모리 위치** | 메서드 영역(Method Area) | 힙(Heap) 메모리 |
| **공유 범위** | 해당 클래스의 모든 객체가 공유 | 각 객체마다 고유한 데이터 |
| **접근 방법** | `클래스명.멤버명` | `참조변수명.멤버명` |
| **특징** | • 객체 생성 없이 접근 가능<br>• 프로그램 종료까지 메모리에 상주<br>• GC 대상이 아님 | • 객체 생성 후 접근 가능<br>• 참조가 없어지면 GC 대상<br>• 객체별로 독립적인 메모리 공간 |

### 🚀 사용 시기

| 사용 시기 | Static 사용 | Instance 사용 |
|----------|-------------|---------------|
| **데이터 성격** | • 공통 상수 값 (`Math.PI`, `Integer.MAX_VALUE`)<br>• 객체 간 공유해야 하는 데이터 (카운터, 설정값) | • 객체의 고유한 상태 값 (이름, 나이, 색상 등)<br>• 각 객체마다 다른 값을 가져야 하는 데이터 |
| **메서드 성격** | • 유틸리티성 메서드 (`Arrays.sort()`, `Integer.parseInt()`)<br>• 팩토리 메서드 패턴<br>• 객체 상태와 무관한 기능 | • 객체의 상태를 조작하는 행위 (setter, getter 메서드)<br>• 객체별로 다른 동작이 필요한 경우 |
| **메모리 관리** | • 프로그램 전체에서 하나만 존재해야 하는 값 | • 객체마다 독립적으로 관리되어야 하는 값 |
| **사용 예시** | • `Math.max(a, b)`<br>• `Integer.parseInt(str)`<br>• `Arrays.sort(array)` | • `person.getName()`<br>• `car.accelerate()`<br>• `account.withdraw(amount)` |

<a id="factory-method"></a>
- **팩토리 메서드 패턴?**
  
  
### ⚡ 메모리 영역별 주요 특징 및 주의사항

| 메모리 영역 | 장점 | 단점 | 주의사항 |
|------------|------|------|----------|
| **Stack** | • 빠른 접근 속도<br>• 자동 메모리 관리 | • 크기 제한<br>• StackOverflowError 발생 가능 | 재귀 호출 시 스택 오버플로우 주의 |
| **Heap** | • 동적 크기 조절<br>• 큰 메모리 공간 | • 상대적으로 느린 접근<br>• GC 오버헤드 | 메모리 누수 방지, OutOfMemoryError 주의 |
| **Method Area** | • 코드 공유로 메모리 효율성<br>• 클래스 정보 중앙 관리 | • 런타임 중 변경 불가<br>• 프로그램 종료까지 유지 | Static 변수 남용 시 메모리 누수 가능 |

### ⚠️ 주의사항

1. **Static 메서드에서는 Instance 멤버에 직접 접근 불가**
   ```java
   public class Example {
       int instanceVar = 10;
       
       public static void staticMethod() {
           // System.out.println(instanceVar); // 컴파일 에러!
       }
   }
   ```

2. **메모리 누수 주의**
   - Static 변수에 대용량 객체를 참조하면 GC되지 않아 메모리 누수 발생 가능

3. **멀티스레드 환경에서의 동기화**
   - Static 변수는 모든 스레드가 공유하므로 동기화 고려 필요

이렇게 Static과 Instance 멤버의 생명주기를 이해하면, 효율적이고 안전한 Java 프로그램을 작성할 수 있습니다! 🚀