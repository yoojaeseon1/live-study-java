### 정의

- 함수형 인터페이스에 있는 추상 메소드를 구현하는 식

- 메소드의 파라미터가 함수형 인터페이스일 경우 파라미터 자리에 사용할 수 있다.

- 기본식

	(파라미터) -> {구현부}

	- 파라미터 이름은 임의로 지어도 된다.(개수만 일치하면 됨)

ex)

    List<Integer> list = new ArrayList<>();

    Collections.sort(list, (o1,o2) -> Integer.compare(o1,o2));

	// 위의 람다 표현식을 풀어쓰면 다음과 같다.
	   Collections.sort(list, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return Integer.compare(o1,o2);
            }
        });

- Collections.sort의 두 번째 파라미터가 함수형 인터페이스인 Comparator다.

- Comparator 인터페이스에 있는 유일한 추상메소드인 compare를 구현하는 것이다.

- comparator의 제네릭 타입을 보고 o1, o2의 타입을 추론한다.

### 메소드 참조 (:: 표현식)

- 이미 만들어져 있는 메소드가 함수형 인터페이스의 추상메소드와 파라미터 개수 / 타입, 리턴타입이 같으면 사용할 수 있다.

#### 클래스::인스턴스 메소드

첫 번째 파라미터가 메소드의 수신자(호출하는 클래스 or 인스턴스), 나머지가 메소드의 파라미터

ex) 

	(x,y) -> x.compareToIgnoreCase(y) = String::compareToIgnoreCase


#### 클래스::static 메소드

- 파라미터가 전부 static 메소드의 파라미터

ex) 

	x -> Objects.isNull(x) = Objects::isNull

	(x,y) -> Integer.compare(x,y) = Integer::compare // 파라미터가 순서대로 들어가기 때문에 오름차순으로만 가능하고, 내림차순으로 할 경우 메소드 참조를 사용하면 안된다. 

#### 인스턴스::인스턴스 메소드

- 파라미터가 전부 인스턴스 메소드의 파라미터

ex) 

	x -> System.out.println(x) = System.out::println

- out은 System클래스에 있는 PrintStream 타입의 인스턴스다.(public static final PrintStream out = null;)

x의 타입은 컴파일러가 문맥을 통해 확인한다.

ex) 

	List<String> list = new ArrayList<>();

	list.forEach(System.out::println); // x -> System.out.println(x)


x의 타입을 list의 제네릭을 보고 String임을 판단한다.

