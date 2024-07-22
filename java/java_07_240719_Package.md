# 10. 패키지, Import
* Java 파일 중 관련된 파일들만 정리하여 모아둔 폴더
* 한 패키지 내에서 같은 이름의 java 파일은 만들 수 없다.
* 같은 이름의 java파일은 서로 다른 패키지에 만들 수 있다.

* 패키지 이름 정하는 방법
  - 패키지 명은 application을 개발하는 회사의 domain 역순으로 작성
  - 만약 naver.com 에서 package_exam application을 개발한다면, com.naver가 기본 패키지 명이 된다.
  - 기본 패키지명이 작성된 후 업무명을 "."을 붙여 작성한다.
  - 만약, 업무명이 "회원관리" 라면 업무명은 "member.management" 혹은 "management.member"로 작성한다.
---
# 접근제어 지시자
* 패키지와 별개의 문제로 클래스, 멤버변수, 메소드 등을 외부 클래스나 인스턴스가 접근을 제한하는 기능이 있다.

  ![image](https://github.com/user-attachments/assets/ec268ba5-7437-4805-b20e-32f6001f784a)

---
# 정보은닉과 캡슐화
* 인스턴스의 정보를 표현하는 멤버변수는 데이터의 보호를 위해 외부로부터 접근이 제한된다.
* 멤버변수의 접근제한자는 private으로 선언하고 생성자를 통해 멤버변수의 값을 접근할 수 있다.

```java
public class memeber(){
	private String id;
	private String name;
	public Memeber(String id, String name){
		this.id = id;
		this.name = name
	}

}
```
* Memeber 클래스의 인스턴스 member는 멤버변수에 직접 접근할 수 없도록 되어 있어, 내부의 값이 무엇인지 확인 불가능하다.
* Article 클래스에서 인스턴스 member의 id 값을 확인할 수 없다.
* 이러한 경우, 인스턴스 member 값을 반환하는 별도의 인스턴스 메소드가 필요한다. 이것을 Getter()라 한다.

```java
public class memeber(){
	private String id;
	private String name;
	public Memeber(String id, String name){
		this.id = id;
		this.name = name
	}
	public String getId(){
		return this.id;
	}

	public String getName(){
		return this.name;
	}

}
```

```java
public class Article() {
	
	public static void main(String[] args) {
		
		Member member = new Member("ID", "관리자");
		String id = member.getId();
		String name = member.getName();
		
		System.out.println(id);
		System.out.println(name);
		
		
	}
	
}
```
---
# 클래스 변수
* 인스턴스가 아닌 클래스로 접근할 수 있는 변수를 의미한다.
* 클래스 변수는 "클래스로 변수에 접근이 가능하다" 라는 점에서 application 내부의 모든 Java 파일에서 공유되는 "공통 변수"이다.
* 모든 Java 파일이 공유하는 변수이므로, 클래스 변수의 값이 변경되어 있을 때, 원치 않는 결과가 생성될 수 있다.
* 따라서, 클래스 변수의 값이 변경되지 않도록 static 뒤 final keyword 선언하여 클래스 상수로 사용한다. 

---

# 클래스 메소드
* 클래스 변수와 마찬가지로 클래스 메소도는 인스턴스가 아닌 클래스로 접근할 수 있는 메소드를 의미한다.
* 클래스로 접근하기 때문에, 멤버변수의 영향을 받을 수 없고 parameter의 영향을 받는다.
  - 클래스 메소드는 인스턴스로 접근할 수 없다.
  ![image](https://github.com/user-attachments/assets/1378d801-e8e4-4ce2-ad0a-4acdc9a56026)

---
# 메소드 오버로딩
* Java에서 작성하는 변수나 메소드(클래스, 인스턴스 생성자)는 유일한 이름으로 구분
* 변수 중복선언, 메소드 중복선언 할 수 없다
* 단, 메소드는 메소드 오버로딩 기법을 통해 동일한 이름의 메소드를 정의하고 사용할 수 있다.
* 메소드 오버로딩의 기준
  - 동일한 메소드명
  - 파라미터의 갯수가 다르거나 파라미터의 type이 다르다.

# 생성자 오버로딩
* 생성자 또한, 메서드와 같이 오버로딩이 가능하다.
* 클래스 내에서 생성자를 정의하는 경우, 기본 생성자는 자동으로 만들지 않는다. 

