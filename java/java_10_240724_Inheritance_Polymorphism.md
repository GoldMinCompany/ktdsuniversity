# 15. 상속과 다형성

### * 상속 실습

### 1. ContactMain
```java
package com.ktdsuniversity.edu.extendsexam;

public class ContactMain {
	public static void main(String[] args) {
		
		Contact[] contactArray = new Contact[4];
		contactArray[0] = new Contact("민규", "010-4757-0218");
		contactArray[1] = new Contact("미연", "010-3193-8727");
		contactArray[2] = new Contact("용식", "010-4944-5119");
		contactArray[3] = new Contact("지영", "010-2360-0960");
		
		
		for(int i=0; i<contactArray.length; i++) {
			
			contactArray[i].printContact();
			
		}
		
		
		
		EmailContact kim = new EmailContact("김","010-1234-9784","kim@naver.com");
		kim.printContact();
		
		
		
		
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

