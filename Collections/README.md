# Java Collections Framework - 계층화된 구조

## 📁 폴더 구조

```
Collections/
├── Java Collection Framework 개요.md (전체 개요)
├── Core-Interfaces/           (핵심 인터페이스)
│   ├── Iterator.md           ✅
│   ├── Collection-Interface.md ✅
│   └── Map-Interface.md      📝 추가 예정
├── List/                     (List 인터페이스와 구현체)
│   ├── List-Interface.md     ✅
│   ├── ArrayList.md          📝 추가 예정
│   ├── LinkedList.md         📝 추가 예정
│   ├── Vector.md             📝 추가 예정
│   └── Stack.md              📝 추가 예정
├── Set/                      (Set 인터페이스와 구현체)
│   ├── Set-Interface.md      📝 추가 예정
│   ├── HashSet.md            📝 추가 예정
│   ├── LinkedHashSet.md      📝 추가 예정
│   ├── TreeSet.md            ✅
│   └── EnumSet.md            📝 추가 예정
├── Queue/                    (Queue 인터페이스와 구현체)
│   ├── Queue-Interface.md    📝 추가 예정
│   ├── Deque-Interface.md    📝 추가 예정
│   ├── PriorityQueue.md      📝 추가 예정
│   └── ArrayDeque.md         📝 추가 예정
├── Map/                      (Map 인터페이스와 구현체)
│   ├── Map-Interface.md      📝 추가 예정
│   ├── Map-Entry-Interface.md 📝 추가 예정
│   ├── HashMap.md            📝 추가 예정
│   ├── LinkedHashMap.md      📝 추가 예정
│   ├── TreeMap.md            📝 추가 예정
│   └── Hashtable.md          📝 추가 예정
└── images/                   (이미지 파일들)
    └── collection-interface.png
```

### ⭐ 기초 개념
1. [Java Collection Framework 개요](./Java%20Collection%20Framework%20개요.md)
2. [Iterator](./Core-Interfaces/Iterator.md)
3. [Collection Interface](./Core-Interfaces/Collection-Interface.md)

### ⭐ List 계열
1. List Interface (List 폴더)
2. ArrayList, LinkedList, Vector, Stack

### ⭐ Set 계열  
1. Set Interface (Set 폴더)
2. HashSet, LinkedHashSet, TreeSet, EnumSet

### ⭐ Queue 계열
1. Queue Interface, Deque Interface (Queue 폴더)
2. PriorityQueue, ArrayDeque

### ⭐ Map 계열
1. Map Interface, Map.Entry Interface (Map 폴더)
2. HashMap, LinkedHashMap, TreeMap, Hashtable

## 📋 각 폴더별 설명

### 🔧 Core-Interfaces
- **목적**: 모든 컬렉션의 기반이 되는 핵심 인터페이스들
- **포함**: Iterator, Collection, Map의 최상위 인터페이스들

### 📋 List
- **특징**: 순서가 있고 중복을 허용하는 컬렉션
- **포함**: List Interface와 모든 List 구현체들
- **공통점**: 인덱스 기반 접근 가능

### 🔢 Set  
- **특징**: 중복을 허용하지 않는 컬렉션
- **포함**: Set Interface와 모든 Set 구현체들
- **공통점**: 수학적 집합 개념 구현

### 🚶 Queue
- **특징**: FIFO 방식의 대기열 컬렉션
- **포함**: Queue, Deque Interface와 모든 Queue 구현체들
- **공통점**: 순서 기반 처리

### 🗺️ Map
- **특징**: 키-값 쌍으로 데이터를 저장
- **포함**: Map Interface와 모든 Map 구현체들
- **공통점**: 키를 통한 값 접근

## ✅ 완료된 파일들
- ✅ **Java Collection Framework 개요.md** - 전체 JCF 구조 소개
- ✅ **Iterator.md** - 컬렉션 순회 인터페이스
- ✅ **Collection-Interface.md** - 컬렉션 공통 메서드 정의
- ✅ **List-Interface.md** - 순서가 있는 컬렉션 인터페이스
- ✅ **TreeSet.md** - 정렬된 Set 구현체

## 📝 추가 예정 파일들 (총 18개)

### Core-Interfaces (1개)
- Map-Interface.md

### List (4개)
- ArrayList.md
- LinkedList.md  
- Vector.md
- Stack.md

### Set (4개)
- Set-Interface.md
- HashSet.md
- LinkedHashSet.md
- EnumSet.md

### Queue (4개)
- Queue-Interface.md
- Deque-Interface.md
- PriorityQueue.md
- ArrayDeque.md

### Map (6개)
- Map-Interface.md
- Map-Entry-Interface.md
- HashMap.md
- LinkedHashMap.md
- TreeMap.md
- Hashtable.md

---
