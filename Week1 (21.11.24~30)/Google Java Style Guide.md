# 들어가기 전
취업 준비를 하며 우아한형제들의 프로그래밍 교육 프로그램인 우테코에 대해 알게 되어 지원하게 되었습니다. 
서류와 1차 코딩테스트 결과 운이 좋게 우테코 프리코스를 수강할 기회가 생겨 오늘부터 미션을 수행하게 되었습니다. 프리코스는 매주 주어진 미션을 수행하며 주도적으로 프로그래밍 학습하며 성장을 하는 과정입니다. 프리코스를 참여하며 [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html#s4.8.1-enum-classes)에 대해 알게 되었고 해당 가이드를 완벽히 이해하고 잊혀질 때쯤 쉽게 복습하기위해 번역을 하며 정리해보고자 합니다.

<hr>

# 1. 소개
이 문서는 Java프로그래밍의 소스코드에 대한 Google의 코딩 표준의 완벽한 정의 문서 입니다. Java 소스파일은 여기에 있는 규칙을 준수하는 경우에만 Google스타일에 있다고 설명할 수 있습니다.

다른 프로그래밍 스타일 가이드와 마찬가지로 서식의 미적 문제뿐만 아니라 다른 유형의 규약이나 코딩 표준도 다루고 있다. 그러나 이 문서는 우리가 보편적으로 따르는 엄격한 규칙에 주로 초점을 맞추고 있으며, (인간이든 도구이든) 명확하게 집행할 수 없는 조언은 피하고 있습니다.

## 1.1 용어 정리
용어들은 본 문서에서는 달리 명시되지 않은 한 다음과 같다.

1. class라는 용어는 일반적인 class, enum class, interface 또는 어노테이션 타입의@interface를 포괄적으로 칭합니다.
2. class의 member라는 용어는 중첩 클래스, 필드, 메서드 또는 생성자를 포괄한 용어입니다. 즉 이니셜라이저와 주석을 제외한 클래스의 모든 최상위 콘텐츠를 의미하기 위해 포괄적으로 사용됩니다.
3. comment라는 용어는 항상 구현 comment를 나타냅니다. 우리는 "documentation comments"이라는 문구를 사용하지 않고, "Javadoc"이라는 공통 용어를 사용합니다.

기타 "용어 참고사항"은 문서 전체에 가끔 나타납니다.

## 1.2 가이드 노트
이 문서의 예제 코드는 비표준 입니다. 즉, 예제는 Google 스타일이지만 코드를 표현하는 유일한 스타일리시한 방법을 설명하지 않을 수 있습니다 . 예제에서 선택한 선택적 형식 지정은 규칙으로 적용되어서는 안 됩니다.

<hr>

# 2 소스파일의 기본
## 2.1 파일명
소스 파일 이름은 포함하는 최상위 클래스의 대소문자를 구분하는 이름과 `.java` 확장자로 구성됩니다.

## 파일 인코딩: UTF-8
소스 파일은 UTF-8로 인코딩됩니다.

## 2.3 특수문자
### 2.3.1 공백 문자
해당 라인을 끝내는 문자를 제외하고는 ASCII horizontal space character (0x20)는 소스 파일에서 나타나는 유일한 공백 문자입니다. 

이것은 다음을 의미합니다.
1. 문자열 및 문자 리터럴의 다른 모든 공백 문자는 이스케이프됩니다.
2. 들여쓰기에는 탭 문자가 사용되지 않습니다.

### 2.3.2 특수 이스케이프 시퀀스
특수 이스케이프 시퀀스(`\b`, `\t`, `\n`, `\f`, `\r`, `\'`, `\`, `\\`)를 가진 문자들은 해당 8진수(예: `\012`) 또는 유니코드(예: `\u000a`) 이스케이프대신 해당 시퀀스를 사용합니다.

### 2.3.3 Non-ASCII 문자
나머지 Non-ASCII 문자의 경우 실제 유니코드 문자(예: `∞`) 또는 이에 상응하는 유니코드 이스케이프(예: `\u221e`)가 사용됩니다. 유니코드가 문자열 리터럴 및 주석 외부로 이스케이프되는 것은 강력히 권장되지 않지만 선택은 코드 를 읽고 이해하기 쉽게 만드는 것에 달려 있습니다.

> 팁: 유니코드 이스케이프 사례에서, 때로는 실제 유니코드 문자를 사용하는 경우에도 설명 설명이 매우 유용할 수 있습니다.

Example
![](https://images.velog.io/images/seongwon97/post/3641c689-2a4d-46d9-ad45-6fb417180e6c/image.png)

> 팁: 일부 프로그램이 ASCII가 아닌 문자를 제대로 처리하지 못할 수 있다는 두려움 때문에 코드 가독성을 낮추지 마십시오. 그런 일이 발생하면 해당 프로그램이 손상 되어 수정 되어야 합니다 .

<hr>

# 3 소스파일 구조
소스 파일은 다음과 같은 순서 로 구성됩니다 .

1. 라이센스 또는 저작권 정보 (있는 경우)
2. Package statement (서술)
3. Import statements
4. 정확히 하나의 최상위 클래스

정확히 하나의 빈 줄 은 존재하는 각 섹션을 구분합니다.

## 3.1 라이선스 또는 저작권 정보 (있는 경우)
라이선스 또는 저작권 정보가 파일에 속해 있으면 여기에 속합니다.

## 3.2 Package statement
Package statement는 줄바꿈을 하지 않습니다. 또한 열 제한(섹션 4.4 열 제한:100)도 적용되지 않습니다.

## 3.3 Import statements
### 3.3.1 No wildcard imports
Wildcard imports과 static은 사용하지 않습니다.
> Wildcard import는 `import java.util.*`와 같이 `*`을 의미한다.

### 3.3.2 No line-wrapping (줄바꿈 없음)
Import statements는 줄바꿈을 하지 않습니다. 또한 열 제한(섹션 4.4 열 제한:100)도 적용되지 않습니다.

### 3.3.3 순서와 간격
import는 다음과 같은 순서를 따라야한다.

1. 단일 블록에 모든 static import하기
2. 단일 블록에 모든 non-static import하기

static import와 non-static import가 있는 경우, 공백의 줄을 추가하여 두개의 블록을 구분합니다. 그 외에는 공백의 구분 줄이 있으면 안됩니다.

각 블록내에서 가져온 이름은 ASCII정렬 순서로 나타내야합니다.  (참고: '.'가 ';'보다 먼저 정렬되기 때문에 import문 내에서는 ASCII 정렬 순서를 따르지 않는다.)


### 3.3.4 클래스에 static을 import하지 않기
static import는 static중첩 클래스에 사용되지 않는다. 그것들은 일반적인 import를 해야한다.

## 3.4 클래스 선언
### 3.4.1 정확히 하나의 최상위 클래스 선언
각 최상위 클래스는 자체 소스 파일에 있어야합니다. 

### 3.4.2 content의 순서
클래스의 member와 initializer의 순서는 학습력에 큰 영향을 끼칩니다. 그러나 옳바른 방법은 없으며 클래스마다 다른 방법으로 콘텐츠를 정렬할 수 있습니다.

중요한 것은 각 클래스가 논리적인 순서를 사용한다는 것인데, 이는 유지관리자가 질문을 하면 그의 질문에 답을 할 수 있어야합니다. 

잘못된 예를 들면, 새로운 메서드들을 단순하게 클래스의 끝에 추가하는 것이 있습니다. 이는 논리적인 순서가 아니라 단순히 날짜 순서대로 추가한 것입니다.


#### 3.4.2.1 Overloads: never split
클래스 내에 여러개의 Constructor가 존재하거나 동일한 이름의 Method가 존재한다면 이들 사이에 다른 코드 없이 순차적으로 나타내야합니다. (private member도 포함)

<hr>

# 4. Formatting
용어 정리: block-like construct(블록과 유사한 구성?구조?)는 클래스, 메서드, 생성자의 본문(body)를 나타냅니다. 
섹션 4.8.3.1인 Array initializer에 따르면 모든 Array initializer는 선택적으로 block-like construct처럼 처리될 수 있습니다.

## 4.1 Brace (중괄호)
### 4.1.1 선택 사항인 경우에도 중괄호는 사용한다.
`if`, `else`, `for`, `do`, `while` 구문에서 body가 비어있거나 단 한 줄의 코드만 있어도 중괄호는 있어야한다.
### 4.1.2 비어있지 않은 블록 : K & R Style
중괄호는 비어있지 않은 블록과 block-like construct에 대해서 Kernighan and Ritchie style를 따른다.

- 열리는 중괄호 앞에는 줄 바꿈이 없어야한다.
- 열리는 중괄호 뒤에는 줄 바꿈을 해야한다.
- 닫는 중괄호 앞에는 줄 바꿈을 해야한다.
- 명령문, 메서드, 생성자 또는 클래스의 본문이 끝났을 때는 닫는 중괄호 다음에 줄을 바꿔야한다. 만약 else나 콤마뒤에 나오는 부분일 경우 줄 바꿈 하지 않는다.

Example:
```java
return () -> {
  while (condition()) {
    method();
  }
};

return new MyClass() {
  @Override public void method() {
    if (condition()) {
      try {
        something();
      } catch (ProblemException e) {
        recover();
      }
    } else if (otherCondition()) {
      somethingElse();
    } else {
      lastThing();
    }
  }
};
```
emun 클래스들은 예외가 있다. (섹션 4.8.1)

### 4.1.3 빈 블록 : 간결할 수 있다
빈 블록이나  block-like construct는 K & B 스타일 일 수도 있다.(섹션 4.1.2에 설명된다.) 그 대안으로 다중 블록 문(여러 블록을 직접 포함하는 문, `if/else` 또는 `try/catch/finally`)의 일부가 아니면 중괄호(`{ }`)를 열자마자 문자나 줄 바꿈 없이 바로 닫을 수 있습니다.

```java
  // 허용됩니다.
  void doNothing() {}

  // 허용됩니다.
  void doNothingElse() {
  }
```
```
  // 허용 불가!!! 멀티 블록에서는 비어있는 블록을 쓸 수 없습니다.
  try {
    doSomething();
  } catch (Exception e) {}
```

## 4.2 블록 들여쓰기: +2 spaces
새로운 블록이나 블록과 block-liked construct가 열릴 때마다 두 칸씩 들여쓰기를 한다. 블록이 끝나면 들여쓰기는 이전의 레벨로 돌아가며 들여쓰기 레벨은 블록 전체의 코드와 주석에 모두 적용됩니다. (4.1.2 비어있지 않은 블록 : K & R Style의 예제 코드를 참조하십시오.)

## 4.3 한 줄에 하나의 명령어(statement)
각 명령어 뒤에는 줄 바꿈이 옵니다.

## 4.4 열 제한: 100
Java코드에서 열 제한은 100자입니다. 여기서 문자들은 유니코드의 코드 포인트를 의미하며 아래의 명시된 경우를 제외하고는 이 제한을 초과하는 모든 줄은 섹션 4.5에 설명된 대로 줄 바꿈을 해야합니다. 
> 각 유니코드의 코드 포인트는 디스플레이에 표시된 너비가 더 크거나 작더라도 하나의 문자로 계산된다. 예를 들어 full-width character(한국어, 중국어 등을 의미)를 사용하는 경우 규칙에서 제한한 글자 수 보다 일찍 줄 바꿈 할 수 있다.

예외:
1. 열 제한을 준수할 수 없는 행 (ex. Javadoc의 긴 URL또는 긴 JSNI 메서드 참조)
2. `package`와 `import`문 (섹션 3.2 package statement & 3.3 import statements)
3. shell에 잘라내어 붙여넣을 수 있는 주석

## 4.5 줄 바꿈 (Line-wrapping)
용어 정리: 한 줄을 차지할 수 있는 코드가 여러 줄로 나뉘면 이 활동을 줄 바꿈이라고 한다.

모든 상황에서 줄 바꿈하는 방법을 정확히 보여주는 포괄적이고 결정적인 공식은 없습니다. 하지만 매우 자주 동일한 코드 조각을 줄 바꿈하는 몇 가지 유효한 방법이 있습니다.

> 참고: 줄 바꿈의 일반적인 이유는 열 제한 오버플로를 방지하기 위한 것이지만 실제로 열 제한에 맞는 코드라도 작성자의 재량에 따라 줄 바꿈 될 수 있습니다.

> 팁: 메서드 또는 지역 변수를 추출하면 줄 바꿈 없이 문제를 해결할 수 있습니다.

### 4.5.1 언제 다음 문장으로 내릴지
줄 바꿈의 주요 원칙은 더 높은 수준(level)에서 바꾸는 것입니다. 또한:

1. non-assignment 연산자에서 줄 바꿈이 일어난 경우 줄 바꿈은 기호 이전에 위치합니다.  (C++ 및 JavaScript와 같은 다른 언어에서 Google 스타일에 사용되는 것과 동일한 방식이 아닙니다.)
- operator-like 기호에도 동일하게 적용된다	
    - the dot separator (`.`)
    - the two colons of a method reference (`::`)
    - an ampersand in a type bound (`<T extends Foo & Bar>`)
    - a pipe in a catch block (`catch (FooException | BarException e)`)
2. 대입(assignment) 연산자에서 줄 바꿈이 일어나면 일반적으로 기호 뒤에 줄 바꿈이 일어나지만 어느 쪽에서 일어나도 상관없습니다.
    - 이것은 향상된 for문(`foreach`)의 "assignment-operator-like"콜론에도 적용됩니다.
3. 메서드 또는 생성자 이름 뒤에 여는 괄호가 있을 때 줄바꿈을 합니다..
4. 콤마 앞에 오는 토큰에 연결되어 있을 때 줄바꿈을 합니다.
5. 줄은 람다식의 인접한 화살표에서는 바뀌지 않습니다.. 하지만 람다의 몸체가 한 줄로 되어 있다면 바꿔도 됩니다. 예를들면:
```java
MyLambda<String, Long, Object> lambda =
    (String label, Long value, Object obj) -> {
        ...
    };

Predicate<String> predicate = str ->
    longExpressionInvolving(str);
```
> 참고: 줄 바꿈의 주요 목표는 코드를 명확하게 하는 것입니다. 코드 가 반드시 가장 적은 수의 줄에 들어가는 것은 아닙니다.
### 4.5.2 들여쓰기 지속은 최소 +4 스페이스
줄 바꿈을 할 때, 첫 번째 줄 이후의 각 줄은 원래의 줄에서 최소 +4스페이스만큼 들여쓰기를 해줘야합니다.

연속된 행이 여러개인 경우 들여쓰기는 +4이상으로 변경 가능합니다. 일반적으로 두 개의 연속된 행은 구문적으로 병렬인 요소로 사용할 때 동일한 들여쓰기의 레벨을 사용합니다.

수평 정렬에 관한 섹션 4.6.3은 특정 토큰을 이전 라인과 정렬하기 위해 가변적인 숫자의 공간을 사용하는 관행을 다룹니다.


## 4.6 공백
### 4.6.1 수직 공백
하나의 공백 줄은 아래와 같은 상황에 사용합니다.
1. 연속적인 멤버 또는 클래스의 초기화: 필드, 생성자, 메서드, 중첩 클래스, 정적 초기화, 인스턴스 초기화
   - 예외: 연속적인 두개의 필드 사이의 공백은 선택입니다. 이러한 공백은 논리적 그룹을 만드는데 필요합니다.
   - 예외: enum 상수의 공백 줄은 섹션 4.8.1에서 다룹니다.
2. 이 문서의 다른 섹션에서도 필요합니다. (섹션 3의 소스파일 구조, 그리고 섹션 3.3의 import구문)

하나의 공백 줄은 어디서나 가독성을 향상시킬 수 있습니다. 예를들어 코드를 논리적으로 나눌 때 사용합니다. 첫 번째 멤버 나 초기화 앞에있는 빈 줄 또는 클래스의 마지막 멤버 나 초기화 뒤에 오는 공백 줄은 권장되거나 권장되지 않는다.

다수의 연속된 공백 줄은 허용되지만 요구되지 않습니다.

### 4.6.2 가로 공백
리터럴, 주석 및 Javadoc을 제외하고 언어 또는 기타 스타일 규칙이 필요한 곳을 넘어서는 경우 단일 ASCII 공간이 다음 위치에만 나타납니다

1. 예약어를 나누는 경우, `if`, `for`, `catch`와 같은 예약어 이후 나오는 여는 괄호`(`에서 사용한다.
2. 예약어를 나누는 경우, `else`, `catch`같은 예약어 이후 닫는 중괄호`}`에서 사용한다.
   - ex) `} else {`, `} catch {`
3. 아래 2가지 경우를 제외하고 중괄호`{` 를 열기 전에 사용한다.
   - `@SomeAnnotation({a, b})` (no space is used)
   - `String[][] x = {{"foo"}}`
4. 이항 또는 삼항 연산자의 양쪽에 사용한다. 이것은 "operator-like"기호에도 적용된다.
   - 인접한 타입 바인딩의 엠퍼센드 연산자에서 사용한다. `<T extends Foo & Bar>`
   - 여러 예외를 처리하는 catch블록 프이프에서 사용한다. `catch (FooException | BarException e)`
   - 향상된 for("foreach")문의 콜론(` : `)에서 사용한다.
   - 화살표를 사용한 람다식 표현에서 사용한다. `(String str) -> str.length()`
   - 하지만 두개의 콜론(`::`)을 사용하는 경우 공백을 주지 않는다. `Object::toString`
   - dot(.)을 사용하는 경우에도 공백을 주지 않는다. `object.toString()`
5. `,:;` 혹은 캐스팅 할 때 닫는 괄호`)` 뒤에서 사용한다.
   - `Parent child = (Parent) childObject`
6. 더블 슬래시`//`에서 양쪽에 사용한다. 여러 수의 공백이 허용되지만 필수사항은 아니다.
7. 타임과 변수 선언 사이에 사용한다. `List<String> list`
8. 선택사항 : 배열 선언문 사이의 공백은 선택사항이다.
   - `new int[] {5, 6}` 와`new int[] { 5, 6 }`는 모두 가능하다.
9. 타임 어노테이션과 `[]` 또는 `...`사이에 사용한다.

이 규칙은 라인의 시작 또는 끝에 추가 공백을 요구하거나 금지하는 것이 아닙니다. 이 규칙들은 내부 공간만을 다루는 내용입니다.

### 4.6.3 수평정렬: 요구되지 않음
용어 정리: 수평정렬(Horizontal alignment)은 특정 토큰이 이전 줄의 특정 다른 토큰 아래에 표시하기위해 다양한 수의 추가 공백을 추가하는 방법입니다.

이러한 작업은 허용되지만 Google style에서는 요구되지 않습니다.(필수사항 X) 
심지어 이미 쓰고 있는 상황에서도 수평 정렬을 유지하는게 필수가 아니라고 말합니다.

여기 수평정렬의 예시가 있습니다.
```java
private int x; // this is fine
private Color color; // this too

private int   x;      // permitted, but future edits
private Color color;  // may leave it unaligned
```
> 팁: 정렬은 가독성을 높여줍니다. 하지만 이것은 추후의 유지관리에 문제점을 만들 수 있습니다. 추후에 한 줄만 수정한다고 가정해보면 이 변경으로 이전에 맞춰둔 서식이 엉망이 될 수 있습니다. 허용은 됩니다. 그리고 이것은 개발자가 근처 줄의 공백을 조정하도록 촉구하며 연속적인 재형식화를 유발할 수 있습니다. 최악의 경우 혼잡하지 않은 상황이 될 수 있지만 근본적으로 버전 히스토리 정보가 손상되고 검토자를 드리게하고 merge충돌이 일어나게 됩니다.

## 4.7 소괄호 그룹
선택적 그룹화 괄호는 작성자와 검토자가 코드 없이 코드를 잘못 해석하거나 코드를 읽기 쉽게 만들 가능성이 없다는 데 동의하는 경우에만 생략된다. 모든 독자가 전체 Java 연산자 우선 순위 테이블을 암기한다고 가정하는 것은 타당하지 않습니다. -> 가능하면 소괄호로 감싸라~

## 4.8 특별한 구조
### 4.8. Enum 클래스
enum 상수 뒤에 따라오는 각각의 콤마 뒤의 줄바꿈은 선택사항입니다. 추가로 공백줄도 허용됩니다. (일반적으로 하나만 허용)
```java
private enum Answer {
  YES {
    @Override public String toString() {
      return "yes";
    }
  },

  NO,
  MAYBE
}
```
메서드가 없거나 documentation이 없는 enum클래스는 배열 초기화(initializer)와 같은 포멧으로 작성될 수 있습니다. (섹션 4.8.3.1참조)
```java
private enum Suit { CLUBS, HEARTS, SPADES, DIAMONDS }
```
enum클래스는 클래스이므로 다른 클래스 포메팅 규칙들이 적용됩니다.

### 4.8.2 변수 선언
#### 4.8.2.1 선언당 하나의 변수만!
모든 변수 선언은 한 개씩합니다. 
`int a, b;`와 같은 선언은 사용하지 않습니다. 

예외: 여러개의 변수 선언은 for문의 헤더에만 허용합니다.

#### 4.8.2.2 필요할 때 선언
지역변수는 블럭이나 block-like construct가 시작될 때 습관적으로 쓰일 필요는 없습니다. 그 대신, 지역 변수는 범위를 좁히기 위해 그것들이 처음 사용될 때 가까운 위치에 선언합니다. (이유가 있어야한다.) 지역 변수 선언은 전형적으로 초기화를 시키거나 선언과 동시에 초기화를 시킵니다.

### 4.8.3 배열
#### 4.8.3.1 배열 초기화는 "block-like"에서 가능하다.
배열 초기화는 "block-like construct"처럼 포매팅 될 수 있습니다.
예를 들면 아래의 경우는 모두 가능합니다.
```java
new int[] {           new int[] {
  0, 1, 2, 3            0,
}                       1,
                        2,
new int[] {             3,
  0, 1,               }
  2, 3
}                     new int[]
                          {0, 1, 2, 3}
```

#### 4.8.3.2 C스타일의 배열 선언은 사용 No
대괄호는 변수 옆이 아닌 타입 옆에 붙어야합니다. 
`String[] args` -> O
`String args[]` -> X

### 4.8.4 Switch구문
용어 정리: switch블럭의 중괄호 안에는 여러개의 구문 그룹들이 들어간다. 각각의 구문 그룹들은 하나 또는 여러개의 switch라벨 (`case FOO:`또는 `default:`)이 붙습니다.

#### 4.8.4.1 들여쓰기
다른 블록들처럼 switch블록의 들여쓰기는 +2입니다.

switch라벨 이후 줄 바꿈이 오고 들여쓰기 레벨이 +2가 됩니다. 그 다음에 오는 switch라벨은 이전의 들여쓰기 레벨을 따릅니다.

#### 4.8.4.2 실패: 주석
switch블록 안에서 구문 그룹들은 갑자기 종료될 수 있습니다.(`break`, `continue`, `return` 또는 exception에 의해서) 혹은 다음 구문으로 실행이 넘어가는 것을 표시할 수 있습니다. 이러한 상황에서는 (`// fall through`)와 같은 주석을 달 수 있습니다. 이 특별한 주석은 마지막 구문에는 올 필요가 없습니다. 
```java
switch (input) {
  case 1:
  case 2:
    prepareOneOrTwo();
    // fall through
  case 3:
    handleOneTwoOrThree();
    break;
  default:
    handleLargeNumber(input);
}
```
`case1`에서는 뒤에 주석이 필요없습니다. 오직 마지막 구문에만 쓰이면 됩니다.


#### 4.8.4.3 `default`케이스가 존재합니다
각각의 switch구문은 `default`구문에 코드가 없더라도 `default`구문을 포함하고 있습니다.

예외: enum타입의 switch구문은 다른 모든 경우의 처리를 다 하였다면 default를 생략 가능합니다. 이것은 IDE나 다른 정적 분석 둘에서 놓친 것들을 경고해줍니다.

### 4.8.5 어노테이션
어노테이션을 documentation block이후에 클래스나 함수 혹은 생성자에 적용됩니다. 그리고 각각의 어노테이션들은 그들만의 줄을 가지고 있습니다. (즉, 한줄에 하나의 어노테이션을 갖고 있다.) 이것들의 줄바꿈(개행)은 줄바꿈에 해당되지 않습니다. 그래서 들여쓰기 레벨이 증가하지 않습니다. 
Example
```java
@Override
@Nullable
public String getNameIfPresent() { ... }
```

예외: 파라미터가 없는 단일 어노테이션은 다음 예제와 같이 한줄에 쓸 수 있습니다.
```java
@Override public int hashCode() { ... }
```

필드에 적용되는 어노테이션은 한 줄에 쓸 수 있습니다. 이 경우 여러개의 어노테이션(파라미터가 있어도 가능)이 한 줄에 쓸 수 있습니다.
```java
@Partial @Mock DataLoader loader;
```

파라미터나 지역변수, 타입에 적용되는 어노테이션들의 특정한 포매팅 규칙은 없습니다.

### 4.8.6 주석
이 섹션은 구현 주석에 대한 내용입니다. Javadoc의 섹션 7에 있습니다.

모든 줄 바꿈 앞에는 임의의 공백이 오고 그 뒤에는 구현 주석이 올 수 있습니다. 이러한 주석은 행을 비어있지 않게 만듭니다.

#### 4.8.6.1 블럭 주석 스타일
블럭 주석 스타일은 둘러쌓인 코드와 같은 들여쓰기 레벨을 가집니다. 
`/* ... */` 나 `// ...`의 스타일을 가집니다.
여러 줄의 `/* ... */`주석은 각각의 줄이 `*`로 시작하여야합니다.
```java
/*
 * This is          // And so           /* Or you can
 * okay.            // is this.          * even do this. */
 */
```
주석은 별표 또는 기타 문자로 그려진 박스에 넣지 않습니다.

> 팁: 여러 줄의 주석에는 문단 형식으로 re-wrap을 하고 싶다면 `/* ... */`를 사용합니다. 대부분의 포매터들은 `// ...`스타일의 주석 블록의 re-wrap을 지원하지 않습니다.

### 4.8.7 접근 제한자
클래스 및 멤버 수정자가 존재하는 경우 아래의 Java에서 권장하는 순서대로 나타냅니다.
```java
public protected private abstract default static final transient volatile synchronized native strictfp
```

### 4.8.8 숫자 리터럴
long의 값을 가지는 정수 리터럴은 대문자 L의 접미사를 가집니다. 
소문자를 사용하는 경우 1과 헷갈릴 수 있어서 대문자를 사용합니다.
예를 들면 3000000000l 대신 3000000000L을 사용합니다.

<hr>

# Naming
## 5.1 모든 식별자에 대한 공통 규칙
식별자는 반드시 ASCII문자와 숫자를 사용해야합니다. 그리고 가끔은 userscore(`_`)를 이용하기도 합니다. 그러므로 유효한 식별자의 이름은 정규식 `\w+`와 매칭됩니다.

구글 스타일에 따르면 특수한 접미사 또는 접두사는 사용하지 않습니다. 
예를 들면 `name_`, `mName`, `s_name`, `kName`와 같은 식별자는 사용하지 않습니다.

## 5.2 식별자 타입에 대한 규칙
### 5.2.1 패키지 이름
패키지 이름은 모두 소문자로 작성하며 긴 단어의 경우 단순히 연결하여 작성합니다.(언더바도 사용 금지)
- com.example.deepspace (O)
- com.example.deepSpace (X)
- com.example.deep_space (X)

### 5.2.2 클래스 이름
클래스의 이름은 **UpperCamelCase**로 작성합니다.

클래스의 이름은 일반적으로 명사나 명사구입니다. 예를 들면 `Character`또는 `ImmutableList`를 사용합니다.
인터페이스의 이름 또한 명사나 명사구를 사용합니다. 하지만 가끔씩 형용사나 형용사구가 사용되기도 합니다.(ex. `Readable` ) 

어노테이션은 따로 잘 만들어진 규칙이 없습니다.

테스트 클래스들은 테스트 하고자하는 클래스의 이름이 앞에 오고 `Test`를 붙여줍니다. 
예를 들면 `HashTest`, `HashIntegrationTest`와 같이 정합니다.

### 5.2.3 메서드 이름
메서드의 이름도 **UpperCamelCase**로 작성합니다.

메서드의 이름은 일반적으로 동사나 동사구를 사용합니다. 예를 들면 `sendMessage` 또는 `stop`이 있습니다.

Underscore(`_`)는 JUnit테스트에서 논리적 컴포넌트를 분리시키기 위해 각각의 lowerCamelCase로 변경시켜 나올 수 있습니다. 
하나의 전형적인 패턴은 `<methodUnderTest>_<state>` 입니다. 예를 들면 `pop_emptyStack`와 같이 합니다. 테스트 메서드 이름엔 정답이 없습니다.

### 5.2.4 상수 이름
상수의 이름은 CONSTANT_CASE를 사용합니다. CONSTANT_CASE는 모두 대문자이며 각각의 단어는 언더스코어(`_`)로 구분합니다. 하지만 상수가 정확히 무엇일까요?

상수는 변경될 수 없고 그것들의 메서드에서 부작용이 보여서는 안되는 `static final` 필드를 의미합니다. 이것은 primitive, String, 불변타입 그리고 불변 컬렉션을 포함합니다. 만약 어떠한 인스턴스의 상태가 바뀐다면 그것은 상수가 아닙니다. 

```java
// Constants
static final int NUMBER = 5;
static final ImmutableList<String> NAMES = ImmutableList.of("Ed", "Ann");
static final ImmutableMap<String, Integer> AGES = ImmutableMap.of("Ed", 35, "Ann", 32);
static final Joiner COMMA_JOINER = Joiner.on(','); // because Joiner is immutable
static final SomeMutableType[] EMPTY_ARRAY = {};
enum SomeEnum { ENUM_CONSTANT }

// Not constants
static String nonFinal = "non-final";
final String nonStatic = "non-static";
static final Set<String> mutableCollection = new HashSet<String>();
static final ImmutableSet<SomeMutableType> mutableElements = ImmutableSet.of(mutable);
static final ImmutableMap<String, SomeMutableType> mutableValues =
    ImmutableMap.of("Ed", mutableInstance, "Ann", mutableInstance2);
static final Logger logger = Logger.getLogger(MyClass.getName());
static final String[] nonEmptyArray = {"these", "can", "change"};
```
이것들의 이름은 일반적으로 명사나 명사구를 사용합니다.

### 5.2.5 상수가 아닌 필드의 이름
상수가 아닌 필드의 이름은 **lowerCamelCase**를 사용합니다.

이름은 전형적으로 명사나 명사구를 이용합니다. 
ex) `computedValues`, `index` 

### 5.2.6 파라미터의 이름
파라미터의 이름은 **lowerCamelCase**를 사용합니다.

public 메서드에서 하나의 문자를 사용한 파라미터의 이름은 피해야합니다.

### 5.2.7 지역 변수의 이름
지역 변수의 이름은 **lowerCamelCase**를 사용합니다.

심지어 `final`이나 불변, 지역 변수는 상수로 간주되어서는 안되고 상수 스타일로 기술해서도 안됩니다.

### 5.2.8 타입 변수의 이름
각각의 변수 타입은 아래의 두가지 스타일을 따릅니다.

- 하나의 대문자, 선택적으로 뒤에 하나의 숫자가 따라올 수 있다.
ex) `E`, `T`, `X`, `T2`
- 클래스를 위해서 사용되는 이름의 형식에 `T`대문자가 따라오는 형식
ex) `RequestT`, `FooBarT`

## 5.3 Camel case: defined
때때로 영어 구문을 카멜 케이스로 바꾸는 이유가 하나 이상 존재한다. 예를 들어, "IPv6" 또는 "iOS"와 같은 특이한 구조나 두문자어가 있을 때처럼 말이다. 예측 가능성을 높이기 위해 Google Style은 다음과 같은 결정론적 체계를 지정합니다.

이름을 산문 형태로 시작한다:
1. 구를 평문의 ASCII로 변경하고 어퍼스트로피(`'`)를 없앤다. 예를 들면 "Müller's algorithm"를 "Muellers algorithm"로 변경한다.
2. 결과를 단어로 나누고 남은 공백과 구두점으로 나눈다. (일반적으로 하이픈(`-`)으로 나눈다)
   - 추천 : 어떤 단어가 이미 카멜 케이스 방식이 쓰인다면 이것을 구성하는 부분들로 나눈다. (ex. "AdWords" -> "ad words")
   ※ iOS는 카멜 케이스가 아니다. 이 부분은 어떠한 약속에도 위배됨으로 적용하지 않는다.
3. 이제 모두 lowercase로 바꾸고 그리고 첫번째 글자를 대문자로 바꿔줍니다.
   - 각각의 단어는 Upper camel case를 적용한다.
   - 첫번째로 오는 단어는 lower camel case를 적용한다.
4. 마침내 단어들을 하나의 식별자로 합칩니다.

![](https://images.velog.io/images/seongwon97/post/54ae9849-51c5-4e21-9660-0dbc76a02d8e/image.png)

*은 사용가능하나 권장되지 않는다.

> 노트: 일부 단어들은 영어에서 모호하게 하이픈(`-`)으로 연결된다. 예를 들어 "nonempty"와 "non-empty"가 모두 올바르므로 메서드 이름 checkNonempty와 checkNonempty도 모두 정확합니다.

<hr>

# Reference
https://google.github.io/styleguide/javaguide.html#s4.8.1-enum-classes
https://github.com/JunHoPark93/google-java-styleguide
https://github.com/binghe819/TIL/blob/master/JAVA/%EA%B8%B0%ED%83%80/google%20java%20style%20guide.md