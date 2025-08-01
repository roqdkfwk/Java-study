# TreeSet

## 📖 TreeSet이란?

**JCF(Java Collection Framework)**의 일부로, 정렬된 순서로 요소를 저장하는 `Set` 인터페이스의 구현체입니다.

**이진탐색트리(Binary Search Tree)**의 구조로 되어있어, 데이터를 넣을 때 자동으로 정렬됩니다.

일반적인 `Set`보다 데이터 추가, 삭제에는 시간이 오래 걸리지만 정렬되어 저장된다는 점 때문에 조회가 빠릅니다.

**기본 정렬은 오름차순 정렬**이며, 생성자의 매개변수로 `Comparator` 클래스를 구현하면 정렬 방법을 설정할 수 있습니다.

---

## 🔍 TreeSet의 특징

### 1️⃣ 정렬된 순서 유지
- `TreeSet`은 요소를 자동으로 **오름차순으로 정렬**합니다
- 내부적으로 이진탐색트리, 그 중에서도 **레드-블랙 트리로 구현**되어 있습니다
- `Comparator`를 사용하여 사용자가 정렬 순서를 정의할 수 있습니다

```java
TreeSet<Integer> set = new TreeSet<>(Comparator.reverseOrder());  // 내림차순으로 정렬
set.add(5);
set.add(1);
set.add(7);

Iterator<Integer> iter = set.iterator();
while (iter.hasNext()) {
    System.out.println(iter.next());
}

// 출력결과: 7 5 1
```

### 2️⃣ 중복 불허
- `Set` 인터페이스를 구현하므로, **중복된 요소를 허용하지 않습니다**
- 이미 존재하는 요소를 추가하려고 하면, 추가 동작이 무시됩니다

### 3️⃣ 빠른 검색, 추가, 제거
- 요소 추가, 제거, 검색 등의 기본 연산의 시간 복잡도는 **`O(log n)`**입니다

---

## 📋 주요 메소드

| 메서드 | 설명 | 시간복잡도 |
|--------|------|-----------|
| `boolean add(E e)` | 주어진 객체를 저장 후 성공하면 `true`, 중복 객체면 `false`를 반환 | `O(log n)` |
| `boolean remove(Object o)` | 특정 요소(o)를 제거한다. 제거에 성공하면 `true`, 특정 요소가 존재하지 않으면 `false`를 반환 | `O(log n)` |
| `boolean contains(Object o)` | 특정 요소(o)가 포함되어 있는지 확인한다. 특정 요소가 Set에 존재하면 `true`, 존재하지 않으면 `false`를 반환 | `O(log n)` |
| `E first()` | Set에서 가장 작은 요소를 반환. Set이 비어 있으면 `NoSuchElementException`이 발생 | `O(log n)` |
| `E last()` | Set에서 가장 큰 요소를 반환. Set이 비어 있으면 `NoSuchElementException`이 발생 | `O(log n)` |
| `E pollFirst()` | Set에서 가장 작은 요소를 제거하고 반환. Set이 비어 있으면 `null`을 반환 | `O(log n)` |
| `E pollLast()` | Set에서 가장 큰 요소를 제거하고 반환. Set이 비어 있으면 `null`을 반환 | `O(log n)` |
| `NavigableSet<E> subSet(E fromElement, E toElement)` | 지정된 범위의 요소들을 `NavigableSet<E>`형태로 반환 | `O(log n)` + 범위 내 요소 개수 |
| `NavigableSet<E> headSet(E toElement)` | 지정된 요소보다 작은 모든 요소들을 `NavigableSet<E>`형태로 반환 | `O(log n)` + 범위 내 요소 개수 |
| `NavigableSet<E> tailSet(E fromElement)` | 지정된 요소보다 크거나 같은 모든 요소들을 `NavigableSet<E>`형태로 반환 | `O(log n)` + 범위 내 요소 개수 |
| `int size()` | Set에 포함된 요소의 개수를 반환 | `O(1)` |
| `void clear()` | Set에 포함된 모든 요소를 제거 | `O(n)` |

---

## 💻 TreeSet 사용법

### 🔹 TreeSet 선언

```java
TreeSet<Integer> set = new TreeSet<>();  // TreeSet 생성
set.add(7);
set.add(4);
set.add(9);
set.add(1);
set.add(5);

System.out.println(set);  // [1, 4, 5, 7, 9] - 자동 정렬됨
```

### 🔹 TreeSet에 값이 추가되는 과정

이진 검색 트리에 7, 4, 9, 1, 5의 순서로 값을 저장하는 경우:

```
        7
       / \
      4   9
     / \
    1   5
```

### 🔹 TreeSet 값 삭제

```java
TreeSet<Integer> set = new TreeSet<>();
set.add(1);
set.add(2);
set.add(3);

set.remove(1);  // set에서 1을 제거
System.out.println(set);  // [2, 3]

set.clear();  // set에서 모든 값을 제거
System.out.println(set);  // []
```

### 🔹 TreeSet 크기 구하기

```java
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(1, 2, 3));  // 초기값 지정
System.out.println(set.size());  // 3
```

### 🔹 TreeSet 값 출력

```java
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(4, 2, 3));  // 초기값 지정

System.out.println(set);         // [2, 3, 4] - 자동 정렬됨
System.out.println(set.first()); // 2 - 가장 작은 값
System.out.println(set.last());  // 4 - 가장 큰 값

// Iterator 사용
Iterator<Integer> iter = set.iterator();
while (iter.hasNext()) {
    System.out.println(iter.next());
}

// Enhanced for문 사용
for (Integer value : set) {
    System.out.println(value);
}
```

### 🔹 TreeSet 범위 검색

```java
TreeSet<Integer> set = new TreeSet<>();
set.addAll(Arrays.asList(1, 3, 5, 7, 9, 11, 13));

// subSet: 3 이상 9 미만
NavigableSet<Integer> subSet = set.subSet(3, 9);
System.out.println(subSet);  // [3, 5, 7]

// headSet: 7 미만
NavigableSet<Integer> headSet = set.headSet(7);
System.out.println(headSet);  // [1, 3, 5]

// tailSet: 7 이상
NavigableSet<Integer> tailSet = set.tailSet(7);
System.out.println(tailSet);  // [7, 9, 11, 13]
```

---

## 🆚 다른 Set 구현체와 비교

| 특징 | HashSet | LinkedHashSet | TreeSet |
|------|---------|---------------|---------|
| **정렬** | ❌ | ❌ | ✅ (자동 정렬) |
| **삽입 순서 유지** | ❌ | ✅ | ❌ |
| **시간 복잡도** | O(1) | O(1) | O(log n) |
| **메모리 사용량** | 적음 | 보통 | 많음 |
| **사용 시기** | 단순한 중복 제거 | 삽입 순서 중요 | 정렬된 데이터 필요 |

---

## 🔗 관련 문서

- [📖 **Iterator 사용법 자세히 보기**](./Iterator.md) - TreeSet 순회를 위한 Iterator 사용법과 주의사항

---

## ⚠️ 주의사항

### 1️⃣ Comparable 구현 필요
TreeSet에 저장되는 객체는 `Comparable` 인터페이스를 구현하거나, `Comparator`를 제공해야 합니다.

```java
// 에러 발생 - Comparable을 구현하지 않은 클래스
class Person {
    String name;
    int age;
}

TreeSet<Person> set = new TreeSet<>();  // ClassCastException 발생 가능
```

### 2️⃣ null 값 허용하지 않음
TreeSet은 null 값을 허용하지 않습니다.

```java
TreeSet<String> set = new TreeSet<>();
set.add(null);  // NullPointerException 발생
```

### 3️⃣ 동기화되지 않음
TreeSet은 스레드 안전하지 않습니다. 멀티스레드 환경에서는 동기화가 필요합니다.

```java
// 동기화된 TreeSet 생성
Set<Integer> syncSet = Collections.synchronizedSet(new TreeSet<>());
```

---

## 🎯 언제 사용할까?

### ✅ TreeSet을 사용하면 좋은 경우
- **정렬된 순서로 데이터를 유지**해야 할 때
- **범위 검색**이 자주 필요할 때
- **중복을 제거하면서 정렬**도 필요할 때
- **최솟값, 최댓값**을 자주 조회해야 할 때

### ❌ TreeSet을 피해야 하는 경우
- **단순한 중복 제거**만 필요할 때 (HashSet 사용)
- **삽입 순서**를 유지해야 할 때 (LinkedHashSet 사용)
- **성능이 중요**하고 정렬이 불필요할 때
- **대용량 데이터**를 다룰 때 (O(log n) 오버헤드)

---

## 📚 실무 활용 예시

```java
// 학생 성적 관리 시스템
public class ScoreManager {
    private TreeSet<Integer> scores = new TreeSet<>();
    
    public void addScore(int score) {
        scores.add(score);
    }
    
    public int getHighestScore() {
        return scores.last();
    }
    
    public int getLowestScore() {
        return scores.first();
    }
    
    public Set<Integer> getScoresInRange(int min, int max) {
        return scores.subSet(min, max + 1);
    }
    
    public void printAllScores() {
        System.out.println("모든 성적 (정렬됨): " + scores);
    }
}
```

TreeSet은 **정렬과 중복 제거를 동시에 처리**해야 하는 상황에서 매우 유용한 자료구조입니다! 🚀