# 추상화
* 복잡한 자료, 모듈, 시스템으로 부터 핵심적인 개념 또는 기능을 간추려내는 것
  * 제공해야 하는 기능이 아직 구현되지 않은 상태
  * 기능이 구현되어 있지 않기 때문에 인스턴스화 불가능
  * Default Method인 경우 구현하지 않고 추상 클래스나 인터페이스를 구현하는 경우 추상 메소드를 반드시 구현해야 한다.
  * 추상 메소드를 구현할 때, 구조를 변경할 수 없다.
  * 추상화란, 기능에 대한 표준이며 규약
 
* 추상화의 이점
  * 인터페이스나 추상클래스만으로 코드 작성이 가능하다.
  * 하나의 인터페이스나 추상클래스로 여러 개의 구현 클래스를 만들 수 있다.(다형성)
  * 여러 개의 구현 클래스를 이용해 유연한 상황대처 가능하다.
    * 갑자기 다른 기능 구현을 추가하고자 할 때, 인터페이스만 가져와 상속시키고 필요한 것만 수정한다.

* 추상클래스와 인터페이스 차이
  * 추상클래스와 인터페이스는 추상 메소드를 만든다.
  * 일반적으로 추상메소드의 비율로 추상화 정도를 나타낸다.

* 추상클래스 - Class
  * 추상메소드와 일반메소드가 혼재할 가능성이 높다(불완전 추상화)
 
* 인터페이스 - interface
  * 추상메소드만 존재한다(완전 추상화)
 
* 추상클래스의 활용
  * 기존 코드를 확장할 때, 기존의 코드는 수정하지 않고 추상클래스를 활용해 전 후에 해야할 일을 작성한다.

* 인터페이스의 활용
  * 제공되는 기능에 대해서는 알고 있지만, 환경에 따라 기능의 구현이 변경되어야 할 때
    * 환경, OS, Database, 사용자 입력 값등의 여러 요인 존재 

# 인터페이스 실습

```java
package com.ktdsuniversity.edu.interfaceexam.animals;

public interface Move {
	
	
	public void walk();
	public void run();

}
```

```java
package com.ktdsuniversity.edu.interfaceexam.animals;

public interface Bark {
	
	public void bark();

}
```

```java
package com.ktdsuniversity.edu.interfaceexam.animals;

public interface Eat {
	public void eat();
}
```

```java
package com.ktdsuniversity.edu.interfaceexam.animals;

public interface Fly {
	/**
	 * Default Abstract Method
	 * Default method : 추상 메소드가 기본적으로 하는 일을 미리 만들어 주는 것
	 */
	public default void fly() {
		//여기서는 변수를 사용할 수 없다.
		System.out.println("날아 다닙니다");
		
	}

}

```


```java
package com.ktdsuniversity.edu.interfaceexam.animals;

public class Dog implements Move, Bark, Eat {

	
	private String name;
	
	public Dog(String name) {
		this.name = name;
	}
	
	@Override
	public void walk() {
		System.out.println(name + "이 걷습니다.");
		
	}
	@Override
	public void run() {
		System.out.println(name + "이 뜁니다.");
	}
	
	@Override
	public void bark() {
		System.out.println(name + " : 멍멍");
	}
	
	@Override
	public void eat() {
		System.out.println(name + "이 사료를 먹습니다.");
	}
}
```
```java
package com.ktdsuniversity.edu.interfaceexam.animals;

public class Bird implements Move, Bark, Eat, Fly {

	
	protected String name;
	
	public Bird(String name) {
		this.name = name;
	}
	
	@Override
	public void fly() {
		
		System.out.println(this.name + "이 납니다.");
	}

	@Override
	public void eat() {
		System.out.println(this.name + "이 부리로 모이를 먹습니다.");
		
	}

	@Override
	public void bark() {
		System.out.println(this.name + " : 지저깁니다.");
		
	}

	@Override
	public void walk() {
		System.out.println(this.name + " 두 다리로 걷습니다.");
		
	}

	@Override
	public void run() {
		System.out.println(this.name + " 두 다리로 뜁니다.");
		
	}

}

```
```java
package com.ktdsuniversity.edu.interfaceexam.animals;

/**
 * Bird
 * 	interface : Move, Bark, Eat, Fly
 * 	Bird is a Move
 *  Bird is a Bark
 *  Bird is a Eat
 *  Bird is a Fly
 * 
 * Chicken
 *  extends Bird
 *  Chicken is a Bird
 *  Chicken is a Move
 *  Chicken is a Bark
 *  Chicken is a Eat
 *  Chicken is a Fly
 */


public class Chicken extends Bird{

	
	public Chicken(String name) {
		super(name);
	}
	
	//Bird의 fly()함수를 다시 재정의한 것
	//Interface의 fly()함수를 다시 재정의 한것이 아니다.
	@Override
	public void fly() {
		
		System.out.println(super.name + " 3초간 버덕입니다.");
	}

}
```

```java
package com.ktdsuniversity.edu.interfaceexam.animals;

import com.ktdsuniversity.edu.interfaceexam.animals.interfaces.Human;

/**
 * Person is a Human
 * Human is a Move
 * Human is a Eat
 * 
 * Person is a Human
 * Person is a Move
 * Person is a Eat
 */
public class Person implements Human {
	
	
	private String name;
	
	public Person(String name) {
		this.name = name;
	}
	
	
	@Override
	public void walk() {
		
		System.out.println(this.name + " 두 발로 걷습니다.");
	}

	@Override
	public void run() {
		System.out.println(this.name + " 두 발로 뜁니다.");
		
	}

	@Override
	public void eat() {
		System.out.println(this.name + " 숟가락으로 밥을 먹습니다.");
		
	}

	@Override
	public void work() {
		System.out.println(this.name + " 이 일을 합니다.");
		
	}

	@Override
	public void speak() {
		
		System.out.println(this.name + " 말을 합니다");
	}

	@Override
	public void think() {
		System.out.println(this.name + " 생각을 합니다.");
		
	}

}
```

```java
package com.ktdsuniversity.edu.interfaceexam.animals;

import com.ktdsuniversity.edu.interfaceexam.animals.interfaces.Bark;
import com.ktdsuniversity.edu.interfaceexam.animals.interfaces.Eat;
import com.ktdsuniversity.edu.interfaceexam.animals.interfaces.Fly;
import com.ktdsuniversity.edu.interfaceexam.animals.interfaces.Move;

public class Main {
	
	
	public static void flying(Fly fly) {
		
		fly.fly();
		
	}
	
	public static void moving(Move move) {
		
		move.run();
		move.walk();
		
	}
	
	
	public static void eating(Eat eat) {
		
		eat.eat();
		
	}
	
	public static void barking(Bark bark) {
		
		bark.bark();
		
	}
	
	
	public static void main(String[] args) {
		
		//상속 is a -> sub class is a super class
		//인터페이스 : is a -> instance is a interface
		//Dog class -> Move, Eat, Bark
		//Dog의 타입이 Move, Eat Bark가 될 수 있다.
		
		Dog ppoppy = new Dog("뽀삐");

		ppoppy.walk();
		ppoppy.run();
		ppoppy.eat();
		ppoppy.bark();
		
		Eat eat = ppoppy;
		eat.eat();
		
		Move move = ppoppy;
		move.walk();
		move.run();
		
		Bark bark = ppoppy;
		bark.bark();
		
		
		//flying은 Dog에서 정의하지 않았으므로, 실행되지 않는다.
		//flying(ppoppy);
		Main.eating(ppoppy);
		Main.barking(ppoppy);
		Main.moving(ppoppy);

		
		Bird 비둘기 = new Bird("구구");
		
		비둘기.walk();
		비둘기.run();
		비둘기.eat();
		비둘기.bark();
		
		Main.flying(비둘기);
		Main.eating(비둘기);
		Main.barking(비둘기);
		Main.moving(비둘기);
		
		Chicken 닭 = new Chicken("꼬꼬");
		닭.walk();
		닭.run();
		닭.eat();
		닭.bark();
		
		Main.flying(닭);
		Main.eating(닭);
		Main.barking(닭);
		Main.moving(닭);
		
		Duck duck = new Duck("덕덕");
		
		duck.fly();
		
		Person kim = new Person("김");
		kim.eat();
		kim.speak();
		kim.think();
		kim.walk();
		kim.work();
		
		// flying(kim);
		// barking(kim);
		moving(kim);
		eating(kim);
		
		
	}
}
```
# 익명 클래스
* 추상클래스나 인터페이스를 강제로 인스턴스화 시키는 방법이다.
* 추상클래스나 인터페이스를 인스턴스화 할 수 없는 이유
  * 구현되지 않는 추상메소드가 있기 때문
  * 인스턴스화의 조건은 모든 메소드가 구현되어 있어야 한다.

* 추상클래스나 인터페이스를 강제로 인스턴스화 하려면 인스턴스를 만들 때 추상메소드를 구현
* 익명클래스의 사용
  * 단 한번 사용할 인스턴스를 만들 때 사용
 
* 추상클래스 또는 인터페이스를 딱 한번 사용할 때 클래스를 정의하는 것은 매우 비효율적이다.
  * 이러한 경우, 클래스를 정의하지 않고 즉시 구현하여 인스턴스를 만드는 익명클래스를 만드는 것이 유리


# 익명 클래스 실습

```java
package com.ktdsuniversity.edu.interfaceexam.practice;

public interface AddressBook {	
	public void contactCheckUp();
	public void contactDelete();
	public void contactCorrection();
	public void contactAdd();

}
```


```java
package com.ktdsuniversity.edu.interfaceexam.practice;

public class myPhone implements AddressBook {
	
	private String phoneNumber;
	
	public myPhone(String phoneNumber) {
		
		this.phoneNumber = phoneNumber;
	}

	@Override
	public void contactCheckUp() {
		System.out.println("연락처 조회 : " + this.phoneNumber);
		
	}

	@Override
	public void contactDelete() {
		System.out.println("연락처 삭제 : " + this.phoneNumber);
		
	}

	@Override
	public void contactCorrection() {
		System.out.println("연락처 수정 : " + this.phoneNumber);
		
	}

	@Override
	public void contactAdd() {
		System.out.println("연락처 추가 : " + this.phoneNumber);
		
	}
	
	

}
```


```java
package com.ktdsuniversity.edu.interfaceexam.practice;

public class myPhoneMain {

	public static void main(String[] args) {
		
		String anonyName = "금민규";
		String anonyPhoneNumber = "010-4757-0218";
		
		AddressBook addressbook = new AddressBook() {

			private String name = anonyName;
			private String phoneNumber = anonyPhoneNumber;
			@Override
			public void contactCheckUp() {
				
				System.out.println(this.name + "의 연락처 확인 : " + this.phoneNumber);
			}

			@Override
			public void contactDelete() {
				
				System.out.println(this.name + "의 연락처 삭제 : " + this.phoneNumber);
			}

			@Override
			public void contactCorrection() {
				
				System.out.println(this.name + "의 연락처 수정 :" + this.phoneNumber);
				
			}

			@Override
			public void contactAdd() {
				
				System.out.println(this.name + "의 연락처 추가 :" + this.phoneNumber);
				
			}
			
			
		};
		
		addressbook.contactCheckUp();
		addressbook.contactDelete();
		addressbook.contactDelete();
		addressbook.contactAdd();
		
		
		myPhone myphone = new myPhone("010-4757-0218");
		
		myphone.contactAdd();
		myphone.contactCheckUp();
		myphone.contactCorrection();
		myphone.contactDelete();

	}

}
```
