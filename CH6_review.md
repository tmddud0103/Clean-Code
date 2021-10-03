# CH6 Objects and Data Structures



There is a reason that we keep our variables private.

변수를 비공개로 정의하는 이유가 있다... => 뭔말이고

We want to keep the freedom to change their type or implementation on a whim or an impulse. 

충동이든 변덕이든, 변수 타입이나 구현을 맘대로 바꾸고 싶어서다. 

Why, then, do so many programmers automatically add getters and setters to their objects, exposing their private variables as if they were public?

그렇다면 어째서 수많은 프로그래머가 조회get 함수와 설정set 함수를 당연하게 공개public해 비공개 변수를 외부에 노출할까?



## 자료 추상화

###### 목록 6-1 구체적인 Point 클래스

```C++
public class Point { 
  public double x; 
  public double y;
}
```

###### 목록 6-2 추상적인 Point 클래스

```c++
public interface Point {
  double getX();
  double getY();
  void setCartesian(double x, double y); 
  double getR();
  double getTheta();
  void setPolar(double r, double theta); 
}
```

목록 6-2는 구현을 완전히 숨긴다. 그에 반해 목록 6-1은 내부 구조를 노출하고, 개별적으로 좌표값을 읽고 설정하게 강제한다. 

변수 사이에 함수라는 계층을 계층을 넣는다고 구현이 저절로 감춰지지는 않는다. 구현을 감추려면 **추상화**가 필요하다!



## 자료 / 객체 비대칭

#### 객체는 추상화 뒤로 자료를 숨긴 채 자료를 다루는 함수만 공개한다

#### 자료 구조는 자료를 그대로 공개하며 별다른 함수는 제공하지 않는다

두 정의는 본질적으로 상반된다.

###### 목록 6-5 절차적인 도형 (Procedural Shape)

```
public class Square { 
  public Point topLeft; 
  public double side;
}

public class Rectangle { 
  public Point topLeft; 
  public double height; 
  public double width;
}

public class Circle { 
  public Point center; 
  public double radius;
}

public class Geometry {
  public final double PI = 3.141592653589793;
  
  public double area(Object shape) throws NoSuchShapeException {
    if (shape instanceof Square) { 
      Square s = (Square)shape; 
      return s.side * s.side;
    } else if (shape instanceof Rectangle) { 
      Rectangle r = (Rectangle)shape; 
      return r.height * r.width;
    } else if (shape instanceof Circle) {
      Circle c = (Circle)shape;
      return PI * c.radius * c.radius; 
    }
    throw new NoSuchShapeException(); 
  }
}
```

객체 지향 프로그래머가 위 코드를 본다면 코웃음을 칠지도 모르겠다. 클래스가 절차적이라 비판한다면 맞는 말이다. 하지만 그런 비웃음이 100% 옳다고 말하기는 어렵다. **만약 Geometry 클래스에 둘레 길이를 구하면 `perimeter()` 함수를 추가하고 싶다면? 도형 클래스는 아무 영향도 받지 않는다!** 도형 클래스에 의존하는 다른 클래스도 마찬가지다! **반대로 새 도형을 추가하고 싶다면 Geometry 클래스에 속한 함수를 모두 고쳐야 한다.** 그래서 두 조건은 완전히 정반대라고 할 수 있다.

이번에는 목록 6-6을 살펴보자. 객체 지향적인 도형 클래스다. 새 도형을 추가해서 기존 함수에 아무런 영향을 미치지 않는다. 반면 새 함수를 추가하고 싶다면 도형 클래스 전부를 고쳐야 한다.

###### 목록 6-6 다형적인 도형 (Polymorphic Shape)

```
public class Square implements Shape { 
  private Point topLeft;
  private double side;
  
  public double area() { 
    return side * side;
  } 
}

public class Rectangle implements Shape { 
  private Point topLeft;
  private double height;
  private double width;

  public double area() { 
    return height * width;
  } 
}

public class Circle implements Shape { 
  private Point center;
  private double radius;
  public final double PI = 3.141592653589793;

  public double area() {
    return PI * radius * radius;
  } 
}
```

앞서도 말했듯이, 두 방식은 사실상 반대다! 그래서 객체와 자료 구조는 근본적으로 양분된다.

**객체 지향 코드에서 어려운 변경은 절차적인 코드에서 쉬우며, 절차적인 코드에서 어려운 변경은 객체 지향 코드에서 쉽다!**



## 디미터 법칙

디미터 법칙은 잘 알려진 휴리스틱heuristic(경험에 기반하여 문제를 해결하거나 학습하거나 발견해 내는 방법)으로, 모듈은 자신이 조작하는 객체의 속사정을 몰라야 한다는 법칙



디미터 법칙은 "클래스 C의 메서드 f는 다음과 같은 객체의 메서드만 호출해야 한다"고 주장



## 자료 전달 객체

자료 구조체의 전형적인 형태는 공개 변수만 있고 함수가 없는 클래스다. 이를 때로는 자료 전달 객체Data Transfer Object, DTO라 한다.

```
public class Address { 
  public String street; 
  public String streetExtra; 
  public String city; 
  public String state; 
  public String zip;
}
```



#### 활성 레코드

DTO의 특수한 형태다.활성 레코드는 데이터베이스 테이블이나 다른 소스에서 자료를 직접 변환한 결과다.


**활성 레코드는 자료 구조로 취급한다.** 



## 결론

객체는 동작을 공개하고 자료를 숨긴다. 그래서 기존 동작을 변경하지 않으면서 새 객체 타입을 추가하기는 쉬운 반면, 기존 객체에 새 동작을 추가하기는 어렵다.

새로운 자료 타입을 추가하는 유연성이 필요하면 객체가 더 적합하다. 다른 경우로 새로운 동작을 추가하는 유연성이 필요하면 자료 구조와 절차적인 코드가 더 적합하다. 우수한 소프트웨어 개발자는 편견 없이 이 사실을 이해해 직면한 문제에 최적인 해결책을 선택한다.











