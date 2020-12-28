# Generic

## Medium

- [Java의 Generics](https://medium.com/@joongwon/java-java%EC%9D%98-generics-604b562530b3)
    - 매개변수화 타임
    ```java
    List<String> list = new ArrayList<>();
    ```

    - 언바운드 와일드카드 타입
    ```java
    // 1. Object 클래스에서 제공되는 기능만 이용할 경우
    // 2. 타입 파라미터에 의존적이지 않은 경우
    private void someMethod(List<?> argList) { /* ... */ }
    ```

    - 바운드 타입 매개변수
    ```java
    // Integer 의 서브타입으로 제한
    public class Box<T extends Number> { /* ... */ }
    ...
    Box<Integer> // 가능
    Box<String> // 불가능
    ```

    - 재귀적 타입 바운드
    ```java
    // Comparable 을 구현한 타입으로 제한
    public class Box<T extends Comparable<T>> { /* ... */ }

    public class SomeType implements Comparable<SomeType> { /* ... */ }

    Box<SomeType> // 가능
    ```

    - 제네릭의 서브타이핑
    ```java
    public class Box<T> { /* ... */ }
    ...
    Box<Number> sBox = new Box<Integer> // 컴파일 에러
    ```

    - 와일드카드 서브 타이핑
    ```java
    public void someMethod(Box<? extends Number> box) { /* ... */ }
    somMethod(new Box<Integer>()); // 가능
    ```

    - 바운드 와일드카드 타입
    ```
    <? extends T> : T 의 구체적인 방향으로 타입 변환 허용, 읽기만 허용
    <? super T> : T 의 추상적인 방향으로 타입 변환 허용, 쓰기만 허용
    ```