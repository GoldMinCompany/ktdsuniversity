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
		//int price = random.nextInt();
		
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

### * 변수의 범위

```
변수는 중괄호 내부에서만 사용될 수 있다. 

중괄호 내부에 선언된 변수는 중괄호 밖에서 사용될 수 없다. 
```

```java

public static void main(String[] args) {
	
		int number = 10;
		int other = 15;
		
		int bigNumber = 0;
		
		/*
		 *  number의 수와 other 수 중 큰 숫자는 무엇인가?
		 *  if와 else를 통해 비교를 한다. 
		 *  
		 */
		
		if(number > other) {
			//System.out.println(number);
			//System.out.println(number +"와 " + other + "중 큰 숫자는 " + number + "입니다.");
			bigNumber = number; 
		}
		else {
			//System.out.println(other);
			//System.out.println(number + "와 " + other + "중 큰 숫자는 "+ other + "입니다.");
			bigNumber = other;
		}
		System.out.println(number + "와 " + other+"중 큰 숫자는 "+bigNumber+"입니다.");
		
}
```

### * if문을 활용한 랜덤 게임

```java
public static void main(String[] args) {
		
		/* 주석으로 사용한 코드를 Pseudo Code(의사 코드)라 하고 코드의 논리식을 문자열로 나열
		 * 1. 랜덤한 숫자 하나를 생성한다.
		 * 2. 사용자가 숫자 입력
		 * 3. 랜덤한 숫자와 사용자가 입력한 숫자가 같은지 비교한다. 
		 * 4. 같다면... "정답!"출력
		 * 5. 랜덤한 숫자가 사용자 입력한 숫자보다 작은지 비교... "Down!"출력
		 * 6. 랜덤한 숫자가 사용자 입력한 숫자보다 큰지 비교... "Up!" 출력  
		 */
		
		Random random = new Random();
		Scanner keyboard = new Scanner(System.in);
		
		
		System.out.print("숫자 입력 : ");
		int number = keyboard.nextInt();
		
		int randomNumber = random.nextInt(50)+1;
		System.out.println("랜덤 숫자 : "+ randomNumber);
		
		String msg = null;
		
		
		if(number == randomNumber) {
			
			//System.out.println("정답!");
			msg = "정답!";
		}
		else if(number > randomNumber) {
			
			//System.out.println("Up!");
			msg = "Up!";
		}
		else if(number < randomNumber) {
			 
			//System.out.println("Down!");
			msg = "Down!";
		}
		
		System.out.println(msg);
}
```

### *if, if else문 연습문제

```java
public static void main(String[] args) {
		
		int korScore = 90;
		int engScore = 88;
		int mathScore = 70;
		int progScore = 80;
		
		int sum = 0;
		double avg = 0;
		
		sum = korScore + engScore + mathScore + progScore ;
		avg = (double) sum / 4;
		
		String grade = null;
		
		if(avg >= 95) {
			
			grade = "A+";
		
		}else if(avg >= 90){
			
			grade = "A";
			
		}else if(avg >= 85){
			
			grade = "B+";
			
		}else if(avg >= 80){
			
			grade = "B";
			
		}else if(avg >= 70){
			
			grade = "C";
			
		}
		else{
			
			grade = "F";
			
		}
		
		System.out.println("성적 : " + grade);
}
```

```java
public static void main(String[] args) {
		
		int money = 1_000_000;
		
		int father = 40;
		int mother = 36;
		int daughter = 11;
		
		
		int adultOneWayFlightFare = 300_000;
		int kidOneWayFlightFare = 120_000;
		
		String msg = null;
		
		
		int fee = 0;
		
		if(father >= 19) {
			
			fee += adultOneWayFlightFare;
			
		}
		else {
			fee += kidOneWayFlightFare;
		}
		
		if(mother >= 19) {
			
			fee += adultOneWayFlightFare;
		}
		else {
			
			fee += kidOneWayFlightFare;
		}
		
		if(daughter >= 19) {
			fee += adultOneWayFlightFare;
		}
		else{
			
			fee += kidOneWayFlightFare;
		}
		
		if(money >=  fee) {
			msg = "여행가자!";
		}
		else {
			msg = "다음에 가자!";
		}
		
		System.out.println("요금 : " + fee);
		System.out.println(msg);
	
}
```

```java
public static void main(String[] args) {
		
		Scanner keyboard = new Scanner(System.in);
		
		System.out.print("아버지 나이 : ");
		int father = keyboard.nextInt();
		
		System.out.print("어머니 나이 : ");
		int mather = keyboard.nextInt();
		
		System.out.print("딸 나이 : ");
		int daughter = keyboard.nextInt();
		
		
		int adultOneWayFlightFare = 300_000;
		int kidOneWayFlightFare = 120_000;
		
		int adultCount = 0;
		int kidCount = 0;
		
		if(father >=19) {
			adultCount ++;
		}
		else {
			kidCount ++;
		}
		
		if(mather >=19) {
			adultCount ++;
		}
		else {
			kidCount ++;
		}
		
		if(daughter >=19) {
			adultCount ++;
		}
		else {
			kidCount ++;
		}
			
		int fee = adultCount * adultOneWayFlightFare + kidCount * kidOneWayFlightFare;
		System.out.println("요금 : " + fee);
	
}
```
### * Switch 문
```
개발자들 사이에서 거의 사용하지 않는다.

순차적으로 실행되지 않아도 조건에 따라 결과 값을 출력할 수 있다. 
```

### * while 문
```
반복 제어문, 조건이 False가 될 때까지 반복을 수행하는 Keyword이다.

while(조건){
	반복될 code block
}

조건은 알고 있지만, 언제까지 반복되는지는 알기 어렵다. 
```

```java
public static void main(String[] args) {
		
		System.out.println("맞출때까지 반복되는 Up Down 게임!");
		
		/*
		 * Random random = new Random(); 
		 * Scanner keyboard = new Scanner(System.in);
		 */
		
		double randomFloatingPoint = Math.random(); // 부동소수점 형태의 난수 생성
		int correctNumber = (int) (randomFloatingPoint * 100);
		
		System.out.println("correctNumber :" + correctNumber);
		
		Scanner keyboard = new Scanner(System.in);
		
		
		while(true) {
			
			System.out.println("숫자를 입력하세요 : ");
			int answer = keyboard.nextInt();
			
			if(correctNumber == answer) {
				System.out.println("정답!");
				break; //반복문 끝, 현재흐름을 중단시키는 keyword! 
			}
			else if(correctNumber > answer) {
				System.out.println("Up!");
			}
			else {
				System.out.println("Down!");
			}
			
		}
		

}
```

### * for 문

```
원하는 만큼만 반복시킨다.

for(초기값; 반복조건; 증감식){

}
```
### * for 문 실습 

```java
for(int i=1; i<11; i++){
	System.out.println(i);
}
```

```java
public static void main(String[] args) {
		/*
		 * for(초기값; 반복조건; 증감식){
		 * 		반복할 코드
		 * }
		 */
		
		for(int i=1; i<101; i++) {
			
			//System.out.println(i);
			
		}


		//1. 1부터 1000까지 합 출력
		int sum = 0;
		for(int i=1; i<1001; i++) {
			//System.out.println(i);
			sum += i;
			
		}
		
		System.out.println("총합 : " + sum);

		// 2. 1부터 100 중 짝수의 합 출력
		sum = 0;
		for(int i=1; i<101; i++) {
			
			if(i % 2 == 0) {
				//System.out.println(i);
				sum += i;
			}
			
		}
		
		System.out.println("총합 : " + sum);


		// 2. 1부터 100중 짝수의 합 출력
		sum = 0;
		for(int i=2; i<101; i += 2) {
			
			sum += i;
			
		}
		System.out.println("총합 : "+ sum);
		

}
```
### * 이중 for문

```java
public static void main(String[] args) {
		/*
		 * for(초기값; 반복조건; 증감식){
		 * 		반복할 코드
		 * }
		 */
		
		//3. 구구단 3단을 출력, 2중 for문
		
		for(int i=3; i<4 ; i++) {
			for(int j=1; j<10; j++) {
				
				System.out.println(i+" X "+j+" = "+ i*j);
				
			}
			
		}
		//4. 2단부터 9단까지 구구단 출력, 이중 for문 사용
		for(int i=2; i<10; i++) {
			for(int j=1; j<10; j++)
			{
				System.out.println(i+" X "+j+" = "+ i*j);
			}
			System.out.println();
			
		}
		//5.
		Scanner keyboard = new Scanner(System.in);
		
		int x = keyboard.nextInt();
		int y = keyboard.nextInt();
		
		for(int i=1; i<y+1; i++) {
			System.out.println(x + " X " + i + " = " + x*i);	
		}		
}
```
