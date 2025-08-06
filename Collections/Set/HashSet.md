# HashSet Class

## 📁 목차

1. [기본 구조](#-기본-구조)
2. [주요 특징](#-주요-특징)
   - [장점](#-장점)
   - [단점](#-단점)
3. [HashSet 내부 구조](#-hashset-내부-구조)
   - [HashMap 기반 구현](#hashmap-기반-구현)
   - [PRESENT 더미 객체](#present-더미-객체)
   - [메모리 효율성](#-메모리-효율성)
4. [HashSet 객체 생성](#-hashset-객체-생성)
   - [기본 생성자](#기본-생성자)
   - [초기값과 함께 생성](#초기값과-함께-생성)
5. [주요 메서드](#-주요-메서드)
   - [요소 추가](#-요소-추가)
   - [요소 삭제](#-요소-삭제)
   - [요소 검색](#-요소-검색)
   - [컬렉션 연산](#-컬렉션-연산)
   - [기타 메서드](#-기타-메서드)
6. [성능 특성](#-성능-특성)
7. [HashSet vs 다른 Set 구현체 비교](#-hashset-vs-다른-set-구현체-비교)
8. [사용 권장 상황](#-사용-권장-상황)
   - [HashSet 사용이 적합한 경우](#-hashset-사용이-적합한-경우)
   - [HashSet 사용을 피해야 하는 경우](#-hashset-사용을-피해야-하는-경우)
9. [관련 문서](#-관련-문서)

---

`HashSet`은 Java Collection Framework에서 제공하는 Set 인터페이스의 대표적인 구현체로,  
해시 테이블을 이용하여 **중복을 허용하지 않는 요소들을 저장**하는 컬렉션이다.

## 🏗️ 기본 구조

- **내부 구조**  
  `HashMap`을 이용하여 구현 (배열과 연결 노드를 결합한 자료구조)
- **데이터 저장**  
  중복을 허용하지 않으며, 저장 순서를 보장하지 않음
- **접근 방식**  
  해시 함수를 사용하여 **가장 빠른 임의 검색 접근 속도**를 제공
- **특징**  
  추가, 삭제, 검색, 접근성이 모두 뛰어나지만 순서를 전혀 예측할 수 없음

## ✨ 주요 특징

### 🚀 장점
- **최고의 검색 성능**  
  해시 테이블 기반으로 평균 `O(1)` 시간에 검색 가능
- **빠른 추가/삭제**  
  해시 함수를 통해 빠른 추가/삭제 연산 지원
- **중복 자동 제거**  
  동일한 요소의 중복 저장을 자동으로 방지
- **메모리 효율성**  
  내부적으로 HashMap을 사용하지만 value에는 하나의 더미 객체만 사용

### ⚠️ 단점
- **순서 보장 안됨**  
  요소들의 저장 순서나 정렬 순서를 전혀 예측할 수 없음
- **null 값 하나만 허용**  
  null 값은 최대 하나만 저장 가능
- **스레드 안전하지 않음**  
  멀티스레드 환경에서 동기화 처리 필요

## 🔧 HashSet 내부 구조

### HashMap 기반 구현

```java
public HashSet() {
    map = new HashMap<>();
}
```

`HashSet`은 내부적으로 `HashMap`을 사용하여 데이터를 저장한다. `HashMap`은 `key-value` 쌍으로 데이터를 저장하는데,  
`HashSet`에서는 실제 데이터를 `key`에 저장하고 `value`는 더미 데이터를 사용

### PRESENT 더미 객체

```java
// Dummy value to associate with an Object in the backing Map
static final Object PRESENT = new Object();
```

`HashSet`에서는 `HashMap`의 모든 `key`에 동일한 더미 값인 `PRESENT`를 저장한다.  
이 객체는 단 한 번만 생성되어 모든 `key`가 동일한 `PRESENT` 객체를 참조

### 💾 메모리 효율성

실제 메모리에 `PRESENT` 객체는 단 한 번만 저장되고, 모든 `key`는 동일한 `PRESENT` 객체만 참조하므로 **key의 개수에 관계없이 8바이트의 메모리만 차지**합니다.

## 🔧 HashSet 객체 생성

### 기본 생성자
```java
// 기본 생성 (초기 용량 16, 로드 팩터 0.75)
HashSet<String> set = new HashSet<>();

// 초기 용량 지정
HashSet<String> set = new HashSet<>(32);

// 초기 용량과 로드 팩터 지정
HashSet<String> set = new HashSet<>(32, 0.8f);
```

### 초기값과 함께 생성
```java
// 배열을 이용한 생성
HashSet<String> set1 = new HashSet<>(Arrays.asList("A", "B", "C"));

// 다른 컬렉션으로부터 생성
HashSet<String> set2 = new HashSet<>(set1);

// Collection.of를 이용한 생성 (Java 9+)
HashSet<String> set3 = new HashSet<>(Set.of("X", "Y", "Z"));
```

## 📋 주요 메서드

### ➕ 요소 추가
| 메서드 | 설명 | 시간복잡도 |
|--------|------|------------|
| `boolean add(E e)` | Set에 요소 추가, 중복이면 false 반환 | `O(1)` |
| `boolean addAll(Collection<? extends E> c)` | 컬렉션의 모든 요소를 Set에 추가 | `O(m)` |

```java
HashSet<String> set = new HashSet<>();
set.add("Apple");     // true 반환
set.add("Banana");    // true 반환
set.add("Apple");     // false 반환 (중복)
// 결과: [Apple, Banana] (순서는 보장되지 않음)

set.addAll(Arrays.asList("Cherry", "Date"));
// 결과: [Apple, Banana, Cherry, Date] (순서는 보장되지 않음)
```

#### ⭐ add() 메서드 내부 구현
```java
public boolean add(E e) {
    return map.put(e, PRESENT) == null;
}
```
- `HashMap`의 `put()` 메서드는 기존 key가 있으면 이전 value를 반환, 없으면 null 반환
- 새로운 요소가 추가되면 null이 반환되어 `add()`는 true를 반환
- 중복 요소면 PRESENT가 반환되어 `add()`는 false를 반환

### ➖ 요소 삭제
| 메서드 | 설명 | 시간복잡도 |
|--------|------|-----------|
| `boolean remove(Object o)` | 지정된 요소 제거 | `O(1)` |
| `boolean removeAll(Collection<?> c)` | 컬렉션에 포함된 모든 요소 제거 | `O(n)` |
| `boolean retainAll(Collection<?> c)` | 컬렉션에 포함된 요소만 유지 | `O(n)` |
| `void clear()` | 모든 요소 제거 | `O(n)` |

```java
HashSet<String> set = new HashSet<>(Arrays.asList("A", "B", "C", "D"));

set.remove("B");        // true 반환
set.remove("Z");        // false 반환 (요소 없음)
// 결과: [A, C, D]

set.removeAll(Arrays.asList("A", "C"));
// 결과: [D]
```

#### ⭐ remove() 메서드 내부 구현
```java
public boolean remove(Object o) {
    return map.remove(o) == PRESENT;
}
```
- `HashMap`의 `remove()` 메서드는 제거된 value를 반환
- 요소가 존재했으면 PRESENT가 반환되어 true를 반환
- 요소가 없었으면 null이 반환되어 false를 반환

### 🔍 요소 검색
| 메서드 | 설명 | 시간복잡도 |
|--------|------|----------|
| `boolean contains(Object o)` | 지정된 요소 포함 여부 확인 | `O(1)` |
| `boolean containsAll(Collection<?> c)` | 컬렉션의 모든 요소 포함 여부 확인 | `O(n)` |
| `boolean isEmpty()` | Set이 비어있는지 확인 | `O(1)` |
| `int size()` | Set에 포함된 요소 개수 반환 | `O(1)` |

```java
HashSet<String> set = new HashSet<>(Arrays.asList("A", "B", "C"));

System.out.println(set.contains("B"));      // true
System.out.println(set.contains("Z"));      // false
System.out.println(set.containsAll(Arrays.asList("A", "B")));  // true
System.out.println(set.size());             // 3
System.out.println(set.isEmpty());          // false
```

#### ⭐ contains() 메서드 내부 구현
```java
public boolean contains(Object o) {
    return map.containsKey(o);
}
```
- `HashMap`에서 key의 존재 여부를 확인하는 것과 동일
- 해시 테이블의 특성상 평균 `O(1)` 시간에 검색 가능

### 🔄 컬렉션 연산
| 메서드 | 설명 | 시간복잡도 |
|--------|------|-----------|
| `Iterator<E> iterator()` | Set의 요소들을 순회하는 iterator 반환 | `O(1)` |
| `Object[] toArray()` | Set의 모든 요소를 배열로 반환 | `O(n)` |
| `<T> T[] toArray(T[] a)` | 지정된 타입 배열로 반환 | `O(n)` |

```java
HashSet<String> set = new HashSet<>(Arrays.asList("A", "B", "C"));

// Iterator를 이용한 순회
Iterator<String> iterator = set.iterator();
while (iterator.hasNext()) {
    System.out.println(iterator.next());
}

// Enhanced for문을 이용한 순회
for (String element : set) {
    System.out.println(element);
}

// 배열로 변환
String[] array = set.toArray(new String[0]);
```

### 🔧 기타 메서드
| 메서드 | 설명 | 시간복잡도 |
|--------|------|-----------|
| `boolean equals(Object o)` | 다른 Set과의 동등성 비교 | `O(n)` |
| `int hashCode()` | Set의 해시코드 반환 | `O(n)` |
| `HashSet<E> clone()` | HashSet의 얕은 복사본 생성 | `O(n)` |

## 📊 성능 특성

| 연산 | 평균 시간복잡도 | 최악 시간복잡도 | 설명 |
|------|---------------|----------------|------|
| **추가 (add)** | `O(1)` | `O(n)` | 해시 충돌이 많은 경우 성능 저하 |
| **삭제 (remove)** | `O(1)` | `O(n)` | 해시 충돌이 많은 경우 성능 저하 |
| **검색 (contains)** | `O(1)` | `O(n)` | 해시 충돌이 많은 경우 성능 저하 |
| **순회 (iterator)** | `O(n)` | `O(n)` | 모든 요소를 방문 |

⚠️ **참고**: 최악의 경우는 모든 요소가 같은 해시 버킷에 저장되어 연결 리스트 형태가 되는 경우이다.

## 🔄 HashSet vs 다른 Set 구현체 비교

| 특징 | HashSet | LinkedHashSet | TreeSet |
|------|---------|---------------|---------|
| **순서** | 순서 없음 | 삽입 순서 유지 | 정렬된 순서 |
| **검색 성능** | `O(1)` | `O(1)` | `O(log n)` |
| **추가/삭제 성능** | `O(1)` | `O(1)` | `O(log n)` |
| **메모리 사용량** | 낮음 | 중간 | 높음 |
| **용도** | 빠른 검색 | 순서 유지 + 빠른 검색 | 정렬된 데이터 |

## 💡 사용 권장 상황

### ✅ HashSet 사용이 적합한 경우
- **빠른 검색이 중요한 경우**: 중복 확인, 존재 여부 검사 등
- **순서가 중요하지 않은 경우**: 단순히 고유한 요소들의 집합이 필요할 때
- **집합 연산이 필요한 경우**: 합집합, 교집합, 차집합 등
- **대용량 데이터 처리**: 메모리 효율성과 빠른 성능이 필요할 때

```java
// 중복 제거
List<String> listWithDuplicates = Arrays.asList("A", "B", "A", "C", "B");
Set<String> uniqueElements = new HashSet<>(listWithDuplicates);
// 결과: [A, B, C] (순서는 보장되지 않음)

// 빠른 존재 확인
HashSet<String> validCodes = new HashSet<>(Arrays.asList("KOR", "USA", "JPN"));
if (validCodes.contains(inputCode)) {
    // 유효한 코드 처리
}
```

### ❌ HashSet 사용을 피해야 하는 경우
- **순서가 중요한 경우**: [LinkedHashSet](./LinkedHashSet.md) 고려
- **정렬된 데이터가 필요한 경우**: [TreeSet](./TreeSet.md) 고려
- **멀티스레드 환경**: Collections.synchronizedSet() 또는 ConcurrentHashMap.newKeySet() 고려
- **범위 검색이 필요한 경우**: TreeSet이나 NavigableSet 구현체 고려

## 🔗 관련 문서

- [Set Interface](./Set-Interface.md)
- [LinkedHashSet](./LinkedHashSet.md)
- [TreeSet](./TreeSet.md)
- [Collection Interface](../Core-Interfaces/Collection-Interface.md)
- [HashMap](../Map/HashMap.md)

## 📚 참고
- [Java Collection Framework 개요](../Java%20Collection%20Framework%20개요.md)
- [Collections 계층화된 구조](../README.md)