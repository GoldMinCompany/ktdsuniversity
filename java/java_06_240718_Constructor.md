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

### * 생성자를 직접 정의하여 사용하는 이유
1. **멤버변수의 초기화**
2. 인스턴스 생성과 동시에 다른 메소드 호출

### * this 
현재 호출되고 있는 인스턴스를 의미한다. 특시, 생성자 내부에서 this는 **생성자가 만든 인스턴스**를 가리킨다.

### * 생성자, this 실습
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
---
# 9. 데이터 클래스

* 속성은 있지만, 기능이 없는 클래스
* 일반적으로 VO(Value Object) 클래스로 부른다. 값만 정의된 클래스
* java14부터 Data Class를 손쉽게 정의할 수 있다.

* has a 관계 : 클래스 내부에 멤버변수로 클래스가 있는 것을 has a 관계라 한다. 
* is a 관계

### * 데이터 클래스 실습

```java
package franchise;
/**
 * 자판기에서 판매하는 상품
 * Data Class
 * 지역변수와 생성자만 선언
 */
public class BeverageSolution {
	
	String name; //상품 이름
	int price; //상품 가격
	int stock; //상품 재고
	
	public BeverageSolution(String name, int price, int stock) {
		
		this.name = name;
		this.price = price;
		this.stock = stock;
	}

}
```

```java
package franchise;

/**
 * 음료수를 판매하는 자판기
 * 4개의 음료수를 판매
 * has a 관계 
 */
public class BeverageVendingMachineSolution {
	
	BeverageSolution itemOne;
	BeverageSolution itemTwo;
	BeverageSolution itemThree;
	BeverageSolution itemFour;
	
	//멤버변수 초기화
	public BeverageVendingMachineSolution(BeverageSolution itemOne, BeverageSolution itemTwo, BeverageSolution itemThree, BeverageSolution itemFour) {
		
		this.itemOne = itemOne;
		this.itemTwo = itemTwo;
		this.itemThree = itemThree;
		this.itemFour = itemFour;
		
	}
	
	
	//주문하기
	public BeverageSolution selectItemButton(String itemName, int pressTime) {
		
		if(this.itemOne.name.equals(itemName)) {
			
			if(this.itemOne.stock >= 0 && this.itemOne.stock >= pressTime) {
				this.itemOne.stock -= pressTime;
				
				BeverageSolution outputItem = new BeverageSolution(this.itemOne.name,
																	this.itemOne.price * pressTime, pressTime);
				return outputItem; // 손님에게 상품을 전달한다. 
			}
			else {
				System.out.println("상품이 품절되었습니다.");
				return null;
			}
			
			
		}
		else if(this.itemTwo.name.equals(itemName)) {
			
			if(this.itemTwo.stock >= 0 && this.itemTwo.stock >= pressTime) {
				this.itemTwo.stock -= pressTime;
				
				BeverageSolution outputItem = new BeverageSolution(this.itemTwo.name,
																	this.itemTwo.price * pressTime, pressTime);
				return outputItem; // 손님에게 상품을 전달한다. 
			}
			else {
				System.out.println("상품이 품절되었습니다.");
				return null;
			}
			
		}
		else if(this.itemThree.name.equals(itemName)) {
			
			if(this.itemThree.stock >= 0 && this.itemThree.stock >= pressTime) {
				this.itemThree.stock -= pressTime;
				
				BeverageSolution outputItem = new BeverageSolution(this.itemThree.name,
																	this.itemThree.price * pressTime, pressTime);
				return outputItem; // 손님에게 상품을 전달한다. 
			}
			else {
				System.out.println("상품이 품절되었습니다.");
				return null;
			}
			
		}
		else if(this.itemFour.name.equals(itemName)) {
			
			if(this.itemFour.stock >= 0 && this.itemFour.stock >= pressTime) {
				this.itemFour.stock -= pressTime;
				
				BeverageSolution outputItem = new BeverageSolution(this.itemFour.name,
																	this.itemFour.price * pressTime, pressTime);
				return outputItem; // 손님에게 상품을 전달한다. 
			}
			else {
				System.out.println("상품이 품절되었습니다.");
				return null;
			}
			
		}
		else {
			System.out.println("그런 상품은 없습니다.");
		}
		return null; // 메모리에 할당된 데이터가 없는 상태
	}
	//입고
	public void fillItem(String itemName, int stock) {
		
		if(this.itemOne.name.equals(itemName)) {
			this.itemOne.stock += stock;
		}
		else if(this.itemTwo.name.equals(itemName)) {
			this.itemTwo.stock += stock;
		}
		else if(this.itemThree.name.equals(itemName)) {
			this.itemThree.stock += stock;
		}
		else if(this.itemFour.name.equals(itemName)) {
			this.itemFour.stock += stock;
		}
		else {
			System.out.println("그런 상품은 팔지 않습니다.");
		}
		
	}
	//재고
	public void printStock() {
		
		System.out.println(this.itemOne.name + " : " + this.itemOne.stock + "개" );
		System.out.println(this.itemTwo.name + " : " + this.itemTwo.stock + "개" );
		System.out.println(this.itemThree.name + " : " + this.itemThree.stock + "개" );
		System.out.println(this.itemFour.name + " : " + this.itemFour.stock + "개" );

	}
	

}
```

```java
package franchise;

public class BeverageMainSolution {

	public static void main(String[] args) {
		
		BeverageSolution bacchas = new BeverageSolution("박카스", 900, 15);
		BeverageSolution monster = new BeverageSolution("몬스터", 1500, 20);
		BeverageSolution hotsix  = new BeverageSolution("핫식스", 1300, 10);
		BeverageSolution milkiss = new BeverageSolution("밀키스", 1400, 5);
		
		
		BeverageVendingMachineSolution bvms = new BeverageVendingMachineSolution(bacchas, monster, hotsix, milkiss);

		
		bvms.printStock();
		BeverageSolution myItem = bvms.selectItemButton("스타벅스 더블샷", 2000);
		System.out.println(myItem);
		//System.out.println(myItem.name);
		
		BeverageSolution mySecondItem = bvms.selectItemButton("밀키스", 3);
		System.out.println(mySecondItem);
		System.out.println("구매한 상품의 이름 : "+mySecondItem.name);
		System.out.println("구매한 상품의 가격 : "+mySecondItem.price);
		System.out.println("구매한 상품의 개수 : "+mySecondItem.stock);
		
		bvms.printStock();
		
		bvms.selectItemButton("밀키스", 10);
		bvms.printStock();
		
		
		
		BeverageSolution myThirdItem = bvms.selectItemButton("밀키스", 3);
		System.out.println(myThirdItem);
		//System.out.println("구매한 상품의 이름 : "+myThirdItem.name);
		//System.out.println("구매한 상품의 가격 : "+myThirdItem.price);
		//System.out.println("구매한 상품의 개수 : "+myThirdItem.stock);
		
		bvms.fillItem("밀키스", 10);
		bvms.printStock();
		
	}

}
```

# 9. 유틸리티 클래스

* 기능만 존재하고 속성이 없는 클래스
* 물리적으로 존재하지 않지만, OOP에서 흔하게 사용하는 클래스
* 일반적으로 Utility 클래스 만들 때 사용
* static이 붙음

