# 8. 메서드 

* 하나의 기능을 하는 코드의 묶음이다.
* 메서드가 하는 기능에 따라 parameter가 필요할 수 있다.
* 처리결과를 반환하거나 반환하지 않을 수 있다.

1. 반환타입 void : 반환이 없는 메서드 
2. 반환타입 int  : int 형으로 return 메서드
3. 반환타입 double : double 형으로 return 메서드
4. 반환타입이 void가 아닌경우 : 반드시 return 키워드를 마지막에 작성한다.
5. return 키워드는 값을 반환할 뿐만아니라, 메서드의 종료를 알린다. 

* 동일한 기능의 코드가 여러번 중복되어 있는 경우 해당 코드를 수행하는 기능을 한번만 둔다.
* 메서드 실행에 필요한 parameter가 있는 경우, 호출 시 data를 반드시 전달한다.
* 변수의 경우, 메서더의 중괄호 밖에 있지만 중괄호 내부에서만 변수사용이 가능하다. 

```
접근제한자  예약어   반환타입   메서드명   (parameter, ...) {  }

1. 접근제한자 : public, private, protected
2. 예약어 : static, abstract, synchronized, final
3. 반환타입 : void, Primitive Type, Reference Type

```
### * 메서드 실습
```java
public class SimpleMethod {
	
	public static void greeting() {
		System.out.println("Hello, Method");
	}
	
	public static void addNumber() {
		int numberOne = 10;
		int numberTwo = 30;
		
		System.out.println(numberOne + " + " + numberTwo + " = " + (numberOne + numberTwo));
	}
	
	public static void gugudan() {
		
		for(int i=2; i<10; i++) {
			for(int j=1; j<10; j++) {
				System.out.println(i + " X " + j + " = " + i*j);
			}
					System.out.println();
		}
		
	}
	
	public static void main(String[] args) {
		
		gugudan();
		
		greeting();
		greeting();
		greeting();
		greeting();
		greeting();
		greeting();
		addNumber();
		
		for(int i = 0; i < 5; i++) {
			addNumber();
		}
	}
}
```

```java
public class SimpleParameter {
	
	public static void printHello(String name, String message) {
		
		System.out.println(name + "씨, "+ message);
		
	}

	public static void main(String[] args) {
		
		printHello("불안이", "안녕하세요?");
		printHello("당황이", "반갑습니다?");
		printHello("따분이", "안녕?");
		printHello("부럽이", "안녕히가세요.");
		printHello("기쁨이", "부러워요.");
		
	}

}
```
### * 사칙연산이 가능한 계산기
```java
public class SimpleCalculator {

	public static int getplus(int x, int y) {

		return x + y;
	}

	public static int getminus(int x, int y) {

		return x - y;
	}

	public static int getmultiple(int x, int y) {

		return x * y;
	}

	public static int getdivide(int x, int y) {

		return x / y;
	}

	public static void main(String[] args) {

		Scanner keyboard = new Scanner(System.in);

		System.out.print("첫번째 수 입력 : ");
		int firstNumber = keyboard.nextInt();

		System.out.print("두번째 수 입력 : ");
		int secondNumber = keyboard.nextInt();
		
		System.out.println();
		System.out.println("덧셈 : " + getplus(firstNumber, secondNumber));
		System.out.println("뺄셈 : " + getminus(firstNumber, secondNumber));
		System.out.println("곱하기 : " + getmultiple(firstNumber, secondNumber));
		System.out.println("나누기 : " + getdivide(firstNumber, secondNumber));
	}

}
```
### * 메서드 연습문제1
```java
public class PracticeOne {
	
	public static int convertToTime(int minutes, int seconds) {
		
		return minutes * 60 + seconds;
	}
	
	public static void main(String[] args) {
		
		int minutes = 5;
		int seconds = 50;
		int time = 0;
		
		time = convertToTime(minutes, seconds);
		
		System.out.println(time + "초");
		
	}

}
```


### * 메서드 연습문제2
```java
public class PracticeTwo {
	
	public static int convertToMinutes(int processTime) {
		
		return processTime / 60;
		
	}
	
	public static int converToSeconds(int processTime) {
		
		return processTime % 60;
	}
	
	
	public static void main(String[] args) {
		
		int processTime = 145;
		int minutes = 0;
		int seconds = 0;
		
		minutes = convertToMinutes(processTime);
		seconds = converToSeconds(processTime);
		
		System.out.println(minutes+"분 "+ seconds + "초");
		
		
		
	}

}
```


### * 메서드 연습문제3
```java
public class PracticeThree {
	
	public static int convertToFahrenheit(int celsius) {
		
		return (celsius * 9 / 5) + 32;
		
	}
	
	public static void main(String[] args) {
		
		int celsius = 30;
		int fahrenheit = 0;
		
		fahrenheit = convertToFahrenheit(celsius);
		
		System.out.println("섭씨 : " + celsius);
		System.out.println("화씨 : " + fahrenheit);
		
	}

}

```


### * 메서드 연습문제4
```java
public class PracticeFour {
	
	public static int sumScore(int korScore, int engScore, int mathScore, int progScore) {
		
		return korScore + engScore + mathScore + progScore;
		
	}
	
	public static double getAverage(int sum, int subjectCount) {
		
		return (double) sum / subjectCount;
	}
	
	public static String getGrade(double average) {
		
		String grade = null;
		
		if(average >= 95) {
			grade = "A+";
			return grade;
		}
		else if(average >=90) {
			grade = "A";
			return grade;
			
		}
		else if(average >=85) {
			grade = "B+";
			return grade;
		}
		else if(average >=80) {
			grade = "B";
			return grade;
			
		}
		else if(average >=70) {
			grade = "C";
			return grade;
		}
		else{
			grade = "F";
			return grade;
		}
		
		
	}
	
	
	public static void main(String[] args) {
		
		int korScore = 90;
		int engScore = 88;
		int mathScore = 70;
		int progScore = 80;
		
		int sum = sumScore(korScore, engScore, mathScore, progScore);
		System.out.println("총합 : "+ sum);
		
		double average = getAverage(sum, 4);
		System.out.println("평균 : "+ average);
		
		
		String grade = null;
		grade = getGrade(average);
		System.out.println("성적 : " + grade);
	}
}
```


### * 메서드 연습문제5
```java
public class PracticeFive {
	
	public static int travelExpenses(int age) {
		
		final int adultOneWayFlightFare = 300_000;
		final int kidOneWayFlightFare = 120_000;
		
		
		final int ADULT_AGE = 19;
		
		if(age >= ADULT_AGE) {
			return adultOneWayFlightFare;
		}
		else {
			return kidOneWayFlightFare;
		}
	}
	
	public static void isTripPossible(int sumtravelExpenses) {
		
		int money = 1_000_000;
		
		if(money >= sumtravelExpenses) {
			
			System.out.println("여행가자!");
			
		}else {
			System.out.println("다음에 가자!");
		}
		
	}
	
	
	public static void main(String[] args) {
		
		
		int father = 40;
		int mother = 36;
		int daughter = 11;
		
		int sumtravelExpenses = travelExpenses(father) + travelExpenses(mother) + travelExpenses(daughter);
		System.out.println("총 경비 : " + sumtravelExpenses);
		isTripPossible(sumtravelExpenses);
		
		
		
		
	}

}
```
---
# 9. 클래스, 인스턴스, 생성자

## 1. 클래스 
* 클래스를 변수로 만든 것이 객체이다.
* 존재할 수 있는 사물을 추상화 시켜놓은 개념
* Java에서는 Reference Type의 자료형에 해당한다. 

## 2. 객체지향프로그램(OOP)
* 문제를 객체로 표현하여 사람이 알기 쉽도록 풀어나갈 수 있는 방법을 의미한다.

## 3. 객체
* 클래스를 변수로 만든 것을 의미
* 일반적으로 객체를 "인스턴스"라 부른다.
* 클래스를 인스턴스로 만드는 과정을 "인스턴스화"한다고 표현한다. 

## 4. 실세계에 존재하는 모든 질량이 있는 사물들은 "속성"과 "기능(행동)"을 갖는다.
* 속성이란 사물이 가지는 여러 특징, 특성을 말한다.
* 기능이란, 클래스(객체)가 할 수 있는 기능(행동)을 의미한다. 

