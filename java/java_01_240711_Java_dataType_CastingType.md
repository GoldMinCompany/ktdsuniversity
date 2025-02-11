# Java, Ecplise 설치 및 환경설정

JAVA는 JVM(Java Virture Machine)이 있어 한번의 개발로 어느 OS에서든 실행이 가능하게 만들어준다. JDK(Java Develop Kit)는 java 개발 환경을 조성해주며, 이곳에는 위의 JVM과 자바로 만들어진 프로그램을 구동하는 환경을 구성하는 기본 모듈의 모임인 JRE가 존재한다.

Java.exe와 Javac.exe는 이 파일이 존재하는 디렉터리에서 실행이 가능하지만, 이것을 전역에서 사용할 수 있도록 하기 위해서는 고급 시스템 속성의 환경변수를 추가 하는 것이다.

Java 개발에 있어 대문자i와 소문자L과 같이 비슷한 문자가 가독성을 해치는데 문자의 폭을 고정하여 가독성을 해결해주는 개발용 폰트가 존재한다.

Java 개발 툴인 Eclipse를 설치했다면, 상단의 Window>preferences>Java>Installed JREs로 이동하여 JRE버전을 선택할 수 있다. 상위 버전을 선택하더라도 이전의 버전에서 만든 자바 프로그램은 실행이 가능하다.

프로젝트를 생성하는 방법은 File>New>project>Java project로 생성할 수 있으며, 주의점으로 module 생성을 체크 해제 해주어야 한다. 이것을 체크한 경우 프로그램으로 작동하는 것이 아닌 프로그램의 실행을 도우는 도구가 된다. 만약 체크한 상태로 만들었다면 src 하위의 생성된 module을 삭제하면 된다.

# 자료형 

- 메모리(Ram)에 데이터를 할당하기 위한 타입

	Primitive type(기본형)

	Reference type(참조형)


* 숫자(정수형)

  byte : 1byte

  short : 2byte

  int : 4byte

  long : 8byte

* 숫자(부동소수점)

  float : 4byte

  double : 8byte

* boolean : 1byte, True 또는 False

* 문자

  char : 1byte

```
package sample_application;

  public class HelloJava {
	
	  public static void main(String[] args) {
		  System.out.println("Hello World!");
		  System.out.println("");
		  System.out.println(40);
		  System.out.println(1 + 5);
		  System.out.println(10 / 6);
		  System.out.println(5 * 6);
		  System.out.println(true);
		  System.out.println(false);
	}

}

```

# 변수와 변수의 범위

변수의 정의 : int number;

변수 값 할당 : number = 10;

변수의 정의와 할당 : int number = 10;

```
package sample_application;

public class NumberTypeExam {
	
	public static void main(String[] args) {
		
		// int 기본 타입
		byte byteNumber = 1;
		System.out.println(byteNumber);
		
		short shortNumber = 10;
		System.out.println(shortNumber);
		
		int intNumber = 1_000_000_000;
		System.out.println(intNumber);
		
		long longNumber = 3_000_000_000L;
		System.out.println(longNumber);
		
		
		float floatNumber = 50;
		double doubleNumber = 100;
		
		
		System.out.println(floatNumber);
		System.out.println(doubleNumber);
		
		// double 기본 타입
		float floatNumber2 = 10.33333333333F;
		double doubleNumber2 = 50.666666666;
		
		System.out.println(floatNumber2);
		System.out.println(doubleNumber2);
	}
	
}
```
# 상수

변수가 정의되고 최초 한번 값이 할당되면 다시 재정의할 수 없다. 

```
public static void main(String[] args){

  final int speedOfLight = 299_792_458;

  System.out.println(speedOfLight);

  /*error
  * speedOfLight = 10;
  */


}
```

# 형 변환 

```
public static void main(String[] args){

  int normalNumber = Integer.MAX_VALUE;
  long bigNumber = normalNumber;
  System.out.println(normalNumber);
  System.out.println(bigNumber);

}
```

Q. 4byte int 변수를 8 byte long 변수에 할당하면 어떤일이 발생하는가?

A. long의 범위가 더 크기 때문에 int 값을 할당하는데 문제가 발생하지 않는다. 이를, "묵시적 형변환"이라 한다. 

```
public static void main(String[] args){

  long bigNumber = Long.MAX_VALUE;
  int normalNumber = bigNumber; //Error
  System.out.println(bigNumber);
  System.out.println(normalNumber);

```

위 코드의 Error를 제거하기 위해 "명시적 형변환"을 해야한다. 

"명시적 형변환" 이란 강제로 형을 변경하는 것을 의미한다. 

명시적 형변환을 사용할 때, 항상 정수 overflow가 발생하지 않도록 주의해야 한다. 

```
public static void main(String[] args){

  long bigNumber = Long.MAX_VALUE;
  int normalNumber = (int)bigNumber;
  System.out.println(bigNumber);
  System.out.println(normalNumber);

}
```

## TypeCasting 실습예제 및 과제 
```
package sample_application;
/**
 * <pre>
 * Document 주석 
 * 클래스나 메소드 혹은 멤버변들을 설명하기 위한 가이드
 * 
 * 해당 클래스나 메소드 혹은 멤버변수에 마우스를 가져다 올리면
 * 주석이 툴팁으로 나타난다.
 * </pre> 
 */
public class TypeCastingExam {
	/**
	 * 형변환 예제 실습 번호 
	 */
	int testCaseNumber;
	
	/**
	 * 클래스의 실행을 담당하는 메소드. 
	 * 
	 * @param args 실행할 때 전달된 데이터 
	 */
	public static void main(String[] args) {
		
		/*
		 * short shortNumber = 10_000; int intNumber = shortNumber; //묵시적 형변환
		 * 
		 * long longNumber = 2_100_000_000L * 2; System.out.println(longNumber);
		 * 
		 * int intNumber2 = (int) longNumber; System.out.println(intNumber2);
		 * 
		 * System.out.println(Integer.MAX_VALUE); System.out.println(Integer.MAX_VALUE +
		 * 1);
		 * 
		 * int score1 = 100; int score2 = 30;
		 * 
		 * double divResult = (double) score1 / score2; System.out.println(divResult);
		 * 
		 * 
		 * double divResult2 = (score1 * 1.0) / score2 ; System.out.println(divResult2);
		 */
		//Q1
		
		int math = 80;
		int history = 70;
		int english = 90;
		int science = 85;
		
		int sum = math + history + english + science;
		
		double avg = (double) sum / 4 ;
		
		System.out.println("평균 : " + avg);
		
		//Q2

		int x = 9725;
		double y = (double) x / 100;
		
		System.out.println(y);
		
		//Q3

		double z = 93.167*100.0;
		z = Math.round(z);
		z /= 100.0;
		
		System.out.println(z);
		
		
		double average2 = 93.167;
		double tempAverage = average2 * 100;
		System.out.println(tempAverage);
		
		long intAverage = Math.round(tempAverage);
		System.out.println(intAverage);
		
		average2 = (double) intAverage / 100 ;
		System.out.println(average2);
		
		System.out.println(round(average2, 2));
		
		
		
		/*
		 * 여러 줄을 주석으로 사용할 수 있는 문법
		 * Multi line Comment
		 */
		
		
	}
	public static double round(double value, int point) {
		
		
		double result = value * Math.pow(10, point);
		result = Math.round(result);
		result /= Math.pow(10, point);
		return result;
	}
}


```



