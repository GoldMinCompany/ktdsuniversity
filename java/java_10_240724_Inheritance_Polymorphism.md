# 15. 상속과 다형성
* 상속이란, 부모클래스의 속성과 기능을 자식클래스에게 그대로 물려주는 것을 말한다.
* 클래스의 멤버변수와 메소드가 필요한 경우
  - 클래스를 직접 수정한다.
    - 연결되어 있던 코드를 모두 수정해야할 가능성이 매우 높다
    - 결국, 하나의 기능 추가 및 변경을 위해 모든 코드를 수정해야할 수 도 있다. 
  - 기존의 있는 클래스(부모 클래스)를 상속을 받아 필요한 변수나 기능만 추가한다.
    - 연결 되어있던 기존의 코드에 영향을 주지 않고 안정하게 수정사항을 반영할 수 있다.
---
상속은 확장의 개념이다. 확장의 개념을 사용하여 부모클래스의 기능과 속성을 추가하여 부모클래스를 만들 수 있다.  상속 받을 클래스를 선언할 때, extends 키워드를 사용하여 상속을 받는다. 

SOLID중 S와 O는 단일책임의 원칙(하나의 기능이 바뀌면 다른 기능에 문제가 생김), 개방폐쇄원칙(확장은 열려 있고, 수정은 닫혀있음)
자식클래스가 부모클래스를 상속 받을 때, 오류가 발생한다. 이것은 부모클래스의 생성자가 존재하는 경우, 자식클래스에서 반드시 super 키워드를 사용하여 부모클래스의 생성자를 호출해야한다. 

자바는 기본적으로 Object라는 클래스를 상속받고 있다. 새 프로젝트를 생성할 때, superclass에 Object Class가 설정되어 있다.

접근제한지시자 private 상속받은 클래스에서 사용이 불가능하다. method Overloading은 parameter의 갯수나 type에 따라 메소드가 달라지는 것을 의미한다. method Overloading는 상위 클래스, 하위 클래스에 따라서 메소드가 달라지는 것을 일컫는다. 

* 상속 받은 클래스 내에서 같은 이름의 메소드를 생성 시, 왼쪽의 번호줄 옆에 화살표가 생긴다. 이것은 메소드가 Override된 것을 의미한다.

Override 할 때, 성능 저하가 발생한다. 이것을 JVM에게 알려주어야 한다. Override method 바로 위에 @Override를 입력한다. 또한, 자바는 다중 상속이 되지 않는다. 

### * 상속의 유연성
* 상위의 클래스(부모 클래스)에서 속성과 기능을 추가시키는 경우, 상속받은 클래스에서 추가된 속성과 기능을 사용할 수 있다.

### * 상속을 사용하는 이유

1. 캡슐화 : 여러 기능을 메소드(캡슐)로 묶어 처리
2. 추상화 : 기능만 정의하고 구현 하지 않음
3. 다형성 : 상속 및 구현 대상(부모 클래스) type에 포함되는 것을 허가, 하나의 타입으로 여러가지 표현을 생성할 수 있다. 
4. 상속 : 부모클래스의 속성과 기능을 추가한다.

### * is a 관계를 이용한 다형성

자식 클래스에서 부모 클래스에 정의된 속성과 기능을 사용할 수 있지만, 부모 클래스에서 자식 클래스에서 정의된 메소드를 사용할 수 없다. instanceof 키워드를 사용하여 해당 인스턴스가 자식 인스턴스인지 확인 후 명시적 형변환을 통해 코드를 수정한다. 


### * 상속 실습

### 1. ContactMain
```java
package com.ktdsuniversity.edu.extendsexam;

public class ContactMain {
	public static void main(String[] args) {
		
		Contact[] contactArray = new Contact[5];
		contactArray[0] = new Contact("민규", "010-4757-0218");
		contactArray[1] = new Contact("미연", "010-3193-8727");
		contactArray[2] = new Contact("용식", "010-4944-5119");
		contactArray[3] = new Contact("지영", "010-2360-0960");
		contactArray[4] = new EmailContact("김", "010-2360-0960", "kim@korea.com");
		
		for(int i=0; i<contactArray.length; i++) {
			
			contactArray[i].printContact();
			contactArray[i].phoneCall();
			
			if(contactArray[i] instanceof EmailContact) {
				//EmailContact is a Contact
				//Contact is not a EmailContact
				//is a 관계가 역전 되어 있는 경우, 명시적 형변환을 사용한다.
				EmailContact emailContactInstance = (EmailContact) contactArray[i];
				emailContactInstance.sendEmail();
			}
			
		}
		
		
		
	}

}
```

### 2. Contact
```java
package com.ktdsuniversity.edu.extendsexam;

public class Contact {
	
	private String name;
	private String phone;
	
	/**
	 * 사용자의 요청으로 주소 추가
	 */
	private String address;
	
	public Contact(String name, String phone) {
		
		this.name = name;
		this.phone = phone;
	}
	
	public String getName() {
		return this.name;
	}
	
	public String getPhone() {
		return this.phone;
	}
	
	public void printContact() {
		
		System.out.println("이름 :" + this.name + ", 연락처 : " + this.phone + ", 주소 : " + this.address);
	}
	
	public void phoneCall() {
		
		System.out.println("이름 : " + this.name + "에게 전화를 겁니다");
	}
	
}

```
### 3. EmailContact
```java
package com.ktdsuniversity.edu.extendsexam;


/**
 * 이름, 전화번호를 가지고 있는 연락처(Contact)클래스를 확장한 클래스
 * 확장해서 이메일만 추가
 * 
 * 확장의 대상이 되는 클래스(Contact)의 생성자가 존재할 경우
 * 확장을 하는 클래스(EmailContact)에서 반드시 해당 생성자를 호출해야 한다. 
 * 
 * sub class is a Super class
 * EmailContact is a Super class
 * 
 */
public class EmailContact extends Contact{

	/**
	 * 사용자의 요청으로 이메일 추가
	 */
	private String email;
	
	public EmailContact(String name, String phone, String email) {
		// Contact 클래스의 기본 생성자를 호출하는 방법 : super();
		super(name, phone); // public Contact(String name, String phone) {...}호출
		
		this.email = email;
		
	}
	public String getEmail() {
		return this.email;
	}
	
	@Override
	public void printContact() {
		
		super.printContact();
		System.out.println("이메일 : " + this.email);
		
	}
	
	public void sendEmail() {
		
		System.out.println("이름 : " + super.getName() + "에게 이메일을 보냅니다.");
		
	}
	
}

```
---
### 1. Vehicle
```java
package com.ktdsuniversity.edu.extendsexam.practice;

public class Vehicle {
	/**
	 * 자동차 모델명
	 */
	private String name;
	
	
	/**
	 * Vehicle 생성자
	 */
	public Vehicle(String name) {		
		this.name = name;
	}
	
	/**
	 * 시동걸기
	 */
	
	public void start() {
		
		System.out.println("차량이 출발합니다.");
		
	}
	
	public void printVehicle() {
		
		System.out.println("모델명 : " + this.name);
	}
	
}
```

### 2. EV
```java
package com.ktdsuniversity.edu.extendsexam.practice;

public class EV extends Vehicle {

	private int battery;
	
	public EV(String name, int battery) {
		super(name);
		this.battery = battery;
		
	}
	
	public void checkBattery() {
		
		if(this.battery > 0) {
			System.out.println("배터리 잔량 : " + this.battery);
		}
		else {
			
			this.battery = 0;
			System.out.println("배터리 잔량 : " + this.battery);
			
		}
		
	}
	
	public void printEV() {
		
		super.printVehicle();
		System.out.println("배터리 잔량 : " + this.battery);
	}
	
}
```
### 3. SportsCar
```java
package com.ktdsuniversity.edu.extendsexam.practice;

public class SportsCar extends Vehicle {
	
	public SportsCar(String name) {
		
		super(name);
		
	}
	
	public void turboMode() {
		
		System.out.println("터보모드");
		
	}
	
	public void printSportsCar() {
		
		super.printVehicle();
	}
}
```
### 4. BatMobile
```java
package com.ktdsuniversity.edu.extendsexam.practice;

public class BatMobile extends SportsCar{

	public BatMobile(String name) {
		super(name);
	}
	
	public void batPort() {
		
		System.out.println("배트포트 분리");
	}
	
	public void printBatPort() {
		
		super.printVehicle();
		
	}
}
```

### 5. CarMain
```java
package com.ktdsuniversity.edu.extendsexam.practice;

public class CarMain {
	
	public static void main(String[] args) {
		
		Vehicle vehicle = new Vehicle("현대자동차");
		vehicle.start();
		vehicle.printVehicle();
		
		
		EV ev = new EV("테슬라", 100);
		ev.checkBattery();
		ev.start();;
		
		
		SportsCar sc = new SportsCar("페라리");
		sc.start();
		sc.turboMode();
		
		
		BatMobile bm = new BatMobile("배트모빌");
		bm.start();
		bm.batPort();
		bm.turboMode();
		
		
	
	}
}
```

