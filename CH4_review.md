# CH4 Comments

> “Don’t comment bad code—rewrite it.”

Nothing can be quite so damaging as an old crufty comment that propagates lies and misinformation.

Inaccurate comments are far worse than no comments at all.

=> 쓰지 말라는 것이 아니라 코드를 애초에 깔끔하게 쓰면 된다는 뜻

이건 마치 교과서 위주로 공부를 열심히 하면 서울대 가는데 왜 학원 다니냐랑 같은 이야기 아닐까....



## 주석은 나쁜 코드를 보완하지 못한다

One of the more common motivations for writing comments is bad code. 

캬...

Clear and expressive code with few comments is far superior to cluttered and complex code with lots of comments.

주석을 써서 설명할 시간에 코드를 깨끗하게 치워라



## 코드로 너의 의도를 설명해라

```c++
// Check to see if the employee is eligible for full benefits
if ((employee.flags & HOURLY_FLAG) && 
 (employee.age > 65)) 
```

```c++
if (employee.isEligibleForFullBenefits())
```

둘 다 못알아 먹겠는데 어카누...

=> 함수 이름으로 설명이 가능하게 써라



## 좋은 주석

### 법적인 주석

#### 각 소스 파일 첫머리에 들어가는 저작권 정보와 소유권 정보 등

```c++
// Copyright (C) 2003, 2004, 2005 by Object Montor, Inc. All right reserved.`
// GNU General Public License
```

### 정보제공

```c++
// Returns an instance of the Responder being tested.
protected abstract Responder responderInstance();
```

### 의도를 설명

아까는 쓰지 말라며...?

Sometimes a comment goes beyond just useful information about the implementation and provides the intent behind a decision.

```c++
public int compareTo(Object o)
{
	if(o instanceof WikiPagePath)
    {
        WikiPagePath p = (WikiPagePath) o;
        String compressedName = StringUtil.join(names, "");
        String compressedArgumentName = StringUtil.join(p.names, "");
        return compressedName.compareTo(compressedArgumentName);
    }
	return 1; // we are greater because we are the right type.
}
```

### 설명

```c++
public void testCompareTo() throws Exception
{
    WikiPagePath a = PathParser.parse("PageA");
    WikiPagePath ab = PathParser.parse("PageA.PageB");
    WikiPagePath b = PathParser.parse("PageB");
    WikiPagePath aa = PathParser.parse("PageA.PageA");
    WikiPagePath bb = PathParser.parse("PageB.PageB");
    WikiPagePath ba = PathParser.parse("PageB.PageA");
    assertTrue(a.compareTo(a) == 0); // a == a
    assertTrue(a.compareTo(b) != 0); // a != b
    assertTrue(ab.compareTo(ab) == 0); // ab == ab
    assertTrue(a.compareTo(b) == -1); // a < b
    assertTrue(aa.compareTo(ab) == -1); // aa < ab
    assertTrue(ba.compareTo(bb) == -1); // ba < bb
    assertTrue(b.compareTo(a) == 1); // b > a
    assertTrue(ab.compareTo(aa) == 1); // ab > aa
    assertTrue(bb.compareTo(ba) == 1); // bb > ba
}
```

### 결과를 경고

```c++
// Don't run unless you 
// have some time to kill.
```

시간이 많지 않으면 코드를 돌리지 말아라

### TODO

앞으로 해야 할 일

필요하지만 당장 구현하기 어려운 업무를 기술

### 중요성 강조

```c++
String listItemContent = match.group(3).trim();
// the trim is real important. It removes the starting 
// spaces that could cause the item to be recognized
// as another list.
new ListItemWidget(this, listItemContent, this.level + 1);
return buildList(text.substring(match.end()));
```

#### 공개 API에서 Javadocs

설명이 잘 된 공개 API는 참으로 유용하고 만족스럽다. 공개 API를 구현한다면 반드시 훌륭한 Javadocs 작성을 추천한다. 하지만 여느 주석과 마찬가지로 Javadocs 역시 독자를 오도하거나, 잘못 위치하거나, 그릇된 정보를 전달할 가능성이 존재하는 것 역시 잊으면 안 된다.

위는 무슨 내용인지 몰라 긁어왔다.



## 나쁜 주석

### 웅얼거리는 주석

아무 의미 없이 달리는 주석

=> 주석의 의미가 전달이 안되면(이해가 안되면) 제대로 된 주석이 아니다

### 같은 이야기를 반복

전혀 필요없는 코드

### 오해의 여지가 있는 주석

잘못된 정보로 인해 코드가 느려지거나 오류를 만들어 낼 수 있다

### 의무적으로 다는 주석

모든 변수에 주석을 달아야 한다는 규칙은 어리석은 생각이다

코드를 복잡하게 만들고 거짓을 퍼트리고, 혼동과 무질서를 이끈다

### 저널 주석

module every time they edit it. => 커밋을 의미하는 것 같다

Nowadays, however, these long journals are just more clutter to obfuscate the module.

### 시끄러운 주석 => 필요가 없는 주석

```c++
/** The name. */
private String name;
/** The version. */
private String version;
/** The licenceName. */
private String licenceName;
/** The version. */
private String info;
```

### 위치 표시

자주 사용하면 가독성만 해친다

필요할 때만 아주 드물게 사용하는 편이 좋다

### 닫는 괄호에 다는 주석

이런 생각이 든다면 함수를 줄이려 시도하자

### 저자 표시

Source code control systems are very good at remembering who added what, when.

### 주석 처리 코드

Those systems will remember the code for us. We don’t have to comment it out any more. Just delete the code. We won’t lose it. Promise.

### 전역 정보

근저에 있는 코드만 기술해라

### 너무 많은 정보

``` c++
/*
 RFC 2045 - Multipurpose Internet Mail Extensions (MIME) 
 Part One: Format of Internet Message Bodies
 section 6.8. Base64 Content-Transfer-Encoding
 The encoding process represents 24-bit groups of input bits as output 
 strings of 4 encoded characters. Proceeding from left to right, a 
 24-bit input group is formed by concatenating 3 8-bit input groups. 
 These 24 bits are then treated as 4 concatenated 6-bit groups, each 
 of which is translated into a single digit in the base64 alphabet. 
 When encoding a bit stream via the base64 encoding, the bit stream 
 must be presumed to be ordered with the most-significant-bit first. 
 That is, the first bit in the stream will be the high-order bit in 
 the first 8-bit byte, and the eighth bit will be the low-order bit in 
 the first 8-bit byte, and so on.
 */
```

Don’t put interesting historical discussions or irrelevant descriptions of details into your comments.





## 결론

최소한의 정보로 가독성을 해치지 않는 선에서 사용해라

주석으로 이해를 시킬 코드면 코드를 더 간단하게 짜는게 좋다