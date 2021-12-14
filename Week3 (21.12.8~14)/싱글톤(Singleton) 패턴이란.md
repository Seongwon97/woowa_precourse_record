# 싱글톤 패턴이란?
> 소프트웨어 디자인 패턴에서 싱글턴 패턴(Singleton pattern)을 따르는 클래스는, 생성자가 여러 차례 호출되더라도 실제로 생성되는 객체는 하나이고 최초 생성 이후에 호출된 생성자는 최초의 생성자가 생성한 객체를 리턴한다. 이와 같은 디자인 유형을 싱글턴 패턴이라고 한다. 주로 공통된 객체를 여러개 생성해서 사용하는 DBCP(DataBase Connection Pool)와 같은 상황에서 많이 사용된다. 
- 출처: [위키피디아](https://ko.wikipedia.org/wiki/%EC%8B%B1%EA%B8%80%ED%84%B4_%ED%8C%A8%ED%84%B4)

간단히 설명하면 싱글톤 패턴은 **객체의 인스턴스를 한개만 생성되게 하는 패턴**입니다.

이러한 패턴은 주로 프로그램 내에서 하나로 공유를 해야하는 객체가 존재할 때 해당 객체를 싱글톤으로 구현하여 모든 유저 또는 프로그램들이 해당 객체를 공유하며 사용하도록 할 때 사용됩니다. 

즉, 싱글톤 패턴은 아래와 같은 상황에 사용을 합니다.
- 프로그램 내에서 하나의 객체만 존재해야 한다.
- 프로그램 내에서 여러 부분에서 해당 객체를 공유하여 사용해야한다.

## 싱글톤 패턴을 사용하는 이유
하나의 인스턴스만을 사용하는 싱글톤 패턴의 이점은 다음과 같습니다

**1. 메모리 측면의 이점**
싱글톤 패턴을 사용하게 된다면 한개의 인스턴스만을 고정 메모리 영역에 생성하고 추후 해당 객체를 접근할 때 메모리 낭비를 방지할 수 있다.

**2. 속도 측면의 이점**
생성된 인스턴스를 사용할 때는 이미 생성된 인스턴스를 활용하여 속도 측면에 이점이 있다.

**3. 데이터 공유가 쉽다**
전역으로 사용하는 인스턴스이기 때문에 다른 여러 클래스에서 데이터를 공유하며 사용할 수 있다. 하지만 동시성 문제가 발생할 수 있어 이 점은 유의하여 설계하여야 한다.

## 싱글톤 패턴 구현하기
```java
public class Singleton {
    // 단 1개만 존재해야 하는 객체의 인스턴스로 static 으로 선언
    private static Singleton instance;

    // private 생성자로 외부에서 객체 생성을 막아야 한다.
    private Singleton() {
    }

    // 외부에서는 getInstance() 로 instance 를 반환
    public static Singleton getInstance() {
        // instance 가 null 일 때만 생성
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}

```
싱글톤 패턴의 기본적인 구현 방법은 다음과 같습니다.

먼저 `private static`으로 Singleton객체의 instance를 선언하고 `getInstance()`메서드가 처음 실행 될 때만 하나의 instance가 생성되고 그 후에는 이미 생성되어진 instance를 return하는 방식으로 진행이 됩니다.

여기서 핵심은 `private`로 된 기본 생성자입니다. 생성자를 `private`로 생성을 하며 외부에서 새로운 객체의 생성을 막아줘야 합니다.


```java
// 같은 instance인지 Test
public class Application {
    public static void main(String[] args) {
        Singleton singleton1 = Singleton.getInstance();
        Singleton singleton2 = Singleton.getInstance();

        System.out.println(singleton1);
        System.out.println(singleton2);
    }
}
        /** Output
         * vendingmachine.Singleton@15db9742
         * vendingmachine.Singleton@15db9742
         **/
```
싱글톤 객체를 생성하는 위의 코드를 실행해보면 두 객체가 하나의 인스턴스를 사용하여 같은 주소 값을 출력하는 것을 확인하실 수 있습니다.

## Multi-thread에서의 싱글톤
multi-thread환경에서 싱글 톤을 사용하게 된다면 다음과 같은 문제가 발생할 수 있습니다.

### 문제점1. 여러개의 인스턴스 생성
Multi-thread환경에서 instance가 없을 때 동시에 아래의 `getInstance()`메서드를 실행하는 경우 각각 새로운 instance를 생성할 수 있습니다.
```java
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
```

### 문제점 2. 변수 값의 일관성 실패
다음과 같은 코드가 실행이 되었을 때 여러개의 thread에서 `plusCount()`를 동시에 실행을 한다면 일관되지 않은 값들이 생길 수 있습니다.
```java
public class Singleton {
    private static Singleton instance;
    private static int count = 0;
    
    private Singleton() {
    }
    
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
    
    public static void plusCount() {
        count++;
    }
}
```

### 해결법 1. 정적 변수선언에서 인스턴스 생성
이러한 문제는 아래와 같이 `static` 변수로 singleton 인스턴스를 생성하는 방법으로 해결할 수 있습니다. 아래와 같이 초기에 인스턴스를 생성하게 된다면 multi-thread환경에서도 다른 객체들은 getInstance를 통해 하나의 인스턴스를 공유할 수 있습니다. 
```java
public class Singleton {
    private static Singleton instance = new Singleton();

    private Singleton() {
    }

    public static Singleton getInstance() {
        return instance;
    }
}
```
### 해결법 2. synchronzied의 사용
아래의 코드와 같이 `synchronzied`를 적용하여 multi-thread에서의 동시성 문제를 해결하는 방법입니다. 하지만 **해당 방법은 Thread-safe를 보장하기 위해 성능 저하가 발생**할 것입니다.
```java
public class Singleton {
    public class Singleton {
        private static Singleton instance;

        private Singleton() {}
        
        public static synchronzied Singleton getInstance() {
            if(instance  == null) {
                instance  = new Singleton();
            }
            return instance;
        }
    }
}
```

# 결론
싱글톤 패턴은 메모리, 속도, 데이터 공유 측면에서 이점이 있습니다. 하지만 그렇다고 해서 싱글톤 패턴이 무조건 좋은 것은 아닙니다. 앞서 말했듯이 multi-thread환경에서는 동시성 문제가 발생할 수 있기에 싱글톤 패턴을 사용하고자 한다면 사용하기 앞서 "해당 객체의 인스턴스가 한 개만 존재하여야 하는지?"의 여부와 "사용을 하였을 때의 동시성 문제가 발생하지 않는지"를 체크를 하며 사용해야 할 것 같습니다.

# Reference
- https://tecoble.techcourse.co.kr/post/2020-11-07-singleton/
- https://velog.io/@kyle/%EC%9E%90%EB%B0%94-%EC%8B%B1%EA%B8%80%ED%86%A4-%ED%8C%A8%ED%84%B4-Singleton-Pattern
- https://injae-kim.github.io/dev/2020/08/06/singleton-pattern-usage.html
