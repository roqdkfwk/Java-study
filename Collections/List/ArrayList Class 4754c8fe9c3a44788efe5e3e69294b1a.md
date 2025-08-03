# ArrayList Class

![Untitled](Untitled.png)

- 내부적으로 `Object[]`배열을 이용하여 만든 리스트
- 데이터의 저장 순서가 유지되고 중복을 허용
- 배열을 이용하기 때문에 인덱스를 이용해 **요소에 빠르게 접근**할 수 있다.
- 크기가 고정되어 있는 배열과 달리 데이터의 양에 따라 **가변적으로 공간을 늘리거나 줄인다.**
- 배열이 꽉 찰 때마다 **배열을 copy하는 방식으로 공간을 늘리므로 이 과정에서 지연이 발생한다.**
- **단방향 포인터 구조**로 자료에 대한 순차적인 접근에 강점이 있어 **조회가 빠르다.**
- 데이터를 리스트 중간에 삽입, 삭제할 경우, **중간에 빈 공간이 생기지 않도록 요소들의 위치를 앞뒤로 자동으로 이동**시키기 때문에 **삽입, 삭제 동작은 느리다.**
- 단, **순차적으로 추가, 삭제하는 경우에는 가장 빠르다.**

- 배열 장단점
    - **처음 선언한 배열의 크기(길이)는 변경할 수 없다.** 이를 **정적 할당(static allocation)**이라 한다.
    - 데이터의 크기가 정해져 있을 경우 메모리 관리가 편하다.
    - **메모리에 연속적으로 나열되어 할당**하기 때문에 **index를 통한 색인(접근)속도가 빠르다.**
    - index에 위치한 하나의 데이터(element)를 삭제하더라도 해당 index에는 빈 공간으로 계속 남아있다.

- ArrayList 장단점
    - **리스트의 길이가 가변적**이다. 이를 **동적 할당(dynamic allocation)**이라 한다.
    - 배열과 달리 메모리에 연속적으로 나열되어있지 않고 **주소로 연결되어있는 형태**이기 때문에 **index를 통한 색인(접근)속도가 배열보다 느리다.**
    - 데이터(element) 사이에 빈 공간을 허용하지 않는다.
    - **객체 데이터**를 다루기 때문에 적은 양의 데이터만 쓸 경우 배열에 비해 차지하는 메모리가 커진다.
    
    <aside>
    💡 primitive type인 int 타입의 경우 크기는 4Byte이다.
    반면에 Wrapper 클래스인 Integer는 32bit JVM 환경에서, 객체의 헤더(8Byte), 원시 필드(4Byte), 패딩(4Byte)으로 **최소 16Byte의 크기를 차지**한다. 추가로 이러한 객체 데이터들을 다시 주소로 연결하기 때문에 16 + a가 된다.
    
    </aside>
    
- ArrayList 객체 생성
    
    ```java
    	
    ArrayList<Integer> list = new ArrayList<>();  // 타입 설정 Integer 객체만 가능
     
    ArrayList<Integer> list = new ArrayList<>(10);  // 초기 용량 지정
    
    ArrayList<Integer> list1 = new ArrayList<>(Arrays.asList(1, 2, 3));  // 배열을 넣어 생성
    
    ArrayList<Integer> list2 = new ArrayList<>(list1);  // 다른 컬렉션으로부터 그대로 요소를 받아와 생성 (ArrayList를 인자로 받는 API를 사용하기 위해서 Collection 타입 변환이 필요할 때 많이 사용)
    ```
    
- 메소드
    - ArrayList 요소 추가
        
        
        | **메소드** | **설명** |
        | --- | --- |
        | `boolean add(Object obj)` | ArrayList의 마지막에 객체를 추가한다. 추가에 성공하면 true를 반환 |
        | `void addAll(Collection c)` | 주어진 컬렉션의 모든 객체를 저장한다. |
    - ArrayList 요소 삽입
        
        
        | **메소드** | **설명** |
        | --- | --- |
        | `void add(int index, Object element)` | 지정된 위치(index)에 객체를 저장한다. 자리에 있던 기존의 데이터는 뒤로 밀려나고 삭제되지는 않는다. |
        | `void addAll(int index, Collection c)` | 지정한 위치부터 주어진 컬렉션의 데이터를 저장한다. 자리에 있던 기존의 데이터는 뒤로 밀려나고 삭제되지는 않는다. |
        - 요소 삽입 시 주의점
            - 위치를 지정하여 삽입할 때 **인덱스가 리스트의 capacity를 넘지 않도록 조절**이 필요하다.
            - 리스트의 capacity에 맞추더라도 **마지막에 적재된 요소(size 값)에서 벗어나면** `IndexOutOfBoundsException`이 발생한다.
            - **ArrayList는 데이터가 연속된 자료구조**이기 때문이다.
        - 
            
            ```java
            boolean add(Object obJ)
            
            ArrayList<String> list = new ArrayList<>(10);
            
            list.add("A");
            list.add("B");
            list.add("C");
            list.add("D");
            list.add("E");
            list.add("F");
            
            list.size(); // 6
            ```
            
            ![Untitled](Untitled%201.png)
            
            ```java
            void addAll(Collection c)
            
            ArrayList<String> list1 = new ArrayList<>();
            list1.add("1");
            list1.add("2");
            
            ArrayList<String> list2 = new ArrayList<>();
            list2.add("3");
            list2.add("4");
            
            list1.addAll(list2);  // list1에 list2의 내용을 추가
            
            System.out.println(list1);  // [1, 2, 3, 4]
            ```
            
            ```java
            ArrayList<String> list = new ArrayList<>();
            list.add("1");
            list.add("2");
            list.add("3");
            list.add("4");
            list.add("5");
            
            list.remove(2);  // 2번째 인덱스 자리의 요소 삭제
            
            System.out.pringln(list);  // [1, 2, 4, 5]
            ```
            
    - ArrayList 요소 삭제
        
        
        | **메소드** | **설명** |
        | --- | --- |
        | `Object remove(int index)` | 지정된 위치(index)에 있는 객체를 제거한다. |
        | `boolean remove(Object obj)` | 지정된 객체를 제거한다. (성공하면 true) |
        | `booelan removeAll(Collection c)` | 지정된 컬렉션에 저장된 것과 동일한 객체들을 ArrayList에서 제거한다. |
        | `void clear()` | ArrayList를 완전히 비운다 |
        | `boolean retainAll(Collection c)` | ArrayList에 저장된 객체 중에서 주어진 컬렉션과 공통된 것들만 남기고 제거한다.
        (removeAll 반대버전) |
    - ArrayList 요소 검색
        
        
        | **메소드** | **설명** |
        | --- | --- |
        | `boolean isEmpty()` | ArrayList가 비어있는지 확인한다. |
        | `boolean contains(Object ojb)` | 지정된 객체(obj)가 ArrayList에 포함되어 있는지 확인한다. |
        | `int indexOf(Object obj)` | 지정된 객체(obj)가 저장된 위치를 찾아 반환한다. |
        | `int lastIndexOf(Object obj)` | 지정된 객체(obj)가 저장된 위치를 뒤에서부터 역방향으로 찾아 반환한다. |
    - ArrayList 요소 얻기
        
        
        | **메소드** | **설명** |
        | --- | --- |
        | `Object get(int index)` | 지정된 위치(index)에 저장된 객체를 반환한다. |
        | `List subList(int fromIndex, int toIndex)` | fromIndex부터 toIndex 사이에 저장된 객체를 반환한다. |