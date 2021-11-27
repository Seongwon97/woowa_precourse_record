# Unit Test란? 
Unit test는 프로그래밍을 할 때 소스코드의 특정 모듈(메서드)이 의도된 대로 정확히 작동하는지 검증하는 절차입니다. 다시 말해 작성한 모든 메서드들에 대해서 테스트케이스를 작성하는 것을 의미합니다.

## Unit Test의 장점
Unit Test를 진행하게 된다면 <font color = "red">하나의 기능을 독립적으로 테스트</font>를 하며 코드 변경으로 인해 문제가 발생하여도 짧은 시간안에 해당 문제를 파악할 수 있습니다. 
- 새로운 기능 추가 시 수시로 빠르게 테스트 할 수 있다.
- 리팩토링 시에 안정성을 확보할 수 있다.
- 테스팅에 대한 시간과 비용을 절감할 수 있다.
- 코드에 대한 문서가 될 수 있다.

이러한 장점들이 있어 실무에서는 Unit Test를 선호하고 있다고 합니다.


## 좋은 Unit Test를 하는 방법
- 1개의 테스트 함수에 대해서는 assert를 최소화해야한다.
- 1개의 테스트 함수에는 1가지 개념만을 테스트하여야 한다.

좋고 깨끗한 테스트 코드는 FIRST라는 5가지 규칙을 따라야 한다.
- Fast: 테스트는 빠르게 동작하여 자주 돌릴 수 있어야 한다.
- Independent: 각각의 테스트는 독립적이며 서로 의존해서는 안된다.
- Repeatable: 어느 환경에서도 반복 가능해야 한다.
- Self-Validating: 테스트는 성공 또는 실패로 bool 값으로 결과를 내어 자체적으로 검증되어야 한다.
- Timely: 테스트는 적시에 즉, 테스트하려는 실제 코드를 구현하기 직전에 구현해야 한다.


> 출처: https://mangkyu.tistory.com/143 [MangKyu's Diary]


## Java의 간단한 Unit Test
- Java의 Unit Test는 최근에는 JUnit5와 AssertJ 라이브러리를 많이 사용한다고 합니다.
   - JUnit5 : 자바의 단위 테스트를 위한 테스팅 프레임워크
   - AssertJ : 자바의 단위 테스트를 돕기 위해 다양한 문법을 지원하는 라이브러리
   <br>
- 요즘 Unit test는 주로 한개의 단위를 3가지로 나눠서 처리하는 given-when-then패턴을 사용하는 추세라 한다.
   - given(준비) : 어떠한 데이터가 준비되었을 때
   - when(실행) : 어떠한 함수를 실행하면! (조건을 지정한다고 생각하면 된다.)
   - then(검증) : 어떠한 결과가 나와한다.
   - verify : 메서드가 호출된 횟수, 타임아웃 시간 체크를 검사할 때 사용 (부가적)

> `when()`에서 메서드를 통해 조건을 걸 때 매개변수 값이 어떤 값이라도 상관없으면 `any..`로 시작하는 메서드를 사용하며 특정 값을 넣어야 한다면 `eq()`메서드를 활용합니다.
example ) 
```java 
when(cal.add(anyInt(), anyInt())). ~~
when(cal.add(eq(1), eq(4)). ~~
```

**옳바른 입력값 테스트**
```java
    @DisplayName("옳바른 입력값 테스트")
    @Test
    void validInputTest() {
        // given
        final InputUtil inputUtil = new InputUtil();
        System.setIn(new ByteArrayInputStream("123".getBytes()));

        // when
        final List<Integer> userInput = inputUtil.getPlayerAnswer();

        // then
        Integer[] resultArr = {1, 2, 3};
        List<Integer> result = Arrays.asList(resultArr);
        assertThat(userInput).isEqualTo(result);
    }
```

**잘못된 입력값 길이 테스트 (2자리 수)**
```java
    @DisplayName("잘못된 입력값 길이 테스트 (2자리 수)")
    @Test
    void invalidInputLengthTest1() {
        // given
        final InputUtil inputUtil = new InputUtil();
        System.setIn(new ByteArrayInputStream("96".getBytes()));

        // when
        final RuntimeException exception = assertThrows(
                RuntimeException.class, () -> inputUtil.getPlayerAnswer());

        // then
        assertThat(exception.getMessage()).isEqualTo(INPUT_ERROR_LENGTH);
    }
```

`@Test`는 해당 메서드가 Unit test라는 것을 명시해주는 어노테이션입니다.
`@DisplayName`를 붙이면 테스트를 실행 하였을 때 나오는 테스트 이름이 바뀌게 됩니다.
<img src="https://images.velog.io/images/seongwon97/post/67f3a51c-6519-47e0-bbc7-47c42771ce06/image.png">


## Stub, Mock의 개념
프로그래밍을 하게되면 대부분의 객체는 메서드를 시행할 때 다른 객체들과 메시지를 주고받으면서 작업을 하는 것을 알 수 있습니다. 하지만 Unit test는 하나의 모듈에 대한 독립적인 테스트이기 때문에 다른 객체와 메시지를 주고 받는 경우 문제가 발생합니다. 이를 해결하기 위해 메시지를 주고받는 다른 객체 대신에 <font color = "red">**Mock Object(가짜 객체)**</font>를 생성하여 테스트합니다.
또 생성한 Mock Object를 주입하고 어떤 결과를 반환하라고 정해진 결과값을 준비하는데 이것을 <font color = "red">**Stub**</font>이라고 합니다. 

> **Mock** : Unit Test를 할 모듈과 메시지를 주고받는 객체를 대신할 가짜 객체이다.
**Stub** : 실제 코드나 아직 준비되지 못한 코드를 호출하여 수행할 때 호출된 요청에 대해 미리 준비해둔 결과를 제공하는 테스트 메커니즘이다. 

## Mockito란?
Mockito는 개발자가 동작을 직접 제어할 수 있는 가짜(Mock) 객체를 지원하는 테스트 프레임워크입니다. 앞서 말했듯이 개발을 하다보면 객체들간의 의존성이 존재하여 테스트의 어려움이 있습니다. 이러한 문제를 해결하기 위해 Mock Object를 만들어 주입시켜주는데 이를 지원해주는 라이브러리가 Mockito입니다.

>※ Mockito는 과거에는 static메서드를 지원하지 않는 단점이 있어 PowerMock을 대안으로 사용했으나 Mockito 3.4.0부터는 static method도 지원하고 있습니다.

### Mockito사용법
#### 1. Mock 객체의 의존성 주입
Mockito에서 Mock object에 의존성을 주입하기 위해서는 크게 3가지의 어노테이션을 사용합니다.
- `@Mock` : Mock 객체를 만들어 반환해주는 어노테이션

> `@Mock`을 사용하지 않고 `mock()`메서드를 통해서도 mock객체를 만들 수 있다.
example) 
```java
Hint hint = mock(Hint.class);
```

- `@Spy`: Stub하지 않은 메소드들을 원본 메소드 그대로 사용하는 어노테이션
- `@InjectMocks`: @Mock 또는 @Spy로 생성된 가짜 객체를 자동으로 주입시켜주는 어노테이션

#### 2. Stub
Mockito에서는 아래의 stub메서드들을 지원하고 있습니다.
`doReturn()`: Mock 객체가 특정한 값을 반환해야 하는 경우 사용
```java
    doReturn(3).when(p).add(anyInt(), anyInt());
```
`doNothing()`: Mock 객체가 아무 것도 반환하지 않는 경우 사용(void)
```java
@Test
public void example(){
    Person p = mock(Person.class);
    doNothing().when(p).setAge(anyInt());
    p.setAge(20);
    verify(p).setAge(anyInt());
}
```
`doThrow()`: Mock 객체가 예외를 발생시키는 경우 사용
```java
@Test(expected = IllegalArgumentException.class)
public void example(){
    Person p = mock(Person.class);
    doThrow(new IllegalArgumentException()).when(p).setName(eq("JDM"));
    String name = "JDM";
    p.setName(name);
}
```

### Stub의 doReturn, thenReturn의 차이
Mockito를 사용하다보면 코드들에 `doThrow`, `doReturn`등의 형식이 아닌 `thenReturn()`, `thenThrow()`등의 형식도 볼 수 있을 것이다. 둘의 차이점은 다음과 같습니다.

**doReturn**
- <font color = "red">실제로 메서드를 호출</font>하고 리턴값을 임의로 정의할 수 있다.
- 메서드를 실제로 수행하여 메서드 작업이 오래걸릴 경우 끝날 때 까지 기다려야한다.
- 실제 메서드를 호출하기 때문에 대상 메서드에 문제점이 있을 경우 발견할 수 있다.
```java
doReturn(6).when(cal).add(2, 4);
```

**thenReturn**
- 메서드를 <font color = "red">실제로 호출하지 않으며</font> 리턴값을 임의로 정의할 수 있다.
- 실제 메서드를 호출하지 않기 때문에 대상 메서드에 문제점이 있어도 알 수 없다.

```java
when(cal.add(2, 4)).thenReturn(6); 
```

<hr>

# Reference
- https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%9B_%ED%85%8C%EC%8A%A4%ED%8A%B8
- https://mangkyu.tistory.com/143
- https://mangkyu.tistory.com/144
- https://beomseok95.tistory.com/294
- https://jdm.kr/blog/222
- https://medium.com/@SlackBeck/mock-object%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-85159754b2ac
- https://medium.com/@SlackBeck/%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%8A%A4%ED%85%81-test-stub-%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-ff9c8840c1b0
- https://m.blog.naver.com/inho1213/80110527396
- https://royleej9.tistory.com/entry/Mockito-doReturn-thenReturn
- http://daplus.net/java-mockito-doreturn-%EA%B3%BC-when-%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90/