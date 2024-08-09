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
			//lineStream.parallel() -> 병렬 처리
			//1. Stream에 들어있는 한줄의 내용을 띄어쓰기로 자른다. 
			long count = lineStream.parallel()
					       .flatMap(line -> Arrays.stream(line.split(" ")))
			//2. 각 단어들 중 중복된 단어들은 모두 지운다.
					       .distinct()
					       .filter(word -> word.trim().length() > 0) //공백제거
					       .filter(word -> word.matches("^[가-힣]+$")) // 한글만 가져온다
			//3. 중복이 제거된 단어들의 갯수를 센다.
			//		       .forEach(word -> System.out.println(word));
									.count();
			
			System.out.println("단어갯수 : " + count);
			
		}
	}
	
	public static void main(String[] args) {
		
		wordCounter();
	}

}
```

```java
package com.ktdsuniversity.edu.stream.generate.map;

import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;

public class Main {
	
	public static void mapToStream() {
		
		Map<String, String> map = new HashMap<>();
		map.put("A", "a");
		map.put("B", "b");
		map.put("C", "c");
		map.put("D", "d");
		map.put("E", "e");
		
		//Predicate <T> -> T를 주면 boolean 반환
		//Function <T, R> -> T를 주면 R을 반환
		//Consumer <T> -> T를 주면 void 반환
		
		//BiPredicate <T, R> -> (T, R)를 주면 boolean 반환
		//BiFunction <T, U, R> -> (T, U)를 주면 R을 반환
		//BiConsumer <T, R> -> (T, R)를 주면 void 반환
		
		
		//Map Stream
		//map을 Stream으로 변경하기 위해서는 map을 entryset으로 변경해야한다.
		map.entrySet() // Set<Entry<String, String>>
		   .stream()  // Stream<Entry<String, String>>
		   .forEach(entry -> {
			   System.out.println("대문자 : " + entry.getKey() + " // 소문자 : " + entry.getValue());
		   }); // Param : Entry<String, String>
		
		
		//Lambda forEach
		map.forEach((key, value)-> {
			
			System.out.println("대문자 : "+ key + ", 소문자 : " + value);
		});
		
		
		// Normal While
		
		String key = null;
		Iterator<String> keyset = map.keySet().iterator();
		while(keyset.hasNext()) {
			
			key = keyset.next();
			String value = map.get(key);
			
			System.out.println("대문자 : "+ key + ", 소문자 : " + value);
			
		}
		
	}
	
	public static void main(String[] args) {
		mapToStream();
	}

}
```
