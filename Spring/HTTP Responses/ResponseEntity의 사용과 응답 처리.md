# ResponseEntity의 사용과 응답 처리

### ResponseEntity.ok()와 ResponseEntity.ok().body()의 차이

**ResponseEntity.ok()**

```java
return ResponseEntity.ok("문자열");
```

- `ResponseEntity.ok()`는 편리하게 상태 코드 200 (OK)를 생성하고 응답 본문(body)를 설정할 수 있는 메서드이다.
- 괄호 안에 문자열을 넣으면 해당 값이 응답 본문에 설정된다.
- `ResponseEntity.status(HttpStatus.OK).body(…)`를 호출하는 것과 동일하다.
    - `.status(HttpStatus.OK)`는 `.status(200)`과 동일하게 동작한다.

**ResponseEntitiy.ok().body()**

```java
return ResponseEntity.ok().body("문자열");
```

- `ResponseEntity.ok()`를 호출한 뒤 `body()` 메서드를 사용해 **응답 본문을 명시적으로 설정**한다.
- **상태 코드와 본문을 명확히 구분하여 표현**하려는 경우 사용한다.

`ok()`나 `body()`는 제네릭 타입으로 설계되어 있어 문자열 외에도 **객체, 컬렉션 등의 다양한 변수를 사용**할 수 있으며 **두 메서드의 동작은 동일**하다.



### ResponseEntity.ok()와 ResponseEntity.ok().build()의 차이
---
**ResponseEntity.ok()**

```java
return ResponseEntity.ok();
```

- `ResponseEntity.ok()`는 상태코드 200과 함께 **기본적으로 null을 응답 본문으로 설정**한다.

**ResponseEntity.ok().build()**

```java
return ResponseEntity.ok().build();
```

- `build()`를 호출하면 **응답 본문이 완전히 비어 있는 상태로 생성**된다.
