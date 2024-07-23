# 13. String Concatenation
* 패키지가 다른 겨우 반드시 import 문을 작성해야 하지만, String은 Import를 하지 않아도 사용가능하다.
* Java는 자주 사용하는 내장 클래스를 java.lang 패키지에 모두 넣어 배포한다. 이 패키지에 있는 클래스는 import 하지 않아도 사용가능하다. 


* String은 가변바이트
* 문자열 + 모든 타입의결과는 문자열이 된다.

```java
System.out.println("10+5의 결과는" + 10 + 5); // 10+5결과는 105입니다.
System.out.println("10+5의 결과는" + (10 + 5)); // 10+5결과는 15입니다.

```
