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
