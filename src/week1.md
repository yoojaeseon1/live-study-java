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

---

### JIT 컴파일러란 무엇이며 어떻게 동작하는지

---

### JVM 구성 요소

---

### JDK와 JRE의 차이

JRE : JVM + Java API(기본 제공 라이브러리(ex) rt.jar))

JDK : 컴파일러 + JRE + 디버거 등의 개발 도구

java : .class파일 실행. 지정한 클래스를 로드한 후 해당 클래스의 main 메소드를 호출

javaw : 실행할 때 콘솔창이 없다는 것만 제외하고 java와 같은 기능.

javap : 역어셈블러 (.class -> .java)

javadoc : 자동 문서 생성기. 소스파일에 추가되어 있는 주석을 이용해 API형식의 문서를 자동으로 생성해준다.(공식 reference 사이트와 같은 양식으로 만들어준다.)



---