# Lambda

> Lambdas are a shorthand for the construction of a new
instance of a class type that is essentially Object enhanced by
a single method.

- 결국 자바는 클래스를 통해 인스턴스를 생성해야 한다. 메소드만 따로 존재할 수 없음
- 람다는 Object 클래스를 상속하고 메소드의 개수가 1개인 클래스로 변환된다.

<br>


### 예시 코드 

```java
public class Functional<T extends Comparable<T>> {
    public interface Filtator<T extends Comparable<T>> {
        public boolean exec(T arg);
    }
    
    private List<T> list;

    public Functional(List<T> list) {
        this.list = list;
    }

    public List<T> getList() {
        return this.list;
    }

    public Functional<T> filter(Filtator<T> op) {
        List<T> newList = new ArrayList<>();
        for (T e : this.list) {
            if (op.exec(e)) {
                newList.add(e);
            }
        }
        return new Functional<T>(newList);
    }
}
```

- 람다식을 받기 위한 싱글 메소드 인터페이스 정의
- 1개의 인자를 받고 boolean 타입을 리턴하는 메소드

```java
public interface Filtator<T extends Comparable<T>> {
    public boolean exec(T arg);
}
```

<br>

**람다 사용**

```java
System.out.println(smartList.filter(arg -> {
	return (arg.compareTo(5) > 0) ? true : false;
}).getList());
// init : [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
// result : [6, 7, 8, 9, 10]
```

- 아래의 람다식이 Filtator 를 구현하는 클래스로 정의된 후에 인스턴스 생성 후 메소드 호출 

`arg -> { return (arg.compareTo(5) > 0) ? true : false;}`



## Reference
- Java in a Nutshell 7th edition
- [Notes about Java 8 Lambda Expressions](https://medium.com/theagilemanager/notes-about-java-8-lambda-expressions-b9acef61c315)