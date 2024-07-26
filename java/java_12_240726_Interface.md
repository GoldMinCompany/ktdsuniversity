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
