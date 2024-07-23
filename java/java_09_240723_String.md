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

```java
package com.ktdsuniversity.edu.string.immutable;

public class ImmutableString {
	
	public static void makeMultiLineStringOverJava15() {
		
		String dummyText = """ 
Lorem Ipsum is simply dummy text of the printing and typesetting industry.
Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book.
It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged.
It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.""";
		
		System.out.println(dummyText);
		
		
		
	}
	
	
	public static void hugeStringConcat() {
		
		
		String dummyText = "Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.";
		
		for(int i=0; i<100000; i++) {
			
			dummyText += "Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.";
			
		}
		
		
		System.out.println(dummyText);
		
	}
	
	
	
	/**
	 * String 값에 + 연산을 이용해
	 * 숫자나 boolean 등 다른 타입의 값을 더하면
	 * 그 결과는 반드시 String이 된다. 
	 */
	public static void concatenation2() {
		
		int numberOne = 10;
		int numberTwo = 5;
		
		String resultString = numberOne + "+" + numberTwo + "의 결과는 " + numberOne + numberTwo + "입니다.";
		System.out.println(resultString);
		
		Student student = new Student("케이티");
		String classNameString = "student 인스턴스의 클래스는 " + Student.class;
		System.out.println(classNameString);
		
		
		String instanceString = "student 인스턴스는 " + student;
		System.out.println(instanceString);
	}
	
	
	/**
	 * String 의 값에 + 연산을 할 경우 새로운 메모리로 할당되는지 확인한다.
	 */
	public static void concatenation() {
		
		String hello = "안녕하세요?";
		/**
		 * Hash data를 조회하는 기능 
		 * Reference Type(Instance)의 메모리 주소를 확인하는 방법
		 */
		int hashData = System.identityHashCode(hello); // 메모리 주소를 가져온다.
		System.out.println(hashData + ">" + hello);
		
		hello += " 반갑습니다.";
		hashData = System.identityHashCode(hello); // 메모리 주소를 가져온다.
		System.out.println(hashData + ">" + hello);
		
		
		
	}
	
	
	public static void main(String[] args) {
		
		
		ImmutableString.concatenation();
		ImmutableString.concatenation2();
		//ImmutableString.hugeStringConcat();
		ImmutableString.makeMultiLineStringOverJava15();
		
	}
	
}
```

# 13. StringBuffer

* StringBuffer 인스턴스의 append() Method를 사용해 문자열을 이어 붙인다.
* 마지막으로 StringBuffer의 toString() Method를 사용해 하나의 문자열 인스턴스로 반환한다.
---

# 14. 배열(Array)

* 동일한 타입의 값들을 메모리에 차례대로 나열시킨 구조를 말한다.
* 생성자를 호출 할 때, new 키워드를 사용한다.
* 메모리에 데이터 공간들이 나열되어 있고, 이것이 하나의 메모리 묶음으로 취급되어 Reference Type이다.
* 주소값을 찾기 위하여 배열은 0부터 시작한다

### * 배열 실습
```java
package com.ktdsuniversity.edu.array;

public class ArrayExam {
	
	public static void main(String[] args) {
		
		int[] scoreArray = new int[7];
		System.out.println(scoreArray); //[I@5305068a : I(Integer) [(Array)
		
		// 배열 인덱스에 값을 할당한다.
		scoreArray[0] = 10;
		scoreArray[1] = 20;
		scoreArray[2] = 30;
		scoreArray[3] = 40;
		scoreArray[4] = 50;
		scoreArray[5] = 60;
		scoreArray[6] = 70;
		
		// 배열을 탐색하면서 출력하고 합계를 구하기

		int sum = 0;
		
//		sum += scoreArray[0];
//		sum += scoreArray[1];
//		sum += scoreArray[2];
//		sum += scoreArray[3];
//		sum += scoreArray[4];
//		sum += scoreArray[5];
//		sum += scoreArray[6];
		
		for(int i=0; i< scoreArray.length; i++) {
			
			sum += scoreArray[i];
			
		}
		
		System.out.println("합계 : "+ sum);
		
		double avg = sum / (double) scoreArray.length;
		System.out.println("평균 : " + avg);
		
//		String[] nameArray = new String[7];
//		System.out.println(nameArray); // [Ljava.lang.String;@1f32e575
		
		
		
		
	}

}
```
