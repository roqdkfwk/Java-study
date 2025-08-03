# 싱글톤 패턴 (Singleton Pattern)

## 📁 목차

1. [싱글톤 패턴이란?](#1-싱글톤-패턴이란)
2. [대표적 구현 6종 & 문제-대응 요약](#2-대표적-구현-6종--문제-대응-요약)
   - [① Eager Initialization](#①-eager-initialization)
   - [② Lazy (Non-thread-safe)](#②-lazy-non-thread-safe)
   - [③ Lazy + synchronized (Thread-safe)](#③-lazy--synchronized-thread-safe)
   - [④ Double-Checked Locking (DCL)](#④-double-checked-locking-dcl)
   - [⑤ Initialization-on-Demand Holder](#⑤-initialization-on-demand-holder-정적-내부-클래스-)
   - [⑥ Enum 싱글톤](#⑥-enum-싱글톤)
3. [Spring의 Singleton Scope](#3-spring의-singleton-scope)
4. [공통 문제점 & 대응 전략](#4-공통-문제점--대응-전략)
5. [정리](#5-정리)
6. [권장사항](#6-권장사항)
   - [일반적인 용도](#-일반적인-용도)
   - [보안이 중요한 경우](#-보안이-중요한-경우)
   - [Spring 환경](#-spring-환경)

---

## 1. 싱글톤 패턴이란?

**싱글톤 패턴은 애플리케이션 전체에서 단 하나의 객체 인스턴스만 생성,공유하고 전역적으로 접근할 수 있도록 하여  
동일 객체를 매번 새로 만들지 않고 재사용함으로써 메모리와 생성 비용을 절감하는 디자인 패턴이다.  
주로 데이터베이스 연결, 네트워크 소켓, 스레드풀처럼 무겁고 전역적으로 사용되는 리소스를 다루는 클래스에 적용된다.**

---

## 2. 대표적 구현 6종 & 문제-대응 요약

### ① Eager Initialization

```java
public class Singleton {
    // 싱글톤 클래스의 객체를 담을 인스턴스 변수
    private static final Singleton INSTANCE = new Singleton();
    
    // 생성자를 private로 선언해서 외부에서 new를 사용할 수 없게 한다.
    private Singleton() {}
    
    public static Singleton getInstance() {
        return INSTANCE;
    }
}
```

**특징**
- ✔ 한번만 미리 만들어두는 가장 간단한 방법
- ✔ `static final`이므로 multi-thread 환경에서도 안전
  

- ❌ 클래스 로딩 시점에 무조건 메모리에 적재
  - 만약 리소스가 큰 객체인 경우 메모리 낭비가 발생
- ❌ 예외 처리 곤란

<br/>

**해결·보완 포인트**
- 리소스가 가벼운 경우에 사용하면 큰 문제가 발생하지 않는다.

---

### ② Lazy (Non-thread-safe)

```java
public class Singleton {
    // 싱글톤 클래스의 객체를 담을 인스턴스 변수
    private static Singleton INSTANCE;

    // 생성자를 private로 선언해서 외부에서 new를 사용할 수 없게 한다.
    private Singleton() {}
    
    // 외부에서 메서드를 호출하는 순간에 초기화를 진행한다.
    public static Singleton getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new Singleton();
        }
        return INSTANCE;
    }
}
```

**특징**
- ✔ 필요할 때 생성
  - 메서드를 호출했을 때 인스턴스가 null인 경우 새로 초기화, null이 아닌 경우 이미 있는 것을 반환
- ✔ 미사용 시에도 메모리를 차지하는 이전 방법의 단점 극복  
  

- ❌ 스레드 세이프 하지 않아 중복 인스턴스가 생성될 수 있다.
  - INSTANCE가 아직 초기화 되지 않은 상태에서 두 개의 스레드가 존재할 때,
  - 첫 번째 스레드가 if문을 통과하고 객체를 생성하기 직전에
  - 두 번째 스레드가 if문에 도착하는 경우, INSTANCE가 아직 null인 상태이므로 두 번째 스레드도 if문을 통과한다.
  - 따라서 두 개의 스레드가 각각 INSTANCE를 초기화한다. 

<br/>

**해결·보완 포인트**
- ③, ④, ⑤ 방식으로 보완

---

### ③ Lazy + synchronized (Thread-safe)

```java
public class Singleton {
  private static Singleton INSTANCE;
  
  private Singleton() {}

  // synchronized 메서드
  public static synchronized Singleton getInstance() {
    if (INSTANCE == null) {
      INSTANCE = new Singleton();
    }
    return INSTANCE;
  }
}
```

**특징**
- ✔ `synchronized` 키워드를 사용해서 메서드에 스레드가 하나씩만 접근할 수 있도록 설정 (동기화)  
  → Thread-safe
  - `synchronized` 키워드가 뭐지❓❓❓
  

- ❌ 여러 개의 모듈에서 객체를 가져올 때, `synchronized` 메서드를 매번 호출하여 동기화 처리 작업에 오버헤드 발생   
  → 성능 하락

<br/>

**해결·보완 포인트**
- 성능이 중요하지 않은 초기화용 객체에만 사용

---

### ④ Double-Checked Locking (DCL)

```java
public class Singleton {
    private static volatile Singleton INSTANCE;
    
    public static Singleton getInstance() {
        if (INSTANCE == null) {
            synchronized (Singleton.class) {
                if (INSTANCE == null) INSTANCE = new Singleton();
            }
        }
        return INSTANCE;
    }
}
```

**특징**
- ✔ 첫 호출만 lock을 사용
  - 이후 호출은 lock을 사용하지 않는다.
- ✔ 이때 인스턴스 필드에 `volatile` 키워드를 붙여야만 I/O 불일치 문제를 해결할 수 있다.
  - volatile이 뭐지❓❓❓
  

- ❌ 코드 복잡
- ❌ `volatile` 누락 시 재주문 문제
- ❌ JVM에 따라서 여전히 스레드 세이프 하지 않는 경우가 발생할 수 있다.

<br/>

**해결·보완 포인트**
- JDK 5 이상, `volatile` 필수

---

### ⑤ Initialization-on-Demand Holder (정적 내부 클래스) ⭐

```java
public class Singleton {
    private Singleton() {}
    
    // static 내부 클래스 이용
    // Holder로 만들어서 클래스가 메모리에 로드되지 않고 getInstance 메서드가 호출되어야 로드된다.
    private static class Holder {
        private static final Singleton INSTANCE = new Singleton();
    }
    
    public static Singleton getInstance() { 
        return Holder.INSTANCE;
    }
}
```

**특징**
- 권장되는 두 개의 방법 중 하나이다.
- ⭐⭐⭐ Lazy + 100% Thread-safe (JVM class-loading 보장)
- ✔ 락 없음
- 사실상 문제 거의 없음

<br/>

#### ⭐⭐⭐100% Thread-safe한 이유  
- 위의 방식이 thread-safe할 수 있는 이유는 **JVM의 클래스 로딩과 초기화 규칙** 덕분이다.
  - 클래스의 초기화는 단 한 번만 이루어지고, 이 과정은 thread-safe하다.
  - 이에 따라 `new Singleton()`으로 객체를 초기화하는 과정은 처음 초기화된 이후에는 더 이상 발생하지 않는다. 
  - JVM의 클래스 로딩 및 초기화 규칙(링크 달기)
- Holder 패턴에서 `Holder` 클래스는 `getInstance()`메서드가 호출되며 **처음 참조되는 순간**에 로드된다.
- 이때 JVM은 `Holder`의 static 필드 초기화를 단 한 번만, 동기화된 상태에서 수행한다.
- 따라서 `INSTANCE`는 여러 스레드가 동시에 접근하더라도 중복 없이 생성된다.

<br/>

#### ❓ final의 사용 이유
 `final` 키워드를 제거해도 `new Singleton()`를 통한 초기화는 한 번만 수행되는데 왜 `final` 키워드를 쓸까?
- `final` 키워드를 사용하지 않으면 코드상에서 **재할당**이 가능하다.
- 따라서 `final` 키워드를 사용하면 불변 객체로 만들면 코드 상에서 참조를 바꾸는 실수를 방지하여 불변 싱글톤을 보장할 수 있다.

<br/>

**해결·보완 포인트**
- **가장 많이 권장되는 일반적 구현**

---

### ⑥ Enum 싱글톤

```java
public enum Config {
    INSTANCE;
}
```

**특징**
- ✔ 리플렉션·직렬화·DCL 모두 안전
  - 리플렉션이 뭐지❓❓❓
- ✔ 코드 최단
- ❌ 다른 클래스를 상속할 수 없음
- ❌ 지속적인 핫-리로드 환경에서 enum 재정의 어려움

<br/>

**해결·보완 포인트**
- 설정, 로그처럼 "절대 하나"인 객체에 적합

---

## 3. Spring의 Singleton Scope

**Spring 등 DI 컨테이너의 "singleton scope"** 는 내부적으로 ⑤와 유사한 Holder 기법 + 동적 proxy로 관리하며,  
테스트 격리·라이프사이클·클래스로더 문제를 프레임워크가 대신 해결한다.

---

## 4. 공통 문제점 & 대응 전략

| 범주 | 발생 원인                                  | 대응 방법                                               |
|------|----------------------------------------|-----------------------------------------------------|
| **Thread Safety** | Lazy 초기화 중 동시 접근                       | ③,④,⑤,⑥으로 구현 또는 컨테이너 관리                             |
| **Reflection 우회** | `setAccessible(true)`로  <br/>private 생성자 호출 | ① 생성자 내부에서 "두 번째 호출" 감지 후 예외 throw<br>② Enum 싱글톤 사용 |
| **직렬화 / 역직렬화 복제** | `readObject()` 가 새로운 객체 생성             | `readResolve()` 메서드 구현 or Enum 싱글톤                  |
| **클래스 로더 분리** | 각 로더마다 독립적 정적영역                        | 루트 로더에 클래스를 위치 or JNDI/Registry를 통해 공유              |
| **테스트 격리·대체 어려움** | 전역 상태 고정                               | 인터페이스 뒤에서 팩토리 주입(DI), 테스트용 리셋 메서드,  <br/>컨테이너 빈으로 전환     |
| **과도한 전역 의존성** | 무분별한 `getInstance()` 호출                | "정말 하나뿐이어야 하는 객체"로 범위 제한, 대부분은 DI 빈으로 전환            |

---

## 5. 정리

1. **싱글톤은 "전역에서 단 1개"를 강제하는 생성 패턴**이다.


2. 구현은 **Eager → Lazy(synchronized) → DCL → Holder → Enum** 순으로 진화해 왔으며,  
최신 JVM에서는 **Holder (일반용)** 또는 **Enum (절대적 단일·보안 중요 객체용)** 이 가장 안전·간결하다.

   
3. 주요 위험은 **동시성, 리플렉션, 직렬화, 클래스 로더, 테스트 격리** 다섯 가지이며, 각 구현마다 이를 해결·완화하는 기법이 다르다.


4. **DI 프레임워크**를 사용할 수 있다면 "컨테이너-singleton 스코프"가 대부분의 문제(생성 시점, 라이프사이클, 테스트)를 대신 해결해 주므로,  
**직접 싱글톤을 구현할 일은 오히려 줄어드는 추세**다.

---

## 6. 권장사항

### 🎯 일반적인 용도
```java
// Holder 패턴 사용 (⑤번)
public class DatabaseConnection {
    private DatabaseConnection() {}
    
    private static class Holder {
        private static final DatabaseConnection INSTANCE = new DatabaseConnection();
    }
    
    public static DatabaseConnection getInstance() {
        return Holder.INSTANCE;
    }
}
```

### 🔒 보안이 중요한 경우
```java
// Enum 사용 (⑥번)
public enum SecurityManager {
    INSTANCE;
    
    public void doSecurityCheck() {
        // 보안 로직
    }
}
```

### 🌱 Spring 환경
```java
// DI 컨테이너 활용
@Component
@Scope("singleton") // 기본값
public class MyService {
    // Spring이 싱글톤으로 관리
}
```
