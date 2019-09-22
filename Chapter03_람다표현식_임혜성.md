# 람다 표현식

들어가기에 앞서 람다 표현식이란, 익명클래스와 비슷?

* 이 장에서 주요 설명
	* 람다 표현식을 어떻게 만드는가?
	* 어떻게 사용하는가?
	* 어떻게 코드를 간결하게 만드는가?

* 추가적 설명
	* JAVA8의 중요 인터페스
	* 메서드 레퍼런스



## 3.1 람다란 무엇인가?

정의, 메서드로 전달할 수 있는 익명 함수를 단순화한 것.

* 람다의 특징
	* **익명** :메서드 이름 없음)
	* **함수** : 메서드와 달리 특정 클래스에 종속되지 않음, BUT 파라미터 리스트, 바디, return 형식, 예외 리스트는 포함
	* **전달** : 람다 표현식을 메서드 인수로 전달하거나 변수로 저장 가능
	* **간결성** : 익명 클래스처럼 자질구레한 코드를 구현할 필요 없음

결과적으로, **JAVA8 이전의 자바로 할 수 없었던 것을 새롭게 제공하는 것이 아니라 코드를 좀 더 쉽게 구현함으로써, 코드가 간결하고 유연해짐에 초점을 둠**

* 간단한 예제
	* ![return_string_type](https://user-images.githubusercontent.com/40616436/65386848-6b27d800-dd7b-11e9-8f13-d6635767435a.jpg)
* 람다의 표현식
	* ![image3_1](https://user-images.githubusercontent.com/40616436/65386842-606d4300-dd7b-11e9-9f2b-69af2fba70d6.jpg)

* 예제 3-1 JAVA8의 유효한 람다 표현식
	* ![example3_1](https://user-images.githubusercontent.com/40616436/65386838-5a776200-dd7b-11e9-8ec7-52c059646265.jpg)



## 3.2 어디에, 어떻게 람다를 사용할까?

* 결론, 함수형 인터페이스라는 문맥에서 람다 표현식을 사용

### 3.2.1 함수형 인터페이스

* 함수형 인터페이스
	* 정확히 하나의 추상 메서드만을 지정하는 인터페이스
	* public interface Comparator<T> {
		int compare(T o1, T o2);
}
* 함수형 인터페이스 사용한 람다 표현식과 익명 클래스
		* ![선언 후 람다 사용](https://user-images.githubusercontent.com/40616436/65386835-50edfa00-dd7b-11e9-8eac-5f6b1938b12e.jpg)

### 3.2.2 함수 디스크립터

* 정의, 함수형 인터페이스의 추상 메서드 시그치너 == 람다 표현식의 시그니처
즉, 람다 표현식의 시그니처를 서술하는 메서드를 **함수 디스크립터**

* 람다 표현식의 유효성을 확인하는 방법
	* 변수에 할당
	* 함수형 인터페이스를 인수로 받는 메서드로 전달
	* 함수형 인터페이스의 추상 메서드와 같은 시그니처를 갖음

* @FunctionalInterface
	* 함수형 인터페이스임을 가리키는 어노테이션
	* 해당 어노테이션을 사용했는데, 두 개이상의 메서드를 사용할 시 컴파일 에러가 발생



## 3.3 람다 활용: 실행 어라운드 패턴

* 람다와 동작 파라미터화로 유연하고 간결한 코드를 구현 예제
	* ![실행 어라운드 패턴](https://user-images.githubusercontent.com/40616436/65386832-492e5580-dd7b-11e9-84f9-ec5ec2b8bdf3.jpg)



## 3.4 함수형 인터페이스 사용

* 다양한 람다 표현식을 사용하기 위해서는 공통의 함수 디스크립터를 기술하는 함수형 인터페이스가 집합이 필요
	* JAVA8의 새로운 함수형 인터페이스
		* Predicatew, Consumer, Function

### 3.4.1 Predicate

* java.util.functoin.Predicate<T>
	* test라는 추상 메서드를 정의
	* test는 제네릭 형식의 T의 객체를 인수로 받아 boolean으로 반환
	* EX) Predicate<String> nonEmpty = (String s) -> !s.isEmpty();
		* T -> boolean의 시그니처

### 3.4.2 Consumer

* java.util.function.Consumer<T>
	* void를 반환하는 accept라는 추상 메서드 정의
	* EX) Consumer<Integer> printInt = (Integer i) -> Sysout(i);

### 3.4.3 Function

* java.util.function.Function<T, R>
	* 제네릭 형식 T를 인수로 받아 R 객체를 반환해주는 apply라는 추상 메서드 정의
	* EX) Function<String, Integer> strLength = (String s) -> s.length();

**제네릭 함수형 인터페이스에서는 참조형만 사용 가능(Byte, Integer..)**

* 기본형 특화를 통해 박싱 진행하거나 진행 안함. 
	* 어라운드 패턴에서 정의한 함수형 인터페이스를 JAVA8이 제공한 함수형 인터페이스 활용
		* ![function 그림](https://user-images.githubusercontent.com/40616436/65386830-416eb100-dd7b-11e9-8677-c620b7827812.jpg)



## 3.5 형식 검사, 형식 추론, 제약

### 3.5.1 형식 검사

* 대상 형식 : 람다가 전달될 메서드 파라미터나 람다가 할당되는 변수에서 기대되는 람다 표현식의 형식 (Predicate, Consumer..Runnable)
* 람다의 시그니처와 함수형 인터페이스의 시그니처가 동일한가?!
* Ex) ![KakaoTalk_20190922_171632859](https://user-images.githubusercontent.com/40616436/65386828-37e54900-dd7b-11e9-915f-7f9795a90456.jpg)
  *  filter 메소드를 확인 -> filter(List<Apple> inventotry, Predicate<Apple> p)
    * 즉, 람다 표현식의 대상 형식은 Predicate라는 것을 확인
  * Predicate<Apple> 인터페이스의 추상 메서드는 무엇인가?
    * boolean test(T t) -> boolean test(Apple apple)
    * 즉, Apple을 인수로 받아 boolean을 반화하는 test 메서드임을 확인
      * Apple -> boolean
  * 함수 디스크립터는 Apple -> boolean이므로 람다의 시그니처와 일치
    * 즉, 형식 검사가 성공적으로 완료

### 3.5.2 같은 람다, 다른 함수형 인터페이스

* 대상 형식의 특징으로 인해 같은 람다 표현식이라도 호환되는 추상 메서드를 가진 함수형 인터페이스로 사용될 수 있음.
* 예제
  * Callable<T> =  () -> T
    * Callable<Integer> test1 = () -> 42;
  * PrivilegedAction<T>  = () -> T
    * PrivilegedAction<Integer>  = () -> 42;
* 람다의 바디에 일반 표현식이 있으면, void를 반환하는 함수 디스크립터(Consumer와 같은..)와 호환 됨.
  * Predicate<String> p = s -> list.add(s); //boolean을 return 하는 함수형 인터페이스
  * Consumer<String> b = s -> list.add(S); //void를 가진 함수형 인터페이스
  * list.add(); 는 boolean을 return 하는 메서드, Consumer는 void 반환을 기대하지만 boolean 반환도 호환 가능
* 주로 사용되는 람다 표현식
  * Runnable r = () -> void

### 3.5.3 형식 추론

* 람다 표현식 추론
  * 람다 표현식이 사용된 대상 형식(Predicate, Consumer..)를 이용해서 람다 표현식과 관련된 함수형 인터페이스를 추론
    * 즉, 대상 형식으로 함수 디스크립터를 알 수 있으므로, 컴파일러는 **람다의 시그니처**도 추론 가능
    * Ex) filter(inventory, a -> "green".equals(a.getColor()));
      * 기존(**Apple a**)와 달리 형식을 추론하여 **a**로만 사용

### 3.5.4 지역 변수 사용

* 람다 표현식의 람다 캡쳐링
  * 외부에서 정의된 변수를 사용할 수 있음.
    * 이 때, 변수는 final로 선언되어 있건, final로 선언된 변수와 똑같이 사용되어야 함
    * ![KakaoTalk_20190922_180932805](https://user-images.githubusercontent.com/40616436/65386823-2d2ab400-dd7b-11e9-993d-a80e54aaf56e.jpg)
* 지역 변수의 제약
  * 인스턴스 변수는 힙, 지역 변수는 스택
  * 제약 이유
    * Ex) 현재 람다는 스레드에서 실행. -> 람다에서 지역 변수에 바로 접근한다 가정 -> 해당 지역 변수를 할당하는 스레드 종료 -> BUT 람다를 실행하는 스레드는 해당 지역 변수에 접근하려 할 수 있음.
  * 자바 구현에서는 본래 변수에 접근을 허용하지 않고, 자유 지역 변수의 복사본을 제공
  * 즉, 복사본의 값이 바뀌지 않아야 하므로, final 형식(한번만 값을 할당)을 가져야 한다는 제약이 생김 

## 3.6 메서드 레퍼런스

* 기존의 메서드 정의를 재활용해서 람다처럼 사용 가능

### 3.6.1 요약

* 메서드 레퍼런의 중요성
  * 메서드 레퍼런스는 특정 메서드만을 호출하는 람다의 축약형
  * 명시적으로 메서드 명을 참조하여 가독성을 높일 수 있음
  * 문법 파악
    * Apple::getWeight
      * Apple 클래스에 정의된 getWeight의 메서드 레퍼런스
      * 실제로 메서드를 호출하는 것이 아니므로 ()는 필요없음.
      * 결과적으로, **Apple::getWeight** == **(Apple a) -> a.getWeight()**
* 람다와 메서드 레퍼런스의 단축 표현 예제
  * ![KakaoTalk_20190922_183243335](https://user-images.githubusercontent.com/40616436/65386817-200dc500-dd7b-11e9-850e-f572d1b2ce97.jpg)
* 메서드 레퍼런스는 기능이 아닌 하나의 메서드를 참조하는 람다를 편리하게 표현할 수 있는 문법으로 간주
* 메서드 레퍼런스 만들기
  * Integer의 parseInt 메서드 => Integer::parseInt
  * String의 length 메서드 =>  String::length
  * Test t에 getValue 메서드 사용 => t::getValue
* 람다 표현식을 메서드 레퍼런스로 변환하기
* ![KakaoTalk_20190922_184959368](https://user-images.githubusercontent.com/40616436/65386813-18e6b700-dd7b-11e9-89ae-864c912a1535.jpg)

### 3.6.2 생성자 레퍼런스

* 인수가 없는 생성자
  * Supplier의 () -> Apple와 같은 시그너처
    * 생성자 레퍼런스
      * Supplier<Apple> c1 = Apple::new;
    * 람다 표현식
      * Supplier<Spple> c1 = () -> new Apple();
* 인수가 있는 생성자
  * Apple(Integer wieght)이 시그너처 == Function 인터페이스의 시그너처와 동일
    * 생성자 레퍼런스
      * Function<Integer, Apple> c2 = Apple::new;
    * 람다 표현식
      * Function<Integer, Apple> c2 = (weight) -> new Apple(weight);
* 생성자 레퍼런스를 사용시 중요 사항
  * 사용하려는 생성자 레퍼런스와 일치하는 시그너처를 갖는 함수형 인터페이스가 필요.
  * JAVA8에서 제공하는 함수형 인터페이스가 없을 시 직접 함수형 인터페스를 구현해야 함.
    * EX) 세개의 인수를 갖는 생성자 레퍼런스
      * ![KakaoTalk_20190922_192736532](https://user-images.githubusercontent.com/40616436/65386808-0b313180-dd7b-11e9-9ffe-82c6d8f0a9ec.jpg)



## 3.7 람다, 메서드 레퍼런스 활용하기

### inventory.sort(comparing(Apple::getWeight)); 를 만들자

---

### 3.7.1 1단계: 코드 전달

* JAVA8 LIST API에 sort 메서드를 활용
  * void sort(Comparator<? super E> c)의 시그너처를 갖음
    - 이 방식은 객체 안에 동작을 포함시키는 방식으로 다양한 전략을 전달할 수 있음
  * 1단계 코드
    * ![KakaoTalk_20190922_194142582](https://user-images.githubusercontent.com/40616436/65386803-010f3300-dd7b-11e9-8164-1473680a82ce.jpg)

### 3.7.2 2단계: 익명 클래스 사용

* Comparator를 한번만 사용하므로 익명 클래스 이용
  * ![KakaoTalk_20190922_194551156](https://user-images.githubusercontent.com/40616436/65386778-b1c90280-dd7a-11e9-9a00-93f5070f3229.jpg)

### 3.7.3 3단계: 람다 표현식 사용

* Comparator의 함수 시그너처
  * (T, T) -> int
* 람다 표현식 사용
  * inventory.sort((Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight()));
* 형식 추론 적용
  * inventory.sort((a1, a2) -> a1.getWeight().compareTo(a2.getWeight()));
* 가독성을 더욱 향상 시키자
  * Comparator는 정적 메서드인 Comparing(9장 설명)를 포함
  * Comparator<Apple> c = Comparator.comparing((Apple a) -> a.getWeight());
    * 사과를 비교하는데 사용할 키를 어떻게 추출할 것인지 지정하는 한 개의 인수만 포함
* 더욱 간소화 된 소스
  * import static java.util.Comparator.comparing;
  * inventory.sort(comparing((a) -> a.getWeight()));

### 3.7.4 4단계: 메서드 레퍼런스 사용

* inventory.sort(comparing(Apple::getWeight));
* 코드만 짧아진 것이 아니라 의미도 명확해짐!
  * Apple을 weight별로 비교 후 inventory에 정렬해라!



## 3.8 람다 표현식을 조합할 수 있는 유용한 메서드

* 람다 표현식의 조합
  * 함수형 인터페이스의 제공으로, 간단한 여러 개의 람다 표현식을 조합해서 복잡한 람다 표현식을 만들 수 있음.
  * 한 함수의 결과가 다른 함수의 입력이 되도록 두 함수 조합 가능
    * 함수형 인터페이스에서 추가로 메서드를 제공 => **디폴트 메서드 사용**
    * 추상 메서드가 아니므로 함수형 인터페이스 정의를 벗어나지 않는다.

### 3.8.1 Comparator 조합

* 역정렬
  * inventory.sort(comparing(Apple::getWeight).reversed());
    * 무게를 내림차순으로 정렬
* Comparator 연결
  * 정렬 우선 순위
    * 1순위 무게, 2순위 원산지
  * 두번째 비교자 메서드(thenComparing)
    * ![KakaoTalk_20190922_200603364](C:\Users\user\Documents\java8\ch3 이미지\KakaoTalk_20190922_200603364.jpg)
    * thenComparing은 comparing과 같이 함수를 인수로 받아 첫번재 비교자를 이용해 두 객체가 같다고 판단 시 두번째 비교자에게 객체를 전달

### 3.8.2 Predicate 조합

* Predicate<T> pre = (T t) -> boolean

* negate, and, or 메서드 제공
  * Predicate<Apple> redApple은 빨간색 사과로 정의 
  * negate(빨간색이 아닌 사과)
    * Predicate<Apple> notRedApple = redApple.negate();
  * and(빨간색이면서 무거운 사과)
    * Predicate<Apple> redAndHeavyApple = redApple.and((a) -> a.getWeight() > 150);
  * or(빨간색이면서 무거운 사과 또는 그냥 녹색 사과)
    * Preidcate<Apple> redAndHeavyAppleOrGreenApple = 
    * redApple.and((a) -> a.getWeight() > 150).or(a -> "green".equals(a.getColor()));

### 3.8.3 Function 조합

* Function<T, R> func = (T t) -> R r
* andThen, compose 메서드 제공
  * andThen(A.andThen(B))
    * A에서 B 순으로 실행
    * EX)
      * ![KakaoTalk_20190922_203855045](https://user-images.githubusercontent.com/40616436/65386763-93fb9d80-dd7a-11e9-9358-a5cb8c4adce0.jpg)
        * f 후 g
  * compose(A.compose(B))
    * B에서 A 순으로 실행
    * EX)
      * ![KakaoTalk_20190922_203923737](https://user-images.githubusercontent.com/40616436/65386759-880fdb80-dd7a-11e9-976b-adbfffc6d3a1.jpg)
        * g 후 f

## 3.9 수학을 좋아하지 않으므로 패스

