# Introduction
이번 우테코 프리코스를 진행하며 `NsTest` 클래스의 메서드들과 `Assert` 클래스의 메서드를 활용한 테스트 코드를 실행시키다보니 막히는 부분이 있어 정리를 하게 되었습니다.

이번 정리를 통해 앞으로 남은 2주간의 프리코스 과정과 2차 테스트에서의 테스트는 지금보다 쉽게 진행할 수 있을 것이라 믿습니다😁😁

<hr>

## camp.nextstep.edu.missionutils.test.NsTest
아래에서 볼 테스트 코드의 람다식을 보면 `run`메서드를 확인할 수 있습니다.
해당 메서드에 대해 먼저 알아보도록 하겠습니다.

### run
```java
    protected final void run(final String... args) {
        command(args);
        runMain();
    }
```

#### command
```java
    private void command(final String... args) {
        final byte[] buf = String.join("\n", args).getBytes();
        System.setIn(new ByteArrayInputStream(buf));
    }
```
command메서드는 `String.join`을 통해 parameter값들 사이에 `\n`을 넣어 배열을 연결시키고 `System.setIn`을 이용하여 콘솔을 통해 입력이 되는 것 처럼 해주는 메서드 입니다. 

#### runMain
```java
    protected abstract void runMain();
```
runMain은 추상 클래스로 테스트할 실제 함수를 통해 override해주고 사용해야합니다. 그래서 실제 ApplicationTest파일을 보면 아래와 같이 재정의된 것을 확인할 수 있습니다.
```java
    // Application.java
    @Override
    public void runMain() {
        Application.main(new String[]{});
    }
```

<hr>

## camp.nextstep.edu.missionutils.test.Assertions

프리코스에서 주어진 Test들은 `assertRandomNumberInRangeTest`와 `assertSimpleTest`메서드를 통해 작성되었습니다. 해당 메서드들은 모두 `camp.nextstep.edu.missionutils.test.Assertions`에 존재하며 두 메서드에 대해 알아보도록 하겠습니다.

### assertSimpleTest
```java
    public static void assertSimpleTest(final Executable executable) {
        assertTimeoutPreemptively(SIMPLE_TEST_TIMEOUT, executable);
    }
```
assertSimpleTest는 파라미터로 들어오는 executable을 `SIMPLE_TEST_TIMEOUT`내에 수행할 수 있는지 확인하는 메서드이다.
   - 여기서 executable은 우테코 1주차 과제의 테스트 코드를 보면 람다식이 위치한 것을 확인할 수 있습니다.
   - SIMPLE_TEST_TIMEOUT은 Assertions클래스 필드에 상수로 정의되어 있으며 실행시간?을 정해주는 변수인것 같습니다. 
```java
    private static final Duration SIMPLE_TEST_TIMEOUT = Duration.ofSeconds(1L);
    private static final Duration RANDOM_TEST_TIMEOUT = Duration.ofSeconds(10L);
```

#### 우테코에서 주어진 Test code
```java
    @Test
    void 예외_테스트() {
        assertSimpleTest(() ->
                assertThatThrownBy(() -> runException("1234"))
                        .isInstanceOf(IllegalArgumentException.class)
        );
    }
```
> - `assertSimpleTest()` : 괄호 안에 있는 식이 1초 이내에 실행되는지 Test한다.
- `assertThatThrownBy()` : 해당 메서드는 괄호안에 위치한 람다식 안에서 발생하는 exception을 throw하는 메서드이다.
- `runException("1234")` : 해당 메서드는 해당 parameter를 통해 main메서드를 시행시키고 exception을 잡아내는 함수이다. 위의 코드에서는 1234의 값을 넣고 실행하였을 때 try/catch문으로 exception을 잡아낸다.
- `.isInstanceOf(IllegalArgumentException.class)` : 발생한 exception가 `IllegalArgumentException.class`인지 확인


<br>

### assertRandomNumberInRangeTest
```java
    public static void assertRandomNumberInRangeTest(
        final Executable executable, // 람다식의 실행할 메서드
        final Integer value, // Input값들?
        final Integer... values
    ) {
        assertRandomTest(
            () -> Randoms.pickNumberInRange(anyInt(), anyInt()),
            executable,
            value,
            values
        );
    }
```
해당 메서드는 parameter들을 받아 assertRandomTest를 실행시키는 메서드입니다. 
assertRandomTest에 대해 알아보도록 하겠습니다.

#### assertRandomTest
```java
    private static <T> void assertRandomTest(
        final Verification verification,
        final Executable executable,
        final T value,
        final T... values
    ) {
        assertTimeoutPreemptively(RANDOM_TEST_TIMEOUT, () -> {
            try (final MockedStatic<Randoms> mock = mockStatic(Randoms.class)) {
                mock.when(verification).thenReturn(value, Arrays.stream(values).toArray());
                executable.execute();
            }
        });
    }
```
- `final Verification verification` : `MockedStatic.java`에 위치한 interface이다. (확실히 어떠한 역할을 하는지 모르겠으나 `When()`의 parameter로 쓰이는 것으로 보아 MockedStatic 객체의 메서드를 하나씩 실행시키도록 해주는 것 같습니다.)
> Mockito 는 Java 에서 테스트 코드를 작성하는데 자주 사용되는 테스트 프레임워크입니다.
기존의 Mokito에서는 static method에 대에서는 Mocking을 지원하지 않았으나 업데이트가 되며 현재는 static method도 테스트 할 수 있게 되었습니다. MockedStatic은 static method를 테스트 할 수 있게 해주는 기능들이 담긴 파일입니다.
- `assertTimeoutPreemptively(RANDOM_TEST_TIMEOUT() -> {})` : 해당 메서드는 10초안에 수행을 마쳐야한다.
- `try (final MockedStatic<Randoms> mock = mockStatic(Randoms.class))` : `Randoms` 클래스의 MockedStatic객체 생성
- `mock.when(verification).thenReturn(value, Arrays.stream(values).toArray());` : verification의 method들을 시행하며 value의 값들을 하나씩 return
- `executable.execute();` : 테스트 수행!!

#### 우테코에서 주어진 Test code
```java
    @Test
    void 게임종료_후_재시작() {
        assertRandomNumberInRangeTest(
                () -> {
                    run("246", "135", "1", "597", "589", "2");
                    assertThat(output()).contains("낫싱", "3스트라이크", "1볼 1스트라이크", "3스트라이크", "게임 종료");
                },
                1, 3, 5, 5, 8, 9
        );
    }
```

# Reference
- https://pjh3749.tistory.com/241
- https://medium.com/@SlackBeck/mock-object%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-85159754b2ac
- https://readystory.tistory.com/190
- https://docs.microsoft.com/ko-kr/dotnet/api/system.string.join?view=net-6.0
- https://www.tutorialkart.com/java/java-system-setin/