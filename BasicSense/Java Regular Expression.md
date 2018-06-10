# Java Regular Expression

### index

- 정규 표현식
- 패턴
- 자바 정규 표현식
- 그룹화
- 표현식
- 자주 사용하는 표현식
- ref

-----

## 정규 표현식

정규 표현식(Regular Expression, regexp, regex, Rational Expression)은 특정한 규칙을 가진 문자열의 집합을 표현하는데 사용하는 형식 언어이다. 프로그래밍 언어와 텍스트 편집에서 문자열의 검색 및 치환을 지원하기 위해 많이 사용된다. 컴퓨터 과학의 정규 연어로부터 유래하였으나 구현체에 따라서 정규 언어보다 더 넓은 언어를 표형할 수 있는 경우도 있으며, 심지어 정규 표현식 자체의 문법도 여려 가지 존재한다. 

-----

## 패턴

정규 표현식이라는 문구는 일치하는 텍스트가 준수해야 하는 "패턴"을 표현하기 위해 특정한 표준의 텍스트 신택스를 의미하기 위해 사용된다. 정규 표현식의 각 문자(즉 패턴을 기술하는 문자열 안의 각 문자)는 메타문자(특별한 의미)로 이해되거나 정규 문자(리터럴, 문자 그대로를 의미)로 이해된다.

정규 표현식의 기본 개념은 특정 목적을 위해 필요한 문자열 집합을 지정하기 위해 쓰이는 식이다. 문자열의 유한 집합을 지정하는 단순한 방법은 문자열의 요소나 멤버를 나열하는 것이다. 그러나 문자열의 원하는 집합을 지정하기 위해 사용할 수 있는 더 간결한 방법들이 있다.

-----

## 자바 정규 표현식

자바에서는 표준 라이브러리를 이용하며, 자바 정규 표현 API는 Java.util.regex package로 1개의 인터페이스와 3개의 클래스를 제공한다.

- MatchResult Interface
- Matcher Class
- Pattern Class
- PatternSyntaxException Class

### Matcher Class

MatchResult 인터페이스를 구현한 클래스로, 문자 시퀀스에서 동일한 오퍼레이션을 구행하기 위해 사용되는 정규 표현 엔진이다.

|         Mathod          |         Description          |
| :---------------------: | :--------------------------: |
|    boolean matches()    |     정규 표현식이 패턴의 일치 여부 확인     |
|     boolean find()      |       패턴에 일치하는 표현식 조회        |
| boolean find(int strat) |   해당 인덱스부터 패턴에 일치하는 표현식 조회   |
|       int start()       |      매칭되는 문자열 시작 인덱스 반환      |
|  int start(int group)   |    지정된 그룹이 매칭되는 시작 인덱스 반환    |
|        int end()        |   매칭되는 문자열 끝의 다음 문자 인덱스 반환   |
|   int end(int group)    | 지정된 그룹이 매칭되는 끝의 다음 문자 인덱스 반환 |
|     String group()      |   매칭된 부분 반환 ( == group(0))   |
| String group(int group) |  매칭된 부분 중 해당 인덱스의 매칭된 부분 반환  |
|    int groupCount()     |   패턴 내의 그룹화한(괄호 지정) 갯수 반환    |

### Pattern Class

정규 표현식의 컴파일된 버전으로, 정규 표현 엔진의 패턴을 규정하는데 사용된다.

|                  Mathod                  |               Description                |
| :--------------------------------------: | :--------------------------------------: |
|   static pattern compile(String regex)   |         주어진 정규 표현식을 컴파일하고, 패턴 반환         |
|   Matcher mather(CharSequence  input)    |       주어진 입력, 패턴과 일치하는 matcher 생성        |
| static boolean matchers(String regex, CharSequence input) | 컴파일과 matcher 메소드의 결합으로 동작하며, 정규 표현식을 컴파일하고 주어진 패턴을 매칭 |
|    String[] split(CharSequence input)    |        주어진 패턴과 맞춰 문자열을 나누어 배열로 반환        |
|             String pattern()             |               정규 표현식 패턴 반환               |

-----

## 그룹화 이해하기

패턴 내에서 그룹을지정하기 위해서 소괄호를 통해 그룹을 설정하며, 소괄호의 갯수만틈 그룹이 생성된다.

```java
import java.util.*;
import java.util.regex.*;

class ExampleRegex{
  public static void main(String[] args){
    String example = "Windo98, WindowME, WindowXP, WindowNT, Window2000, WindowVista, Window7, Window10";
    
    Pattern p = Pattern.compile("(Window)(98|XP|10)");
    Matcher m = p.matcher(example);
    
    int count = 0;
    whiel(m.find()){
      count++;
      System.out.printLn(m.group(0) + ": " + m.group(1) + ", " + m.group(2));
    }
    
    System.out.println("그룹 총 갯수: " + m.group.Count());
  }
}
```

```
Window98: Window, 98
WindowXP: Window, XP
Window10: Window, 10
그룹 총 갯수: 2
```

위의 코드에서 패턴은 ``"(Windoew)(98|XP|10)"``이며, 소괄호를 통해 두 개의 그룹이 생성됐다. ``m.group(0)``은 ``m.group()``과 같으며, ``m.group(1)``과 ``m.group(2)``은 앞에서 소괄호로 그룹화한 두 개의 그룹에 해당한다.

```
Window98: Window,  98
	|		|	    |
group(0) group(1) group(2)
```

-----

## 표현식

| Expression | Description                              |
| ---------- | ---------------------------------------- |
| ^          | 문자열의 시작                                  |
| $          | 문자열의 종료                                  |
| .          | 임의의 한 문자 (문자의 종류를 가리지 않음), (\는 사용 불가)    |
| *          | 문자가 0개 이상 있을 수 있음                        |
| +          | 문자가1개 있음                                 |
| ?          | 문자가 없거나 하나 있음                            |
| []         | 문자의 집합이나 범위를 나타내며, 두 문자 사이는 - 기호로 범위를 표현. []내에 ^ 기호가 선행하다면 not 의미 |
| {}         | 횟수 또는 범위 표현                              |
| ()         | 소괄호 안의 문자를 하나의 문자로 표현                    |
| \|         | 패턴 안에서 or 연산 표현                          |
| \s         | 공백 문자                                    |
| \S         | 공백 문자가 아닌 나머지 문자                         |
| \w         | 알파벳 또는 숫자                                |
| \W         | 알파펫 또는 숫자를 제외한 나머지                       |
| \d         | 숫자 [0-9]                                 |
| \D         | 숫자를 제외한 나머지                              |
| \          | 정규 표현식 역슬래시(\) 기호는 확장 문자. 역슬래시 다음에 일반 문자가 오면 특수 문자로 취급하고, 역슬래시 다음에 특수 문자가 오면 그 문자 자체를 의미 |
| (?i)       | 대소문자를 구분하지 않음의 옵션                        |

Ex.	^[0-9]*&

- ^: 패턴의 시작
- \[0-9]: 0에서 9까지의 숫자 범위 지정
- *: 글자 수 범위 지정(0개 이상)
- $: 패턴 종료

-----

## 자주 사용하는 표현식

| Pattern                                  | Description                              |
| ---------------------------------------- | ---------------------------------------- |
| ^[a-z0-9_-]{3, 16}$                      | Matching a Username<br />소문자, 숫자, 언더스코어, 하이픈이 나올 수 있으며, 3~16개 사이의 문자열을 허용 |
| ^[a-z0-9_-]{6,18}&                       | Matching a Password<br />소문자, 숫자, 언더스코어, 하이픈이 나올 수 있으며, 6 ~ 18개 사이의 문자열을 허용 |
| ^#?([a-f0-9]{6} \| [a-f0-9]{3})$         | Matching a Hex Value<br />은 ?를 사용하기 위함이여, ?는 앞에 문자가 없거나 하나만 있음을 의미<br />소문자 a~f사이의 문자 또는 숫자가 6개 또는 3개를 허용<br />ffffff 같은 Hex 값을 파서가 잡기 위한 표현으로 주로 사용 |
| ^[a-z0-9-]+$                             | Matching a Slug<br />   소문자, 숫자를 허용하여 하이픈이 한개 또는 하나 이상(+)을 허용함<br />mod_rewrite나 pretty URL에서 사용 |
| ^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$ | Matching an Email<br />첫번째 그룹은 URL이  "http://" 또는 "https://" 이거나 둘다 없이 시작함을 허용하며, s 뒤에 물음표는 URL이 http와 https를 모두 허용<br />이어지는 부분은 추가적인 파일과 디렉토리레 대한 부분으로 이 그룹에서는 갯수와 상관없이 슬래쉬(/), 문자, 숫자, 언더스코어, 스페이스, 점(.), 하이픈을 허용하며, 이 그룹은 많은 수의 디렉토리와 파일과 매치된다<br />물음표 대신 별(*)을 사용하는 것은 0 또는 1이 아닌 0 또는 1개 이상의 의미 |
| ^(?:(?:25[0-5] \| 2[0-4][0-9] \|[01]?[0-9][0-9]?)\.){3}(?:25[0-5] \| 2[0-4][0-9] \| [01]?[0-9][0-9]?)$ | Matching an IP Address<br /> 첫번째 그룹의 내용은 1~255사이의 수가 허용됨을 표현한 것이며, 이것을 3번 반복하도록 되어있다. |
| ^<([a-z]+)(\[^<]+)*(?:>(.*)<\/\1>\|\s+\/>)$ | Matching an HTML Tag<br />한 개 이상의 문자를 허용하며, 닫는 태그를 잡을 때까지 찾는다.<br />다음은 태그이 속서이며, > 기호를 제외한 어떤 문자도 허용한다.<br />\+ 기호는 속성과 값을 이루고 별(*) 기호는 한 개 이상의 속성을 허용한다.<br />다음으로 오는 그룹은 내부에 > 기호가 담기며, 컴텐츠 부분과 닫는 태그가 있다. \1은 첫번째 그룹에서의 내용을 표현하는데 사용되며, "/>"가 이어지는 한 개 이상의 공백을 허용 |

-----

### ref

- [정규 표현식](https://ko.wikipedia.org/wiki/%EC%A0%95%EA%B7%9C_%ED%91%9C%ED%98%84%EC%8B%9D)
- [[JAVA] 정규표현식, Matcher 메서드 사용방법과 그룹 개념이해](http://sexy.pe.kr/tc/532)
- [Java Regex 자바 정규 표현식](http://tworab.tistory.com/46)
- [Class Matcher](https://docs.oracle.com/javase/7/docs/api/java/util/regex/Matcher.html)

