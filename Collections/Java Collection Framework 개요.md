# Java Collection Framework (JCF)

Java Collection Framework는 자바에서 데이터를 저장하고 조작하는 통합된 아키텍처를 제공한다.

## 🏗️ 전체 구조

### 1. Collection 인터페이스 계층
- **Collection**: 모든 컬렉션의 루트 인터페이스
- **List**: 순서가 있고 중복을 허용하는 컬렉션
- **Set**: 중복을 허용하지 않는 컬렉션  
- **Queue**: FIFO 방식의 컬렉션

### 2. Map 인터페이스 계층
- **Map**: 키-값 쌍으로 데이터를 저장하는 컬렉션

---

## 📋 List 인터페이스

순서가 있고 중복을 허용하는 데이터 집합

### 구현 클래스
- **[ArrayList](./ArrayList.md)** - 동적 배열 구조, 빠른 접근
- **[LinkedList](./LinkedList.md)** - 연결 리스트 구조, 빠른 삽입/삭제
- **[Vector](./Vector.md)** - 동기화된 ArrayList
- **[Stack](./Stack.md)** - LIFO 구조

---

## 🔢 Set 인터페이스

중복을 허용하지 않는 데이터 집합

### 구현 클래스
- **[HashSet](./HashSet.md)** - 해시 테이블 기반, 빠른 성능
- **[LinkedHashSet](./LinkedHashSet.md)** - HashSet + 삽입 순서 유지
- **[TreeSet](./TreeSet.md)** - 이진 검색 트리 기반, 자동 정렬

---

## 🚶 Queue 인터페이스

FIFO 방식의 대기열 처리

### 구현 클래스
- **[LinkedList](./LinkedList.md)** - Queue 인터페이스도 구현
- **[PriorityQueue](./PriorityQueue.md)** - 우선순위 기반 힙 구조
- **[ArrayDeque](./ArrayDeque.md)** - 동적 배열 기반 양방향 큐

---

## 🗺️ Map 인터페이스

키-값 쌍으로 데이터를 저장

### 구현 클래스
- **[HashMap](./HashMap.md)** - 해시 테이블 기반, 빠른 성능
- **[LinkedHashMap](./LinkedHashMap.md)** - HashMap + 삽입 순서 유지
- **[TreeMap](./TreeMap.md)** - Red-Black Tree 기반, 자동 정렬
- **[Hashtable](./Hashtable.md)** - 동기화된 HashMap

---

## 🔧 주요 인터페이스

- **[Iterator](./Iterator.md)** - 컬렉션 순회를 위한 표준 인터페이스
- **[Collection Interface](./Collection-Interface.md)** - 모든 컬렉션의 공통 메서드 정의
- **[List Interface](./List-Interface.md)** - 순서가 있는 컬렉션 인터페이스
- **[Set Interface](./Set-Interface.md)** - 중복 불허 컬렉션 인터페이스
- **[Map Interface](./Map-Interface.md)** - 키-값 매핑 인터페이스

---

## 💡 선택 가이드

### 성능 우선
- **빠른 접근**: ArrayList, HashMap
- **빠른 삽입/삭제**: LinkedList
- **정렬 필요**: TreeSet, TreeMap

### 기능 우선
- **중복 허용**: List 계열
- **중복 불허**: Set 계열  
- **키-값 매핑**: Map 계열
- **순서 중요**: LinkedList, LinkedHashSet, LinkedHashMap
