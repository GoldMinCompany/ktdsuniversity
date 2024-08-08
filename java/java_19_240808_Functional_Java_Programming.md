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

# Find 

# Matching
