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
	
	public void fly();

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
	
		
		
	}
}
```
