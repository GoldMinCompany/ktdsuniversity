```java

public static final int PRICE;

//클래스 상수 초기화 영역
static {
  PRICE = 1000;
}
```

# 13. String(Array)

* contains
* endsWith : 마지막 문자열로 시작하는가?
* equals
* equalsIgnoreCase
* indexOf
* lastIndexOf : 파일의 확장자 또는 파일의 이름을 알고 싶을 때 사용
* length : 문자열의 길이
* matches : "문자가 어떤 형태를 가지고 있는가?"
* replace : Ctrl + H의 기능 문자열 변수에 바꾸고자 하는 단어를 찾아 변환
* replaceAll
* startsWith : 첫번째 문자열로 시작하는가?
* subString : 문자열 잘라내기, indexOf와 lastIndexOf와 같이 쓰인다.
* trim
* split : String[] 반환
* toLower : 소문자로 변환
* toUpper : 대문자로 변환
* toString : String 반환

### * String 실습
```java
public class StringExam {
	
	/**
	 * 파라미터에서 파일의 이름만 추출해서 반환
	 * @param filePath
	 */
	public static String extractFileName(String filePath) {
		
		return filePath.substring(filePath.lastIndexOf("\\") + 1);
		
		
	}
	
	public static void printSubStringExtractFileNameResult() {
		
		String filePath = "C:\\dev_program\\images\\pen.png";
		String fileName = extractFileName(filePath);
		System.out.println(fileName);
		
		
		filePath = "C:\\dev_program\\uploadedImage\\mouse.png";
		fileName = extractFileName(filePath);
		System.out.println(fileName);
		
		filePath = "C:\\programs\\mouse.png";
		fileName = extractFileName(filePath);
		System.out.println(fileName);
	}
	
	public static void printSubStringDateTimeResult() {
		
		String dateTime = "2023-05-02 14:56:13";
		//dateTime에서 연도만 추출
		
		String year = dateTime.substring(0, 4);
		System.out.println(year);
		//dateTime에서 월만 추출
		String month = dateTime.substring(5, 7);
		System.out.println(month);
		
		
		//dataTime에서 일만 추출
		
		//dataTime에서 시만 추출
		
		//dataTime에서 분만 추출
		
		//dataTime에서 초만 추출
		String seconds = dateTime.substring(17);
		System.out.println(seconds);
		
		
	}
	
	public static void printEndsWithResult() {
		
		String fileName = "pencil.gif";
		
		//fileName이 .gif로 끝나는가?
		
		boolean isGifFile = fileName.endsWith(".gif");
		System.out.println(isGifFile);
		
		isGifFile = fileName.endsWith(".GIF");
		System.out.println(isGifFile);
		
		
		isGifFile = fileName.endsWith(".gif") || fileName.endsWith(".GIF");
		
		System.out.println(isGifFile);
		
		
		// fileName의 값을 대(소)문자로 변경 후.
		// .GIF 혹은 .gif로 검사
		
		isGifFile = fileName.toLowerCase().endsWith(".gif");
		System.out.println(isGifFile);
		
	}
	
	
	public static void printSplitResult() {
		
		String phoneNumber = "+82-10-1234-5678";
		
		/**
		 * +82 : 국가 번호
		 * 10
		 * 1234 : 개통시 기지국 번호
		 * 5678 : 개인번호
		 */
		
		String[] splittedPhoneNumber = phoneNumber.split("-");
		System.out.println(splittedPhoneNumber[0]);
		System.out.println(splittedPhoneNumber[1]);
		System.out.println(splittedPhoneNumber[2]);
		System.out.println(splittedPhoneNumber[3]);
		
	}
	
	
	
	public static void printReplaceAllResult() {
		
		String phoneNumber = "010-1234-1234";
		
		
		// phoneNumber에서 숫자가 아닌 모든 것을 제거
		// 01012341234
		/*
		 * [^0-9] : 숫자가 아닌 글자 
		 * ^ 이 그룹 지정나 내부에 있을 때 !(not)의 역할을 한다.  
		 */
		String dirtyPhoneNumber = phoneNumber.replaceAll("[^0-9]", "");
		System.out.println(dirtyPhoneNumber);
		
	}
	
	
	public static void printReplaceResult() {
		
		String title = "<script>alert('alert! 내가 해킹할꺼야')</script>";
		
		// <, > 를 title에서 제거 (Sanitazing
		String dirtyTitle = title.replace("<", "&");
		dirtyTitle = dirtyTitle.replace(">", "");
		
		System.out.println(dirtyTitle);
		
		
	}
	
	
	
	public static void printIsAlphabetFormatResult() {
		
		String cardName = "Keum Min Kyu";
		
		//영문자 영대문자로만 구성이 되어있는가?
		boolean isAlphabet = cardName.matches("^[A-Za-z0-9 ]+$");
		System.out.println(isAlphabet);
	}
	
	public static void printIsHangulFormatResult() {
		
		String age = "마흔";
		// ㄱ ~ ㅎ
		// ㅏ ~ ㅣ
		// ㄱ ~ ㅎ
		// 가힣
		boolean isHangul = age.matches("^[가-힣]+$"); //한글로 시작해서 한글로만 끝난다. 
		System.out.println(isHangul);
		
		if(isHangul) {
			
			System.out.println("한글 나이는 "+ age +"입니다.");
		}
		else {
			
			System.out.println(age + "는 한글 형태가 아닙니다.");
		}
		
		
	}
	
	
	
	public static void printIsNumberFormatResult() {
		
		String age = "마흔";
		// age에 할당된 값이 숫자로만 구성되어 있는지 검증
		
		/*
		 * ^[0-9]+$ : 숫자로 시작해서 숫자로 끝나는 형태, 문자가 숫자로만 이루어져 있는가? 
		 * ^(캐럿) : ~으로 시작한다. 
		 * [ ] : Group 지정자. 내부의 지정된 문자 중 한 글자.
		 * [0-9] : 숫자 그룹 지정. 0부터 9까지의 모든 숫자 중 한 글자.
		 * + : 앞에 지정된 문자 혹은 그룹이 한 글자 이상 반복된다. 
		 * [0-9] + : 숫자가 하나 이상 반복된다.
		 * $ : ~으로 끝난다.
		 */
		boolean isNumber = age.matches("^[0-9]+$"); 
		System.out.println(isNumber);
		
		if(isNumber) {
			
			// age 문자열을 숫자로 변경
			// int를 클래스로 만들어 준 것이 Integer 
			int numAge = Integer.parseInt(age);
			System.out.println(numAge);
			System.out.println(numAge * 2);
			
			
		}
		else {
			
			
			System.out.println(age + "숫자형태가 아닙니다.");
		}
		
		
		
	}
	
	public static void printLastIndexResult() {
		
		/*
		 * \(Back slash) : Escape Code
		 * "안녕하세요, 제 이름은 "금민규"입니다." 
		 * String hello = "안녕하세요, 제 이름은 \"금민규\"입니다.";
		 * \\ : 원(통화기호) 표시
		 * \" : 상 따옴표
		 * \' : 홑 따옴표
		 * \r : 개행 후 커서를 가장 앞으로
		 * \n : 줄 개행
		 * \t
		 * 
		 */
		
		String filePath = "C:\\dev_program\\images\\pen.png";
		// filePath에서 pen.png가 시작되는 위치를 알고싶다. 
		
		int lastIndexOfBackSlash = filePath.lastIndexOf("\\");
		System.out.println(lastIndexOfBackSlash);
	}
	
	public static void printIsNullOrEmptyResult() {
	
		/*
		 * String instance.trim()
		 * Exam>
		 * 		String abc = "  a b c  ";
		 * 		abc.length; 
		 * 		abc.trim(); --> "a b c"
		 * 		abc.trim().length(); --> 5
		 * 		
		 * 		String abc = "     ";
		 * 		abc.length(); --> 5
		 * 		abc.trim(); --> ""
		 * 		abc.trim().length(); --> 0
		 * 
		 */
		
		
		String str1 = "";
		boolean isEmpty = str1 == null || str1.trim().length() == 0;
		System.out.println(isEmpty); 
		
		String str2 = "   ";
		isEmpty = str2 == null || str2.trim().length() == 0;
		System.out.println(isEmpty);
		
		
		String str3 = "   aa aaa   ";
		isEmpty = str3 == null || str3.trim().length() == 0;
		System.out.println(isEmpty);
		
		String str4 = null;
		isEmpty = str4 == null || str4.trim().length() == 0;
		System.out.println(isEmpty);
		
		
		
		
		
	}
	
	public static void printIsEmptyResult() {
		
		String str = "    ";
		boolean isEmpty = str.isEmpty(); // 길이가 0일 때만 true
		System.out.println(isEmpty);
		
		String str2 = "";//
		isEmpty = str2.isEmpty();
		System.out.println(isEmpty);
		
		String str3 = "a";
		isEmpty = str3.isEmpty();
		System.out.println(isEmpty);
		
	}
	
	public static void printIsBlankResultOnJava11() {
		
		String str = "    ";
		boolean isBlank = str.isBlank();
		System.out.println(isBlank);
		
		String str2 = "";// Blank String 문자열이 없는 String
		isBlank = str2.isBlank();
		System.out.println(isBlank);
		
		String str3 = "a";
		isBlank = str3.isBlank();
		System.out.println(isBlank);
	}
	
	
	
	public static void printIndexOfResult() {
		
		String fileName = "score.xlsx";
		int dotIndex = fileName.indexOf('.');
		System.out.println(dotIndex);
		
		int xIndex = fileName.indexOf("x");
		System.out.println(xIndex);
		
		int bigXIndex = fileName.indexOf("X");
		System.out.println(bigXIndex); 
		
		
		int extentionNameIndex = fileName.indexOf(".xlsx");
		System.out.println(extentionNameIndex);
	}
	
	
	
	public static void printFormatResult() {
		/*
		 * %s : String 할당
		 * %d : Decimal 할당
		 * %f : floating point number 할당, 부동소수점
		 * %.2f : 부동소수점들을 소수점 이하 두 자리만 표현
		 * %.4f : 부동소수점들을 소수점 이하 네 자리만 표현
		 */
		
		String format = "%s에서 교육하는 %s %d 교육입니다.";
		String formattedString = String.format(format, "ktdsuniversity", "java", 22);
		System.out.println(formattedString);
		
	}

	public static void printStartsWithResult() {
		
		String address = "https://edu.ktdsuniversity.com";
		boolean isSecureProtocol = address.startsWith("https://");
		System.out.println(isSecureProtocol);
		
		boolean isNonSecureProtocol = address.startsWith("http://");
		System.out.println(isNonSecureProtocol);
		
	}
	
	public static void printContainsResult() {
		
		String address = "서울특별시 서초구 효령로 176";
		boolean isSeocho = address.contains("서초");
		System.out.println(isSeocho);
		
		boolean isGangnam = address.contains("강남");
		System.out.println(isGangnam);
	}
	
	
	public static void main(String[] args) {
		
		StringExam.printContainsResult();
		StringExam.printStartsWithResult();
		StringExam.printFormatResult();
		StringExam.printIndexOfResult();
		StringExam.printIsBlankResultOnJava11();
		StringExam.printIsEmptyResult();
		StringExam.printIsNullOrEmptyResult();
		StringExam.printLastIndexResult();
		StringExam.printIsNumberFormatResult();
		StringExam.printIsHangulFormatResult();
		StringExam.printIsAlphabetFormatResult();
		StringExam.printReplaceResult();
		StringExam.printReplaceAllResult();
		StringExam.printSplitResult();
		StringExam.printEndsWithResult();
		StringExam.printSubStringDateTimeResult();
		StringExam.printSubStringExtractFileNameResult();
	}

}
```
