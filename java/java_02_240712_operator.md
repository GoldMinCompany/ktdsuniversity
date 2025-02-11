
# 6. 연산자

* 할당 연산자 : =

* 산술 연산자 : +, -, *, /, %

* 비교 연산자 : ==, >, >=, <, <=, !=

		Primitive Type에서만 사용되고, 왼쪽에서 오른쪽으로 연산이 진행된다. 

		비교 연산자의 결과 값은 True 또는 False이다. 

* 논리 연산자 : &&, ||, !

* 비트 연산자

### * 연산자 우선순위(PEMDAS)

![image](https://github.com/user-attachments/assets/dfa430fd-22d2-4e55-971c-d7427d2cc3e3)

### * 일반 산술 연산자, 짧은 표현식 실습 

```java
public static void main(String[] args) {
		
		// 일반 산술 연산자 실습
		int number1 = 10;
		number1 = number1 + 2;
		System.out.println(number1); // 12
		// Ctrl + F11 ==> 클래스 실행
		
		int number3 = 30;
		number3 = number3 + 6;
		System.out.println(number3);
		
		int number4 = 40;
		number4 = number4 - 10;
		System.out.println(number4);
		
		int number5 = 50;
		number5 = number5 * 12;
		System.out.println(number5);
		
		int number6 = 60;
		number6 = number6 / 14;
		System.out.println(number6);
		
		int number7 = 70;
		number7 = number7 % 16;
		System.out.println(number7);
		
		// 짧은 표현식 실습
		// 생긴 배경 : 코드를 짧은 시간내에 효율적으로 작성하기 위해
		
		int number2 = 20;
		number2 += 4;
		
		System.out.println(number2);
		
		number3 += 6;
		number4 -= 10;
		number5 *= 12;
		number6 /= 14;
		number7 %= 16;
		
		System.out.println(number3);
		System.out.println(number4);
		System.out.println(number5);
		System.out.println(number6);
		System.out.println(number7);
		
	}
```


### * 단항 연산자 실습

```java

public static void main(String[] args) {
		
		//스스로에게 1을 더하는 실습
		//일반 연산자
		
		int number1 = 0;
		number1 = number1 + 1;
		//System.out.println("number1 = " + number1);
		
		// 짧은 연산자
		number1 = 0; // 0으로 초기화(재할당)
		number1 += 1;
		//System.out.println("number1 = " + number1);
		
		// 단한 연산자
		number1 = 0; // 0으로 초기화(재할당)
		
		number1 ++ ; // 대입 후 number1 증가 
		System.out.println("number1 = " + number1++);
		System.out.println("number1 = " + number1++);
		System.out.println("number1 = " + number1++);
		System.out.println("number1 = " + number1);
		
		int number2 = 0;
		++number2; //number2 증가시킨 후 대입
		System.out.println(number2); // 1
		
		System.out.println(++number2);
		System.out.println(++number2);
		System.out.println(++number2);
		System.out.println(number2);
}
```

### * 산술 연산자 실습

```java
public static void main(String[] args) {
//Quiz 1
//		int minutes = 5 ;
//		int seconds = 50;
//		
//		int time = 0;
//		time = minutes * 60 + seconds;
//		System.out.println("time : " + time);
		
//Quiz 2
		int processTime = 145 ;
		
		int minutes = 0;
		int seconds = 0;
		
		minutes = processTime / 60;
		seconds = processTime % 60;
		
		System.out.println("minutes : " + minutes);
		System.out.println("seconds : " + seconds);
		
//Quiz 3
		int celsius = 30;
		int fahrenheit = 0;
		
		fahrenheit = (celsius * 9/5) + 32;
		System.out.println("섭씨 : " + celsius);
		System.out.println("화씨 : " + fahrenheit);
		
	}
```

### * 논리연산자, 3항 연산자 실습

```java
public static void main(String[] args) {
		
		//키보드에서 숫자를 입력받고 변수에 할당
		//Ctrl + Shift + O
		Scanner keyboard = new Scanner(System.in);
		
		System.out.println("0보다 크고 100보다 작은 숫자를 입력하세요.");
		// 키보드에서 숫자 입력을 받아 변수에 할당
		int firstNumber = keyboard.nextInt();
		System.out.println("방금 입력한 숫자는 "+ firstNumber + "입니다.");
		
		boolean isGreaterThanZero = firstNumber > 0 ;
		System.out.println("0보다 큰가요? " + isGreaterThanZero);
		
		// 3항 연산자
		// 조건(boolean의 결과) ? true 일 때의 처리 : false 일 때의 처리
		// firstNumber가 0보다 작은 숫자인 경우에는 "음수입니다."
		// firstNumber가 0보다 큰 숫자인 경우에는 "양수입니다. "
		System.out.println(firstNumber > 0 ? "양수입니다. " : "음수입니다. ");
		
		
		boolean isLessThanHundred = firstNumber < 100 ;
		System.out.println("100보다 작은가요? " + isLessThanHundred);
		
		
		System.out.println(firstNumber < 100 ? "알맞은 숫자입니다. " : "너무 큰 숫자입니다. ");
		
		
		boolean isValidNumber = isGreaterThanZero && isLessThanHundred;
		System.out.println("0보다 크고 100보다 작은가요? "+isValidNumber);
		System.out.println(isValidNumber? "성공" : "실패");
		
}
```

### * 논리연산자 연습문제

```java
public static void main(String[] args) {
		
		Scanner keyboard = new Scanner(System.in);
		
		/*
		 * 조건
		 * 1. 정수형 변수 두개를 생성
		 * 2. keyboard.nextInt()를 이용해 값을 각각의 변수에 할당
		 * 3. 두 변수의 값 중 큰 값을 가진 변수의 값을 출력!(3항 연산자 사용)
		 */
		
		int firstNumber = keyboard.nextInt();
		int secondNumber = keyboard.nextInt();

		int biggestNumber = (firstNumber > secondNumber) ? firstNumber : secondNumber;
		System.out.println(biggestNumber);

}

```

### * 논리연산자 Dead Code (&&, ||)
`
- and 연산(&&)에서 dead code가 발생하는 원인 : 첫 번째 피연산자가 false인 경우 두번째 피연산자를 검증하지 않아도 false이기 때문
- or  연산(||)에서 dead code가 발생하는 원인 : 첫 번째 피연산자가  True인 경우 두번째 피연산자를 검증하지 않아도  True이기 때문

`

# 7. 실행흐름 제어

* if - else	

`변수의 값에 따라 실행의 흐름을 바꾸어야 할 때 사용할 수 있음. 여러 경우의 수 중 단 하나의 경우만 실행함.`

* switch

* while

* for

### * if else 실습

```java

public static void main(String[] args) {
		
		/*
		 * 내가 가진 돈이 2000원
		 * 어떤 물건을 사는데 물건의 가격이 랜덤이다
		 * 
		 * 이 물건을 살 수 있으면 "구매 성공" 이라고 출력하고
		 * 살 수 없으면 "구매 실패"라고 출력한다. 
		 * 
		 * 살 수 있는 조건 (내가 가진 돈 >= 물건가격) ==> 구매 성공!
		 */
		int money = 2000;
		
		//Ctrl + Shift + O
		//랜덤한 숫자를 가져오는 코드 
		Random random = new Random();
		
		//int 범위에서 숫자 중의 하나 값을 호출
//		int price = random.nextInt();
		
		int price = random.nextInt(4000); // 0~3999 중 하나 
		
		System.out.println("내가 가진 돈 : " + money);
		System.out.println("상품의 가격  : " + price);
		
		if (money >= price) {
			System.out.println("구매 성공!");
		}else {
			System.out.println("구매 실패!");
		}
		
		

}

```
