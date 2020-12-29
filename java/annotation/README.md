# Annotation
주된 목적은 메타데이터 전달
- 기본 목적은 프로그램 의미를 변경해서는 안됨
    - 엄격하게는 프로그램 실행에 영향을 주지 않아야 함
    - 대신 컴파일러 및 기타 사전 실행 단계에 대한 정보를 제공해야 함
- 최근 사용되는 어노테이션은 `의미론적 의미 없음` 규칙을 위반하는 경우가 많음
    - @Autowired 는 적절한 컨테이너 외부에서는 사용할 수 없음

## 몇가지 속성
- 모두 `java.lang.annotation.Annotation`을 확장
- 다른 인터페이스를 확장할 수 없음
- 인수가 없는 메소드만 정의 할 수 있음
- 예외를 발생시키는 메소드를 정의할 수 없음
- 메소드의 반환 유형에 대한 제한이 있음
- 메소드에 대한 기본 반환 값을 가질 수 있음

## 기본적으로 지원하는 어노테이션
- @Override
    - 선언한 메소드가 오버라이드 되었다는 것을 표시
    - 상위 클래스, 인터페이스에서 해당 메소드를 찾을 수 없다면 컴파일 에러 발생시킴
- @Deprecated
    - 해당 메소드가 더 이상 사용되지 않음을 표시
    - 만약 사용할 경우 컴파일 경고
- @SuppressWarnings
    - 선언한 곳의 컴파일 경고를 무시하도록 함
- @SafeVarargs
    - 제네릭 같은 가변인자의 매개변수를 사용할 때의 경고를 무시
- @FunctionalInterface
    - 함수형 인터페이스(람다)를 지정
    - 만약 메서드가 존재하지 않거나, 2개 이상의 메소드가 존재할 경우 컴파일 오류 발생

## 커스텀 어노테이션
`@interface` 키워드와 `@Target`, `Retention` 메타 어노테이션을 통해 커스텀 어노테이션 사용 가능

### 메타 주석
- @Target : 소스 코드 내에 합법적으로 배치될 수 있는 위치 지정
    - ElementType 의 enum 들
    - TYPE
    - FIELD
    - METHOD
    - PARAMETER
    - CONSTRUCTOR
    - LOCAL_VARIABLE
    - ANNOTATION_TYPE
    - PACKAGE
    - TYPE_PARAMETER
    - TYPE_USE
- @Retention : `javac`, `java runtime` 이 해당 주석을 처리하는 방법 지정
    - RetentionPolicy 의 enum 들
    - SOURCE : 컴파일될 때 javac에 의해 삭제됨
    - CLASS : 클래스 파일에 존재하지만 `JVM`에서 엑세스할 수 없음. 잘 사용되지 않음. 
    - RUNTIME : 사용자 코드가 런타임에 해당 어노테이션에 엑세스할 수 있음
- @Inherited
- @Documented

### 예시
- 선언 및 사용
```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface MethodAnnotation {
    String name();
    int id() default 5;
}

public class ExampleClass {
    
    @MethodAnnotation(name = "example", id = 1)
    public void exampleMethod() {
        // ...
    }

    @MethodAnnotation(name = "usingDefault")
    public void defaultMethod() {
        // ...
    }
}
```
- 리플렉션으로 어노테이션 값 가져오기
```java
public static void main(String[] args) {
    Method method = TheClass.class.getMethod("exampleMethod");
    Annotation[] annotations = method.getDeclaredAnnotations();

    for (Annotation annotation : annotations) {
        if (annotation instanceof MethodAnnotation) {
            MethodAnnotation ma = (MethodAnnotation)annotation;

            System.out.println(ma.name()); // "example"
            System.out.println(ma.id()); // 1
        }
    }
}
```


## Reference
- Java in a Nutshell 7th edition
- [자바 커스텀 어노테이션 만들기](https://advenoh.tistory.com/21)