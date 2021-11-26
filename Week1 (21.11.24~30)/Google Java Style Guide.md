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

# Formatting
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
### 4.5.
### 4.5.
### 4.5.

<hr>

# Reference
https://google.github.io/styleguide/javaguide.html#s4.8.1-enum-classes
https://github.com/JunHoPark93/google-java-styleguide
https://github.com/binghe819/TIL/blob/master/JAVA/%EA%B8%B0%ED%83%80/google%20java%20style%20guide.md