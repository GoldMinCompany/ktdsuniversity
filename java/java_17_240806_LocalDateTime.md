# LocalDateTime

* Java 1.8 이후 Calendar를 대체하기 위해 날짜, 시간, 날짜와 시간 모두를 처리할 수 있는 Class가 3개 추가
* 날짜만 가져오기 LocalDate Class
* 시간만 가져오기 LocalTime Class
* 날짜와 시간 모두 가져오기 LocalDateTime Class

```java
package com.ktdsuniversity.datetime;

import java.time.DayOfWeek;
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.LocalTime;
import java.time.Period;
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
		
		DateTimeFormatter dtf = DateTimeFormatter.ofPattern("HH:mm:ss.SSS");
		String formattedTime = dtf.format(now);
		System.out.println(formattedTime);
		
		
	}
	
	public static void printNowDateTime() {
		

		//날짜와 시간을 모두 가져오는 기능
		//java.time.LocalDateTime
		
		LocalDateTime now = LocalDateTime.now();
		System.out.println(now);
		
		DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy년 MM월 dd일 HH:mm:ss.SSS 'ms'");
		String formattedDateTime = dtf.format(now);
		System.out.println(formattedDateTime);
		
	}
	
	public static void printResultAddDate() {
		
		// LocalDate에 날짜를 더하기
		LocalDate date = LocalDate.of(2024, 8, 6);
		System.out.println("before : " + date);
		
		DayOfWeek dayOfWeek = date.getDayOfWeek(); //반환 타입이 DayOfWeek
		System.out.println(dayOfWeek);
		
		date = date.plusDays(4); // 반환 타입이 LocalDate
		date = date.plusMonths(2);
		date = date.plusYears(5);
		
		dayOfWeek = date.getDayOfWeek();
		
		System.out.println("after : " + date);
		System.out.println(dayOfWeek);
		
	}
	
	public static void printResultMinusDate() {
		
		// 2024년 10월 16일 LocalDate를 만든다.
		// 4년 8개월 12일을 뺀 결과를 출력한다.
		
		LocalDate date = LocalDate.of(2024, 10, 16);
		System.out.println("before : " + date);
		
		date = date.minusDays(12);
		date = date.minusMonths(8);
		date = date.minusYears(4);
		
		System.out.println("after : " + date);
	}
	
	public static void printNowDayOfWeek() {
		
		LocalDate now = LocalDate.now();
		
		DayOfWeek dayOfWeek = now.getDayOfWeek();
		System.out.println(dayOfWeek);
		
	
		
	}
	
	public static void printResultAddTime() {
		//Local Time
		LocalTime date = LocalTime.of(17, 51, 35);
		System.out.println("before : " + date);
		
		date = date.plusHours(0);
		date = date.plusMinutes(2);
		date = date.plusSeconds(15);
		
		System.out.println("after : " + date);
	}
	
	public static void printResultMinusTime() {
		//Local Time
		
		LocalTime date = LocalTime.of(17, 49, 51);
		System.out.println("before : " + date);
		
		date = date.minusHours(2);
		date = date.minusMinutes(5);
		date = date.minusSeconds(1);
		
		System.out.println("after : " + date);
		
	}
	
	public static void printResultAddDateTime() {
		//LocalDateTime
		
		LocalDateTime datetime = LocalDateTime.of(2024, 8, 24, 17, 51, 53);
		System.out.println("before : " + datetime);
		
		datetime = datetime.plusYears(1);
		datetime = datetime.plusMonths(2);
		datetime = datetime.plusDays(3);
		datetime = datetime.plusHours(4);
		datetime = datetime.plusMinutes(5);
		datetime = datetime.plusSeconds(6);
		
		System.out.println("after : " + datetime);
		
	}
	
	public static void printResultMinusDateTime() {
		//LocalDateTime
		
		LocalDateTime datetime = LocalDateTime.of(2024, 8, 24, 17, 51, 53);
		System.out.println("before : " + datetime);
		
		datetime = datetime.minusYears(1);
		datetime = datetime.minusMonths(2);
		datetime = datetime.minusDays(3);
		datetime = datetime.minusHours(4);
		datetime = datetime.minusMinutes(5);
		datetime = datetime.minusSeconds(6);
		
		System.out.println("after : " + datetime);
		
		
	}
	
	public static void printNextWorkingDay() {
		//LocalDate를 이용해 현재날짜로부터 다음 영업일 이후의 날짜를 구하세요.
		
		LocalDateTime now = LocalDateTime.now();
		System.out.println(now);


		int workingDays = 0;
			
		while(true) {
				
			now = now.plusDays(1);
			DayOfWeek dayOfWeek = now.getDayOfWeek();
				
			if(dayOfWeek != DayOfWeek.SATURDAY && dayOfWeek != DayOfWeek.SUNDAY) {
					
				workingDays ++;
			}
				
			if(workingDays == 6) {
					break;
			}
				
		}
			
		DateTimeFormatter dtf = DateTimeFormatter.ofPattern("6일 뒤 영업일은 yyyy년 MM월 dd일 (E요일) 입니다.");
		String formattedDate = dtf.format(now);
		System.out.println(formattedDate);
	
	}
	
	public static void printCorrectDatePeriod() {
		
		// 올바른 기간 검색 파라미터인가?
		
		LocalDate from = LocalDate.of(2024, 12, 31);
		LocalDate to = LocalDate.of(2024, 12, 31);
		
		// from이 to보다 같거나 과거인가?
		boolean isCorrectDatePeriod = from.isBefore(to) || from.isEqual(to);
		System.out.println(isCorrectDatePeriod);
		
	}
	
	public static void printBetween() {
		
		LocalDate startDate = LocalDate.of(2020, 1, 13);
		LocalDate endDate = LocalDate.of(2024, 8, 6);
		
		//startDate와 endDate의 차이는?
		Period between = Period.between(startDate, endDate);
		System.out.println(between);
		

		int year = between.getYears();
		int month = between.getMonths();
		int days = between.getDays();
		
		System.out.println(year+"년 "+month+"개월 "+days+"일");
		
		
	}
	
	public static void main(String[] args) {
		
		
		printNowDate();
		System.out.println();
		printNowTime();
		System.out.println();
		printNowDateTime();
		System.out.println();
		printResultAddDate();
		System.out.println();
		printResultMinusDate();
		System.out.println();
		printNowDayOfWeek();
		System.out.println();
		printResultAddTime();
		System.out.println();
		printResultMinusTime();
		System.out.println();
		printResultAddDateTime();
		System.out.println();
		printResultMinusDateTime();
		System.out.println();
		printNextWorkingDay();
		System.out.println();
		printCorrectDatePeriod();
		System.out.println();
		printBetween();
	}

}
```
