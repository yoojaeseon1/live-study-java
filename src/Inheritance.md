참고 사이트 : https://yadon079.github.io/2020/java%20study%20halle/week-06#%EB%8B%A4%EC%9D%B4%EB%82%98%EB%AF%B9-%EB%A9%94%EC%84%9C%EB%93%9C-%EB%94%94%EC%8A%A4%ED%8C%A8%EC%B9%98-dynamic-method-dispatch

## 상속

### 자바 상속의 특징

- 단일 상속
- Object 클래스를 제외한 모든 클래스는 암묵적으로 Object의 자손 클래스
- Multi-Level Inheritance
  - 단일 상속이 여러 단계에 걸쳐 일어날 수 있다.
  - 상속의 상속의 상속의...

---

### 상속이란?

- 기존의 클래스를 `재사용`하여 새로운 클래스를 작성하는 것
- 상속을 통해 클래스를 작성하면 적은 양의 코드로 새로운 클래스를 작성할 수 있다.
- 코드를 공통적으로 관리할 수 있기 때문에 코드의 추가 / 수정이 용이하다.

```java
public class Child extends Parent{
	// ...
}
```

- 부모 클래스 : 조상 클래스, 상위(super) 클래스, 기반(base) 클래스
- 자식(child) 클래스 : 자손 클래스, 하위(sub) 클래스, 파생된(derived) 클래스
  - 부모 클래스의 모든 멤버를 상속 받는다.
    - 자식 클래스의 멤버 수 >= 부모 클래스의 멤버 수
  - 부모 클래스에 멤버 변수가 추가되면 자식 클래스에도 자동적으로 추가된다.
  - 자식 클래스에 추가하는 것은 부모 클래스에 영향이 가지 않는다.

---

### 하나의 조상, 다수의 자손

```java
class Parent {}
class Child1 extends Parant {}
class Child2 extends Parant {}
```

- Child1과 Child2는 Parent를 상속 받고 있다.
- 하지만 자식 클래스 간에는 아무런 관계도 성립하지 않는다.
- 자바에 형제 관계는 없다.

```java
class Parent { }
class Child1 extends Parent { }
class GrandChild extends Child1 { }
```

- 자손 클래스(GrandChild)는 조상 클래스(Child1, Parent)의 모든 멤버 변수를 물려 받는다.
- Parent와는 간접적인 상속관계를 가지가 된다.

---

### 클래스 관의 관계 - 포함

- 상속 이외의 클래스를 재사용하는 방법
- 클래스 간의 포함(Composite)관계를 맺어주면 된다.
- 한 클래스의 멤버 변수로 다른 클래스 타입의 참조 변수를 선언하면 된다.

```java
class Circle {
    private int x;      // 원점 x 좌표
    private int y;      // 원점 y 좌표
    private int r;      // 반지름
}

class Point {
    private int x;      // x 좌표
    private int y;      // y 좌표
}

// 대신
class Circle {
    private Point point;
    private int r;      // 반지름
}

```

---

### 단일 상속(Single Inheritance)

- 자바에서는 단일 상속만을 허용한다.(다른 객체지향 언어인 C++에서는 허용한다.)
- 이유
  - 서로 다른 클래스로부터 상속받은 멤버의 이름이 중복되면 구별 할 수 있는 방법이 없다.
  - 클래스 간의 관계가 복잡해진다.

---

### super 키워드

- 부모 클래스로부터 상속 받은 필드 / 메소드를 참조하는데 사용되는 참조 변수다.
- 부모 클래스의 필드명와 자식 클래스의 필드명이 중복 될 때 구분 하는 용도로 사용한다.
  - 이 점을 제외하고는 this와 super는 같다.
- 모든 인스턴스 메소드에는 자신이 속한 인스턴스의 주소가 지역변수로 저장된다.
  - 이 것이 this와 super의 값이 된다.(this, super = 주솟 값)
- static 메서드(클래스 메서드)는 인스턴스(객체)와 관련이 없다.
  - 때문에 this와 마찬가지로 super도 static 메서드에서는 사용할 수 없다.
  - 인스턴스 메서드에서만 super를 사용할 수 있다.

```java
public class App {

    public static void main(String[] args) {

        Child child = new Child();
        child.method();

    }

}

class Parent {
    private int x = 10;

    public int getX() {
        return x;
    }

    public void setX(int x) {
        this.x = x;
    }
}

class Child extends Parent {
    private int x = 20;
    
    @Override
    public int getX() {
        return x;
    }

    @Override
    public void setX(int x) {
        this.x = x;
    }

    public void method() {

        System.out.println("x = " + getX());
        System.out.println("this.x = " + this.getX());
        System.out.println("super.x = " + super.getX());
    }
}
```

출력

```
x = 20
this.x = 20
super.x = 10
```

- 메소드를 오버라이딩 하지 않았다면 this.method() / super.method() / method() 셋 다 부모 클래스의 메소드를 호출한다.
- getter / setter를 오버라이딩 하지 않으면 부모의 필드를 get / set 한다. 
  - 부모의 필드를 초기화 / 리턴하는 부모의 메소드는 부모의 필드 값을 초기화 / 리턴한다.(같은 이름의 필드가 자식에 있어도)

---

- 메소드도 super를 써서 호출할 수 있다.
  - 조상 클래스의 메소드를 오버라이딩한 이후 조상 클래스의 메소드를 호출할 경우에 사용

```java
class Point {
    private int x;
    private int y;

    public String getLocation() {
        return "x : " + x + ", y : " + y;
    }
}

class Point3D extends Point {
    private int z;

    public String getLocation() {
        return super.getLocation() + ", z : " + z;
    }
}
```

---

### Super()

- 조상 클래스의 생성자
- this()가 생성자 인 것과 같은 원리다.
- 조상 클래스의 필드를 먼저 초기화 해줘야 한다.
  - 자식 클래스의 생성자는 본인의 필드만 초기화 한다.
  - 상속 받았다면 부모의 필드도 초기화 할 생성자가 있어야되는게 당연하다.
  - 자식 클래스의 필드 값이 부모 클래스의 필드 값을 사용할 수도 있기 때문에 먼저 부모 클래스의 필드를 초기화되어 있어야 한다.
  - 그래서 자식클래스 생성자 위에 부모 클래스의 생성자가 있어야 하는 것이다.
  - 부모 클래스 생성자의 호출은 상속관계를 거슬러 올라가 최고 조상인 Object 클래스의 생성자인 Object()까지 가서 끝난다.
  - 그래서 Object 클래스를 제외한 모든 클래스의 생성자는 첫 줄에 반드시 (super생성자를 포함하는)자신의 다른 생성자, super생성자를 호출해야 한다.
  - 그렇지 않으면 컴파일러가 `super();`를 자동적으로 추가해준다.
  - 결론 : 자식 클래스에는 부모 클래스의 모든 필드를 생성자(super)가 반드시 있어야 한다.

```java
public class App {

    public static void main(String[] args) {

        Point3D point3D = new Point3D();

        System.out.println("point3D.getX() = " + point3D.getX());
        System.out.println("point3D.getY() = " + point3D.getY());
        System.out.println("point3D.getZ() = " + point3D.getZ());


    }

}


class Point {
    private int x;
    private int y;

    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int getX() {
        return x;
    }

    public int getY() {
        return y;
    }
}

class Point3D extends Point {

    private int z;

    Point3D() {
        this(100, 200, 300);
    }

    public Point3D(int x, int y, int z) {
        super(x, y);
        this.z = z;
    }

    public int getZ() {
        return z;
    }
}
```

---

### 메소드 오버라이딩

- 조상 클래스(부모의 슈퍼 클래스에서도 가능)의 메소드를 자식 클래스에서 재 정의(Overriding)하는 것

- 조건

  - 조상 클래스의 메소드와 선언부가 일치해야 한다.
    - 리턴 타입
    - 메소드 명
    - 파라미터 타입 리스트(개수, 순서)

- 리턴 타입의 경우 Java 5 부터 공변 반환 타입(Convariant return type)이 추가되어, 반환타입을 자손 클래스의 타입으로 변경하는 것이 가능하다.

  - 리턴 타입이 기본형(Primitive) 타입이 아닌 경우에만 가능하다.

  ```java
  class Point {
      private int x;
      private int y;
  
  
      Point(int x, int y) {
          this.x = x;
          this.y = y;
      }
  
  
      public Point printTest(int a, int b) {
          System.out.println(a+b);
          return new Point(2,3);
      }
  
  }
  
  class Point3D extends Point {
  
      private int z;
  
      Point3D() {
          this(100, 200, 300);
      }
  
      public Point3D(int x, int y, int z) {
          super(x, y);
          this.z = z;
      }
  
      @Override
      public Point3D printTest(int a, int b) {
          System.out.println(a+b);
          return new Point3D();
      }
  }
  ```

  - 부모 클래스의 메소드는 리턴 타입 : Point
  - 오버라이딩한 자식 클래스의 메소드의 리턴 타입 : Point3D(부모 리턴 타입의 서브 클래스)


---

#### 접근 제어자

- 조상 클래스의 메소드보다 좁은 범위로 변경할 수 없다.
  
  - 넓은것 -> 좁은것 순서
    
    public -> protected -> default(인터페이스의 default메소드, 클래스에선 사용 불가) -> private
    
  - 조상 클래스 메소드의 접근제어자가 `protected`라면
  
  - 자손 클래스 메소드의 접근 제어자는 `protected` 또는 `public`이어야 한다.
  
  - 대부분 같은 범위의 접근 제어자를 사용한다.

---

#### 예외(Exception)

- 조상 클래스의 메소드보다 조상 타입의 Exception을 선언할 수 없다.
- 아래의 경우에는 자손에서 더 많은 Exception을 선언 했지만를 조상 Exception을 선언하지는 않았기 때문에 가능하다.

```
java

class Parent {
    void parentMethod() throws IOException, SQLException {
        ...
    }
}

class Child extends Parent {
    void parentMethod() throws IOException, SQLException, RuntimeException {
        ...
    }
}
```

- 단순히 선언된 Exception의 개수가 문제가 아니다.

```
java

class Child extends Parent {
    void parentMethod() throws Exception {
        ...
    }
}
```

- Exception은 최 상위 Exception이기 때문에 불가능하다.(오버라이딩 되지 않는다.)

---

#### static 메소드의 오버라이딩

- 조상 클래스의 static 메소드를 자손 클래스에서 오버라이딩 할 수 있다.
  - 가능한 이유는 static 메소드는 상속되지 않으므로 자손에서는 새로운 메소드를 정의한 것이다.
  - 상속되지 않지만 인스턴스 메소드로의 오버라이딩은 할 수 없다.
- 인스턴스 메서드 / static 메소드 간의 오버라이딩은 불가능하다. (static -> 인스턴스의 오버라이딩도 안되는 것이다.)

#### 오버라이딩 vs 오버로딩

- 오버라이딩 : 조상 클래스의 메소드를 자손 클래스에서 재정의(같은 메소드를 재정의)

- 오버로딩 : 새로운 메소드를 정의

  - 메소드 시그니쳐(메소드 명, 파라미터 타입 리스트(개수, 순서))로 구분된다.
  - 메소드 명은 같으면서 파라미터 타입 리스트가 다른 경우에 새로운 메소드로 보는 것
  - 파라미터의 타입은 같으면서 파라미터 명만 다른 경우엔 같은 메소드이므로 오버라이딩이 된다.(근데 파라미터 명을 바꾸는 일은 거의 없다.)

  ```
  java
  class Parent {
      void parentMethod() { }
  }
  
  class Child extends Parent {
      void parentMethod() { }         // 오버라이딩
      void parentMethod(int i) { }    // 오버로딩
  
      void childMethod() { }
      void childMethod(int i) { }     // 오버로딩
  }
  ```

---

### 다이나믹 메소드 디스패치(Dynamic Method Dispatch)