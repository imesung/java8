# 스트림


## 4.1 스트림이란 무엇인가?

* 스트림 API의 특징
	* 선언형
		* 더 간결하고 가독성이 좋아짐
	* 조립할 수 있음
		* 유연성이 좋아짐
	* 병렬화
		* 성능이 좋아짐



## 4.2 스트림 시작하기

### 스트림은 무엇인가?

* 연속된 요소
	* 컬렉션의 주제는 데이터, 스트림의 주제는 계산
* 소스
	* 리스트로 스트림을 만들 시 스트림의 요소는 리스트의 요소와 같은 순서를 유지
* 데이터 처리 연산
	* 함수형 프로그래밍 언어에서 일반적으로 지원하는 연산과 데이터베이스와 비슷한 연산을 지원
* 스트림 중요 특징
	* 파이프라이닝
		* 스트림 연산끼리 연결하여 파이프라인을 구성할 수 있도록 스트림 자신을 반환
	* 내부 반복
		* for문 방식과 달리 스트림은 내부 반복을 지원
* ![KakaoTalk_20190930_152200411](https://user-images.githubusercontent.com/40616436/65862765-edcb1b80-e3a9-11e9-8905-f0848b991bb5.jpg)
	* menu에 스트림 메서드를 호출하여 요리 리스트로 부터 스트림 얻음
	* filter, map, limit, collect로 데이터 처리 연산을 적용
	* collect를 제외한 모든 연산은 서로 파이프라인으로 형성
	* 마지막으로 collect로 파이프라인을 처리하여 결과를 받음(collect는 list반환)
* ![KakaoTalk_20190930_152739692](https://user-images.githubusercontent.com/40616436/65862841-0b988080-e3aa-11e9-985e-72d4322d7aa7.jpg)

## 스트림과 컬렉션

### 스트림과 컬렉션 차이
* 컬렉션은 연산을 수행할때마다 모든 요소를 메모리에 저장해야 하며, 추가 혹은 삭제가 가능
* 스트림은 요청할 때만 요소를 계산, 추가 혹은 삭제가 불가능
* ![KakaoTalk_20190930_153533350](https://user-images.githubusercontent.com/40616436/65862856-12bf8e80-e3aa-11e9-9596-579debb80534.jpg)

### 4.3.1 한번만 탐색 가능
* 스트림은 단 한번만 소비할 수 있음.
* 스트림의 요소는 소비된다. 즉, 스트림을 받고 사용하게 되면 그걸로 끝. 다시 사용 시 초기 데이터의 스트림을 만들어야 함.



## 4.4 스트림 연산

* ![KakaoTalk_20190930_154545547](https://user-images.githubusercontent.com/40616436/65862929-2bc83f80-e3aa-11e9-80d2-f35bc56b30ed.jpg)
	* (**중간연산**) 파이프라인을 형성하는 그룹 : filter, map, limit
	* (**최종연산**) 파이프라인 실행한 다음 닫는 그룹 : collect

### 4.4.1 중간 연산
* 중간 연산은 다른 스트림을 반환
* 중간 연산을 연결하여 질의를 만들 수 있음
* 최종 연산을 스트림 파이프라인에 실행하기 전까지 아무 연산을 수행하지 않음
* 람다를 활용하여 출력문으로 실행 과정 확인 가능

### 4.4.2 최종 연산
* 보통 최종 연산은 List, Integer, void가 반환

### 4.4.3 스트림 이용하기
* 스트림 이용 과정 세가지 요약
	* 질의를 수행할 데이터 스소(collection)
	* 스트림 파이프라인을 구성할 중간 연산
	* 스트림 파이프라인을 실행하고 결과를 만들 최종연산



# 5. 스트림 활용



## 5.1 필터링과 슬라이싱

### 5.1.1 프레디케이트로 필터링
* **filter** 프레디케이드(boolean 반환)를 인수로 받아서 스트림을 반환 메서드 지원

### 5.1.2 고유 요소 필터링
* **distinct**(중복 제거) 메서드 지원

### 5.1.3 스트림 축소
* **limit(n)** 주어진 사이즈 이하의 크기를 갖는 새로운 스트림을 반환하는 메서드 지원

### 5.1.4 요소 건너뛰기
* **skip(n)** 처음 n개 요소를 제외한 스트림을 반환하는 메서드 지원



## 5.2 매핑

### 특정 데이터 선택

### 5.2.1 스트림의 각 요소에 함수 적용하기
* **map** 함수를 인수로 받는 메서드 지원 - 새로운 버전을 만든다 라는 개념(새로운 스트림을 만듬)
* 예)요리명 List
	* map(Dish::getName);
* 예)요리 이름 길이 List		
	* map(Dish::getName).map(String::length)

### 5.2.2 스트림 평면화
* map 메서드는 String[]을 반환
* Stream<String> 반환을 위해선 array.stream 및 flatMap 활용
	* map(word -> word.split(“”)).flatMap(Arrays::stream)
* flatMAp 메서드는 스트림의 각 값을 다른 스트림으로 만든 다음 모든 스트림을 하나의 스트림으로 연결하는 기능



## 5.3 검색과 매칭

특정 속성이 데이터 집합에 있는지의 여부를 검색하는 데이터 처리 자주 사용

* 쇼트 서킷 기법 (&&, ||와 동일)
	* anyMatch
		* 프레디케이트가 적어도 한 요소와 일치하는지 확인
	* allMatch
		* 모든 요소가 프레디케이트와 일치하는 지 확인
	* nonMatch
		* allMatch의 반대

* findAny
	* 임의의 요소를 반환
	* 값을 어떻게 처리할 것인지 강제하는 기능을 제공하는 것
	* 있으면 다음 거 실행 없으면 안해
* findFirst
	* 첫번째 요소를 찾음
* findAny와 findFirst는 병렬성으로 인해 필요



## 5.4 리듀싱

리듀스 연산을 이용해 스트림 요소의 조합으로 복잡한 질의를 표현

### 5.4.1 요소의 합
* 리듀스 연산 전
	int sum = 0;
	for(int x : numbers) {
		sum += x;
	} 
* 리듀스 연산 적용
	* 필요 정보 - 초깃값 0, 연산식
	int sum = numbers.stream().reduce(0, (a,b) -> a+b);
* 초깃값 없음
	* 초깃값 없는 리듀스도 있으나 이는 optional 객체를 반환함 - Optional은 합계(초기값이 없으면 합계가 없으니)가 없음을 가리킴
	* Optional<Integer> sum = numbers.stream().reduce((a,b) -> (a+b));

### 5.4.2 최댓값과 최솟값
* reduce 연산은 새로운 값을 이용해서 스트림의 모든 요소를 소비할 때까지 람다를 반복 수행하면서 최댓값을 생산! 
	* Optional<Integer> max = numbers.stream().reduce(Integer::max);
	* Integer::max 대신 min도 가능



## 5.6 숫자형 스트림

### 5.6.1 기본형 특화 스트림

* mapToInt, mapToDouble, mapToLogng을 제공
	* 리듀스
		* map(Dish::getCalories).reduce(0, Integer::sum);
	* 기본형 스트림
		* mapToInt(Dish::getCalories).sum(); intStream	반환 후 intStream의 sum 함수 반환

* boxed
	* intstream을 다시 객체 스트림을 변환 가능
	* Stream<Integer> stream = instSream.boxed();

* max, min
	* Optional(Int, Double, Long)을 활용하여 max, min 구할 수 있음
	* OptionalInt oi = menu.stream().mapToInt(Dish::getCalories).max();
	* oi.orElse(1); - 값이 없을 때 기본 최댓값을 명시적으로 설정

### 5.6.2 숫자 범위

* rangeClosed(1, 100)
	* 1부터 100까지의 숫자를 만들 수 있음



## 5.7 스트림 만들기

* Stream.of
	* 값으로 스트림 만듬
	* Stream.of(“a”, “b”);
* Arrays.stream
	* 배열로 스트림 만듬
	* Arrays.stream(numbers).sum();
* Files.lines
	* 파일로 스트림 만들기 가능

* 함수로 무한 스트림 만들기
	* iterate
		* 받은 인수로 새로운 값을 끊임없이 생산할 수 있음 - 언바운드 스트림(무한 스트림)
	* generate
		* SUpplier<T>를 인수로 받아 값을 생산