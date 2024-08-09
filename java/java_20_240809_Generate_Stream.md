# Generate Stream

```java
package com.ktdsuniversity.edu.stream.generate.file;

import java.io.File;
import java.io.IOException;
import java.nio.file.Files;
import java.util.Arrays;
import java.util.stream.Stream;

public class Main {
	
	
	public static void wordCounter() {
		
		// java_exam 폴덜의 data.txt 파일을 읽어서
		// 파일에 작성되어 있는 단어들의 갯수는 몇개인가?
		
		File dataFile = new File("C:\\dev_programs\\java_workspace\\java_exam\\src",
								"data.txt");
		
		// 파일의 내용을 읽어서 Stream<String>으로 변환한다.
		Stream<String> lineStream = null;
		try{
			lineStream = Files.lines(dataFile.toPath());
		}
		catch(IOException ioe) {
			System.out.println(ioe.getMessage());
		}
		
		if(lineStream != null) {
			//1. Stream에 들어있는 한줄의 내용을 띄어쓰기로 자른다. 
			long count = lineStream.flatMap(line -> Arrays.stream(line.split(" ")))
			//2. 각 단어들 중 중복된 단어들은 모두 지운다.
									.distinct()
									.filter(word -> word.trim().length() > 0) //공백제거
									.filter(word -> word.matches("^[가-힣]+$")) // 한글만 가져온다
			//3. 중복이 제거된 단어들의 갯수를 센다.
			//		  .forEach(word -> System.out.println(word));
									.count();
			
			System.out.println("단어갯수 : " + count);
			
		}
	}
	
	public static void main(String[] args) {
		
		wordCounter();
	}

}
```
