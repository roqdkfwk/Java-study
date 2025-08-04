# 📦 Boxing과 Unboxing

Java에서 primitive 타입과 wrapper 클래스 간의 자동 변환 메커니즘

## 📑 목차

1. [Boxing과 Unboxing 개념](#1-boxing과-unboxing-개념)
2. [Primitive 타입과 Wrapper 클래스](#2-primitive-타입과-wrapper-클래스)
3. [Auto Boxing (자동 박싱)](#3-auto-boxing-자동-박싱)
4. [Auto Unboxing (자동 언박싱)](#4-auto-unboxing-자동-언박싱)
5. [Boxing/Unboxing이 발생하는 상황](#5-boxingunboxing이-발생하는-상황)
6. [성능 고려사항](#6-성능-고려사항)
7. [주의사항 및 예외 상황](#7-주의사항-및-예외-상황)
8. [실무 활용 예시](#8-실무-활용-예시)

---

## 1. Boxing과 Unboxing 개념

### 🎯 Boxing (박싱)
**primitive 타입**을 해당하는 **wrapper 클래스 객체**로 변환하는 과정

```java
int primitiveInt = 10;
Integer wrapperInt = Integer.valueOf(primitiveInt); // Boxing
```

### 🎯 Unboxing (언박싱)
**wrapper 클래스 객체**를 해당하는 **primitive 타입**으로 변환하는 과정

```java
Integer wrapperInt = new Integer(10);
int primitiveInt = wrapperInt.intValue(); // Unboxing
```

### 🚀 Auto Boxing/Unboxing (Java 5+)
Java 5부터 컴파일러가 **자동으로** boxing과 unboxing을 수행

```java
// Auto Boxing
int primitiveInt = 10;
Integer wrapperInt = primitiveInt; // 컴파일러가 Integer.valueOf(primitiveInt) 자동 호출

// Auto Unboxing  
Integer wrapperInt = new Integer(10);
int primitiveInt = wrapperInt; // 컴파일러가 wrapperInt.intValue() 자동 호출
```

---

## 2. Primitive 타입과 Wrapper 클래스

### 📋 대응 관계표

| Primitive 타입 | Wrapper 클래스 | Boxing 메서드 | Unboxing 메서드 |
|---------------|--------------|-------------|----------------|
| `byte`        | `Byte`       | `Byte.valueOf()` | `byteValue()` |
| `short`       | `Short`      | `Short.valueOf()` | `shortValue()` |
| `int`         | `Integer`    | `Integer.valueOf()` | `intValue()` |
| `long`        | `Long`       | `Long.valueOf()` | `longValue()` |
| `float`       | `Float`      | `Float.valueOf()` | `floatValue()` |
| `double`      | `Double`     | `Double.valueOf()` | `doubleValue()` |
| `char`        | `Character`  | `Character.valueOf()` | `charValue()` |
| `boolean`     | `Boolean`    | `Boolean.valueOf()` | `booleanValue()` |

---

## 3. Auto Boxing (자동 박싱)

### ✨ 기본 사용법

```java
public class AutoBoxingExample {
    public static void main(String[] args) {
        // 자동 박싱 - 컴파일러가 Integer.valueOf(10) 호출
        Integer num1 = 10;
        
        // 리스트에 primitive 값 추가 시 자동 박싱
        List<Integer> numbers = new ArrayList<>();
        numbers.add(20); // Integer.valueOf(20)으로 자동 변환
        
        // 메서드 파라미터에서 자동 박싱
        printInteger(30); // Integer.valueOf(30)으로 자동 변환
    }
    
    public static void printInteger(Integer value) {
        System.out.println("Integer value: " + value);
    }
}
```

### 🔍 Boxing이 발생하는 주요 상황

```java
// 1. 할당 시
Integer boxedInt = 100;

// 2. 메서드 파라미터 전달 시
public void processInteger(Integer value) { /* ... */ }
processInteger(200); // int 200이 Integer로 박싱

// 3. 컬렉션에 추가 시
List<Integer> list = new ArrayList<>();
list.add(300); // int 300이 Integer로 박싱

// 4. 제네릭 타입 사용 시
Map<String, Integer> map = new HashMap<>();
map.put("key", 400); // int 400이 Integer로 박싱
```

---

## 4. Auto Unboxing (자동 언박싱)

### ✨ 기본 사용법

```java
public class AutoUnboxingExample {
    public static void main(String[] args) {
        Integer wrapperInt = new Integer(50);
        
        // 자동 언박싱 - 컴파일러가 wrapperInt.intValue() 호출
        int primitiveInt = wrapperInt;
        
        // 산술 연산에서 자동 언박싱
        Integer a = 10;
        Integer b = 20;
        int result = a + b; // a.intValue() + b.intValue()로 자동 변환
        
        // 조건문에서 자동 언박싱
        Boolean flag = true;
        if (flag) { // flag.booleanValue()로 자동 변환
            System.out.println("True condition");
        }
    }
}
```

### 🔍 Unboxing이 발생하는 주요 상황

```java
// 1. 할당 시
Integer boxedInt = 100;
int primitiveInt = boxedInt; // 자동 언박싱

// 2. 산술 연산 시
Integer x = 10;
Integer y = 20;
int sum = x + y; // x.intValue() + y.intValue()

// 3. 비교 연산 시
Integer num1 = 50;
Integer num2 = 60;
boolean result = num1 < num2; // num1.intValue() < num2.intValue()

// 4. 메서드 파라미터 전달 시
public void processPrimitive(int value) { /* ... */ }
Integer boxedValue = 75;
processPrimitive(boxedValue); // boxedValue.intValue()로 언박싱

// 5. 배열 인덱스로 사용 시
Integer index = 3;
int[] array = {1, 2, 3, 4, 5};
int value = array[index]; // array[index.intValue()]
```

---

## 5. Boxing/Unboxing이 발생하는 상황

### 📊 상황별 정리

```java
public class BoxingUnboxingSituations {
    public static void main(String[] args) {
        // === Boxing 상황 ===
        
        // 1. 컬렉션 사용
        List<Integer> intList = new ArrayList<>();
        intList.add(1); // Boxing: int -> Integer
        
        // 2. 제네릭 타입 할당
        Integer genericInt = 2; // Boxing: int -> Integer
        
        // 3. 조건부 연산자 (혼합 타입)
        Integer boxed = 10;
        int primitive = 20;
        Integer result1 = true ? boxed : primitive; // primitive이 Boxing됨
        
        // === Unboxing 상황 ===
        
        // 1. 산술 연산
        Integer a = 10, b = 20;
        int sum = a + b; // Unboxing: a.intValue() + b.intValue()
        
        // 2. 증감 연산자
        Integer counter = 0;
        counter++; // Unboxing -> 연산 -> Boxing (counter = Integer.valueOf(counter.intValue() + 1))
        
        // 3. 논리 연산
        Boolean flag1 = true, flag2 = false;
        boolean logicalResult = flag1 && flag2; // 두 Boolean 모두 Unboxing
        
        // 4. 배열 접근
        Integer index = 2;
        int[] array = {10, 20, 30, 40, 50};
        int value = array[index]; // index가 Unboxing됨
    }
}
```

### 🔄 복합적인 Boxing/Unboxing

```java
public class ComplexBoxingUnboxing {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
        
        // 복합적인 Boxing/Unboxing이 발생
        int sum = 0;
        for (Integer number : numbers) { // number는 Boxing된 Integer
            sum += number; // Unboxing: number.intValue()
        }
        
        // 결과를 다시 Boxing
        Integer totalSum = sum; // Boxing: Integer.valueOf(sum)
        System.out.println("Total sum: " + totalSum);
    }
}
```

---

## 6. 성능 고려사항

### ⚡ 성능 비교

```java
public class PerformanceComparison {
    public static void main(String[] args) {
        long startTime, endTime;
        
        // === Primitive 타입 연산 ===
        startTime = System.nanoTime();
        int primitiveSum = 0;
        for (int i = 0; i < 1000000; i++) {
            primitiveSum += i; // 순수한 primitive 연산
        }
        endTime = System.nanoTime();
        System.out.println("Primitive 연산 시간: " + (endTime - startTime) + " ns");
        
        // === Wrapper 클래스 연산 (Boxing/Unboxing 발생) ===
        startTime = System.nanoTime();
        Integer wrapperSum = 0;
        for (Integer i = 0; i < 1000000; i++) { // Boxing/Unboxing이 반복 발생
            wrapperSum += i; // wrapperSum.intValue() + i.intValue(), 결과 Boxing
        }
        endTime = System.nanoTime();
        System.out.println("Wrapper 연산 시간: " + (endTime - startTime) + " ns");
    }
}
```

### 💡 성능 최적화 팁

```java
public class OptimizationTips {
    
    // ❌ 비효율적 - 불필요한 Boxing/Unboxing
    public static Integer inefficientSum(List<Integer> numbers) {
        Integer sum = 0; // Boxing 발생
        for (Integer number : numbers) {
            sum += number; // Unboxing + 연산 + Boxing 반복 발생
        }
        return sum;
    }
    
    // ✅ 효율적 - Boxing/Unboxing 최소화
    public static Integer efficientSum(List<Integer> numbers) {
        int sum = 0; // primitive 타입 사용
        for (Integer number : numbers) {
            sum += number; // Unboxing만 발생
        }
        return sum; // 마지막에 한 번만 Boxing
    }
    
    // ✅ 더 효율적 - Stream API 활용
    public static Integer streamSum(List<Integer> numbers) {
        return numbers.stream()
                     .mapToInt(Integer::intValue) // Unboxing
                     .sum(); // primitive 연산
    }
}
```

---

## 7. 주의사항 및 예외 상황

### ⚠️ NullPointerException 위험

```java
public class NullPointerRisk {
    public static void main(String[] args) {
        // ❌ 위험한 코드 - NPE 발생 가능
        Integer nullInteger = null;
        
        try {
            int result = nullInteger + 10; // NPE! nullInteger.intValue() 호출 시도
        } catch (NullPointerException e) {
            System.out.println("NullPointerException 발생: " + e.getMessage());
        }
        
        // ✅ 안전한 코드 - null 체크
        if (nullInteger != null) {
            int result = nullInteger + 10;
            System.out.println("Result: " + result);
        } else {
            System.out.println("nullInteger는 null입니다.");
        }
    }
}
```

### 🔍 객체 비교 시 주의사항

```java
public class ComparisonIssues {
    public static void main(String[] args) {
        // Integer 캐싱 (-128 ~ 127)
        Integer a1 = 100;
        Integer a2 = 100;
        System.out.println("a1 == a2: " + (a1 == a2)); // true (같은 캐시된 객체)
        
        Integer b1 = 200;
        Integer b2 = 200;
        System.out.println("b1 == b2: " + (b1 == b2)); // false (다른 객체)
        
        // ✅ 올바른 비교 방법
        System.out.println("b1.equals(b2): " + b1.equals(b2)); // true
        
        // 혼합 비교
        Integer boxed = 300;
        int primitive = 300;
        System.out.println("boxed == primitive: " + (boxed == primitive)); // true (Unboxing 발생)
    }
}
```

### 🎯 조건부 연산자 주의사항

```java
public class ConditionalOperatorIssue {
    public static void main(String[] args) {
        Integer boxedNull = null;
        int primitive = 10;
        
        try {
            // ❌ 위험 - NPE 발생
            Integer result = true ? boxedNull : primitive; // boxedNull이 Unboxing 시도됨
        } catch (NullPointerException e) {
            System.out.println("조건부 연산자에서 NPE 발생");
        }
        
        // ✅ 안전한 방법
        Integer result = (boxedNull != null) ? boxedNull : primitive;
        System.out.println("Safe result: " + result);
    }
}
```

---

## 8. 실무 활용 예시

### 💼 실무에서 자주 만나는 상황들

```java
public class RealWorldExamples {
    
    // 1. 데이터베이스 결과 처리
    public static void processDbResult(ResultSet rs) throws SQLException {
        // DB에서 가져온 값은 null일 수 있으므로 Integer 사용
        Integer userId = rs.getObject("user_id", Integer.class);
        
        if (userId != null) {
            // 비즈니스 로직에서는 primitive 타입 사용
            int primitiveUserId = userId; // Unboxing
            processUser(primitiveUserId);
        }
    }
    
    // 2. JSON 데이터 파싱
    public static class UserDto {
        private Integer age; // null 허용을 위해 Integer 사용
        private String name;
        
        public void validateAge() {
            if (age != null && age > 0) { // age != null 체크 필수
                System.out.println("Valid age: " + age);
            }
        }
    }
    
    // 3. 컬렉션과 스트림 처리
    public static void processNumbers(List<String> numberStrings) {
        List<Integer> numbers = numberStrings.stream()
            .map(Integer::valueOf) // String -> Integer (Boxing)
            .filter(num -> num > 0) // Unboxing for comparison
            .collect(Collectors.toList());
        
        // 합계 계산 - 효율적인 방법
        int sum = numbers.stream()
            .mapToInt(Integer::intValue) // Unboxing to IntStream
            .sum(); // primitive 연산
        
        System.out.println("Sum: " + sum);
    }
    
    // 4. 캐시 활용 최적화
    public static void cacheOptimization() {
        // -128 ~ 127 범위의 Integer는 캐시됨
        List<Integer> cachedNumbers = new ArrayList<>();
        for (int i = -128; i <= 127; i++) {
            cachedNumbers.add(i); // 캐시된 Integer 객체 재사용
        }
        
        // 캐시 범위 밖의 숫자는 새 객체 생성
        List<Integer> newNumbers = new ArrayList<>();
        for (int i = 1000; i <= 1100; i++) {
            newNumbers.add(i); // 매번 새로운 Integer 객체 생성
        }
    }
    
    private static void processUser(int userId) {
        System.out.println("Processing user: " + userId);
    }
}
```

### 🎯 최적화된 코드 패턴

```java
public class OptimizedPatterns {
    
    // 효율적인 컬렉션 처리
    public static int sumEvenNumbers(List<Integer> numbers) {
        return numbers.stream()
            .filter(n -> n != null) // null 체크
            .mapToInt(Integer::intValue) // primitive stream으로 변환
            .filter(n -> n % 2 == 0) // primitive 연산
            .sum(); // primitive 연산
    }
    
    // 안전한 null 처리
    public static Integer safeAdd(Integer a, Integer b) {
        if (a == null || b == null) {
            return null;
        }
        return a + b; // Unboxing -> 연산 -> Boxing
    }
    
    // 성능을 고려한 반복문
    public static long efficientLoop(List<Integer> numbers) {
        long sum = 0; // primitive 타입 사용
        for (Integer number : numbers) {
            if (number != null) {
                sum += number; // Unboxing만 발생
            }
        }
        return sum;
    }
}
```

---

## 🎓 핵심 정리

### ✅ 반드시 기억할 것들

1. **Auto Boxing/Unboxing**은 Java 5부터 지원되는 자동 변환 기능
2. **성능**: primitive 타입이 wrapper 클래스보다 메모리와 속도 면에서 효율적
3. **NullPointerException**: wrapper 클래스는 null 값을 가질 수 있어 Unboxing 시 NPE 위험
4. **객체 비교**: `==` 연산자 사용 시 주의 (Integer 캐싱 고려)
5. **실무**: DB 결과나 JSON 파싱 등 null 가능한 상황에서는 wrapper 클래스 사용

### 💡 Best Practices

- **Primitive 타입 우선 사용**: 성능과 메모리 효율성을 위해
- **Null 체크 필수**: wrapper 클래스 사용 시 항상 null 체크
- **equals() 메서드 사용**: 객체 값 비교 시 `==` 대신 `equals()` 사용
- **스트림 최적화**: `mapToInt()`, `mapToDouble()` 등을 활용해 primitive 스트림 사용
- **반복문 최적화**: 대량 데이터 처리 시 Boxing/Unboxing 최소화

---