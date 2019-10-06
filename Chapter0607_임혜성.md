# 스트림으로 데이터 수집

## 컬렉터란?
* 스트림의 모든 항목으로 하나의 결과로 합칠 수 있음.

### import 사용
	import static java.util.stream.Collecotrs.*;

### 요약 연산
* Collecotrs 클래스는 collecotrs.summingInt라는 특별한 요약 팩토리 메서드를 제공
* 합계 및 평균 값 구함

~~~int totalCalories = menu.stream().collect(summingInt(Dish::getCalories));
Double avgCalories = menu.stream().collect(averagingInt(Dish::getCalories));
~~~

### 합계, 평균, 최대, 최소, 수의 모든 정보

* ![KakaoTalk_20191006_142343293](https://user-images.githubusercontent.com/40616436/66267069-d2f41d80-e867-11e9-9656-2e4ef1e3ef32.jpg)

### 문자열 연결
	String shortMenu = menu.stream().map(Dish::getName).collect(joining(“, “));

### 리듀싱 요약 연산
* Bifunction<T, T, T,>를 인수로 받음 - 두 인수를 받아 같은 형식으로 반환
* ![KakaoTalk_20191006_143340061](https://user-images.githubusercontent.com/40616436/66267075-dedfdf80-e867-11e9-9036-bea8fea06afa.jpg)

## 그룹화
### 단일 수준 그룹화
	Map<Dish.Type, List<Dish>> dishesByType = menu.stream().collect(groupingBy(Dish::getType));
	결과 :
	[FISH=[prawns, salmon], other=]french fries, rice, season fruit, pizza], MEAT=[pork, beef, chicken]}

### 다 수준 그룹화
* ![KakaoTalk_20191006_145059404](https://user-images.githubusercontent.com/40616436/66267077-e1dad000-e867-11e9-8fdb-409f86830af8.jpg)
* 결과
	* ![KakaoTalk_20191006_145142213](https://user-images.githubusercontent.com/40616436/66267078-e606ed80-e867-11e9-8469-af0a63b88c20.jpg)

### 서브그룹으로 데이터 수집
	- 종류별 요리 수
	Map<Dish.Type, Long> typesCount = menu.stream().collect(groupingBy(Dish::getType, counting()));
	결과 : {MEAT=3, FISH=2, OTHER=4}
	
	- 종류별로 가장 높은 칼로리를 가진 요리
	Map<Dish.Type, Optional<Dish>> mostCaloricByType = menu.strea().collect(groupingBy(Dish::getType, maxBy(comparingInt(Dish::getCalories))));
	결과 : {FISH=Optional[salmon], OTHER=Optional[pizza], MEAT=Optional[pork]}

### collector와 함께 사용
	- 메뉴에 있는 모든 요리의 칼뢰 합계
	Map<Dish.Type, Integer> totalCaloriesByType = menu.stream().collect(groupingBy(Dish::getType, summingInt(Dish::getCalories)));

* mapping 사용
	* ![KakaoTalk_20191006_150926007](https://user-images.githubusercontent.com/40616436/66267083-fb7c1780-e867-11e9-8ee8-30f5a4387714.jpg)

## 분할 함수
* 프레디케이트를 분류 함수로 사용한 그룹화 기능

```
Map<Boolean, List<Dish>> partitionedMenu = menu.stream().collect(partitioningBy(Dsh::isVegetarian));
결과 : 
{false = [pork, beef, chicken, prawns, salmon],
true = [french fries, rice, season fruit, pizza]}
```

	- 참값의 키로 맵에서 모든 채식요리를 얻을 수 있음
	List<Dish> vegetarianDishes = partitionedMenu.get(true);
	
	- filter 사용
	List<Dish> vegetarianDishes = menu.stream().filter(Dish::isVegetarian).collect(toList());

### 분할 함수 활용
* 채식 요리와 채식 아닌 요리 중 칼로리가 가장 높은 것
	* ![KakaoTalk_20191006_151858081](https://user-images.githubusercontent.com/40616436/66267085-fe770800-e867-11e9-992e-56f9f089171d.jpg)

## Collector 인터페이스
* supplier 메서드
	* 빈 리스트 반환
	* return () -> new ArrayList<T>();
* accumulator 메서드
	* 리듀싱 연산을 수행
	* return (list, item) -> list.add(item);
* finisher 메서드
	* 스트림 탐색 끝난 후 최종 변환값을 결과 컨테이너로 적용
	* return Function.identity();
* combiner 메서드
	* 스트림의 서로 다른 서브파트를 병렬로 처리할 때 결과를 어떻게 처리할 지 정의
	* return (list1, list2) -> {list1.addAll(list2); return list1;
* Characteristics 메서드
	* UNORDERED, CONCURRENT, IDENTITY_FINISH 항목을 포함
	* collect 메서드가 어떤 최적화(병렬화 같은)를 이용해서 리듀싱 연산을 수행할 것인지 도움

# 병렬 데이터 처리와 성능
## 병렬 스트림
* 순차 스트림 -> 병렬 스트림으로 변환
	* .parallel()를 사용하면 병렬 스트림으로 변환
	* .sequential()을 사용하면 병렬에서 직렬 스트림으로 변환
* 동시 사용시 최종적으로 호출된 메서드가 전체 파이프라인에 영향을 줌
	* .parralel().map().sequential().filter().parralel().reduce();
	* 병렬로 실행

### 병렬 스트림 사용 시 주의
* 올바른 자료구조를 선택해야 병렬 실해옫 최적의 성능을 발휘 할 수 있음
* 병렬화 이용 시
	* 스트림을 재귀로 분할
	* 각 서브스트림을 서로 다른 스레드의 리듀싱 연산으로 할당
	* 이들 결과를 하나의 값으로 합쳐야 함

### 공유 상태를 바꾸는 알고리즘에 병렬 사용 시
* 공유된 가변 상태를 사용함으로 올바른 값이 나타나지 않음

### 병렬 스트림 효과적으로 사용
	- 직집 측정 후 사용
	- 자동 박싱과 언박싱은 성능을 크게 저하시킬 수 있음 (기본형 특화 스트림을 사용해라 [intStream, LoginStream, DoubleStream])
	- 전체 파이프라인 연산 비용을 고려(처리해야할 요소 수: N, 하나 요소 처리 시 드는 비용: Q, 총 비용: N*Q
	- 소량의 데이터는 병렬 스트림에 도움 되지 않음

* 스트림 소스와 분해성
	* ![KakaoTalk_20191006_164141019](https://user-images.githubusercontent.com/40616436/66267086-0171f880-e868-11e9-958c-6552163edec9.jpg)

## 포크/조인 프레임워크
* 병렬화할 수 있는 작업을 재귀적으로 작은 작업으로 분할 후 서브태스크 각각의 결과를 합쳐 전체 결과를 만듬
* 포크/조인 과정
	* ![KakaoTalk_20191006_165547817](https://user-images.githubusercontent.com/40616436/66267090-0636ac80-e868-11e9-96a7-fde1049836f6.jpg)

### 포크/조인 효과적으로 사용하는 방법
	- 서브태스크가 모두 시작된 다음 join을 호출
	- RecursiveTask에서는 invoke 사용하지 말아라(순차 코드에서 병렬 계산을 시작할 때만 invoke 사용)
	- 양쪽 모두 fork 메서드 호출보단, 한쪽은 compute를 호출해야 한 태스크에는 같은 스레드를 재사용할 수 있음
	- 디버깅이 어려움
	- 순차 처리보다 무조건 빠른건 아님(빠를려면, 태스크를 여러 독립적인 서브태스크로 분할할 수 있어야함.)

### 작업 훔치기
* ForkJoinPool의 모든 스레드를 거의 공정하게 분할
	* ![KakaoTalk_20191006_174016182](https://user-images.githubusercontent.com/40616436/66267103-25353e80-e868-11e9-8a81-fcad1dab2901.jpg)

## 자동 스트림 분할 Spliterator
* Spliterator는 분할할 수 있는 반복자라는 의미
* Spliterator의 여러 메서드

```
tryAdvance : Spliterator의 요소를 하나씩 순차적으로 소비하면서 탐색해야 할 요소가 남을 시 참을 반환
trySplit : Spliterator의 일부 요소를 분할해서 두번째 Spliterator를 생성하는 메서드
estimateSize : 탐색해야할 요소 수 정보를 제공
```

### 재귀 분할 과정
* trySplit의 결과가 null이면 재귀 분할 과정 종료
	* ![KakaoTalk_20191006_174537845](https://user-images.githubusercontent.com/40616436/66267104-27979880-e868-11e9-9ccb-0ca1a4ad3e4c.jpg)

### Spliterator 특성
* ![KakaoTalk_20191006_174709892](https://user-images.githubusercontent.com/40616436/66267105-29615c00-e868-11e9-8062-692838d06965.jpg)

### 커스텀 Spliterator
	String을 스트림 : stream<Character>
	분할 위치에 따라 잘못된 결과가 나올 수 있음.