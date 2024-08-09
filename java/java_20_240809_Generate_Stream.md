# Generate Stream
```java
package com.ktdsuniversity.edu.stream.collecting.join;

import java.util.List;
import java.util.stream.Collectors;

import com.ktdsuniversity.edu.stream.introduce.Dish;
import com.ktdsuniversity.edu.stream.introduce.DishList;

public class Main {
	
	public static void printJoinAllMenuNames(String separator) {
		
		DishList dishList = new DishList();
		
		List<Dish> menuList = dishList.getDishList();
		
		//모든 메뉴의 이름을 연결해서 출력한다. 
		String menuName = menuList.stream() //Stream<Dish>
								  .map(dish -> dish.getName()) // Stream<String>
								  .collect(Collectors.joining(separator));
		
		System.out.println(menuName);
		
		
	}
	
	public static void main(String[] args) {
		printJoinAllMenuNames("");
		printJoinAllMenuNames("-");
		printJoinAllMenuNames(" , ");
	}

}
```
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

import java.util.ArrayList;
import java.util.HashMap;
import java.util.Iterator;
import java.util.List;
import java.util.Map;

public class Main {
	
	public static void theaterSeat() {
		
		Map<String, List<Boolean>> seats = new HashMap<>();
		
		char A = 'A'; // 65
		System.out.println(A + 0); // 65
		
		char Z = 'Z'; // 90
		System.out.println(Z + 0); // 90
		
		for(char a = 'A'; a < 'Z' + 1; a++) {
			
			
			List<Boolean> emptySeats = new ArrayList<>();
			
			for(int i=0; i < 15; i++) {
				
				emptySeats.add((int)(Math.random()*10) % 2 == 0);
				
			}
			
			seats.put(a + "", emptySeats);
			
		}
		
		
		// D열에 남아있는 자리 중 D5번은 비어있나?
		// 객체지향으로 풀면 더 간단하다. 
		boolean isEmptyDSeats = seats.get("D").contains(true);
		System.out.println(isEmptyDSeats ? "D열에 남아있는 자리가 있습니다" : "D열에 남아있는 자리가 없습니다.");
		
		
		// D열에 남아있는 자리가 있나?
		boolean existsSeat = seats.entrySet() // Set<Entry<String, List<Boolean>>>
								   .stream() // Stream<Entry<String, List<Boolean>>>
								   .filter(entry -> entry.getKey().equals("D")) // Stream<Entry<String, List<Boolean>>>
								   .map(entry -> entry.getValue()) // Stream<List<Boolean>> --> Stream<Boolean>
								   .flatMap(seatsOfD -> seatsOfD.stream()) //Stream<Stream<Boolean>> --> Stream<Boolean>
								   .anyMatch(seat -> seat); // boolean
		
		System.out.println(existsSeat ? "D열에 남아있는 자리가 있습니다" : "D열에 남아있는 자리가 없습니다.");
		
		// Stream을 이용하는 것이 더 비효율적이다. 
		// 1. 일반적인 코드로(객체지향) 작성
		
		boolean emptySeat = seats.get("D").get(4);
		System.out.println(emptySeat ? "D5번 좌석은 앉을 수 있습니다." : "D5번 좌석은 이미 예약되어 있습니다.");
		
		
		// 2. Stream으로 풀어보자
		boolean isEmptyD5 = seats.entrySet() // Set<Entry<String, List<Boolean>>>
								  .stream() // Stream<Entry<String, List<Boolean>>>
								  .filter(entry -> entry.getKey().equals("D")) // Stream<Entry<String, List<Boolean>>>
								  .map(entry -> entry.getValue().get(4)) // Stream<Boolean>
								  .findFirst() // Optional <Boolean>
								  .orElse(false); // boolean
		
		
		System.out.println(isEmptyD5 ? "D5번 좌석은 앉을 수 있습니다." : "D5번 좌석은 이미 예약되어 있습니다.");
	}
	
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
		theaterSeat();
	}

}
```
### 실습 및 과제

```java
package com.ktdsuniversity.edu.stream.generate.map;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.Iterator;
import java.util.List;
import java.util.Map;

public class Main {
	
	public static void theaterSeat() {
		
		Map<String, List<Boolean>> seats = new HashMap<>();
		
		char A = 'A'; // 65
		System.out.println(A + 0); // 65
		
		char Z = 'Z'; // 90
		System.out.println(Z + 0); // 90
		
		for(char a = 'A'; a < 'Z' + 1; a++) {
			
			
			List<Boolean> emptySeats = new ArrayList<>();
			
			for(int i=0; i < 15; i++) {
				
				emptySeats.add((int)(Math.random()*10) % 2 == 0);
				
			}
			
			seats.put(a + "", emptySeats);
			
		}
		
		
		//1. 도전과제
		// TODO : 앉을 수 있는 자리가 5개 이상되는 열의 이름을 출력한다. - forEach(최종함수) 
		/*
		 * psvm(boolean tf){
		 *  return tf
		 *  
		 */
		seats.entrySet()
			 .stream()//Stream<Entry<String,List<Boolean>>>
			 .filter(entry -> entry.getValue()
					 			   .stream()
					 			   .filter(emptyTrue->emptyTrue).count() >= 5) //List<Boolean> -> Stream<Boolean>
			 .map(entry -> entry.getKey())
			 .forEach(key -> System.out.println(key));
		
		
		//2. 도전과제
		// TODO : 열마다 앉을 수 있는 자리를 출력한다. 
		// (예를 들어, A:6, B:7, C:0 ... )
		
		seats.entrySet()
		 	 .stream()//Stream<Entry<String,List<Boolean>>>
		 	 .map(entry -> entry.getKey() + " : " + entry.getValue().stream().filter(isTrue -> isTrue).count())
		 	 .forEach(keyValue -> System.out.println(keyValue));
		
		// D열에 남아있는 자리 중 D5번은 비어있나?
		// 객체지향으로 풀면 더 간단하다. 
		boolean isEmptyDSeats = seats.get("D").contains(true);
		System.out.println(isEmptyDSeats ? "D열에 남아있는 자리가 있습니다" : "D열에 남아있는 자리가 없습니다.");
		
		
		// D열에 남아있는 자리가 있나?
		boolean existsSeat = seats.entrySet() // Set<Entry<String, List<Boolean>>>
								   .stream() // Stream<Entry<String, List<Boolean>>>
								   .filter(entry -> entry.getKey().equals("D")) // Stream<Entry<String, List<Boolean>>>
								   .map(entry -> entry.getValue()) // Stream<List<Boolean>> --> Stream<Boolean>
								   .flatMap(seatsOfD -> seatsOfD.stream()) //Stream<Stream<Boolean>> --> Stream<Boolean>
								   .anyMatch(seat -> seat); // boolean
		
		System.out.println(existsSeat ? "D열에 남아있는 자리가 있습니다" : "D열에 남아있는 자리가 없습니다.");
		
		// Stream을 이용하는 것이 더 비효율적이다. 
		// 1. 일반적인 코드로(객체지향) 작성
		
		boolean emptySeat = seats.get("D").get(4);
		System.out.println(emptySeat ? "D5번 좌석은 앉을 수 있습니다." : "D5번 좌석은 이미 예약되어 있습니다.");
		
		
		// 2. Stream으로 풀어보자
		boolean isEmptyD5 = seats.entrySet() // Set<Entry<String, List<Boolean>>>
								  .stream() // Stream<Entry<String, List<Boolean>>>
								  .filter(entry -> entry.getKey().equals("D")) // Stream<Entry<String, List<Boolean>>>
								  .map(entry -> entry.getValue().get(4)) // Stream<Boolean>
								  .findFirst() // Optional <Boolean>
								  .orElse(false); // boolean
		
		
		System.out.println(isEmptyD5 ? "D5번 좌석은 앉을 수 있습니다." : "D5번 좌석은 이미 예약되어 있습니다.");
	}
	
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
		theaterSeat();
	}

}
```
```java
package com.ktdsuniversity.edu.stream.collecting.join;

import java.util.List;
import java.util.stream.Collectors;

import com.ktdsuniversity.edu.stream.introduce.Dish;
import com.ktdsuniversity.edu.stream.introduce.DishList;

public class Main {
	
	//3. 도전과제
	//채식 메뉴를 아래와 같은 포맷으로 연결한다.
	//이름 : 칼로리 : 타입, 이름 : 칼로리 : 타입 .... 
	
	public static void HomeWork3() {
		
		DishList dishList = new DishList();
		List<Dish> menuList = dishList.getDishList();
		
		
		menuList.stream()
				.filter(dish -> dish.isVegetarian())
				.forEach(dish -> System.out.println(dish.getName() + ":" + dish.getCalories() + ":" + dish.getType()));
		
	}
	
	
	
	
	public static void printJoinAllMenuNames(String separator) {
		
		DishList dishList = new DishList();
		
		List<Dish> menuList = dishList.getDishList();
		
		//모든 메뉴의 이름을 연결해서 출력한다. 
		String menuName = menuList.stream() //Stream<Dish>
								  .map(dish -> dish.getName()) // Stream<String>
								  .collect(Collectors.joining(separator));
		
		System.out.println(menuName);
		
		
	}
	
	public static void main(String[] args) {
		//printJoinAllMenuNames("");
		//printJoinAllMenuNames("-");
		//printJoinAllMenuNames(" , ");
		HomeWork3();
	}

}
```

```java
package com.ktdsuniversity.edu.stream.collecting.list;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

import com.ktdsuniversity.edu.stream.introduce.Dish;
import com.ktdsuniversity.edu.stream.introduce.DishList;

public class Main {
	
	// 4. 도전과제!!
	// 전체 메뉴 중에서 칼로리가 높은 순서대로 정렬한 이후에 상위 5개만 별도의 리스트로 할당 
	// 리스트를 출력
	public static void HomeWork4() {
		
		DishList dishList = new DishList();
		List<Dish> menuList = dishList.getDishList();
		
		List<Dish> highCaloriesDish = menuList.stream()
						 				.sorted((dish1, dish2) -> dish2.getCalories() - dish1.getCalories())
						 				.limit(5)
						 				.toList();
		
		highCaloriesDish.forEach(dish -> System.out.println(dish));
		
	}
	
	// 5. 메뉴의 이름이 띄어쓰기 없는 5글자 이상인 메뉴들 중 칼로리가 낮은 순서대로 상위 3개만 별도의 리스트에 할당 후 리스트 출력
	
	public static void HomeWork5() {
		
		DishList dishList = new DishList();
		List<Dish> menuList = dishList.getDishList();
		
		List<Dish> x = menuList.stream()
							   .filter(dish -> !dish.getName().contains(" "))
							   .filter(dish -> dish.getName().trim().length() > 5)
							   .sorted((dish1, dish2)-> dish1.getCalories() - dish2.getCalories())
							   .toList();
			
		x.forEach(dish -> System.out.println(dish));
	}
	
	
	public static void streamToList() {
		// 전체 메뉴에서 육식메뉴이거나 칼로리가 400미만인 요리만 추출해서 리스트로 저장한다.
		DishList dishList = new DishList();
		List<Dish> menuList = dishList.getDishList();
		
		List<Dish> specialFood = menuList.stream() // Stream<Dish>
										 .filter(dish -> dish.getType() == Dish.Type.MEAT || dish.getCalories() < 400) //Stream<Dish>
										//.collect(Collectors.toList()); // List<Dish>, Since Java 1.8
										 .toList(); //List<Dish> Since Java 16
		
		specialFood.forEach(dish -> System.out.println(dish));
	
	}
	
	
	public static void main(String[] args) {
		//streamToList();
		//HomeWork4();
		HomeWork5();
	}

}
```
