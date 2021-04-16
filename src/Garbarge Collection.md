- JVM의 Runtime Data Area의 Heap 메모리 영역을 관리하는 프로그램

##### 용어

- stop-the-world
  - Major GC가 실행될 때 JVM이 해당 스레드를 제외한 모든 스레드를 멈추는 것
  - 어떤 GC 알고리즘을 사용해도 발생한다.
  - GC 튜닝은 stop-the-world 시간을 줄이는 것을 말한다.

---

##### Heap 영역

- Young Generation

  - Eden
  - Survivor1 / Survivor2

- Old Generation

- 주의 사항

  - Permanent(뜻 : 영구적인) 영역이 java 8부터 삭제되었다.(원래 Heap 영역은 아니었다.)
  - Permanent는 고정된 메모리 영역을 가졌었다.(메모리 초과 에러 자주 발생)
  - Runtime Data Area의 method area를 permGen(Permanent Generation)라고도 부른다.
    - 논리적으로는 heap의 일부이기 때문에 GC의 대상이 아니지만 JVM만든 회사에 따라서 컬렉션 여부를 결정할 수 있다.
  - Metaspace이 생겼다.
    - Heap 영역이 아니다.
    - PermGen의 고정된 메모리 제한을 없애기 위한 것이 목적
    - JVM의 메소드 영역이 Metaspace 영역이 된 것이다.



---

##### Eden

- 인스턴스가 최초 생성되었을 때 오게되는 영역

##### Survivor

- Eden 영역에 있던 인스턴스가 GC가 실행됐을 때 살아남으면 이동하게 되는 영역
- 이동하여 1~2 또는 2~1 영역을 이동하며 계속해서 참조되는지 체크한다.

##### Old

- Survivor 영역에서 참조되고 있던 인스턴스가 일정 시간 이상 참조되면 이동한다.
- 이동하게 되고 여기서 장기간 참조되지 않은 인스턴스가 정리된다.
  - 정리되는 알고리즘은 내부적으로 정해져 있다.

###### metaspace

- 객체나 억류된 문자열을 저장

###### 정리

- young generation에서 오래 살아남은 인스턴스가 old 영역으로 넘어온다.

---

##### 실행되는 영역에 따른 분류

###### Minor GC

- young(Eden, Survivor) 영역에서 실행되는 GC
- 메모리 정리되는 횟수(age bit)에 따라 계산된 대로 이동한다.
  - Eden 영역이 꽉 차면 GC가 실행되고 살아남은 인스턴스 Survivor로 이동
  - Survivor로 이동한 후 살아남으면 1->2, 2->1로 이동(여러 번 이동 age 값에 따라 old로 이동)
  - 대부분의 인스턴스는 Minor GC로 정리된다.

###### Major GC

- Old에 있는 인스턴스에 대해 발생하는 GC
- 시간이 오래 걸리고 실행 중엔 프로세스가 정지된다.
- GC를 실행하는 스레드를 제외한 나머지 스레드는 모두 작업을 멈춘다.

###### Full GC

- Minor, Major의 전체 영역 + Permanent 영역에 대한 GC



---

##### Old 영역의 GC

###### Serial GC

- 싱글 스레드로 작동한다.
- 실무에서는 사용하지 않는다.(성능 너무 떨어짐)

###### Parallel GC

- 멀티 스레드로 작동한다.

###### Parallel Old GC

###### CMS(Concurrent Mark & Sweep) GC

###### G1 GC

- java 7 버전부터 도입
- java 9 버전부터 default



---

##### 소멸 대상 선정 방법

- Reachable / Unreachable
  - Reachable : Root Set에 있는 인스턴스를 직 / 간접적으로 참조하는 것
  - Unreachable : Root Set에 있는 인스턴스를 참조하지 않는 것(삭제)

##### Heap 영역에 있는 객체들이 참조하는 경우

1. 힙 내의 다른 인스턴스를 참조
2. 메서드 실행시 Stack 영역에 올라가는 지역 변수와 파라미터를 참조
3. JNI(Java Native Interface)에 의해 생성된 객체를 참조
4. 메서드 영역의 정적 변수에 의한 참조

2,3,4가 Root Set이다.

##### 실행되는 시점

- Minor GC
  - Eden 영역이 꽉 찼을 때
- Major GC
  - Minor GC가 실패했을 때
- Full GC
  - Minor 또는 Major GC가 실패했을 때

---

##### 주의 사항

- System.gc() 호출
  - 개발자가 직접 GC를 호출할 수도 있다.
  - 하지만 시스템 성능에 큰 영향을 끼치므로 절대 하면 안된다.





