### 인터페이스 vs 추상클래스

#### 인터페이스

- 어떤 서비스를 제공할 것인지에 대한 명세
  - 제공할 서비스의 종류를 의미
  - 어떻게 제공할 것인지와는 무관하다.
- java 8부터 default 메소드 / static 메소드가 추가되었다.
- Java 8에서 default 메소드가 추가되면서 추상클래스의 많은 부분을 인터페이스에서 가능한 것은 사실
- 상수만 만들 수 있다.
- 메소드는 무조건 public abstract으로 선언. 생략 가능
- 변수는 무조건 public static final으로 선언. 생략 가능

##### 강한 결합 vs 느슨한 결합

###### 강한 결합

- 의존하는 대상을 바꾸기 위서는 정의된 메소드를 변경해야 한다.

```java
class A {
    public void methodA(B b) { // B를 사용!!(따라서 B와 관계 있음)
        b.methodB();
    }
}

class B {
    public void methodB() {
        System.out.println("methodB()");
    }
}

class InterfaceTest {
    public static void main(String args[]) {
        A a = new A();
        a.methodA(new B());
    }
}

```

###### 느슨한 결합

- 의존하는 대상을 바꾸기 위해서 메소드의 변경 없이 대상만 변경해주면 된다.

```java
class A {
    public void methodA(I i) {// I를 사용! (따라서 A는 B클래스와 관계 없음.I 인터페이스랑만 관계 있음)
        i.methodB();
    }
}

// 껍데기
interface I {
    public abstract void methodB();
}

// 알맹이
class B implements I {
    public void methodB() {
        System.out.println("methodB()");
    }
}

// 나중에 B를 C로 변경하여도 C만 변경하면 됨. methodB를 호출하는 A를 변경할 필요 없음
class c implements I {
    public void methodB() {
        System.out.println("methodB() in C");
    }
}
```

- 인터페이스는 느슨한 결합으로 의존 대상을 유연하게 바꿀 수 있도록 해준다.

---

##### 인터페이스 레퍼런스

- 하나의 클래스에서 두 개 이상의 인터페이스를 구현 했다면, 해당 인터페이스 타입의 레퍼런스로 만들어 사용할 수 있다.
  - 구현한 모든 인터페이스의 추상메소드를 구현해야 한다.
  - 레퍼런스로 사용한 인터페이스를 통해 구현한 메소드만 호출 할 수 있다.
    - 해당 클래스에서 정의된 메소드 + 다른 인터페이스를 통해 구현한 메소드는 호출할 수 없다.

```java
Map<String, Integer> nameToCount = new TreeMap<>();
TreeMap<String, Integer> nameToCount2 = new TreeMap<>();

String firstKey = nameToCount2.firstKey(); // TreeMap 클래스에 정의된 메소드 nameToCount에서는 호출할 수 없다.
```



##### default 메소드

- 인터페이스의 구현 메소드
- 접근제어자는 public
- 구현한 클래스에서 그대로 사용하거나 오버라이딩해서 사용할 수 있다.
  - 인터페이스의 명세를 확장하면서 구현되어 있는 클래스와의 하위 호환성 유지
  - 인터페이스를 구현한 클래스가 많이 있을 경우 인터페이스의 명세를 추가하면 전부 구현해야하는 번거로움이 있었다.(ex) 많은 사람들이 사용하고 있는 오픈 소스코드)
  - default 메소드를 추가하면 구현한 클래스는 수정이 필요없다. 
  - 필요하면 그대로 호출하고
  - 구현부를 수정하고 싶으면 오버라이딩 하면 된다.
- 인터페이스를 상속받은 새로운 인터페이스에서 default 메소드를 다시 추상 메소드로 만들 수 있다.

```java
public interface Machine {

    int age = 5;

    int moveOne();

    default int moveTwo(){
        return age+2;
    }
    
    static int moveThree(){
        return age+3;
    }
}
```

- 클래스에서 두개 이상의 인터페이스를 구현하고, 인터페이스들에 있는 default메소드의 시그니처가 같은 경우
  - 클래스에서 오버라이딩해서 사용해야 한다.
  - 하지 않으면 컴파일 에러가 발생한다.

##### static 메소드

- 메소드 앞에 static을 붙여 구현 메소드를 만들 수 있다.
- 접근제어자는 public 으로 생략할 수 있다.
- 구현한 클래스 / 상속 받은 인터페이스에서는 오버라이딩이 불가능하다.(없는 메소드로 취급)
  - 클래스에서 호출할 수 없고, 인터페이스에 상속되지 않는다.
- 호출시 `인터페이스.메소드명` 으로 직접 호출 해야한다.

##### private / static private method

- java 9에서 추가되었다.
- private : default 메소드의  private 버전
- static private : static 메소드의 private 버전
- default / static 메소드는 접근 제어자가 public이기 때문에 내부에서만 사용할 목적으로 만들었어도 외부에서 호출이 가능했다.
- 코드의 중복을 피하고 인터페이스의 캡슐화를 유지할 수 있게 해준다.

```java
public interface Car {
    
    void carMethod();

    default void defaultCarMethod() {
        System.out.println("Default Car Method");
        privateCarMethod();
        privateStaticCarMethod();
    }

    private void privateCarMethod() {
        System.out.println("private car method");
    }

    private static void privateStaticCarMethod() {
        System.out.println("private static car method");
    }
}

```

```java
public class ImplementedCar implements Car{

    @Override
    public void carMethod() {
        System.out.println("execute carMethod");
    }
    
}
```

---

#### 추상 클래스

- 변수 필드를 가질 수 있다.

  - 상수도 만들 수 있다.

  - 필드의 상태에 따라 실행 내용이 달라지는 구현 메소드는 추상클래스에만 만들 수 있다.

  - private 필드를 선언할 수 있다.

    

