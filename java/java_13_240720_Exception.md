
# 예외

* 사용자의 잘못된 조작 또는 개발자의 잘못된 코딩으로 인해 발생하는 프로그램 오류를 의미한다.
* 예외가 발생되면 프로그램은 곧바로 종료된다는 점에서 에러와 닮은 점이 있다. 
* 예외는 예외처리(exception handling)을 통해 프로그램을 종료하지 않고 정상 실행 상태가 유지되도록 할 수 있다.


# 에러

* 컴퓨터 하드웨어 관련 고장으로 응용프로그램 실행 오류가 발생하는 것

### 예외의 종류

* 예외에는 두 가지 종류가 있다. 하나는 일반 예외(exception), 다른 하나 실행 예외(runtime exception)이다.
* RuntimeException의 하위 클래스가 아니면 일반 예외 클래스이고, 하위 클래스이면 실행 예외 클래스이다.

### 1. NullPointerException

* 객체가 참조가 없는 상태, 즉 null 값을 갖는 참조 변수로 객체 접근 연산자 도트를 사용했을 때, 발생한다.
* 객체가 없는 상태에서 객체를 사용하려고 했으니 예외를 발생시킨다.

### 2. ArrayIndexOutOfBoundsException

* 배열에서 인덱스 범위를 초과할 경우 실행 예외인 java.lang.ArrayIndexOutOfBoundsException 발생

### 3. NumberFormatException

* Integer.parseInt(String s), Double.parseDouble(String s)와 같이 문자열을 숫자로 변환이 가능하다.
* 위의 메소드는 매개값인 문자열이 숫자로 변환될 수 있다면 숫자로 리턴하지만, 숫자로 변환될 수 없는 문자가 포함되는 경우 NumberFormatException이 발생한다.

### 4. ClassCastException

* 타입 변환(casting)은 상위 클래스와 하위 클래스 간에 발생하고 구현 클래스와 인터페이스 간에도 발생한다.
* 이러한 관계가 아니라면 클래스는 다른 타입으로 변환 될 수 없기 때문에 ClassCastException이 발생한다.
* ClassCastException을 발생시키지 않기 위해서는 타입 변환 전에 변환이 가능한지 instanceof 연산자를 확인한다.
* instanceof 연산의 결과가 true이면 좌항 객체를 우항 타입으로 변환 가능하다.

### 예외 처리 코드

```java
try{
//예외 발생가능 코드
} catch(예외 클래스 e){
//예외 처리
} finally{
//항상 실행
}
```

* try 블록에는 예외 발생 가능 코드가 위치
* try 블록의 코드가 예외 발생없이 실행되면 catch 블록은 실행되지 않고 finally 블록 코드 실행
* try 블록의 코드에서 예외가 발생되면 실행을 즉시 멈추고 catch 블록으로 이동하여 예외 처리 코드 실행, finally 블록 코드 실행
* finally 블록은 생략 가능, 예외 발생 여부와 상관없이 항상 실행할 내용이 있는 경우에만 finally 블록 작성한다.
* 심지어, try블록과 catch 블록에서 return 문을 사용하더라도 finally 블록은 항상 실행된다.

### 예외 throws

* 메소드 내부에서 예외가 발생할 수 있는 코드를 작성할 때, try-catch 블록으로 예외를 처리하는 것이 일반적이다.
* throws 키워드는 메소드 선언부 끝에 작성되어 메소드에서 처리하지 않은 예외를 호출한 곳으로 떠넘기는 역할을 한다.

```java
리턴 타입 메소드이름 (매개변수, ...) throw 예외클래스1, 예외클래스2, ...{
}
```
