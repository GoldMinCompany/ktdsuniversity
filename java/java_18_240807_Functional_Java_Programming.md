# 익명 클래스
* 익명 클래스 인스턴스를 생성 후 인스턴스를 파라미터로 대입하는 것보다 익명 클래스 인스턴스를 파라미터에 대입하고 생성하는 것이 일반적인 방법이다.


# 객체 지향 프로그래밍

* 객체 변수 할당할 수 있어야 한다. 
* 객체를 파라미터로 전송 및 수신할 수 있어야한다.
* 객체가 객체를 생성할 수 있어야 한다.
* 객체가 객체를 반환 시킬 수 있어야한다.

# 함수형 프로그래밍 

* 함수를 변수에 할당할 수 있어야 한다.
* 함수를 파라미터로 전송 및 수신할 수 있어야한다.
* 함수가 함수를 생성할 수 있어야 한다.
* 함수가 함수를 반환 시킬 수 있어야한다.
* 메소드와 함수의 input 값을 통해 정해진다.
* 추상 메소드가 하나만 있는 인터페이스는 lambda로 변경 가능하다.
* @FunctionalInterface의 목적은 lambda로 변경 가능함을 명시적으로 알려주는 기능만 한다.

```java
lg.run(message -> System.out.println(message));
```
위 코드에서 parameter가 하나인 경우 소괄호 생략가능하고 반환타입이 같다면, 중괄호 생략이 가능하며 인터페이스의 추상메소드를 구현한 것이다. 인스턴스를 생성하지 않고 함수형 인터페이스를 paramemter로 전송한다. 인터페이스에 정의된 추상메소드를 함수로 구현한 것이 lambda이다.  

```java
package com.ktdsuniversity.edu.functional_anonymous;

/**
 * 버튼, 테이블(표), 이미지, 텍스트 등등..
 */
public class UI {
	
	public void click(ClickEventListener clickEventListener) {
		clickEventListener.click();
		
	}
	
	public void doubleClick(DoubleClickEventListener doubleClickEventListener) {
		doubleClickEventListener.doubleClick();
		
	}
	
	public void dragAndDrop(DragAndDropEventListener dragAndDropEventListener) {
		dragAndDropEventListener.dragAndDrop();
		
		
	}
	
	public void wheel(WheelEventListener wheelEventListener) {
		wheelEventListener.wheel();
		
		
		
	}
}
```
```java
package com.ktdsuniversity.edu.functional_anonymous;

@FunctionalInterface
public interface ClickEventListener {

	public void click();

}
```

```java
package com.ktdsuniversity.edu.functional_anonymous;

@FunctionalInterface
public interface DragAndDropEventListener {

	public void dragAndDrop();

}

```

```java
package com.ktdsuniversity.edu.functional_anonymous;

// 추상 메소드가 하나만 있는 인터페이스는 무조건 람다로 변경 가능하다.
// @FunctionalInterface의 목적은 람다로 변경 가능함을 명시적으로 알려주는 기능만 한다.
@FunctionalInterface
public interface WheelEventListener {

	public void wheel();

}
```

```java
package com.ktdsuniversity.edu.functional_anonymous;

@FunctionalInterface
public interface DoubleClickEventListener {

	public void doubleClick();


}
```

```java
package com.ktdsuniversity.edu.functional_anonymous;

public class Printer {
	
	public void run(Printable printable) {
		
		printable.print();
		printable.print("반갑습니다. 익명 클래스");
		
		
	}
	

}
```

```java
package com.ktdsuniversity.edu.functional_anonymous;

@FunctionalInterface
public interface Printable {

	public default void print() {
		System.out.println("출력합니다.");
	};
	
	public void print(String message);
}
```

```java
package com.ktdsuniversity.edu.functional.lambda;

import com.ktdsuniversity.edu.functional_anonymous.Printer;
import com.ktdsuniversity.edu.functional_anonymous.UI;

public class Main {
	
	public static void handleButton() {
		
		UI button = new UI();
		button.click(() -> {System.out.println("click");});
		button.doubleClick(() -> {System.out.println("doubleClick");});
		button.dragAndDrop(() -> {System.out.println("drageAndDrop");});
		button.wheel(() -> {System.out.println("wheel");});
		
	}
	
	public static void lgPrinter() {
		
		Printer lg = new Printer();
		lg.run((message) -> { System.out.println(message + "!!");});
		lg.run(message -> System.out.println(message + "!!!")); //
		
	}

	public static void samsungPrinter() {
		
		Printer samsung = new Printer();
		samsung.run((message) -> {System.out.println(message);});
		samsung.run(message -> System.out.println(message + "!"));
		
	}
	
	public static void main(String[] args) {
		samsungPrinter();
		lgPrinter();
		handleButton();
	}
}
```

# Stream

### Stream의 특징
* 병럴처리를 지원하는 내부 반복자이다.
* collection과 배열을 stream으로 변환하여 여러가지 형태로 처리 가능하다.
* 반복문을 사용하지 않고 collection 제어가 가능하다.
* Stream에는 중간함수와 최종함수가 있고 중간함수(예를 들어, filter)는 최종함수가 실행되기 전까지 동작하지 않는다. 최종함수가 실행되기 전까지 실행결과를 확인하고 싶다면, peek 메서드를 사용한다. 

### Stream 사용이유
* 정렬, filtering, mapping을 통해 중복되던 반복문 코드를 사용하지 않아도 된다.
* data를 제어하는 주체가 Java로 변경된다.
  * data가 많을 수록 data를 정제하는 RDB 능력의 한계가 나타난다.
  * 비정형 data는 RDB에서 정제할 수 없다.
* RDB의 역할을 Java의 Stream이 대신 처리한다.

### Stream(), filter(), map(), forEach()
* Stream() : 기존의 코드를 stream으로 변경시키는 메서드이다. 
* filter() : 주어진 조건을 만족하는 요소들로 구성된 stream을 반환한다.
* map() : stream의 각 요소에 대하여 주어진 함수를 적용하여 다른 형태의 요소(타입 변환)로 변환한다.
* forEach() : forEach는 Stream을 반환하지 않으므로, 최종함수이다. 반복 출력

### Stream 실습
```java
package com.ktdsuniversity.edu.stream.introduce;

public class Dish {
	
	    private final String name; 
	    private final boolean vegetarian; 
	    private final int calories; 
	    private final Type type; 

	    public Dish(String name, boolean vegetarian, int calories, Type type ) { 
	        this.name = name; 
	        this.vegetarian = vegetarian; 
	        this.calories = calories; 
	        this.type = type;
	    } 
	    public String getName () { 
	        return name;
	    } 
	    
	    public boolean isVegetarian() { 
	        return vegetarian;
	    } 
	    
	    public int getCalories() { 
	        return calories; 
	    }
	    
	    public Type getType() { 
	        return type;
	    } 
	    
	    @Override 
	    public String toString() { 
	        return name;
	    } 
	    
	    public enum Type { 
	        MEAT, FISH, OTHER
	    }
}
```

```java
package com.ktdsuniversity.edu.stream.introduce;

import java.util.Arrays;
import java.util.List;

public class DishList {
	
	private List<Dish> dishLish;
	
	public DishList() {
		
		this.dishLish = Arrays.asList( 
	            new Dish("pork", false, 800, Dish.Type.MEAT), 
	            new Dish("beef", false, 700, Dish.Type.MEAT), 
	            new Dish("chicken", false, 400, Dish.Type.MEAT), 
	            new Dish("french fries", true, 530, Dish.Type.OTHER), 
	            new Dish("rice", true, 350, Dish.Type.OTHER), 
	            new Dish("season fruit", true, 120, Dish.Type.OTHER), 
	            new Dish("pizza", true, 550, Dish.Type.OTHER), 
	            new Dish("prawns", false, 300, Dish.Type.FISH), 
	            new Dish("salmon", false, 450, Dish.Type.FISH));
		
		
	}
	
	public List<Dish> getDishList(){
		
		
		return this.dishLish;
		
	}

}
```

```java
package com.ktdsuniversity.edu.stream.introduce;

import java.util.List;
import java.util.stream.Stream;

public class Main {
	
	public static void lowCaloricDishes() {
		
		// 400 칼로리 미만의 음식만 걸러와서 메뉴의 이름을 출력한다. 
		DishList dishLish = new DishList();
		
		// 1. 모든 메뉴 목록을 가져온다.
		List<Dish> menuList = dishLish.getDishList();
		
		
		// 2. 메뉴 목록을 스트림으로 변경
		Stream<Dish> dishStream = menuList.stream();
		
		// 3. 메뉴스트림에서 칼로리가 400미만인 메뉴만 가지고 있는 스트림 생성
		// 반환 값이 true이면 모든 메뉴가 스트림으로 옮겨진다. 
		// dishStream과 allDishes가 다른 스트림이다. 메모리 주소가 다르다. 
		// Stream<Dish> allDishes = dishStream.filter( (Dish dish) -> true ); 
		
		// 반환값이 false이면 결과는 비어있다. 
		// Stream<Dish> emptyDishes = dishStream.filter((Dish dish) -> false);
	
		Stream<Dish> lowCaloricDishes = dishStream.filter((Dish dish) -> dish.getCalories() < 400);
		
		// 예상 결과 : rice, season fruit, prawns
		// 중간함수는 최종함수가 실행되기 전까지 절대 동작하지 않는다.
		
		//System.out.println(lowCaloricDishes);
		//스트림에 무엇이 들었는지 확인해보자.
		//디버깅을 확인하기 위한 함수
		//Stream<Dish> peekedStream = lowCaloricDishes.peek((Dish dish)->{System.out.println("중간점검 : " + dish.getName());});
		
		
		// 4. lowCaloricDishes를 Stream<String>으로 변경한다.
		Stream<String> dishNameStream = lowCaloricDishes.map( dish -> dish.getName());

		// 5. 최종 함수를 실행한다. forEach는 최종함수 stream을 반환하지 않으므로 최종함수이다. void
		// lowCaloricDishes.forEach((Dish dish) -> {System.out.println(dish.getName());});
	
		dishNameStream.forEach(dishName -> System.out.println(dishName));
	}
	
	public static void printLowCalroricDishesSimpleVersion() {
		// 400칼로리 미만의 음식만 걸러와서 메뉴의 이름을 출력
		DishList dishList = new DishList();
		List<Dish> menuList = dishList.getDishList();
	
		menuList.stream()
				.filter(dish -> dish.getCalories() < 400)
				.map(dish -> dish.getName())
				.forEach(dishName -> System.out.println(dishName));
	
	}
	
	public static void main(String[] args) {
		lowCaloricDishes();
		
		
		printLowCalroricDishesSimpleVersion();
	}

}
```
