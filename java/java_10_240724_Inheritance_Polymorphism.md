# 15. 상속과 다형성

### * 상속 실습

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

