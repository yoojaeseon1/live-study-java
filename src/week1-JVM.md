
참고 URL : https://d2.naver.com/helloworld/1230

## JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가.

### JVM이란 무엇인가

- JVM(Java Virtual Machine)

    - 자바 소스코드를 컴파일하여 만들어진 바이트 코드 파일(.class) 실행해주는 가상 머신
  
    - 자바 바이트코드가 JRE 위에서 동작하고, 바이코드를 해석하여 실행하는 역할을 JVM이 한다.
  
    - 바이트 코드 파일을 클래스 로더를 통해 읽어 자바 API와 함께 실행하는 역할
      
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

4. 클래스로더가 Runtime Data Area에 바이트코드 파일의 데이터를 각 영역에 맞게 올린다.

5. Runtime Data Area에 올라간 데이터를 실행 엔진이 JIT 컴파일러로 기계어로 번역해 실행한다.



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

- 아래의 결과물은 자바 어셈블리다.

- 진짜 .class파일의 바이트 코드는 사람이 이해하기 쉽지 않다.(기계어 보다는 이해하기 쉽지만 그래도 어렵다.)
  
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

### Class Loader

- 클래스 파일(.class)의 데이터를 Runtime Data Area에 적재(Load)하는 역할

- 아직 바이트코인 상태

#### 종류

- 상위 -> 하위 클래스로더 순서

##### 부트스트랩 클래스로더

- JAVA_HOME/lib에 있는 코어 자바 API(rt.jar에 있다.)클래스를 로드

- Native C로 구현되어 있다.

##### 확장 클래스 로더

- JAVA_HOME/lib/ext에서 클래스를 로드

- java.ext.dirs 환경변수로 지정된 폴더에 있는 클래스를 로드

- 다양한 보안 확장 기능을 로드한다.

- java로 구현되어 있다.

- sun.misc.Launcher 클래스 안에 static 클래스로 구현되어 있다.

- URLClassLoader를 상속받는다.

##### 시스템 클래스로더

- Classpath(-classpath 옵션에 해당하는 위치 또는 java.class.path 환경 변수에 해당하는 위치)에서 클래스를 로딩

- java로 구현되어 있다.

- sun.misc.Launcher 클래스 안에 static 클래스로 구현되어 있다.

- URLClassLoader를 상속받는다.

##### 사용자 정의 클래스로더

- 사용자가 직접 정의한 클래스를 로드한다.


#### 실행 과정

로딩 -> 링크 -> 초기화 -> 실행

##### 로딩

- 클래스로더가 바이트코드 파일을 읽고 Runtime Data Area에 Loading하는 과정

- 로딩 과정은 밑에서 확인

- 로딩되는 자원 / 영역은 Runtime Data Area에서 확인

#### 원칙

##### Delegation Principle(위임 원칙)

- 클래스를 로딩할 때 하위 클래스로더부터 상위 클래스로더 까지 로딩을 위임하는 것이다.

##### 로딩 과정

1. main 메소드가 있는 클래스에서 어플래케이션 클래스로더로 특정 클래스 로딩 요청

2. 어플리케이션 클래스로더는 직접 로딩하지 않고 로딩을 시스템 클래스로더로 위임

3. 시스템 클래스로더는 직접 로딩하지 않고 로딩을 부트스트랩 클래스로더로 위임

4. 부트스트랩 클래스로더의 rt.jar에 해당 클래스가 있으면 반환 -> 없으면 5번

5. 시스템 클래스로더에 JAVA_HOME/lib/ext 또는 java.ext.dirs환경 변수로 지정된 폴더에 해당 클래스가 있으면 반환 -> 없으면 6번

6. 어플리케이션 클래스로더에 클래스 패스에 해당 클래스가 있으면 반환 -> 없으면 ClassNotFoundException 발생

- 이런 방식을 동작하는 이유는 Visibility Principle(가시범위 원칙)과 관련이 있다.

##### Visibility Principle(가시범위 원칙)

- 하위 클래스로더는 상위 클래스로더가 로딩한 클래스를 볼 수 있지만 상위 클래스로더는 하위 클래스 로더가 로딩한 클래스를 볼 수 없다.

- ex) 어플리케이션 클래스로더가 부트스트랩 클래스로더로 부터 로딩된 String.class를 볼 수 없다면 String.class를 사용할 수 없을 것이다.

##### Uniqueness Principle(유일성 원칙)

- 하위 클래스로더는 상위 클래스로더가 로딩한 클래스를 다시 로딩하지 않게 해서 로딩된 클래스의 유일성을 보장한다.

- binary name으로 유일성을 식별한다.

##### 링크

- Verification, Preparation, Resolution의 3단계로 이루어져 있다.

- Verification(확인) : 로딩된 클래스가 자바 언어 명세(Java Language Specification) 및 JVM명세에 명시된대로 잘 구성되어 있는지 확인

- Preparation(준비)

  - 클래스가 필요로 하는 메모리를 할당

- Resolution(분석)

  - 인스턴스들의 실제 주소 값에 대한 심볼릭 링크를 설정

  - 심볼릭 메모리 레퍼런스를 메서드 영역에 있는 실제 레퍼런스로 교체

##### 초기화

- 클래스 변수들을 초기화

- static 필드들을 설정된 값으로 할당

---

### JVM 구성 요소

- Runtime Data Area, Execution Engine, Native Method Interface / Library


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

---

#### Execution Engine

- 클래스로더에 의해 Runtime Data Area에 올라가 있는 바이트 코드를 기계어로 번역해 실행하는 역할

##### 구성

- JIT

- GC

###### JIT(Just In Time) 컴파일러

- 그 순간(Just In Time, 코드를 실행할 때) 컴파일 한다는 의미

- 자바 컴파일러가 변환한 바이트 코드를 기계어로 변환해주는 역할

- JRE 내부에 있다.

- 컴파일된 데이터를 캐싱하고 이후에 실행할 때는 수정된 부분만 컴파일한다.(성능 개선)

- 좀 더 확인 필요

  - 초기 JVM은 인터프리터(바이트 코드 명령어를 하나씩 읽어서 해석하고 실행)만 실행했었기 때문에 속도가 느렸다.

###### 실행 방식

- 인터프리터 방식으로 실행하다가 적절한 시점에 바이트 코드 전체를 컴파일(하여 네이티브 코드로 변경)

- 이후에는 해당 메서드를 더이상 인터프리팅하지않고 (네이티브 코드로) 컴파일된 코드를 직접실행하는 방식

인터프리터를 실행하다 일정 기준이 넘어가면 JIT 컴파일러를 사용한다.


##### 두 번의 컴파일 과정이 나눠져 있는 이유

- 운영체제에 종속적이지 않도록 하기 위해

- 바이트 코드로 변환된 코드를 운영체제에 맞는 기계어로 번역만 되면 동일하게 실행된다.

- JVM을 만드는 사람만 운영체제 별 JVM을 만들고, 사용자는 사용만 하면 된다.

##### 동작 과정

1. Java 소스코드 작성

2. 소스코드를 java 컴파일러(javac. 바이트코드 컴파일러)로 바이트코드로 변환

3. 변환된 바이트 코드를 실행할 때(JVM에 올릴 때) JIT 컴파일러로 기계어로 번역해서 실행

- 인터프리터

- Garbage Collector

  - heap 영역에 생성된 인스턴스 중 참조하지 않는 것들을 메모리에서 제거하는 역할
  
  - 실행되면 GC 스레드를 제외한 모든 스레드의 실행이 멈춘다.
  
  - 실행 시점 
  
    - 시스템이 알아서 실행해주는 시점은 알 수 없다.(참조가 없어지자 마자 제거되는 것을 보장하지 않음)
  
    - 사용자가 System.gc() 메소드를 강제로 호출(사용하지 말 것)
  

---
#### JNI(Java Native Interface)

- 각 운영체제 별로 있는 JVM에서 운영체제의 모든 기능을 담지 못한다.

- 특정 운영체제(Native)의 기능을 사용하고 싶을 때 해당 운영체제가 구현된 언어(C, C++)를 자바 메소드와 연결해준다.

- 자바 메소드를 호출하면 C, C++로 작성된 함수가 실행된다.

- 자바 메소드와 연결해주고 실행해주는 역할

---

### JDK와 JRE의 차이

JRE : JVM + Java API

JDK : JRE + 컴파일러 + 디버거 등의 개발 도구

#### 컴파일 / 실행

- 프로그램을 실행해 컴파일 / 컴파일된 파일을 실행한다.(javac.exe, java.exe 등을 실행)

- javac

  - .java 파일(java 코드파일)을 컴파일. 바이트 코드 파일(.class파일)을 생성한다.
  
  - 동시에 여러 java파일을 컴파일 할 수 있다.

  `javac TreeNode.java RemoteNode.java`
  
  - 직접 정의한 클래스를 사용하면 같은 패키지에 파일이 있어야 한다.
  
  - 같은 패키지에 있으면 사용하는 class가 있는 java파일을 컴파일 하면 해당 class가 정의되어 있는 java파일까지 같이 컴파일 된다.

- java : .class파일 실행. 지정한 클래스를 로드한 후 해당 클래스의 main 메소드를 호출

  - 여러 개의 .class 파일을 동시에 실행해도 맨 처음 입력한 파일의 main 메서드가 실행된다.(첫 파일 뒤에 입력된 내용은 무시한다.)
  
  - 첫 번째 입력한 파일에 main 메서드가 없으면 아래의 에러를 호출한다.
  
```
Error: Main method not found in class RemoteNode, please define the main method as:
   public static void main(String[] args)
```

  - 직접 정의한 클래스의 바이트코드 파일이 같은 패키지(디렉토리)에 없으면 ClassNotFoundException이 발생한다.

```
Exception in thread "main" java.lang.NoClassDefFoundError: TreeNode
        at RemoteNode.solution(RemoteNode.java:19)
        at RemoteNode.main(RemoteNode.java:13)
Caused by: java.lang.ClassNotFoundException: TreeNode
        at java.base/jdk.internal.loader.BuiltinClassLoader.loadClass(BuiltinClassLoader.java:581)
        at java.base/jdk.internal.loader.ClassLoaders$AppClassLoader.loadClass(ClassLoaders.java:178)
        at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:521)
        ... 2 more
```

  - 콘솔 창이 종료되면 프로그램이 종료된다.

- javaw
  
  - 실행될 때 콘솔창 실행 여부와 무관하게 실행된다는 것을 제외하면 java와 같은 기능.

- javap : 역어셈블러

  - 어떤 java 파일로부터 생성되었고 어떠한 필드, 메소드를 갖고있는 파일인지 알려준다.
  
  - 옵션없이 실행
  
```
C:\Users\yoo-pc\Desktop\javaTest>javap RemoteNode
Compiled from "RemoteNode.java"
public class RemoteNode {
  public RemoteNode();
  public static void main(java.lang.String[]);
  public int solution(int, int[][]);
}
```

  - 옵션
    
    - -c : 바이트코드를 읽을 수 있도록 자바 어셈블리로 변역해준다.(바이트 코드는 바이너이 파일이기 때문에 사람이 이해하기 쉽지 않다.)
  
      - 면접에서는 향상된 for문 확인한 내용 설명하기

- javadoc : 자동 문서 생성기. 소스파일에 추가되어 있는 주석을 이용해 API형식의 HTML 파일을 생성해준다.(공식 reference 사이트와 같은 양식으로 만들어준다.)

  - .java파일로 만들어야 한다.
  
  - 주석용 에너테이션이 없으면 만들 수 없다.(클래스 / 메소드 주석 중 하나만 있으면 만들 수 있다.)
  
    - 주석용 에너테이션
      ```
       /**
      * @author yoo
      * @version 1.0
      * @since 2021-04-09
        */
      public class RemoteNode {
      
      
      ...
      
      /**        
      *
      * @param n the number of node
      * @param edge connection state of node
      * @return distance count
        */

    public int solution(int n, int[][] edge) {
      }
  
      ```

---