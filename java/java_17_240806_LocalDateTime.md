# LocalDateTime

* Java 1.8 이후 Calendar를 대체하기 위해 날짜, 시간, 날짜와 시간 모두를 처리할 수 있는 Class가 3개 추가
* 날짜만 가져오기 LocalDate Class
* 시간만 가져오기 LocalTime Class
* 날짜와 시간 모두 가져오기 LocalDateTime Class

```java
package com.ktdsuniversity.datetime;

import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;

public class LocalDateTimeExam {
	
	public static void printNowDate() {
		//날짜만 가져오는 기능
		LocalDate now = LocalDate.now();
		System.out.println(now);
		
		DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy년 MM월 dd일");
		String formattedDate = dtf.format(now);
		System.out.println(formattedDate);
	}
	
	public static void printNowTime() {
		
		//시간만 가져오는 기능
		//java.time.LocalTime
		LocalTime now = LocalTime.now();
		System.out.println(now);
		
		DateTimeFormatter dtf = DateTimeFormatter.ofPattern("HH:mm:ss");
		String formattedTime = dtf.format(now);
		System.out.println(formattedTime);
		
		
	}
	
	public static void printNowDateTime() {
		

		//날짜와 시간을 모두 가져오는 기능
		//java.time.LocalDateTime
		
		LocalDateTime now = LocalDateTime.now();
		System.out.println(now);
		
		DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy년 MM월 dd일 HH:mm:ss");
		String formattedDateTime = dtf.format(now);
		System.out.println(formattedDateTime);
		
	}
	
	public static void main(String[] args) {
		
		
		printNowDate();
		printNowTime();
		printNowDateTime();
		
	}

}
```
