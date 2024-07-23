# 13. String Concatenation
* 패키지가 다른 겨우 반드시 import 문을 작성해야 하지만, String은 Import를 하지 않아도 사용가능하다.
* Java는 자주 사용하는 내장 클래스를 java.lang 패키지에 모두 넣어 배포한다. 이 패키지에 있는 클래스는 import 하지 않아도 사용가능하다. 


* String은 가변바이트
* 문자열 + 모든 타입의결과는 문자열이 된다.

```java
System.out.println("10+5의 결과는" + 10 + 5); // 10+5결과는 105입니다.
System.out.println("10+5의 결과는" + (10 + 5)); // 10+5결과는 15입니다.

```

* Reference Type
  - Immutable : 메모리내의 값이 절대로 변경될 수 없다.
  - Mutable : 메모리 내의 값이 자유롭게 변경될 수 있다.
  - String은 대표적인 Immutable Type이다.
    - String 값의 변경하려는 경우, 새로운 메모리 공간을 할당하고 값을 할당한다.
    - 즉, String이 참조하고 있던 메모리 주소는 끊어지며, 새로운 메모리 공간을 참조한다.

* 문자열 내의 값이 많을 경우, 새로운 메모리 공간을 확보하고 값을 할당하는데 큰 비용(CPU, Memory)이 사용되기 때문에 추천하지 않는다. 즉, 많은 내용의 문자열을 연결할 필요가 있을 때, StringBuffer를 사용하는 것이 비용절약에 효율적인 방법이다.  

## 13. StringBuffer

* StringBuffer 인스턴스의 append() Method를 사용해 문자열을 이어 붙인다.
* 마지막으로 StringBuffer의 toString() Method를 사용해 하나의 문자열 인스턴스로 반환한다.
