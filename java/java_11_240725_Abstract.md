# 추상화

* 추상
  - 여러가지 사물이나 개념에서 공통되는 특성이나 속성 따위를 추출하여 파악하는 작용
  - 사물을 어떤 성질, 공통성, 본질에 착안하여 그것을 추출하여 파악하는 것이다.
  - 그 때, 다른 성질을 배제하는 작용인 사상을 수반하므로 추상과 사상은 동일 적용의 두 측면을 형성한다. 

* 추상 클래스
  - 어떤 기능의 공통된 부분은 모두 구현(추상)
  - 다른 부분들만 구현하지 않는(사상) 메소드가 있는 클래스# 추상화
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
