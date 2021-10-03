# CH7 Error Handling

## Use Exceptions Rather Than Return Codes 

### 리턴 코드 대신 Exceptions를 사용해라

Back in the distant past there were many languages that didn’t have exceptions.

이 전의 프로그래밍 언어에는 exceptions가 없었다.

그래서 개발자들은 예외처리를 해줘야 했다.

하지만 예외처리는 잊어버리기 쉽다. 



##  Try-Catch-Finally문을 먼저 써라

try문은 transaction처럼 동작하는 실행코드로, catch문은 try문에 관계없이 프로그램을 일관적인 상태로 유지



## Unchecked Exceptions를 사용하라

### Checked exception VS Unchecked Exception

Checked Exception과 Unchecked Exception의 가장 명확한 구분 기준은 ‘**꼭 처리를 해야 하느냐**’이다. 

Checked Exception이 발생할 가능성이 있는 메소드라면 반드시 로직을 try/catch로 감싸거나 throw로 던져서 처리해야 한다. 

반면에 Unchecked Exception은 명시적인 예외처리를 하지 않아도 된다. 

이 예외는 피할 수 있지만 개발자가 부주의해서 발생하는 경우가 대부분이고, 미리 예측하지 못했던 상황에서 발생하는 예외가 아니기 때문에 굳이 로직으로 처리를 할 필요가 없도록 만들어져 있다.



## Define Exception Classes in Terms of a Caller’s Needs 

### 사용에 맞게 Exception 클래스를 선언하라

써드파티 라이브러리를 사용하는 경우 그것들을 wrapping함으로써
1. 라이브러리 교체 등의 변경이 있는 경우 대응하기 쉬워진다.
2. 라이브러리를 쓰는 곳을 테스트할 경우 해당 라이브러리를 가짜로 만들거나 함으로써 테스트하기 쉬워진다.
3. 라이브러리의 api 디자인에 종속적이지 않고 내 입맛에 맞는 디자인을 적용할 수 있다.



## 일반적인 상황을 정의해라

catch문에서 예외적인 상황(special case)을 처리해야 하는 경우 코드가 더러워지는 일이 발생할 수 있다.

이런 경우, Martin Fowler의 **Special Case Pattern**을 사용

​	읽어도 뭔지 모르겠음



## Null을 리턴하지 마라

null을 리턴하고 싶은 생각이 들면 위의 Special Case object를 리턴하라는데 이게 뭔데...

```C++
  // BAD!!!!

  public void registerItem(Item item) {
    if (item != null) {
      ItemRegistry registry = peristentStore.getItemRegistry();
      if (registry != null) {
        Item existing = registry.getItem(item.getID());
        if (existing.getBillingPeriod().hasRetailOwner()) {
          existing.register(item);
        }
      }
    }
  }
  
  
  // 위 peristentStore가 null인 경우에 대한 예외처리가 안된 것을 눈치챘는가?
  // 만약 여기서 NullPointerException이 발생했다면 수십단계 위의 메소드에서 처리해줘야 하나?
  // 이 메소드의 문제점은 null 체크가 부족한게 아니라 null체크가 너무 많다는 것이다.
```

```C++
  // Bad
  List<Employee> employees = getEmployees();
  if (employees != null) {
    for(Employee e : employees) {
      totalPay += e.getPay();
    }
  }
```

```C++
  // Good
  List<Employee> employees = getEmployees();
  for(Employee e : employees) {
    totalPay += e.getPay();
  }
  
  public List<Employee> getEmployees() {
    if( .. there are no employees .. )
      return Collections.emptyList();
    }
}
```



## Null을 넘기지 말아라

null을 리턴하는 것도 나쁘지만 null을 메서드로 넘기는 것은 더 나쁘다.



## 결론

Clean code is readable, but it must also be robust. 

깨끗한 코드는 읽기 쉽다, 그러나 항상 견고하지는 않다.

We can write robust clean code if we see error handling as a separate concern, something that is viewable independently of our main logic.

깨끗한 코드와 견고한 코드는 대립되는 목표가 아니다. 예외처리를 로직에서 제거하면 각각에 대해 독립적인 사고가 가능해진다.

