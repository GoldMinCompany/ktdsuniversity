# 9. 생성자

* 클래스를 인스턴스로 만들어주는 특별한 메소드
* 반드시 **new**라는 키워드로 호출
* 생성자 이름은 인스턴스로 만들 클래스의 이름과 동일

```java
Car car = new Car(); // new 키워드를 통해 인스턴스가 생성되고 car에 할당된다.
CraneGameMachine cgm = new CraneGameMachine();
Student student = new Student();
```
* 생성자는 인스턴스를 생성하는 메소드
* 생성자를 정의하지 않았어도,
  생성자가 없는 클래스를 작성하면 **기본생성자**가 자동추가되어 호출가능하다.
  클래스 내부에 어떤 형태의 생성자가 하나라도 있다면, 자바 컴파일러(javac)는 기본형태의 생성자를 생성하지 않는다.
* 생성자는 반환타입이 없다. 생성자는 public **클래스 이름**으로 선언, 반환타입이 없는 메소드, return 키워드를 사용할 수 없다.
* 클래스 내부에 어떤 형태의 생성자가 하나라도 있다면, 자바 컴파일러(javac)는 기본형태의 생성자를 생성하지 않는다.

```
생성자 : 인스턴스를 만들어주는 메소드
메소드 : 특정 기능을 수행하는 코드 묶음
메소드 : 특정 기능을 수행하기 위하여 파라미터를 요구할 수 있다.
메소드 : 기능을 수행한 결과를 반환시킬 수 있지만, 생성자의 경우 반환타입이 없기 때문에 반환시킬 수 없다.
생성자는 메소드의 세번째 기능을 수행할 수 없고 2가지 기능만 수행할 수 있다.
```

### 생성자를 직접 정의하여 사용하는 이유
1. **멤버변수의 초기화**
2. 인스턴스 생성과 동시에 다른 메소드 호출

### * this 
현재 호출되고 있는 인스턴스를 의미한다. 특시, 생성자 내부에서 this는 **생성자가 만든 인스턴스**를 가리킨다.

### 생성자, this 실습
```java

public class ConstructorTest {
	
	String name;
	
	//클래스 내부에 어떤 형태의 생성자가 하나라도 있다면, 자바 컴파일러(javac)는 기본형태의 생성자를 생성하지 않는다. 
	public ConstructorTest() {
		
		System.out.println("ConstructorTest 인스턴스 생성");
		name = "Unknown"; //인스턴스가 생성되고, 코드가 실행된다. 
		System.out.println(name);
	}
	
	public ConstructorTest(String name) {
	
		this.name = name;
	}
}
```

```java

public class ConstructorTestMain {

	public static void main(String[] args) {
		
		/*
		 * 생성자 : 인스턴스를 만들어주는 메소드
		 * 메소드 : 특정 기능을 수행하는 코드 묶음
		 * 메소드 : 특정 기능을 수행하기 위해서 파라미터를 요구할 수 있다.
		 * 메소드 : 기능을 수행한 결과를 반환시킬 수 있다. -> 생성자는 반환타입이 없기 때문에 반환시킬 수 없다. 위의 2가지 기능만 수행할 수 있다.
		 */
		
		ConstructorTest constTest = new ConstructorTest();
		System.out.println(constTest.name);
		constTest.name = "ABC";
		System.out.println(constTest.name);
		constTest.name = "홍길동";
		System.out.println(constTest.name);
		
		ConstructorTest constTest2 = new ConstructorTest("금민규");
		System.out.println(constTest2.name);
	}

}
```
```java

public class CoffeeShop {
	
	int iceAmericano;
	int hotAmericano;
	
	
	public CoffeeShop(int iceAmericano, int hotAmericano) {
		
		this.iceAmericano = iceAmericano;
		this.hotAmericano = hotAmericano;
		
	}
	
	public int orderCoffee(int menu, int quantity) {
		
		if(menu == 1) {
			return this.iceAmericano * quantity;
		}
		
		return this.hotAmericano * quantity;
		
	}

}

```

```java

public class CoffeeShopMain {

	public static void main(String[] args) {
		
		CoffeeShop tomAndToms = new CoffeeShop(5500, 5000);
//		tomAndToms.iceAmericano = 5500;
//		tomAndToms.hotAmericano = 5000;
		
		CoffeeShop mammoth = new CoffeeShop(1400, 1200);
		
//		mammoth.iceAmericano = 1400;
//		mammoth.hotAmericano = 1200;
		
		
		int tomAndTomsIceAmericanoPrice = tomAndToms.orderCoffee(1, 10);
		int tomAndTomsHotAmericanoPrice = tomAndToms.orderCoffee(2, 5);
		
		System.out.println("탐탐 아아 : "+ tomAndTomsIceAmericanoPrice);
		System.out.println("탐탐 아아 : "+ tomAndTomsHotAmericanoPrice);
		
		int mammothIceAmericanoPrice = mammoth.orderCoffee(1, 10);
		int mammothHotAmericanoPrice = mammoth.orderCoffee(2, 10);
		
		System.out.println("메머드 아아 : "+ mammothIceAmericanoPrice);
		System.out.println("메머드 뜨아 : "+ mammothHotAmericanoPrice);
		
	}

}

```


