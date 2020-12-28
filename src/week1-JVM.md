## JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가.

### JVM이란 무엇인가

- JVM(Java Virtudla Machine)

    - 자바 소스코드를 컴파일하여 만들어진 바이트 코드 파일(.class) 실행해주는 가상 머신
    - 운영체제가 달라도 실행되는 내용은 같다.(운영체제에 종속적이지 않다.)
    - 운영체제 별로 JVM이 있고, 소스파일을 각 운영체제에 맞는 JVM에 올리면 같은 내용이 실행 되는 것이다.

---

### 컴파일하는 방법

javac 명령어로 .java 파일을 JVM에서 실행 가능한 바이트코드 파일(.class)로 변환한다.

ex) javac HelloWorld.java(동일 디렉토리에 HelloWorld.class파일이 생성됨)

---

### 실행하는 방법

java 명령어로 .class파일을 실행

ex) java HelloWorld

#### 실행되는 과정

1. 개발자가 .java 파일에 코딩

2. JDK의 javac.exe를 실행해 바이트코드 파일(.class) 생성

3. JRE의 JVM에 .class파일을 올려 실행(main 메소드가 있는 클래스를 찾아서 main 메소드 호출)

---

### 바이트코드란 무엇인가

- 개발자가 코딩한 소스를 컴퓨터(JVM)이 이해할 수 있는 언어로 번역된 코드

- java 컴파일러(javac)로 번역된다.

- 기계어는 아니지만 JVM이 손쉽게 기계어로 변환할 수 있는 코드

#### 자바 코드

    List<Integer> list = new ArrayList<>();
    
    list.add(1);
    list.add(2);

    for (Integer integer : list) {
        System.out.println("integer = " + integer);
    }

#### 바이트코드

  
  ```
  // ArrayList 인스턴스 생성
  L0
  LINENUMBER 8 L0
  NEW java/util/ArrayList
  DUP
  INVOKESPECIAL java/util/ArrayList.<init> ()V
  ASTORE 1

  // list에 초기화
  L1 
  LINENUMBER 10 L1
  ALOAD 1
  ICONST_1
  INVOKESTATIC java/lang/Integer.valueOf (I)Ljava/lang/Integer;
  INVOKEINTERFACE java/util/List.add (Ljava/lang/Object;)Z (itf)
  POP

  // 향상된 for문을 위한 iterator 생성
  L3
  LINENUMBER 13 L3
  ALOAD 1
  INVOKEINTERFACE java/util/List.iterator ()Ljava/util/Iterator; (itf)
  ASTORE 2
  
  // iterator 탐색
  L4
  FRAME APPEND [java/util/List java/util/Iterator]
  ALOAD 2
  INVOKEINTERFACE java/util/Iterator.hasNext ()Z (itf)
  IFEQ L5
  ALOAD 2
  INVOKEINTERFACE java/util/Iterator.next ()Ljava/lang/Object; (itf)
  CHECKCAST java/lang/Integer
  ASTORE 3
  
  // 탐색한 element 출력
  L6
  LINENUMBER 14 L6
  GETSTATIC java/lang/System.out : Ljava/io/PrintStream;
  ALOAD 3
  INVOKEDYNAMIC makeConcatWithConstants(Ljava/lang/Integer;)Ljava/lang/String; [
    // handle kind 0x6 : INVOKESTATIC
    java/lang/invoke/StringConcatFactory.makeConcatWithConstants(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/invoke/CallSite;
    // arguments:
    "integer = \u0001"
  ]
  INVOKEVIRTUAL java/io/PrintStream.println (Ljava/lang/String;)V
  ```
---

### JIT 컴파일러란 무엇이며 어떻게 동작하는지

#### JIT(Just In Time) 컴파일러

- 그 순간(Just In Time, 코드를 실행할 때) 컴파일 한다는 의미

- 자바 컴파일러가 변환한 바이트 코드를 기계어로 변환해주는 역할

- JRE 내부에 있다.

- 컴파일된 데이터를 캐싱하고 이후에 실행할 때는 수정된 부분만 컴파일한다.(성능 개선)

- 초기 JVM은 인터프리터(한줄씩 해석해고 실행)이었기 때문에 속도가 느렸다.

- 인터프리터를 실행하다 일정 기준이 넘어가면 JIT 컴파일러를 사용한다.

#### 두 번의 컴파일 과정이 나눠져 있는 이유

- 운영체제에 종속적이지 않도록 하기 위해

- 바이트 코드로 변환된 코드를 운영체제에 맞는 기계어로 번역만 되면 동일하게 실행된다.

- JVM을 만드는 사람만 운영체제 별 JVM을 만들고, 사용자는 사용만 하면 된다.

#### 동작 과정

1. Java 소스코드 작성

2. 소스코드를 java 컴파일러(javac. 바이트코드 컴파일러)로 바이트코드로 변환

3. 변환된 바이트 코드를 실행할 때(JVM에 올릴 때) JIT 컴파일러로 기계어로 번역해서 실행

---

### JVM 구성 요소

- Class Loader, Runtime Data Area, Execution Engine, Native Method Interface / Library

#### Class Loader

- 클래스 파일(.class)의 데이터를 Runtime Data Area에 적재(Load)하는 역할

- 아직 바이트코인 상태

##### 실행 과정

로딩 -> 링크 -> 초기화

###### 로딩

- 클래스로더가 바이트코드 파일을 읽고 Runtime Data Area에 Loading하는 과정

- 로딩되는 자원 / 영역은 Runtime Data Area에서 확인

###### 링크

- Verification, Preparation, Resolution의 3단계로 이루어져 있다.

Verification : .class파일의 형식이 유효한지 체크

Preparation : 클래스 변수와 기본 값에 필요한 메모리를 메서드 영역에 있는 실제 레퍼런스로 교체

Resolution : 심볼릭 메모리 레퍼런스를 메서드 영역에 있는 실제 레퍼런스로 교체

###### 초기화

- static 변수의 값을 할당

- 이 때 static 영역이 실행된다.

##### 클래스로더 종류

- 로딩 시점에 아래의 클래스로더가 순서대로 실행된다.

##### 부트스트랩 클래스로더

- JAVA_HOME/lib에 있는 코어 자바 API(rt.jar에 있다.)를 제공

##### 시스템 클래스로더

- JAVA_HOME/lib/ext에서 표준 확장을 로드

##### 확장 클래스 로더

- Application Classpath(-classpath 옵션에 해당하는 위치 또는 java.class.path 환경 변수에 해당하는 위치)에서 클래스를 로딩

#### Runtime Data Area

- 로딩된 클래스 데이터가 올라가는 메모리 영역

- method / heap / stack / PC Register / Native Method Stack 영역이 있다.

- 모든 스레드가 공유하는 영역 : method / heap

- 스레드가 개별적으로 가지는 영역(공유X) : stack / PC Register / Native Method Stack

##### method

- 클래스 멤버 변수 명, 데이터 타입, 리턴 타입, 상수, static 변수등이 저장

- 여기에 저장된 정보는 공유된다.

##### heap

- new 연산자로 생성한 인스턴스를 저장

##### stack

- 실행되는 메소드의 지역 변수를 저장

- 스레드 별로 개별적인 stack 영역이 생성된다.

##### PC Resister

- Program Counter(현재 스레드가 실행되는 부분의 주소와 명령)을 저장

- 여러 스레드를 돌아가면서 수행(멀티 스레딩)이 가능하도록 해준다.

- 스레드마다 개별적으로 생성되는 영역

##### Native Method Stack

- 자바 이외의 언어로 작성된 코드를 저장

#### Execution Engine

- 클래스로더에 의해 Runtime Data Area에 올라가 있는 바이트 코드를 기계어로 번역해 실행하는 역할

##### 구성

- JIT Compiler

- Garbage Collector

  - heap 영역에 생성된 인스턴스 중 참조하지 않는 것들을 메모리에서 제거하는 역할
  
  - 실행 시점 
  
    - 시스템이 알아서 실행해주는 시점은 알 수 없다.(참조가 없어지자 마자 제거되는 것을 보장하지 않음)
  
    - 사용자가 System.gc() 메소드를 강제로 호출하면 GC 스레드를 제외한 모든 스레드의 실행이 멈춘다.(사용하지 말 것)

---

### JDK와 JRE의 차이

JRE : JVM + Java API

JDK : 컴파일러 + JRE + 디버거 등의 개발 도구

#### 컴파일 / 실행 명령어

java : .class파일 실행. 지정한 클래스를 로드한 후 해당 클래스의 main 메소드를 호출

javaw : 실행할 때 콘솔창이 없다는 것만 제외하고 java와 같은 기능.

javap : 역어셈블러 (.class -> .java)

javadoc : 자동 문서 생성기. 소스파일에 추가되어 있는 주석을 이용해 API형식의 문서를 자동으로 생성해준다.(공식 reference 사이트와 같은 양식으로 만들어준다.)

---