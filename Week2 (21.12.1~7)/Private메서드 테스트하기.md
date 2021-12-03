# Introduction
지난주 우테코 프리코스 1주차 과정을 진행하며 Unit Test에 대한 전반적인 내용에 대해 학습하였습니다. 하지만 지난주 공부한 Java의 Unit test 내용으로는 Private 메서드에 대해서는 테스트를 하기 힘들어 Private 메서드를 테스트 하는 방법에 대해 공부해보고자 합니다.

> 1주차에 공부한 Unit Test에 대한 자세한 내용은 [Unit Test (단위 테스트)](https://velog.io/@seongwon97/Unit-Test-%EB%8B%A8%EC%9C%84-%ED%85%8C%EC%8A%A4%ED%8A%B8)를 통해 확인해보실 수 있습니다.


# Private Method의 Unit test
Private 메서드는 기본적으로 외부 클래스에서의 접근이 불간으하여 해당 메서드를 Default메서드로 빼거나 Public메서드로 변경하지 않는 이상 테스트 클래스에서 접근이 불가능합니다. 하지만 테스트를 하기 위해 Private메서드를 Public으로 변경하는 행동을 하는 것은 차라리 테스트를 안하는게 더 낫다고 생각 될 정도로 잘못된 행동이라 생각됩니다. 

일반적으로 Private메서드는 따로 개별적인 Test를 진행하지 않거나 Private메서드를 호출하는 Public메서드를 통해 테스트를 진행한다고 합니다. 하지만 프로그래밍을 하다보면 불가피하게 Private메서드를 테스트 해야할 상황이 분명히 존재할 것이기에 학습을 해보고자 합니다.

Private Method를 테스트 하는 방법은 앞서 말한듯이 Public메서드를 통해 간접 테스트를 하는 방법이 있습니다. 그 외에는 Reflection을 이용하여 테스트하는 방법이 존재합니다. 
Reflection을 통한 private메서드 테스트 방법에 대해 알아보도록 하겠습니다.

## Private Method에 접근하기 (Reflection)
```java
public class InputUtils {
    private static void checkCarNameLength(String carName) {
        if (carName.length() > MAX_CAR_NAME_LENGTH) {
            System.out.println(ERROR_CAR_NAME_LENGTH);
            throw new IllegalArgumentException(ERROR_CAR_NAME_LENGTH);
        }
    }
}
```
예시로 올린 checkCarNameLength()메서드는 carName이 MAX_CAR_NAME_LENGTH를 넘으면 exception을 발생시키는 **private메서드** 입니다. 다음 메서드를 테스트 해보는 코드를 작성하면 다음과 같습니다.

**Test코드**
```java
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

import static constant.Constant.*;
import static org.assertj.core.api.Assertions.assertThat;

class InputUtilsTest {
    @DisplayName("[자동차 이름] 5자리 이상의 이름인 경우")
    @Test
    public void checkCarNameLengthTest() throws NoSuchMethodException {
        InputUtils inputUtils = new InputUtils();
        Method method = inputUtils.getClass().getDeclaredMethod("checkCarNameLength", String.class);
        method.setAccessible(true);

        try {
            method.invoke(inputUtils, "christopher");
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            assertThat(e.getCause().getMessage()).isEqualTo(ERROR_CAR_NAME_LENGTH);
        }
    }
}
```
**1. 먼저 Private 메서드를 가진 Class객체를 생성해줍니다.**
```java
    InputUtils inputUtils = new InputUtils();
```
**2. 테스트하고자 하는 private메서드를 호출하는 Method클래스를 만들어줍니다.**
```java
     Method method = inputUtils.getClass().getDeclaredMethod("checkCarNameLength", String.class);
```
※ 이때의 형식은 `(1에서 만든 클래스 객체).getClass().getDeclaredMethod.(테스트 할 메서드 명, 메서드에 들어갈 파라미터 타입)`과 같은 형태로 입력해줍니다.

**3. Method객체에 담겨있는 private메서드가 접근이 가능하도록 만들어줍니다.**
```java
     method.setAccessible(true);
```
**4. `invoke`를 통해 메서드를 실행시켜줍니다.**
```java
    method.invoke(inputUtils, "christopher");
```
※ 형식은 `method.invoke(1에서 만든 클래스객체, 실제 넣을 파라미터 값)`의 형태로 입력해줍니다.

> Exception확인 테스트를 할 때 지난주에 학습한 바와 같이 `final RuntimeException exception = assertThrows(RuntimeException.class, () -> inputUtil.getPlayerAnswer());`에서 exception의 메시지만을 비교하면 코드를 실행하며 `java.lang.reflect.InvocationTargetException`이 발생하였다며 테스트가 실패하는 것을 확인해 보실 수 있습니다. 
이러한 결과가 나오는 이유는 private메서드를 테스트 하기 위해 사용한 리플렉션 API는 호출된 메서드의 예외를 `InvocationTargetException`로 묶기 때문입니다. 이를 해결하기 위해서는 try/catch를 사용하여 `InvocationTargetException`을 잡고 `getCause()`메서드를 사용하여 원래의 Exception내용을 확인해야합니다.


이와 같이 테스트를 진행하면 테스트가 옳바르게 진행되는 것을 확인할 수 있습니다.

![](https://images.velog.io/images/seongwon97/post/a432ab94-056d-4f17-99a5-b7700f94c054/image.png)

# Reference 
- https://www.crocus.co.kr/1665
- http://daplus.net/java-java-lang-reflect-invocationtargetexception%EC%9D%98-%EC%9B%90%EC%9D%B8%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9E%85%EB%8B%88%EA%B9%8C/
- https://til-perfectacle.netlify.app/2018/07/06/test-private-method-with-reflection/