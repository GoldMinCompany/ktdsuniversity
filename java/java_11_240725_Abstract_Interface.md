# 추상화

* 추상
  - 여러가지 사물이나 개념에서 공통되는 특성이나 속성 따위를 추출하여 파악하는 작용
  - 사물을 어떤 성질, 공통성, 본질에 착안하여 그것을 추출하여 파악하는 것이다.
  - 그 때, 다른 성질을 배제하는 작용인 사상을 수반하므로 추상과 사상은 동일 적용의 두 측면을 형성한다. 

* 추상 클래스
  - 어떤 기능의 공통된 부분은 모두 구현(추상)
  - 다른 부분들만 구현하지 않는(사상) 메소드가 있는 클래스 추상화

추상 클래스는 어떤 기능의 공통된 부분을 모두 구현(추상)하고 다른 부분들만 구현하지 않는(사상)메소드가 있는 클래스이다.

구현하지 않는 메소드(Abstract Method), 구현하지 않는 메소드가 있는 클래스(Abstract Class)

public abstract boolean compareType(); 으로 기능이 구현되지 않는 추상메소드를 정의하고 public abstract class AbstractFileTypeChecker으로 추상 클래스로 선언 후 이 추상클래스를 상속시키는 경우, 추상 클래스에 있는 멤버변수를 사용할 수 있고 상속받은 클래스에서 추상메소드를 정의하여 사용한다.

추상 클래스는 추상메소드가 구체적으로 정의되지 않고 선언만 되어 있으므로, 인스턴스를 만들 수 없고 상속을 통해서만 인스턴스를 생성할 수 있다. 공통된 부분은 추상 클래스에 정의하고 공통되지 않는 부분은 사상 메소드로 정의한다. 

---
```java
package com.ktdsuniversity.edu.abstractclass;

public class Upload {

	public static void main(String[] args) {
		
		//추상 클래스는 인스턴스로 만들 수 없다. 반드시 상속을 통해서만 인스턴스를 만들어질 수 있다.
		//AbstractFileTypeChecker testChecker = new AbstractFileTypeChecker("pic.png");
		
		//1. 확장자만 체크하기
		AbstractFileTypeChecker checkFileType1 = new CheckFileType("pic.jpg");
		AbstractFileTypeChecker checkFileType2 = new CheckFileType("pic.xlsx");
		
		boolean isImageFile1 = checkFileType1.isImageFile();
		boolean isImageFile2 = checkFileType2.isImageFile();
		
		System.out.println(isImageFile1);
		System.out.println(isImageFile2);
		
		//2. 마임타임만 체크하기
		
		AbstractFileTypeChecker checkMimeTypeFile1 = new CheckFileMimeType("pic.zip");
		AbstractFileTypeChecker checkMimeTypeFile2 = new CheckFileMimeType("pic.exe");
		
		boolean isImageFile3 = checkMimeTypeFile1.isImageFile();
		boolean isImageFile4 = checkMimeTypeFile2.isImageFile();
		
		System.out.println(isImageFile3);
		System.out.println(isImageFile4);
		
		//3. 확장자와 마임타임 모두 체크하기
		
		AbstractFileTypeChecker checkFileTypeAndMimeType1 = new CheckFileTypeAndMimeType("pic.jpg");
		AbstractFileTypeChecker checkFileTypeAndMimeType2 = new CheckFileTypeAndMimeType("pic.mp3");
		
		boolean isImageFile5 = checkFileTypeAndMimeType1.isImageFile();
		boolean isImageFile6 = checkFileTypeAndMimeType2.isImageFile();
		
		System.out.println(isImageFile5);
		System.out.println(isImageFile6);

	}

}
```


```java
package com.ktdsuniversity.edu.abstractclass;

public abstract class AbstractFileTypeChecker {
	
	protected String fileName; // 상속 클래스, 본인 클래스에서 사용 가능
 	
	public AbstractFileTypeChecker(String fileName) {
		
		this.fileName = fileName;
	
	}
	
	public boolean isImageFile() {
		
		return compareType();
		
	}
	
	public abstract boolean compareType();
}

```


```java
package com.ktdsuniversity.edu.abstractclass;

/**
 * 파일의 확장자를 확인하는 기능.
 */
public class CheckFileType extends AbstractFileTypeChecker {
	
	/**
	 * 확장자를 가지고 있는 파일의 이름
	 * 확장자를 체크 하려는 파일의 정보
	 */
	
	public CheckFileType(String fileName) {
		super(fileName);
	
	
	}
	
	@Override
	public boolean compareType() {
		
		return this.fileName.toLowerCase().endsWith(".jpg");
	}
}

```


```java
package com.ktdsuniversity.edu.abstractclass;

import java.util.Random;

public class CheckFileMimeType extends AbstractFileTypeChecker {
	
	
	
	public CheckFileMimeType(String fileName) {
		super(fileName);
	}
	
	private String getMimeTypeOfFile() {
		
		String[] mimeTypes = {"image/jpeg","image/gif","audio/mp3", "video/avi"};
		Random random = new Random();
		
		return mimeTypes[random.nextInt(mimeTypes.length)];
		
		
	}
	/**
	 * 추상 클래스에 포함되어 있는 추상 메소드
	 * 서브 클래스에서 반드시 Override 한다. 
	 */
	@Override
	public boolean compareType() {
		
		String mimeTypeOfFile = getMimeTypeOfFile();
		
		boolean isImageFile = mimeTypeOfFile.equals("image/jpeg") || mimeTypeOfFile.equals("image/gif");
		
		System.out.println(fileName + "의 mimeTypes은 " + mimeTypeOfFile + "입니다.");
		
		return isImageFile;
	
	}
}

```


```java
package com.ktdsuniversity.edu.abstractclass;

import java.util.Random;

public class CheckFileTypeAndMimeType extends AbstractFileTypeChecker{
	

	public CheckFileTypeAndMimeType(String fileName) {
		super(fileName);
	}
	
	@Override
	public boolean compareType() {
		return isJPEGFile() && isImageMimeType();

	}
	
	private boolean isJPEGFile() {
		
		return this.fileName.toLowerCase().endsWith(".jpg");
		
	}
	
	
	private boolean isImageMimeType() {
		
		String mimeTypeOfFile = getMimeTypeOfFile();
		
		boolean isImageFile = mimeTypeOfFile.equals("image/jpeg") || mimeTypeOfFile.equals("image/gif");
		
		System.out.println(fileName + "의 mimeTypes은 " + mimeTypeOfFile + "입니다.");
		
		return isImageFile;
		
	}
	
	private String getMimeTypeOfFile() {
		
		String[] mimeTypes = {"image/jpeg","image/gif","audio/mp3", "video/avi"};
		Random random = new Random();
		
		return mimeTypes[random.nextInt(mimeTypes.length)];
		
		
	}
}

```
# 인터페이스
* 서로 다른 두 시스템, 장치, 소프트웨어 따위를 서로 이어 주는 부분 또는 그런 접속 장치
* 사용자인 인간과 컴퓨터를 연결하여 주는 장치, 키보드나 디스플레이 따위를 이른다.
* UI(User Interface) 사람과 컴퓨터 시스템 프로그램 간 상호작용을 의미한다.
* 서로 다른 주체 사이의 통신을 위한 규악
* Java Interface : 클래스와 클래스가 상호작용할 수 있도록 표준을 제공
* 완전 추상화 : 메소드 정의만 가능하고 구체적인 기능은 정의하지 않는다.

# 인터페이스 실습

```java
package com.ktdsuniversity.edu.interfaceexam;


/**
 * 인터페이스의 특징
 * 
 * - 메소드만 정의 - 추상 메소드
 * 		- 어떻게 동작하는지는 관심이 없다
 * 		- 인터페이스가 정해놓은 기능만 구현한다.
 * 		- 인터페이스는 추상메소드만 정의한다.  
 * 		- abstract는 기본적으로 생략 가능하다.
 */

public interface Calculator {

	
	
	public int add(int a, int b);
	
	public int sub(int a, int b);
	
	public double div(int a, int b);
	
	public  int mul(int a, int b);

}

```


```java
package com.ktdsuniversity.edu.interfaceexam;

public class SamsungCalc implements Calculator{

	@Override
	public int add(int a, int b) {
	
		return a + b;
	}

	@Override
	public int sub(int a, int b) {
	
		return a - b;
	}

	@Override
	public double div(int a, int b) {

		return a / b;
	}

	@Override
	public int mul(int a, int b) {

		return a * b;
	}

}

```


```java
package com.ktdsuniversity.edu.interfaceexam;

public class LgCalc implements Calculator{

	@Override
	public int add(int a, int b) {
		
		return a + b;
	}

	@Override
	public int sub(int a, int b) {
		
		if(a > b) {
			return a - b;
		}
		
		return b - a;
	}

	@Override
	public double div(int a, int b) {
		
		if(a <= 0 || b <= 0)
		{
			return 0;
		}
		
		return (double) a / b;
	}

	@Override
	public int mul(int a, int b) {
		
		if(a <= 0 || b <= 0)
		{
			return 0;
		}
		
		
		return a * b;
	}

}

```


```java
package com.ktdsuniversity.edu.interfaceexam;

public class AppleCalc implements Calculator {

	@Override
	public int add(int a, int b) {

		return a + b;
	}

	@Override
	public int sub(int a, int b) {
		
		return a - b;
	}

	@Override
	public double div(int a, int b) {
	
		double result = (double) a / b;
		int intResult = (int) (result * 100);
		return intResult / 100.0;
	
	}

	@Override
	public int mul(int a, int b) {
		
		return a * b;
	}

}

```


```java
package com.ktdsuniversity.edu.interfaceexam;

public class Test {

	public static void main(String[] args) {
		
		Calculator ssCalc = new SamsungCalc();
		Calculator lgCalc = new LgCalc();
		Calculator appleCalc = new AppleCalc();
		
		
		int addResult = ssCalc.add(10, 20);
		int subResult = ssCalc.sub(10, 5);
		int mulResult = ssCalc.mul(6, 7);
		double divResult = ssCalc.div(10, 3);
		
		
		System.out.println("SamsungCalc : " + addResult);
		System.out.println("SamsungCalc : " + subResult);
		System.out.println("SamsungCalc : " + mulResult);
		System.out.println("SamsungCalc : " + divResult);
		
		addResult = lgCalc.add(10, 20);
		subResult = lgCalc.sub(10, 20);
		mulResult = lgCalc.mul(10, 20);
		divResult = lgCalc.div(10, 20);
		
		System.out.println("LgCalc : " + addResult);
		System.out.println("LgCalc : " + subResult);
		System.out.println("LgCalc : " + mulResult);
		System.out.println("LgCalc : " + divResult);
		
		addResult = appleCalc.add(10, 20);
		subResult = appleCalc.sub(10, 20);
		mulResult = appleCalc.mul(10, 20);
		divResult = appleCalc.div(10, 20);
		
		System.out.println("AppleCalc : " + addResult);
		System.out.println("AppleCalc : " + subResult);
		System.out.println("AppleCalc : " + mulResult);
		System.out.println("AppleCalc : " + divResult);
	}

}

```
