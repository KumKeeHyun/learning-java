# Interface

## TODO
- 기본적인 인터페이스

- 디폴트 인터페이스

## Default Interface
- 등장 배경 
    - JAVA 8 에서 람다식 추가, Collections 라이브러리 업그레이드를 하는 과정에서 호환성을 잃지 않아야 했음
    - 기존 인터페이스 메커니즘에서 인터페이스를 수정한다면 해당 인터페이스를 구현하고 있었던 클래스들은 컴파일이 않됨
    - 인터페이스에 메소드를 추가해도 구체화된 클래스에 영향이 가지 않도록 하는 디폴트 메소드 메커니즘 추가

<br>

- 작동 방식
    - 구현 클래스는 디폴트 메소드를 구현해도 되고 안해도 됨
    - 만약 구현 클래스가 디폴트 메소드를 재구현 하면 구현한 메소드를 사용
    - 만약 디폴트 메소드를 구현하지 않았다면, 디폴트 메소드 사용

<br>

- 메소드 다중 상속의 모호성
    - 인터페이스에서 부분 구현이 가능해지면서 여러 디폴트 메소드를 상속할 수 있게 됨
    ```java
    interface Vocal {
        default void call() {
            System.out.println("Hello!");
        }
    }

    interface Caller {
        default void call() {
            Switchboard.placeCall(this);
        }
    }

    public class Person implements Vocal, Caller {
        // ... which default is used?
    }
    ```
    - 몇가지 규칙을 추가해서 모호성을 해결
        - 만약 디폴트 메소드가 충돌하는 경우, 구현 클래스는 해당 메소드를 무시하고 무조건 재정의 해야함
        - 필요한 경우 기본 메소드를 호출할 수 있는 구문 제공
        ```java
        public class Person implements Vocal, Caller {
            public void call() { // 무조건 재정의
                // or
                // 이런 구문을 통해 디폴트 메소드 호출 가능
                Vocal.super.call();
                Caller.super.call();
            }
        }
        ```


## Reference
- Java in a Nutshell 7th edition
- [Default & Static Interface Methods](https://medium.com/martinomburajr/java-8-default-static-interface-methods-similar-but-different-a8dcc3460280)