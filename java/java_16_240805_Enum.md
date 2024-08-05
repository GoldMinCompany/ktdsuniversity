# Enum
* Application 내부에서 유일한 객체로 사용할 수 있도록 하는 개발 패턴을 Singleton Pattern이라 한다.
* Singleton Pattern으로 만들어진 Class는 Application에서 단 한번만 인스턴스를 생성할 수 있기 때문에, 처음 만들어진 인스턴스를 변경하지 않고 계속 사용할 수 있도록 한다.
* Enum은 Java에서 가장 완벽한 Singleton Instance이다. 인스턴스 생성 할 필요 없이 한번 정의하는 것으로 유일한 값으로 사용할 수 있기 때문이다.
* 따라서, 상수를 파라미터로 받고자 할 경우 Enum을 사용하는 것이 가장 좋은 방법이다.

### Enum 실습

```java
package com.ktdsuniversity.enumexam;

public enum Type {
	
	ADD, SUB, MUL, DIV;

}
```

```java
package com.ktdsuniversity.enumexam;

public class SimpleCalculator {
	
	
	
	public int getResult(Type type, int numberOne, int numberTwo) {
		
		if(type == Type.ADD) {
			return numberOne + numberTwo;
		}
		else if(type == Type.SUB) {
			return numberOne - numberTwo;
		}
		else if(type == Type.MUL) {
			return numberOne * numberTwo;
		}
		else {	
			return numberOne / numberTwo;
		}
		
		
	}

}
```

```java
package com.ktdsuniversity.enumexam;

public class Main {

	public static void main(String[] args) {
		
		
		
		SimpleCalculator simpleCalculator = new SimpleCalculator();
		
		int result = simpleCalculator.getResult(Type.ADD, 10, 20);
		System.out.println(result);
		result = simpleCalculator.getResult(Type.SUB, 10, 20);
		System.out.println(result);
		result = simpleCalculator.getResult(Type.MUL, 10, 20);
		System.out.println(result);
		result = simpleCalculator.getResult(Type.DIV, 10, 20);
		System.out.println(result);
	}

}
```
# Calendar
* Java에서는 날짜와 시간을 처리할 수 있는 3가지 클래스를 제공한다
  * Date
  * Calendar
  * LocalDateTime(Java 1.8에서 추가)
 
* Java 1.8 이전까지 Calendar가 많이 사용되었지만, Java 버전이 꾸준히 릴리즈 되어 많은 메소들이 Deprecated(권장하지 않음)으로 되어 이제는 사용을 권장하지 않습니다.

* Java 1.8 이전의 버전을 사용하는 Project라면 Calendar를 사용하고 Java 1.8이후의 버전을 사용하는 Project라면 LocalDateTime을 사용한다. 

### Calendar 실습
```java
package com.ktdsuniversity.datetime;

import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;

public class CalendarExam {
	
	/**
	 * 현재 시간을 출력
	 * Calendar 인스턴스 출력
	 */
	public static void printNow() {
		// Calendar 는 singleton Instance 라서 getInstance()메소드를 통해서 가져온다. 
		// 아래 코드가 실행되는 순간의 PC 시간을 가져온다.
		Calendar now = Calendar.getInstance(); 
		System.out.println(now);
		
	}
	/**
	 * 컴퓨터가 처음 개발된 1970년 1월 1일 00시 00분 00초 부터 현재까지 흐른 시간 
	 * 1초 == 1000ms
	 */
	public static void printMilliSeconds() {
		
		long ms = System.currentTimeMillis();
		System.out.println(ms);
		
	}
	
	public static void printNowUseDate() {
		
		//밀리세컨즈를 사용하는 이유
		//메소드가 실행된 시간을 구할 때만 사용한다.
		//예를 들어, 게시글을 등록하는데 걸린 시간 0.012ms 소요됨
		
		long ms = System.currentTimeMillis();
		
		//java.util.Date
		Date now = new Date(ms);
		System.out.println(now);
		
	}
	
	public static void printYearMonthDate() {
		
		//1. Calendar 인스턴스 가져오기
		Calendar now = Calendar.getInstance();
	
		//2. 연도 가져오기
		int year = now.get(Calendar.YEAR);
		
		//3. 월 가져오기
		
		int month = now.get(Calendar.MONTH)+1;
		//4. 일 가져오기
		
		int day =  now.get(Calendar.DAY_OF_MONTH);
		//5. 출력하기
		
		//6. 시 가져오기
		int hour = now.get(Calendar.HOUR);
		//7. 분 가져오기
		int min = now.get(Calendar.MINUTE);
		//8. 초 가져오기
		int second = now.get(Calendar.SECOND);
		System.out.println(year);
		System.out.println(month);
		System.out.println(day);
		System.out.println(hour);
		System.out.println(min);
		System.out.println(second);
		
	
	}
	
	public static void printWeekDay() {
		
		String[] weekdays = {"일","월","화","수","목","금","토"};
		
		//1. Calendar 인스턴스 가져오기
		Calendar now = Calendar.getInstance();	
		//2. 현재 요일 가져오기 (1 ~ 7) (일 ~ 토)
		int dayOfWeek = now.get(Calendar.DAY_OF_WEEK);
		//3. 출력하기(숫자)
		
		System.out.println(weekdays[dayOfWeek-1]);
		System.out.println(dayOfWeek);
	}
	
	public static void printFormattedDateTime() {
		
		//1. Calendar 인스턴스 가져오기
		Calendar now = Calendar.getInstance();
		
		//2. 날짜 포메터 생성 - 정해진 규칙
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		//3. 날짜 포메터를 이용해서 날짜 만들기 
		String dateString = sdf.format(now.getTime());
		//4. 출력
		System.out.println(dateString);
		
	}
	
	public static void printFormattedDate() {
		
		// 연 월 일 출력
		Calendar now = Calendar.getInstance();
		
		SimpleDateFormat sdf = new SimpleDateFormat("yy/MMM/dd");
		
		String dateString = sdf.format(now.getTime());
		
		System.out.println(dateString);
		
		
	}
	
	
	public static void printFormattedTime() {
		
		// 시 분 초 출력
		
		Calendar now = Calendar.getInstance();
		SimpleDateFormat sdf = new SimpleDateFormat("HH:mm:ss.SSS");
		String dateString = sdf.format(now.getTime());
		
		System.out.println(dateString);
		
	}
	
	public static void printAfterDay(int amount) {
		
		//1. Calendar 인스턴스 가져오기
		Calendar now = Calendar.getInstance();
		//2. Calendar 인스턴스에 amount 만큼더하기
		now.add(Calendar.DAY_OF_MONTH, amount);
		//3. SimpleDateFormat으로 출력하기
		
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
		String formmatedDate = sdf.format(now.getTime());
		System.out.println(formmatedDate);
		
	}
	
	public static void printBeforeDay(int amount) {
		
		//1. Calendar 인스턴스 가져오기
		Calendar now = Calendar.getInstance();
		
		//2. Calendar 인스턴스에 amount만큼 빼기
		now.add(Calendar.DAY_OF_MONTH, amount > 0? -amount : amount);
		//3. SimpleDateFormat으로 출력하기
		
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
		String formmatedDate = sdf.format(now.getTime());
		System.out.println(formmatedDate);
				
	}
	
	public static void printAfterMonth(int amount) {
		
		Calendar now = Calendar.getInstance();
		
		now.add(Calendar.MONTH, amount);
		
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
		String formmatedDate = sdf.format(now.getTime());
		System.out.println(formmatedDate);
		
		
	}
	
	public static void printBeforeMonth(int amount) {
		
		
		Calendar now = Calendar.getInstance();
				
		now.add(Calendar.MONTH, amount > 0? -amount : amount);
			
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
		String formmatedDate = sdf.format(now.getTime());
		System.out.println(formmatedDate);
						
		
	}
	public static void printAfterYear(int amount) {
		
		Calendar now = Calendar.getInstance();
		
		now.add(Calendar.YEAR, amount);
		
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
		String formmatedDate = sdf.format(now.getTime());
		System.out.println(formmatedDate);
	}
	
	public static void printBeforeYear(int amount) {
		
		Calendar now = Calendar.getInstance();
		
		now.add(Calendar.YEAR, amount > 0? -amount : amount);
			
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
		String formmatedDate = sdf.format(now.getTime());
		System.out.println(formmatedDate);
						
	}
	
	public static void print(int year, int month, int date) {
		
		//1. Calendar 인스턴스 생성
		
		Calendar calendar = Calendar.getInstance();
		
		//2. 파라미터 변수들을 Calendar 인스턴스에 적용하기
		calendar.set(year, month-1, date, 0, 0, 0);
		
		//3. 포멧에 맞춰 출력하기
		
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		String formattedDate = sdf.format(calendar.getTime());
		
		System.out.println(formattedDate);
	}
	
	public static void print(int year, int month, int date, int hour, int minute, int second) {
		
		//1. Calendar 인스턴스 생성
		
		Calendar calendar = Calendar.getInstance();
		
		//2. 파라미터 변수들을 Calendar 인스턴스에 적용하기
		calendar.set(year, month-1, date, hour, minute, second);
		
		//3. 포멧에 맞춰 출력하기
		
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		String formattedDate = sdf.format(calendar.getTime());
		
		System.out.println(formattedDate);
	}
	//datetime = "2023.05.05"
	//datetime = "2023.05.05 13:10:34"
	public static void print(String datetime) {
		
//		String hintString = "2024-08-05";
//		String[] dateArray = hintString.split("[^0-9 ]");
//		
//		for(String date : dateArray) {
//			System.out.println(date.trim());
//		}
		
		String dateArray = datetime;
		String[] arr = dateArray.split("[^0-9 ]");
		
		
		int year = Integer.parseInt(arr[0].trim());
		int month = Integer.parseInt(arr[1].trim());
		int date = Integer.parseInt(arr[2].trim());
		
		Calendar calendar = Calendar.getInstance();
		
		//2. 파라미터 변수들을 Calendar 인스턴스에 적용하기
		calendar.set(year, month-1, date);

		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
		String formattedDate = sdf.format(calendar.getTime());
		
		System.out.println(formattedDate);
		
		
		
		
	}
	
	public static void printAllDays(int year, int month) {
		
		Calendar calendar = Calendar.getInstance();
		calendar.set(year, month - 1, 1);
		
		//1. 해당 연월의 마지막 날짜가 언제인지 구한다.
		//1-1. 월을 더해서 하루를 뺀다.
		
//		calendar.add(Calendar.MONTH, 1);
//		calendar.add(Calendar.DAY_OF_MONTH, -1);
//		
		
		
		//1-2. 해당 월의 마지막 날짜를 구한다.
		int lastDay = calendar.getActualMaximum(Calendar.DAY_OF_MONTH);
		System.out.println(lastDay);
		//2. 1일 부터 마지막 날짜까지 반복하면서 모든 날짜를 출력한다.
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
		for(int i=0; i<lastDay; i++) {
			String formattedDate = sdf.format(calendar.getTime());
			
			System.out.println(formattedDate);
			calendar.add(Calendar.DAY_OF_MONTH, 1);
			
		}
		
		
	}
	
	public static void printNextWorkingDay(int year, int month, int date) {
		
		//1. Calendar 인스턴스 가져오기
		Calendar calendar = Calendar.getInstance();
		calendar.set(year, month - 1, date);
		
		//2. 다음 영업일을 알기 위해서 하루 더하기
		calendar.add(Calendar.DAY_OF_MONTH, 1);
		
		//3. 하루를 더한 날짜가 영업일이 아니라면 계속 하루 더하기 
		while(true) {
			
			int dayOfWeek = calendar.get(Calendar.DAY_OF_WEEK);
			
			if(dayOfWeek == 1 || dayOfWeek == 7) {
				
				calendar.add(Calendar.DAY_OF_MONTH, 1);
				
			}
			else {
				
				break;
			}
			
		}
		
		
		
		//4. 출력
		
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
		String formattedDate = sdf.format(calendar.getTime());
		System.out.println(formattedDate);
		
	}
	
	
	
	public static void main(String[] args) {
		
		CalendarExam.printNow();
		CalendarExam.printMilliSeconds();
		CalendarExam.printNowUseDate(); // Date 사용됨 
		
		// Calendar 사용
		CalendarExam.printYearMonthDate();
		CalendarExam.printWeekDay();
		//1
		printFormattedDateTime();
		//2
		printFormattedDate();
		printFormattedTime();
		//3
		printAfterDay(1); // 하루 뒤는 언제인가?
		printAfterDay(10);// 10일 뒤는 언제인가?
		printAfterDay(30);// 30일 뒤는 언제인가?
		printAfterDay(100);// 100일 뒤는 언제인가?
		//4
		System.out.println();
		printBeforeDay(1); // 하루 전은 언제인가?
		printBeforeDay(10); // 10일 전은 언제인가?
		printBeforeDay(30); // 30일 전은 언제인가?
		printBeforeDay(100); // 100일 전은 언제인가?
		//5
		System.out.println();
		printAfterMonth(1); // 한달 후
		printAfterMonth(2); // 두달 후
		printAfterMonth(3); // 세달 후
		
		System.out.println();
		printBeforeMonth(1); // 한달 전
		printBeforeMonth(2); // 두달 전
		printBeforeMonth(3); // 세달 전
		
		System.out.println();
		printAfterYear(1); // 1년 후
		printAfterYear(2); // 2년 후
		printAfterYear(3); // 3년 후
		
		System.out.println();
		printBeforeYear(1); // 1년 전
		printBeforeYear(2); // 2년 전
		printBeforeYear(3); // 3년 전
		
		System.out.println();
		print(2023,5,5);
		
		System.out.println();
		print(2024, 4, 5, 13, 12, 53);
		
		System.out.println();
		print("2024년 5월 5일");
		print("2023. 4. 25");
		print("2022년 11월 19일");
		
		System.out.println();
		printAllDays(2024, 2);
		
		System.out.println();
		printNextWorkingDay(2024, 8, 8); // 토요일, 다음 영업일은 8월 5일이 출력되어야 한다.
	}
	
	

}
```
### Calendar 과제
```java
package com.ktdsuniversity.datetime;

import java.util.Calendar;

public class Birthday {
	
	
	public static void printDDay(int birthMonth, int birthDate) {
		
		Calendar now = Calendar.getInstance();
		
		int dayCount = 0;
		
		while(true) {
			
			int nowMonth = now.get(Calendar.MONTH) + 1;
			int nowDate = now.get(Calendar.DAY_OF_MONTH);
			
			if(nowMonth == birthMonth && nowDate == birthDate) {
				
				break;
			}
			
			now.add(Calendar.DAY_OF_MONTH, 1);
			dayCount ++ ;
			
		}
		
		System.out.println("생일까지 " + dayCount + "일 남았습니다." );
		
		
		
	}
	

	public static void nextBirthday() {
		
		Calendar calendar = Calendar.getInstance();
		
		System.out.println(calendar.get(Calendar.MONTH)+1);
		System.out.println(calendar.get(Calendar.DAY_OF_MONTH));
		
		int count = 0;
		while(true) {
			
			if(calendar.get(Calendar.MONTH)+1 == 2 && calendar.get(Calendar.DAY_OF_MONTH) == 18) {
				
				break;
			}
			else {
				
				count ++;
				calendar.add(Calendar.DAY_OF_MONTH, 1);
			}
		}
		
		System.out.println(count);
	
		
	}
	
	public static void main(String[] args) {
		
		nextBirthday();
		printDDay(2, 18);
		
		
		
	}

}
```
