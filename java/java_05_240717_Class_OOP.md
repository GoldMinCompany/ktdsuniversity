# 9. 클래스, 인스턴스, 생성자

### 1. 클래스 
* 클래스를 변수로 만든 것이 객체이다.
* 존재할 수 있는 사물을 추상화 시켜놓은 개념
* Java에서는 Reference Type의 자료형에 해당한다. 

### 2. 객체지향프로그램(OOP)
* 문제를 객체로 표현하여 사람이 알기 쉽도록 풀어나갈 수 있는 방법을 의미한다.

### 3. 객체
* 클래스를 변수로 만든 것을 의미
* 일반적으로 객체를 "인스턴스"라 부른다.
* 클래스를 인스턴스로 만드는 과정을 "인스턴스화"한다고 표현한다. 

### 4. 실세계에 존재하는 모든 질량이 있는 사물들은 "속성"과 "기능(행동)"을 갖는다.
* 속성이란 사물이 가지는 여러 특징, 특성을 말한다.
* 기능이란, 클래스(객체)가 할 수 있는 기능(행동)을 의미한다. 
---
```
1. 코드 작성
2. 테스트
    1) 부정 테스트 : 의도한 대로 동작하지 않는지를 확인
    2) operator가 "exit"일 때, 정상적으로 종료되는가?
    3) 긍정 테스트 : 의도한 대로 동작하는지 확인
```
### * Scanner 버그
```java
public static void main(String[] args) {
	
	Scanner keyboard = new Scanner(System.in);
	System.out.println("숫자를 입력하세요!");
	int age = keyboard.nextInt();

	keyboard.nextLine(); //enter가 입력되므로, 입력라인을 한번 더 추가!
	
	System.out.println("문자를 입력하세요!");
	String operator = keyboard.nextLine();
	
	System.out.println("입력한 값은 " + operator + " 입니다.");
	
	System.out.println(operator == "+"); // false
	// 문자열 비교.
	
	System.out.println(operator.equals("+")); // true
}
```
### * 변수의 종류

* 지역변수 : 메서드 내부에서 정의한 변수 또는 파라미터로 정의된 변수
* 멤버변수 : 클래스 내부에 정의된 변수
* 클래스의 기능으로 만든다. public static void을 클래스의 public void으로 메소드 만든다.

 ### * 클래스 실습

```java
public class Purifier {
	
	/**
	 * 정수기의 남은 물의 양 (단위 liter)
	 */
	double remainWater;
	

	
	/**
	 * 냉수, 온수 선택 버튼(냉수/온수 여부)
	 * 0 : 냉수
	 * 1 : 온수
	 */
	int temperature;
	
	/**
	 * 정수기에 남아있는 얼음의 양(단위 :개)
	 */
	double remainIceBlock;
	
	
	/**
	 * 정수 필터
	 * 값이 100이라면 새 필터
	 * 값이 0이라면 교체해야하는 필터 
	 */
	int filter;
	
	/**
	 * 물 배출 기능 ---> 물 배출 버튼을 누른 행위
	 */
	public void waterExhaust() {
		// 물 배출 버튼을 누르면, 정수 물통의 양이 줄어든다.
		
		if(remainWater > 0) {
			remainWater--;
		}
		
		
		// 남아 있는 물의 양이 0 일 때, 물통의 양이 더 이상 줄어들 수 없다.
		
	}
	/**
	 * 온도 조절
	 */
	
	public void chooseTemperature() {
		//온도 조절 버튼을 누를 때 마다 냉수, 온수가 전환된다.
		
		if(temperature == 0) {
			temperature = 1; // 냉수 일때, 온수 변환
		}
		else {
			temperature = 0; // 온수 일때, 냉수 변환
		}
		
		
	}
	/**
	 * 얼음 만드는 기능
	 */
	public void makeIce() {
		// 정수기에 보관할 수 있는 최대 얼음의 갯수가 100개.
		// 만약, 남아 잇는 얼음의 갯수가 100개 라면 얼음을 생성하지 않는다.
		if(remainIceBlock < 100) {
			remainIceBlock ++;
		}
	}
	/**
	 * 물통에 물을 채운다.
	 */
	public void fillWater() {
		
		//1. 물통에 물을 채운다. 물통의 최대 저장용량 30L로 가정
		//2. 물은 필터를 통해 걸러진다. ---> 필터의 사용량이 줄어든다. 
		
		if(remainWater < 30) {
			remainWater ++;
			
			//2. 물은 필터를 통해 걸러진다. ---> 필터의 사용량이 줄어든다.
			//필터의 사용량이 0이 된다면, 필터는 정상기능을 수행할 수 없다. 
			filter--;
		}
		
	}
	
	/**
	 * 얼음 배출
	 */
	public void iceExhaust() {
		//남아있는 얼음의 갯수가 0보다 클 때만 얼음을 배출한다.
		if(remainIceBlock > 0) {
			remainIceBlock--;
		}
		
		
	}
	
}
```
### 자바는 왜 객체지향 언어를 선택하였는가? 왜 클래스를 만들어야 되는가?

* 코드가 단순해지길 원한다.
* 일반적으로 지역변수는 반드시 값을 할당해야 하지만, 인스턴스마다 다른 값을 가지기 때문에 멤버변수는 미리 할당하지 않는다.

* 클래스를 변수로 사용하려면, 인스턴스를 생성해야 한다.
* 인스턴스가 만들어지면 변수에 할당해야 사용할 수 있다.
* Reference Type의 변수는 "인스턴스"라고 부른다. 예를 들어, Car 클래스는 Reference Type이므로 car 변수는 인스턴스이다.
* Reference Type의 인스턴스는 Primitive Type에 없는 점 연산자(".")를 사용할 수 있다.
### * 클래스 실습
```java

/**
 * 한 학생의 점수를 관리해주는 클래스
 */
public class Student {
	
	/**
	 * 국어점수
	 */
	int korScore;
	/**
	 * 영어점수
	 */
	int engScore;
	/**
	 * 수학점수
	 */
	int mathScore;
	
	public int getSum() {
		
		return korScore + engScore + mathScore;
		
	}
	
	

}
```
```java

/**
 * 클래스의 멤버변수 혹은 클래스의 메소드를 사용하기 위해서는 반드스 클래스를 변수로 생성해야한다. 
 * 
 */
public class ScoreManager {
	
	Student kim;
	Student lee;
	Student park;
	Student choi;
	
	public int getSum() {
		return kim.getSum()+lee.getSum()+park.getSum()+choi.getSum();
	}
	
	public double getAverage() {
		
		return getSum() / 4.0;
		
	}
	
	
	public static void main(String[] args) {
		
		ScoreManager manager = new ScoreManager();
		
		manager.kim = new Student();
		manager.kim.korScore = 100;
		manager.kim.engScore = 90;
		manager.kim.mathScore = 85;
		
		
		manager.lee = new Student();
		manager.lee.korScore = 85;
		manager.lee.engScore = 90;
		manager.lee.mathScore = 95;
		
		
		manager.park = new Student();
		manager.park.korScore = 75;
		manager.park.engScore = 80;
		manager.park.mathScore = 90;
		
		
		manager.choi = new Student();
		manager.choi.korScore = 85;
		manager.choi.engScore = 65;
		manager.choi.mathScore = 100;
		
		int sum = manager.getSum();
		double avg = manager.getAverage();
		
		System.out.println("합계 : " + sum);
		System.out.println("평균 점수 : " + avg);
		
	}

}
```

```java
public class StudentScore {
	
	int java;
	int python;
	int cpp;
	int csharp;
	
	
	public int getSumAllScores() {
		
		int sum = java + python + cpp + csharp;
		
		return sum;
	}
	
	public double getAverage() {
		
		double avg = (double) getSumAllScores() / 4;
		
		return avg;
		
	}
	
	public double getCourseCredit() {
		
		double courseCredit = getAverage();
		return (courseCredit - 55) / 10;
		
	}
	
	
	public String getABCDEF() {
		
		double courseCredit = getCourseCredit();
		
		String grade = null;
		if(courseCredit >= 4.1) {
			grade = "A+";
		}
		else if(courseCredit >= 3.6) {
			grade = "A";
		}
		else if(courseCredit >= 3.1) {
			grade = "B+";
		}
		else if(courseCredit >= 2.6) {
			grade = "B";
		}
		else if(courseCredit >= 1.6) {
			grade = "C";
		}
		else if(courseCredit >= 0.6) {
			grade = "D";
		}
		else {
			grade = "F";
		}
		
		
		return grade;
		
	}
	

}
```
### * 인형뽑기게임
```java
import java.util.Random;

public class ClawMachine {
	
	boolean isInsertCoin;
	int dolls;
	
	public void insertCoin() {
		
		if(dolls > 0) {
			
			isInsertCoin = true;
		}
		else {
			isInsertCoin = false;
		}
		
		if(isInsertCoin) {
			
			System.out.println("게임 시작!");
			System.out.println("남은 인형의 갯수 : " + dolls);
			doGame();
			
		}
		else {
			
			System.out.println("동전을 넣어라!");
		}		
		
	}
	
	public int doGame() {
		
		if(!isInsertCoin) {
			
			return 0;
		}
			
		Random random = new Random();
		int result = random.nextInt(2);
		System.out.println("뽑은 인형의 갯수 : " + result);
		
		if(result == 1) {
			
			dolls -= result;
			isInsertCoin = false;
			
			System.out.println("남은 인형의 갯수 : " + dolls);
			
			return result;
		}
		else {
			
			return result;
			
			
		}
		
	}
}
```

```java

public class ClawMachineExam {

	public static void main(String[] args) {
		
		ClawMachine cm = new ClawMachine();
		cm.isInsertCoin = false;
		cm.dolls = 5;
		
		cm.insertCoin();
	}

}
```


