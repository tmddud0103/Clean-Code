# CH2 

# Meaningful name (의미있는 이름)

## 의도가 명확한 이름을 사용해라

Choosing good names takes time but saves more than it takes.

좋은 이름을 정하는 데에는 시간이 걸리지만 그 이름 (타인이) 해석하는 시간을 줄여준다.

이름을 짓는데에 주의를 가하고 더 좋은 이름을 찾았을 때에는 이름을 바꿔주어라

주석이 필요한 변수의 이름이라면, 그 변수의 이름은 의도가 명확하지 않은 것 이다.

예시  ` int d; // elapsed time in days`

의미를 함축하거나 줄이지 말아라 -> 너 말고 아무도 모른다

모든 사람들이 사전지식을 가지고 있지 않다.



## 잘못된 정보를 피해라

hp 가 체력인지 hypotenuse(빗변)인지 아닌지 그 누가 알겠냐

중의적인 이름, 줄여쓰기를 최대한 하지 말아라

list가 아닌데 accountlist같은 이름을 짓지 말아라 헷갈린다.

비슷해보이는 이름을 짓지 말자

XYZControllerForEfficientHandlingOfStrings

XYZControllerForEfficientStorageOfStrings

(이정도면 길이의 문제도 있다고 생각한다 차라리 주석을 다는게 훨씬 깔끔하지 않을까...?)



## 의미있게 구별해라

a1 a2 a3같이 넘버링을 하거나 noise word(불용어...? 그게 뭔데)를 주의해라

정확한 개념 구분이 되지 않는다



## 발음이 가능한 이름을 사용해라

```c++
class DtaRcrd102 {
private Date genymdhms; 
private Date modymdhms;
private final String pszqint = "102";
/* ... */
};


    
class Customer {
private Date generationTimestamp; 
private Date modificationTimestamp;;
private final String recordId = "102";
/* ... */
};
```

genymdhms (generation date, year, month, day, hour, minute, and second)

별걸 다 줄이지 말아라



## 검색이 쉬운 이름을 사용해라

The length of a name should correspond to the size of its scope.

변수 이름의 길이는 변수의 범위에 비례해서 길어진다 -> **뭔말이고?**

​	큰범위를 다룰수록 구체적으로 검색하기 쉽게



## 인코딩을 피해라

헝가리안 표기법

​	변수명에 변수의 타입을 적지 말자 (int 등)

맴버 접두어

​	맴버 변수에 접두어, 접미어를 붙이지 말자

​	with m_ anymore

​	왜? => unseen clutter =보이지 않는 혼란을 준다

인터페이스, 구현

​	인터페이스와 구현 클래스를 나눠야한다면 구현 글래스의 이름에 정보를 인코딩하자

| Do / Don't | Interface class | Concrete(Implementation) class |
| ---------- | --------------- | ------------------------------ |
| Don't      | IShapeFactory   | ShapeFactory                   |
| Do         | ShapeFactory    | ShapeFactoryImp                |
| Do         | ShapeFactory    | CShapeFactory                  |

위의 예시 이해안감



## 기억력 의존을 피해라

타인이 한번 더 생각해야 하는 변수명을 쓰지 말아라

One difference between a smart programmer and a professional programmer is that the professional understands that clarity is king.

똑똑한 프로그래머와 전문가 프로그래머를 나누는 기준 한가지는 "Clarity(명료함)"이다.



## 클래스의 이름

명사 혹은 명사구를 사용해라. like Customer, WikiPage, Account, and AddressParser. 

Manager, Processor, Data, or Info 같은 단어는 피하자 -> 너무 모호하기 때문에

동사는 사용하지 말아라



## 메서드 이름

동사 혹은 동사구를 사용해라. like postPayment, deletePage, or save.

접근자, 변경자, 조건자 (Accessors, mutators, and predicates)는 get, set, is로 시작하자. 

​																									(추가: should, has 등도 가능)

생성자를 오버로드할 경우 정적 팩토리 메서드를 사용하고 해당 생성자를 private으로 선언한다.

```
// 첫번째 보다 두 번째 방법이 더 좋다.  
Complex fulcrumPoint = new Complex(23.0);  
Complex fulcrumPoint = Complex.FromRealNumber(23.0);  
```



## 귀여우면 안된다 => ?

delete를 쓰지 왜 HolyHandGrenade(성스러운 수류탄)을 쓰냐

[안티오크의 성스러운 수류탄]

> "오 주님, 이 수류탄에 강복하시어 주님께서 주님의 은총으로 주님의 적을 산산조각내소서."
>
> \- 병기서 2장 10절 -

![image-20210808170111073](photo/image-20210808170111073.png)

영미권에서 큰 영향을 끼친 영화, 다수의 패러디가 있다

그렇지만 영미권이 아니면 모른다

=> 특정 문화에서만 사용되는 이름보다 의도를 표현하는 이름을 사용해라



## 한 컨셉에 한 단어를 사용해라

it’s confusing to have a controller and a manager and a driver in the same code base.



## 말장난을 사용하지 말아라

하나의 단어를 두가지 목적으로 사용하지 말아라



## 해법 영역 & 문제 영역 용어를 사용하자

- 개발자라면 당연히 알고 있을 `JobQueue`, `AccountVisitor(Visitor pattern)`등을 사용하지 않을 이유는 없다. 전산용어, 알고리즘 이름, 패턴 이름, 수학 용어 등은 사용하자.
  - (모르는데....?)
- 적절한 프로그래머 용어(위 13)가 없거나 문제영역과 관련이 깊은 용어의 경우 문제 영역 용어를 사용하자.



## 의미 있는 맥락을 추가해라

맥락을 클래스 등으로 감싸서 만들어라

```c++
// Bad
private void printGuessStatistics(char candidate, int count) {
    String number;
    String verb;
    String pluralModifier;
    if (count == 0) {  
        number = "no";  
        verb = "are";  
        pluralModifier = "s";  
    }  else if (count == 1) {
        number = "1";  
        verb = "is";  
        pluralModifier = "";  
    }  else {
        number = Integer.toString(count);  
        verb = "are";  
        pluralModifier = "s";  
    }
    String guessMessage = String.format("There %s %s %s%s", verb, number, candidate, pluralModifier );

    print(guessMessage);
}


// Good
public class GuessStatisticsMessage {
    private String number;
    private String verb;
    private String pluralModifier;

    public String make(char candidate, int count) {
        createPluralDependentMessageParts(count);
        return String.format("There %s %s %s%s", verb, number, candidate, pluralModifier );
    }

    private void createPluralDependentMessageParts(int count) {
        if (count == 0) {
            thereAreNoLetters();
        } else if (count == 1) {
            thereIsOneLetter();
        } else {
            thereAreManyLetters(count);
        }
    }

    private void thereAreManyLetters(int count) {
        number = Integer.toString(count);
        verb = "are";
        pluralModifier = "s";
    }

    private void thereIsOneLetter() {
        number = "1";
        verb = "is";
        pluralModifier = "";
    }

    private void thereAreNoLetters() {
        number = "no";
        verb = "are";
        pluralModifier = "s";
    }
}
```



## 불필요한 맥락을 넣지 말아라

클래스 이름앞에 불필요한 접두사 등을 넣는다면 자동완성이 잘 안될 것이다 => 효율적이지 못하다

예시

​	“Gas Station Deluxe” 라는 어플을 만들 때 클래스의 이름 앞에 GSD를 붙이지 말아라

​	 GSDAccountAddress => G를 누르고 자동완성을 사용할 때 모든 클래스가 다 나올 것이다

​	Address면 충분하다



## Final

서로의 코드를 보고 함수의 이름을 지적하고 비판하는데 두려움을 느끼지 말아라.

이름을 외우는 데에 시간을 쓰지 말고 ''자연스럽게 읽히는 코드''를 짜는데 더 집중을 해라

