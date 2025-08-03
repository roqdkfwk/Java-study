# Collection Interface

Collection Interface는 Java Collection Framework의 핵심 인터페이스로, 모든 컬렉션의 공통 메서드를 정의한다.

## 🏗️ 구조

```
     Iterable
        ↑
   Collection
    ↙   ↑   ↘
 List  Queue  Set
```
![Collection Interface 계층구조](./images/collection-interface.png)
*Collection Interface의 계층 구조*

- **Iterable**: 반복 가능한 객체를 나타내는 최상위 인터페이스
- **Collection**: List, Set, Queue의 상속을 하는 실질적인 최상위 컬렉션 타입인터페이스
- **List, Set, Queue**: Collection을 상속받는 하위 인터페이스들



## 🔧 주요 메서드

Collection Interface에서 제공하는 핵심 메서드

### ➕ 추가 메서드
| 메서드 | 설명 |
|--------|------|
| `boolean add(Object o)` | 지정된 객체(o) 또는 Collection의 객체를 Collection에 추가 |
| `boolean addAll(Collection c)` | 지정된 객체(o) 또는 Collection의 객체들이 Collection에 포함되어 있는지 확인 |

### 🔍 확인 메서드
| 메서드 | 설명 |
|--------|------|
| `boolean contains(Object o)` | 지정된 객체(o) 또는 Collection의 객체들이 Collection에 포함되어 있는지 확인 |
| `boolean containsAll(Collection c)` | 지정된 Collection의 모든 객체들이 포함되어 있는지 확인 |
| `boolean isEmpty()` | Collection이 비어있는지 확인 |
| `int size()` | Collection에 저장된 객체의 개수를 반환 |

### ➖ 제거 메서드
| 메서드 | 설명 |
|--------|------|
| `boolean remove(Object o)` | 지정된 객체 또는 지정된 Collection에 포함된 객체들을 삭제 |
| `boolean removeAll(Collection c)` | 지정된 Collection에 포함된 객체들을 삭제 |
| `boolean retainAll(Collection c)` | 지정된 Collection에 포함된 객체들을 남기고 다른 객체들을 Collection에서 삭제.<br/>실질적 removeAll의 다음 버전. (교집합 동작)<br/>이 작업으로 Collection에 변화가 있으면 true, 없으면 false를 반환 |
| `void clear()` | Collection의 모든 객체를 삭제 |

### 🔄 변환 및 비교 메서드
| 메서드 | 설명 |
|--------|------|
| `boolean equals(Object o)` | 동일한 Collection인지 비교 |
| `int hashCode()` | Collection의 hash code를 반환 |
| `Object[] toArray()` | Collection에 저장된 객체를 객체배열(Object[])로 반환 |
| `Object[] toArray(Object[] a)` | 지정된 배열에 Collection의 객체를 저장해서 반환 |

### 🔁 반복 메서드
| 메서드 | 설명 |
|--------|------|
| `Iterator iterator()` | Collection의 iterator를 얻어서 반환 (상위 Iterable 인터페이스로 상속) |

## ⚠️ 중요한 주의사항

> **💡 Collection 인터페이스의 메서드들은 보면 요소(객체)에 대한 추가, 삭제, 탐색은 다형성을 통해 사용이 가능하지만,  
> 데이터를 get하는 메서드는 보이지 않는다.**
> 
> **왜냐하면 각 컬렉션 자료형마다 구현하는 자료 구조가 제각각이기 때문에 최상위 타입으로 조회하기 까다롭기 때문이다.**  
> 
> 예를 들어, `ArrayList`나 `LinkedList`는 인덱스 기반 접근이므로 `get(index)`를 통해 `index`번째 요소를 가져온다는 개념으로  
> 사용할 수 있지만, `HashSet`의 경우 값을 기반으로 확인하기 위해 `contains(value)`형태로 사용하므로 `get()`메서드가 필요없다.

각 하위 인터페이스(List, Set, Queue)는 Collection Interface를 상속받아 자신만의 특성에 맞는 추가 메서드들을 정의한다.

- **List**: 인덱스 기반 접근 메서드 (`get()`, `set()`, `indexOf()` 등)
- **Set**: 중복 제거에 특화된 메서드들
- **Queue**: FIFO 구조에 맞는 메서드들 (`offer()`, `poll()`, `peek()` 등)

## 📚 관련 문서

- [Java Collection Framework 개요](./Java%20Collection%20Framework%20개요.md)
- [Iterator](./Iterator.md)
- [List Interface](./List-Interface.md)
- [Set Interface](./Set-Interface.md)
- [Queue Interface](./Queue-Interface.md)
