# Filtering

```java
DishList dishList = new DishList();
List<Dish> menuList = dishList.getDishList();

menuList.stream()
        .filter(dish -> !dish.isVegetarian())  
        .filter(dish -> dish.getCalories() > 400)
        .forEach(dish -> System.out.println(dish.getName()));
```
* 위의 코드에서 filter() 함수는 두번 사용되었고 채식요리가 아니고 칼로리가 400이상인 음식들만 filtering 한다.
* filter 함수를 이용하여 조건에 맞는 항목만 filtering 할 수 있다.
* distinct()는 List의 중복된 항목을 제거한다.
* sorted()는 List의 요소값들을 오름차순으로 정렬한다.
# Slicing

```java
DishList dishList = new DishList();
List<Dish> menuList = dishList.getDishList();
		
		
menuList.stream()
        .sorted((firstDish, secondDish)->firstDish.getCalories()-secondDish.getCalories())
        .limit(3)
        .skip(2)
        .forEach(dish -> System.out.println(dish.getName()));
```
* 위의 코드에서 limit() 함수를 통해 칼로리를 기준으로 오름차순으로 정렬된 Stream을 칼로리가 낮은 3개만 추출한다.
* skip() 함수를 통해 Stream의 항목들 건너 뛸 수 있다.
# Mapping
```java
DishList dishList = new DishList();
List<Dish> menuList = dishList.getDishList();

menuList.stream()
        .map(dish -> dish.getName() + " --> " + dish.getCalories())
        .forEach(str -> System.out.println(str));
```
* 위의 코드에서 map 함수를 통해 menuList의 generic을 Dish에서 String으로 변경된다.
* map는 Stream의 data 형식을 변경시키는 함수이다.
* 변경된 결과에 따라서 Stream의 generic으로 변경되고 map함수를 통한 data는 원본 data로 돌아갈 수 없다.

# flatmapping
```java
// 메뉴이름을 글자로 모두 분리해서 하나 씩 출력
// 플랫맵은 파일과 파일을 비교할 때 주로 사용
// 파일을 스트림으로 변경하고 분석중에 다른 파일을 스트림으로 변경 후 본 스트림으로 치환하는 과정
		
		
DishList dishList = new DishList();
List<Dish> menuList = dishList.getDishList();
		
// 배열을 스트림에서 직접 제어 
menuList.stream()
	.map(dish -> dish.getName()) // Dish -> String 으로 변경
	.map(dishName -> dishName.split("")) // String -> String[]
	.forEach(letterArray -> {
		Arrays.asList(letterArray) // String[]을 List<String>으로 변경
		      .forEach(letter -> System.out.println(letter));
	});
		
// 1. 배열을 stream으로 치환해서 간단 사용
// 2. 배열을 stream으로 만드는 방법 
// 3. Arrays.stream(배열) -> Stream<T>
		
menuList.stream()
	.map(dish -> dish.getName()) // Dish->String 변경
	.map(dishName -> dishName.split("")) // String -> String[] 변경
	//.map(dishNameArray -> Arrays.stream(dishNameArray)) // String[]-> Stream<Stream<String>>
	.flatMap(dishNameArray-> Arrays.stream(dishNameArray)) // String[] -> Stream<String> (Stream<String>자체가 배열을 String으로 치환한 결과)
	.distinct()
	.forEach(letter -> System.out.println(">"+letter));
```
# Finding
```java
//menuList에서 채식요리 중 하나 조회한다.
//findAny
DishList dishList = new DishList();
List<Dish> menuList = dishList.getDishList();
		
Optional<Dish> melon = menuList.stream()
			       .filter(dish -> dish.getName().equals("melon"))
			       .findAny();
		
		
System.out.println(melon);
//System.out.println(melon.get()); //Dish Type, java.util.NoSuchElementException
//System.out.println(melon.get().getCalories()); //java.util.NoSuchElementException
		
System.out.println(melon.orElse(null));
System.out.println(melon.orElse(new Dish("수박", true, 400, Dish.Type.OTHER)));
	
Dish tempDish = melon.orElse(new Dish("수박", true, 400, Dish.Type.OTHER));
System.out.println(tempDish);
System.out.println(tempDish.getName());
System.out.println(tempDish.getCalories());
System.out.println(tempDish.getType());
```
* 불러온 Stream의 값이 null 일 수 있기 때문에 Optional이 사용된다.
* Optional은 null값인지 아닌지 확인할 수 있는 isPresent 함수가 있다.
* Optional을 통해 null 요소의 값을 유연하게 처리할 수 있다.
* T get() 함수는 값이 있는 경우 값을 반환하고 null 값인 경우 NoSuchElementException을 발생시킨다. 
# Matching

```java
// 모든 메뉴가 채식요리인가?
// allMatch, 최종함수
DishList dishList = new DishList();
List<Dish> menuList = dishList.getDishList();
		
boolean isAllVegetarian = menuList.stream()
				  .allMatch(dish -> dish.isVegetarian());
		
System.out.println(isAllVegetarian ? "모든 메뉴가 채식요리이다." : "모든 메뉴가 채식요리는 아니다.");
```
* Find와 마찬가지로 특정한 속성이 데이터 집합에 포함되어 있는지 여부를 검색하는데 사용된다.
* Matching에는 allMatch, anyMatch, noneMatch, findFirst, findAny있다.  
