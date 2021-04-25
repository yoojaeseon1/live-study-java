### 향상된 for문 (for each문)

- 배열 또는 iterable 인터페이스를 구현한 클래스에서 사용할 수 있다.
- Collection 인터페이스가 iterable을 상속하고 Collection의 클래스들이 구현하기 때문에 iterator와 향상된 for문을 사용할 수 있다.

#### 의문점

- 배열도 향상된 for문을 사용할 수 있는데 인스턴스지만 정의된 클래스가 없다.
- 바이트코드를 확인해봤지만 그냥 for문과 for each문의 동작 방식이 같다.

