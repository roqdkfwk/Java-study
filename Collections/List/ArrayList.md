# ArrayList Class

## 📁 목차

1. [기본 구조](#-기본-구조)
2. [주요 특징](#-주요-특징)
   - [장점](#-장점)
   - [단점](#-단점)
3. [배열 vs ArrayList 비교](#-배열-vs-arraylist-비교)
   - [기본 배열의 특징](#기본-배열의-특징)
   - [ArrayList의 특징](#arraylist의-특징)
   - [메모리 사용량 비교](#-메모리-사용량-비교)
4. [ArrayList 객체 생성](#-arraylist-객체-생성)
   - [기본 생성자](#기본-생성자)
   - [초기값과 함께 생성](#초기값과-함께-생성)
5. [주요 메서드](#-주요-메서드)
   - [요소 추가](#-요소-추가)
   - [요소 삽입](#-요소-삽입)
   - [요소 삭제](#-요소-삭제)
   - [요소 검색](#-요소-검색)
   - [요소 얻기](#-요소-얻기)
6. [성능 특성](#-성능-특성)
7. [사용 권장 상황](#-사용-권장-상황)
   - [ArrayList 사용이 적합한 경우](#-arraylist-사용이-적합한-경우)
   - [ArrayList 사용을 피해야 하는 경우](#-arraylist-사용을-피해야-하는-경우)
8. [관련 문서](#-관련-문서)

---

ArrayList는 Java Collection Framework에서 가장 많이 사용되는 List 구현체로, 내부적으로 배열을 사용하여 동적 크기 조절이 가능한 리스트입니다.

## 🏗️ 기본 구조

- **내부 구조**  
  `Object[]` 배열을 이용하여 구현
- **데이터 저장**  
  저장 순서가 유지되고 중복을 허용
- **접근 방식**  
  배열을 사용하기 때문에 인덱스를 이용한 빠른 요소 접근 가능
- **크기 조절**  
  데이터의 양에 따라 가변적으로 공간을 늘리거나 줄임

## ✨ 주요 특징

### 🚀 장점
- **빠른 접근**  
  배열 기반으로 인덱스를 통한 요소 접근이 빠름 (`O(1)`)
- **단방향 포인터 구조**  
  순차적인 데이터 접근에 강점
- **가변 크기**  
  동적 할당으로 필요에 따라 크기 조절 가능
- **순차적 추가/삭제**  
  리스트 끝에서의 추가/삭제는 매우 빠름

### ⚠️ 단점
- **중간 삽입/삭제 느림**  
  배열과 달리 메모리에 연속적으로 나열되어 있지 않고, 주소로 연결되어 있는 형태이므로  
  index를 통한 접근 속도가 배열보다 느리다.
- **배열 복사 오버헤드**  
  용량 초과 시 전체 배열을 복사하는 방식으로 공간을 늘리므로 이 과정에서 지연이 발생
- **메모리 사용량**  
  객체 데이터로 인해 primitive 배열 대비 메모리 사용량 증가

## 📊 배열 vs ArrayList 비교

<div style="display: flex; gap: 20px;">

<!-- 기본 배열 표 -->
<div>

**기본 배열의 특징**

| 특징       | 설명                                  |
|------------|---------------------------------------|
| 크기       | 정적 할당 - 처음 선언한 크기는 변경 불가 |
| 메모리 배치 | 연속적으로 나열되어 할당               |
| 접근 속도  | 인덱스를 통한 매우 빠른 접근           |
| 빈 공간    | 삭제 후에도 해당 인덱스의 빈 공간 유지  |

</div>

<!-- ArrayList 표 -->
<div>

**ArrayList의 특징**

| 특징       | 설명                                  |
|------------|---------------------------------------|
| 크기       | 동적 할당 - 필요에 따라 가변적 크기 조절 |
| 메모리 배치 | 주소로 연결되어 있는 형태              |
| 접근 속도  | 배열보다는 느리지만 여전히 빠름        |
| 빈 공간    | 데이터 사이에 빈 공간을 허용하지 않음   |

</div>

</div>

### 💾 메모리 사용량 비교
```
기본 int 타입: 4Byte  

Integer 객체 (32bit JVM):
├── 객체 헤더: 8Byte
├── 원시 필드: 4Byte  
├── 패딩: 4Byte
└── 총 크기: 16Byte + 주소 연결 오버헤드
```

## 🔧 ArrayList 객체 생성

### 기본 생성자
```java
// 기본 생성 (초기 용량 10)
ArrayList<Integer> list = new ArrayList<>();

// 초기 용량 지정
ArrayList<Integer> list = new ArrayList<>(10);
```

### 초기값과 함께 생성
```java
// 배열을 이용한 생성
ArrayList<Integer> list1 = new ArrayList<>(Arrays.asList(1, 2, 3));

// 다른 컬렉션으로부터 생성
ArrayList<Integer> list2 = new ArrayList<>(list1);
```

## 📋 주요 메서드

### ➕ 요소 추가
| 메서드 | 설명 | 시간복잡도 |
|--------|------|------------|
| `boolean add(Object obj)` | 리스트 마지막에 객체 추가 | `O(1)` |
| `void addAll(Collection c)` | 주어진 컬렉션의 모든 객체를 저장 | `O(n)` |

```java
ArrayList<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.add("C");
// 결과: [A, B, C]

ArrayList<String> list2 = new ArrayList<>(Arrays.asList("D", "E"));
list.addAll(list2);
// 결과: [A, B, C, D, E]
```

### 📝 요소 삽입
| 메서드 | 설명 | 시간복잡도 |
|--------|------|------------|
| `void add(int index, Object element)` | 지정된 위치에 객체 저장 | `O(n)` |
| `void addAll(int index, Collection c)` | 지정한 위치부터 컬렉션 데이터 저장 | `O(n)` |

#### ⚠️ 삽입 시 주의점
- 인덱스가 리스트의 capacity를 넘지 않도록 조절 필요
- 마지막 요소의 size 값을 벗어나면 `IndexOutOfBoundsException` 발생
- ArrayList는 연속된 자료구조이므로 중간에 빈 공간 불허

```java
ArrayList<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.add("D");

list.add(2, "C"); // 2번 인덱스에 "C" 삽입
// 결과: [A, B, C, D]
```

### ➖ 요소 삭제
| 메서드 | 설명 | 시간복잡도 |
|--------|------|-----------|
| `Object remove(int index)` | 지정된 위치의 객체 제거 | `O(n)` |
| `boolean remove(Object obj)` | 지정된 객체 제거 | `O(n)` |
| `boolean removeAll(Collection c)` | 컬렉션과 동일한 객체들 제거 | `O(n²)` |
| `void clear()` | ArrayList를 완전히 비움 | `O(n)` |
| `boolean retainAll(Collection c)` | 주어진 컬렉션과 공통된 것들만 남기고 제거 | `O(n²)` |

```java
ArrayList<String> list = new ArrayList<>(Arrays.asList("1", "2", "3", "4", "5"));

list.remove(2); // 2번째 인덱스 요소 삭제
// 결과: [1, 2, 4, 5]

list.remove("4"); // "4" 객체 삭제
// 결과: [1, 2, 5]
```

### 🔍 요소 검색
| 메서드 | 설명 | 시간복잡도 |
|--------|------|----------|
| `boolean isEmpty()` | ArrayList가 비어있는지 확인 | `O(1)` |
| `boolean contains(Object obj)` | 지정된 객체 포함 여부 확인 | `O(n)` |
| `int indexOf(Object obj)` | 지정된 객체의 첫 번째 위치 반환 | `O(n)` |
| `int lastIndexOf(Object obj)` | 지정된 객체의 마지막 위치 반환 | `O(n)` |

```java
ArrayList<String> list = new ArrayList<>(Arrays.asList("A", "B", "C", "B"));

System.out.println(list.contains("B"));     // true
System.out.println(list.indexOf("B"));      // 1
System.out.println(list.lastIndexOf("B"));  // 3
```

### 📖 요소 얻기
| 메서드 | 설명 | 시간복잡도 |
|--------|------|------------|
| `Object get(int index)` | 지정된 위치의 객체 반환 | `O(1)` |
| `List subList(int fromIndex, int toIndex)` | 부분 리스트 반환 | `O(1)` |

```java
ArrayList<String> list = new ArrayList<>(Arrays.asList("A", "B", "C", "D", "E"));

String element = list.get(2);        // "C"
List<String> subList = list.subList(1, 4);  // [B, C, D]
```

## 📊 성능 특성

| 연산 | 시간복잡도 | 설명 |
|------|-----------|------|
| **접근 (get)** | `O(1)` | 인덱스를 통한 직접 접근 |
| **끝에 추가 (add)** | `O(1)` | 용량이 충분한 경우 |
| **중간 삽입/삭제** | `O(n)` | 요소들의 이동 필요 |
| **검색 (contains)** | `O(n)` | 순차 탐색 |

## 💡 사용 권장 상황

### ✅ ArrayList 사용이 적합한 경우
- **읽기 작업이 많은 경우**: 인덱스를 통한 빠른 접근
- **순차적 데이터 처리**: 순서대로 데이터를 처리할 때
- **끝에서의 추가/삭제**: 리스트의 마지막에서 주로 작업할 때
- **메모리 효율성**: 상대적으로 적은 메모리 오버헤드

### ❌ ArrayList 사용을 피해야 하는 경우
- **중간 삽입/삭제가 빈번한 경우**: [LinkedList](./LinkedList.md) 고려
- **멀티스레드 환경**: [Vector](./Vector.md)나 Collections.synchronizedList() 고려
- **큐나 스택 구조**: ArrayDeque 고려

## 🔗 관련 문서

- [List Interface](./List-Interface.md)
- [LinkedList](./LinkedList.md)
- [Vector](./Vector.md)
- [Collection Interface](../Core-Interfaces/Collection-Interface.md)

## 📚 참고
- [Java Collection Framework 개요](../Java%20Collection%20Framework%20개요.md)
- [Collections 계층화된 구조](../README.md)
