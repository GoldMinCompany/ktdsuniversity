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

추상 메소드가 하나만 있는 인터페이스는 무조건 람다로 변경 가능하다.

@FunctionalInterface의 목적은 람다로 변경 가능함을 명시적으로 알려주는 기능만 한다.

lg.run(message -> System.out.println(message + "!!!")); //파라미터가 한개인 경우 소괄호 생략가능, 반환타입이 같다면 중괄호 생략가능, 인터페이스의 추상메소드를 구현

인스턴스 대신 함수형 인터페이스를 파라미터로 전송한다.
인터페이스를 함수로! 바꾸는 것이 lambda

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
