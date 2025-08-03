# Iterator

## 📖 Iterator란?

**Iterator**는 Java Collection Framework에서 컬렉션의 요소들을 **순회(반복)하기 위한 표준 인터페이스**입니다.

컬렉션의 내부 구조를 노출하지 않고도 모든 요소에 접근할 수 있는 **통일된 방법**을 제공합니다.

---

## 🔍 Iterator의 특징

### 1️⃣ 표준화된 순회 방식
- 모든 Collection 구현체에서 동일한 방식으로 요소 순회 가능
- 컬렉션의 타입에 관계없이 일관된 코드 작성 가능

### 2️⃣ 안전한 요소 제거
- 순회 중에 안전하게 요소를 제거할 수 있음
- `ConcurrentModificationException` 방지

### 3️⃣ Fail-Fast 메커니즘
- 순회 중 다른 스레드가 컬렉션을 수정하면 즉시 예외 발생
- 데이터 무결성 보장

---

## 📋 Iterator 인터페이스의 주요 메서드

| 메서드 | 반환 타입 | 설명 | 예외 |
|--------|-----------|------|------|
| `hasNext()` | `boolean` | 다음 요소가 있는지 확인 | - |
| `next()` | `E` | 다음 요소를 반환하고 커서를 이동 | `NoSuchElementException` |
| `remove()` | `void` | 현재 요소를 제거 (선택적 구현) | `UnsupportedOperationException`, `IllegalStateException` |
| `forEachRemaining(Consumer<? super E> action)` | `void` | 남은 모든 요소에 대해 작업 수행 (Java 8+) | - |

---

## 💻 Iterator 기본 사용법

### 🔹 기본적인 순회

```java
import java.util.*;

public class IteratorExample {
    public static void main(String[] args) {
        List<String> fruits = Arrays.asList("apple", "banana", "cherry", "date");
        
        // Iterator 생성
        Iterator<String> iter = fruits.iterator();
        
        // 순회
        while (iter.hasNext()) {
            String fruit = iter.next();
            System.out.println(fruit);
        }
    }
}

// 출력:
// apple
// banana  
// cherry
// date
```

### 🔹 안전한 요소 제거

```java
public class SafeRemovalExample {
    public static void main(String[] args) {
        List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5, 6));
        
        Iterator<Integer> iter = numbers.iterator();
        while (iter.hasNext()) {
            Integer number = iter.next();
            if (number % 2 == 0) {  // 짝수 제거
                iter.remove();
            }
        }
        
        System.out.println(numbers);  // [1, 3, 5]
    }
}
```

### 🔹 잘못된 사용 예시 (ConcurrentModificationException)

```java
public class WrongRemovalExample {
    public static void main(String[] args) {
        List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
        
        // ❌ 잘못된 방법 - ConcurrentModificationException 발생
        for (Integer number : numbers) {
            if (number % 2 == 0) {
                numbers.remove(number);  // 위험!
            }
        }
        
        // ✅ 올바른 방법
        Iterator<Integer> iter = numbers.iterator();
        while (iter.hasNext()) {
            Integer number = iter.next();
            if (number % 2 == 0) {
                iter.remove();  // 안전!
            }
        }
    }
}
```

---

## 🔄 Iterator의 종류

### 1️⃣ Iterator
- 기본적인 순방향 순회 인터페이스
- 모든 Collection에서 지원

```java
Collection<String> collection = new ArrayList<>();
Iterator<String> iter = collection.iterator();
```

### 2️⃣ ListIterator
- `List` 전용 인터페이스
- **양방향 순회** 및 **요소 수정** 가능

```java
List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
ListIterator<String> listIter = list.listIterator();

// 순방향 순회
while (listIter.hasNext()) {
    System.out.println(listIter.next());
}

// 역방향 순회
while (listIter.hasPrevious()) {
    System.out.println(listIter.previous());
}
```

#### ListIterator 추가 메서드

| 메서드 | 설명 |
|--------|------|
| `hasPrevious()` | 이전 요소가 있는지 확인 |
| `previous()` | 이전 요소를 반환하고 커서를 뒤로 이동 |
| `nextIndex()` | 다음 요소의 인덱스 반환 |
| `previousIndex()` | 이전 요소의 인덱스 반환 |
| `set(E e)` | 현재 요소를 새로운 값으로 교체 |
| `add(E e)` | 현재 위치에 새로운 요소 추가 |

### 3️⃣ Spliterator (Java 8+)
- **병렬 처리**를 위한 Iterator
- Stream API에서 내부적으로 사용

---

## 🎯 컬렉션별 Iterator 특징

### 📋 List의 Iterator

```java
List<String> list = new ArrayList<>(Arrays.asList("first", "second", "third"));

// 기본 Iterator
Iterator<String> iter = list.iterator();

// ListIterator (양방향)
ListIterator<String> listIter = list.listIterator();

// 특정 위치부터 시작하는 ListIterator
ListIterator<String> listIter2 = list.listIterator(1); // "second"부터 시작
```

### 🔢 Set의 Iterator

```java
Set<Integer> set = new TreeSet<>(Arrays.asList(3, 1, 4, 1, 5));
Iterator<Integer> iter = set.iterator();

while (iter.hasNext()) {
    System.out.println(iter.next());  // 1, 3, 4, 5 (정렬됨, 중복 제거됨)
}
```

### 🗝️ Map의 Iterator

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 100);
map.put("banana", 200);
map.put("cherry", 300);

// Key에 대한 Iterator
Iterator<String> keyIter = map.keySet().iterator();

// Value에 대한 Iterator  
Iterator<Integer> valueIter = map.values().iterator();

// Entry에 대한 Iterator
Iterator<Map.Entry<String, Integer>> entryIter = map.entrySet().iterator();
while (entryIter.hasNext()) {
    Map.Entry<String, Integer> entry = entryIter.next();
    System.out.println(entry.getKey() + " = " + entry.getValue());
}
```

---

## ⚡ Iterator vs Enhanced For Loop

### Enhanced For Loop (for-each)
```java
List<String> list = Arrays.asList("A", "B", "C");

// Enhanced for loop
for (String item : list) {
    System.out.println(item);
}
```

### 비교표

| 특징 | Iterator | Enhanced For Loop |
|------|----------|-------------------|
| **코드 간결성** | 보통 | 매우 간결 |
| **요소 제거** | ✅ 가능 (`remove()`) | ❌ 불가능 |
| **인덱스 접근** | ❌ 불가능 | ❌ 불가능 |
| **양방향 순회** | ✅ 가능 (ListIterator) | ❌ 불가능 |
| **조건부 제거** | ✅ 안전 | ❌ 위험 |
| **성능** | 동일 | 동일 (내부적으로 Iterator 사용) |

### 사용 지침

#### ✅ Iterator를 사용해야 하는 경우
- 순회 중 요소를 **제거**해야 할 때
- **양방향 순회**가 필요할 때 (ListIterator)
- **조건부 요소 제거**가 필요할 때
- 순회 과정을 **세밀하게 제어**해야 할 때

#### ✅ Enhanced For Loop를 사용해야 하는 경우
- 단순히 **모든 요소를 읽기**만 할 때
- **코드의 간결성**이 중요할 때
- 요소를 **수정하지 않을** 때

---

## 🔧 Iterator 고급 활용

### 1️⃣ forEachRemaining() 사용 (Java 8+)

```java
List<String> list = Arrays.asList("apple", "banana", "cherry");
Iterator<String> iter = list.iterator();

// 첫 번째 요소는 따로 처리
if (iter.hasNext()) {
    System.out.println("첫 번째: " + iter.next());
}

// 나머지 요소들을 람다로 처리
iter.forEachRemaining(item -> System.out.println("나머지: " + item));
```

### 2️⃣ 커스텀 Iterator 구현

```java
public class NumberRange implements Iterable<Integer> {
    private int start;
    private int end;
    
    public NumberRange(int start, int end) {
        this.start = start;
        this.end = end;
    }
    
    @Override
    public Iterator<Integer> iterator() {
        return new Iterator<Integer>() {
            private int current = start;
            
            @Override
            public boolean hasNext() {
                return current <= end;
            }
            
            @Override
            public Integer next() {
                if (!hasNext()) {
                    throw new NoSuchElementException();
                }
                return current++;
            }
        };
    }
}

// 사용 예시
NumberRange range = new NumberRange(1, 5);
for (Integer number : range) {
    System.out.println(number);  // 1, 2, 3, 4, 5
}
```

### 3️⃣ 멀티 컬렉션 동시 순회

```java
public class MultiIteratorExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
        List<Integer> ages = Arrays.asList(25, 30, 35);
        
        Iterator<String> nameIter = names.iterator();
        Iterator<Integer> ageIter = ages.iterator();
        
        while (nameIter.hasNext() && ageIter.hasNext()) {
            System.out.println(nameIter.next() + " is " + ageIter.next() + " years old");
        }
    }
}
```

---

## ⚠️ 주의사항과 모범 사례

### 1️⃣ ConcurrentModificationException 방지

```java
// ❌ 잘못된 예시
List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
for (String item : list) {
    if ("B".equals(item)) {
        list.remove(item);  // Exception 발생!
    }
}

// ✅ 올바른 예시
Iterator<String> iter = list.iterator();
while (iter.hasNext()) {
    String item = iter.next();
    if ("B".equals(item)) {
        iter.remove();  // 안전!
    }
}
```

### 2️⃣ next() 호출 전 hasNext() 확인

```java
// ❌ 위험한 코드
Iterator<String> iter = list.iterator();
String first = iter.next();  // NoSuchElementException 가능!

// ✅ 안전한 코드
Iterator<String> iter = list.iterator();
if (iter.hasNext()) {
    String first = iter.next();
}
```

### 3️⃣ Iterator 재사용 금지

```java
List<String> list = Arrays.asList("A", "B", "C");
Iterator<String> iter = list.iterator();

// 첫 번째 순회
while (iter.hasNext()) {
    System.out.println(iter.next());
}

// ❌ 같은 Iterator로 다시 순회 시도 (아무것도 출력되지 않음)
while (iter.hasNext()) {
    System.out.println(iter.next());
}

// ✅ 새로운 Iterator 생성
iter = list.iterator();
while (iter.hasNext()) {
    System.out.println(iter.next());
}
```

---

## 🎯 실무 활용 예시

### 📋 데이터 필터링과 변환

```java
public class DataProcessor {
    public static List<String> processData(List<String> data) {
        List<String> result = new ArrayList<>();
        Iterator<String> iter = data.iterator();
        
        while (iter.hasNext()) {
            String item = iter.next();
            // 조건에 맞는 데이터만 변환하여 추가
            if (item != null && item.length() > 2) {
                result.add(item.toUpperCase());
            }
        }
        
        return result;
    }
}
```

### 🔍 대용량 데이터 배치 처리

```java
public class BatchProcessor<T> {
    private final int batchSize;
    
    public BatchProcessor(int batchSize) {
        this.batchSize = batchSize;
    }
    
    public void processBatches(Collection<T> data, Consumer<List<T>> batchProcessor) {
        Iterator<T> iter = data.iterator();
        
        while (iter.hasNext()) {
            List<T> batch = new ArrayList<>();
            
            // 배치 크기만큼 데이터 수집
            for (int i = 0; i < batchSize && iter.hasNext(); i++) {
                batch.add(iter.next());
            }
            
            // 배치 처리
            if (!batch.isEmpty()) {
                batchProcessor.accept(batch);
            }
        }
    }
}

// 사용 예시
List<Integer> largeDataSet = IntStream.range(1, 1001).boxed().collect(Collectors.toList());
BatchProcessor<Integer> processor = new BatchProcessor<>(50);

processor.processBatches(largeDataSet, batch -> {
    System.out.println("Processing batch of size: " + batch.size());
    // 실제 배치 처리 로직
});
```

---

## 📚 요약

**Iterator**는 Java에서 컬렉션 순회를 위한 표준 인터페이스로, 다음과 같은 장점을 제공합니다:

### ✅ 핵심 장점
- **표준화된 순회 방식** - 모든 컬렉션에서 일관된 사용법
- **안전한 요소 제거** - 순회 중 요소 제거 시 예외 발생 방지
- **Fail-Fast 메커니즘** - 동시 수정 시 즉시 예외 발생으로 데이터 무결성 보장

### 🎯 사용 지침
- **요소 제거**가 필요하면 **Iterator** 사용
- **단순 읽기**만 필요하면 **Enhanced For Loop** 사용
- **양방향 순회**가 필요하면 **ListIterator** 사용

Iterator를 올바르게 사용하면 안전하고 효율적인 컬렉션 처리가 가능합니다! 🚀