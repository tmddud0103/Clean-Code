# CH6 Objects and Data Structures



There is a reason that we keep our variables private.

변수를 비공개로 정의하는 이유가 있다... => 뭔말이고

We want to keep the freedom to change their type or implementation on a whim or an impulse. 

충동이든 변덕이든, 변수 타입이나 구현을 맘대로 바꾸고 싶어서다. 

Why, then, do so many programmers automatically add getters and setters to their objects, exposing their private variables as if they were public?

그렇다면 어째서 수많은 프로그래머가 조회get 함수와 설정set 함수를 당연하게 공개public해 비공개 변수를 외부에 노출할까?



## 자료 추상화

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



## 자료 / 객체 비대칭



































