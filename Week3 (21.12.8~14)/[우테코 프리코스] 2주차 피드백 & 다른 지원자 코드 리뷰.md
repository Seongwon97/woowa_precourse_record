# 3주차 과제를 하기 앞서...
1주차 과제를 한 후 공통 피드백을 받았을 때는 [2주차 과제 회고](https://velog.io/@seongwon97/%EC%9A%B0%ED%85%8C%EC%BD%94-%ED%94%84%EB%A6%AC%EC%BD%94%EC%8A%A4-2%EC%A3%BC%EC%B0%A8-%EA%B3%BC%EC%A0%9C-%ED%9A%8C%EA%B3%A0) 정리에 간단하게 1주차 과제 피드백 리뷰를 정리하였습니다.

지난주와 같이 이번주에 2주차 과제 피드백을 받게 되었고 피드백을 읽다보니 지난주 과제에 지켜지지 않은 점들이 보여 따로 정리를 하게 되었습니다. 또한 다른 지원자들의 코드와 제가 작성한 코드를 비교하면서도 이번 3주차 과제를 하기 앞서 알아두면 좋은 개발 지식, 개선해야할 점 등을 먼저 정리하고자 합니다.

# 2주차 피드백 정리
1. 기능 목록 구현을 할 때 클래스의 이름, 메서드의 input/output은 언제든지 변경될 수 있기에 너무 세세한 부분까지 정리하기보다 구현해야 할 기능 목록을 정리하는데 집중하여라!! 또한 **예외상황도 정리**하도록 노력하여라!
2. 문자열, 숫자 등으로 사용되는 매직 넘버는 상수(`static final`)으로 이름을 부여해 의도를 드러내야한다.
3. 메서드나 변수명 등의 이름은 축약하지 말고 의도를 드러내도록 하여라.
4. 메서드 라인의 기준 15라인은 공백 라인도 포함이 된다.
5. commit메시지에 `#번호`는 이슈 또는 Pull Request를 참조할 때 사용하는 것이다.
6. 예외 상황을 고려해 프로그래밍하는 습관을 들이도록 하여야한다.
7. 주석은 꼭 필요한 경우에만 남긴다.
8. git을 통해 관리할 자원에 대해서 고민하여라
9. Pull Request를 보내기 전에는 브랜치를 확인하여라.
10. 메서드를 구현하기 앞서 Java api에서 제공을 하는 기능인지 검색을 하며 java api를 적극 활용하여라.
11. 배열 대신 List,Set,Map등의 java Collection을 사용하여라!
12. 객체에 메시지를 보내라
13. 필드(인스턴스 변수)가 많은 것은 객체의 복잡도를 높이고 버그 발생 가능성을 높일 수 있어 필드의 수를 최소화 하여라!
14. 비즈니스 로직과 UI로직이 한 클래스에 있는 것은 단일 책임 원칙에도 위배됨으로 분리하여여야한다.

# 2주차 피드백에서 지키지 못한 것들

## 1. 매직 넘버는 상수(`static final`)으로 이름을 부여해 의도를 드러내야한다.


해당 피드백은 1주차 피드백에서도 존재하였던 피드백입니다. 1,2주차 과제를 진행하며 상수를 적극 활용하였으나 다른 지원자의 코드를 리뷰하다보니 제가 지금까지 생각한 매직넘버의 기준에 잘못된 것이 있다는 것을 깨닫고 2주차 과제에서 상수를 적용하지 못한 코드가 존재하였습니다.

### 1.1. 매직넘버의 잘못된 기준

```java
//  2주차 과제에 작성되었던 코드의 일부
   public static final int INITIAL_VALUE = 0;
   
   private static void checkInvalidAttempNum(String attempStr) {
        for (int i = INITIAL_VALUE; i < attempStr.length(); i++) {
            if (!Character.isDigit(attempStr.charAt(i))) {
                System.out.println(ERROR_INVALID_ATTEMP);
                throw new IllegalArgumentException(ERROR_INVALID_ATTEMP);
            }
        }
    }
```
지금까지 코드에 존재하는 모든 숫자들은 모두 매직 넘버라고 생각했습니다. 그리하여 `for`문 안에서 `int i=0`과 같은 초기화를 할 때도 상수를 사용하였습니다.

하지만 `for`문 안에서의 `int i=0`의 0의 의미는 모두가 알 수 있어 `int i = INITIAL_VALUE`로 바꾸는 것은 의미 없는 상수 변환이라는 것을 깨달았습니다.

> **상수 변환을 할 때 숫자 1을 ONE으로 이름을 짓는 것과 같은 의미없는 상수 변환은 피하도록 하자.**
출처: https://javabom.tistory.com/28

### 1.2. 상수 변환을 놓친 코드
```java
//  2주차 과제에 작성되었던 코드의 일부
    public static void printByAttemp(List<Car> carList) {
        for (Car car : carList) {
            System.out.print(car.getName() + " : ");
            for (int i = INITIAL_VALUE; i < car.getPosition(); i++) {
                System.out.print("-");
            }
            System.out.println();
        }
        System.out.println();
    }
```
해당 코드를 작성할 때 출력에 사용될 ` : `, `-`, `,`는 간단한 문자이기에 상수 변경을 하지 않아도 되겠지~하고 넘어갔었습니다.

지금 다시 생각해보면 정말 안일한 생각이었던 것 같습니다. 좋은 프로그래밍 코드란 다른 누군가가 보더라도 쉽게 이해하는 코드라 생각을 하였는데 제가 작성한 코드는 우테코 과제를 하였던 지원자들 외의 다른 사람들이 봤을 때 출력문의 ` : `, `-`, `,`는 코드를 실행시키기 전까지 무엇을 의미하는지 파악하기 힘들기 때문입니다.

해당 문자들은 아래와 같이 상수로 변경하여 쓰는게 옳았다는 것을 깨달으며 3주차 과제를 비롯한 앞으로의 프로그래밍에서는 이러한 실수를 하지 않겠다는 다짐을 하였습니다.
```java
    private static final String DELIMITER_FIELD = " : ";
    private static final String DELIMITER_WINNER = ", ";
    private static final String CAR_STATUS_BAR = "-";
```
※ 해당 상수 코드는 우테코 프리코스 지원자인 [2012monk](https://github.com/2012monk) 님의 코드에서 발췌하였습니다.


## 2. 객체에 메시지를 보내라
![](https://images.velog.io/images/seongwon97/post/30bb352a-c1ac-43f8-b850-df9d5797efdc/image.png)

이전까지 작성한 코드들은 하나같이 객체에 메시지를 보내는 것이 아닌 get을 통해 객체의 데이터를 꺼내왔기 때문에 해당 피드백의 내용을 보고는 충격을 받았습니다.

아래의 코드는 제가 2주차 과제에 우승자들을 찾기 위해 만든 메서드입니다.
해당 코드를 보면 모두 `getPosition()`을 통해 객체에 메시지를 보내는 것이 아닌 값을 받아오는 형태로 구현된 것을 확인할 수 있습니다.

```java
//  2주차 과제에 작성되었던 코드의 일부
    private static ArrayList<String> findWinnerList(List<Car> carList) {
        HashMap<Integer, ArrayList<String>> rankMap = new HashMap<>();
        int maxPosition = INITIAL_VALUE;

        for (Car car : carList) {
            int carPosition = car.getPosition();
            maxPosition = Math.max(maxPosition, carPosition);

            if (!rankMap.containsKey(carPosition)) {
                rankMap.put(carPosition, new ArrayList<>());
            }
            rankMap.get(carPosition).add(car.getName());
        }
        return rankMap.get(maxPosition);
    }
```

위의 코드를 피드백을 반영한 코드의 형태로 변경을 해보았습니다.
기존의 maxPosition의 판단은 `getPosition()`을 통해 값을 불러와서 판단을 하였다면 변경한 코드는 car에 `maxDistance`의 값을 메시지로 보내어 결과 판단을 하도록 변경하였습니다.

```java
    private static ArrayList<String> findWinnerList(List<Car> carList) {
    
        int maxDistance = INITIAL_VALUE;
        for (Car car : carList) {
            if (!car.isNewMaxPosition(maxDistance)) 
            	continue;
                
            if (car.isSameMaxPosition(maxDistance)) {
            	// 기존 winners객체에 해당 car를 추가
                continue;
            }
          
	    // 새로운 winners객체를 만드는 코드
        }
        // winners객체 return
    }
```

코드를 변경하다보니 이번 과제에서는 클래스의 분리가 많이 부족하다는 것을 새로 느끼게 되었습니다. 2주차 과제의 연습 목표가 **클래스 분리**라 클래스를 많이 분리하였다고 생각하였음에도 불구하고 클래스 분리가 부족했다는 것을 느끼었습니다. 다음 3주차 미션에서는 클래스 분리를 더욱 열심히 해보고자 합니다.

## 3. 비즈니스 로직과 UI 로직을 분리해라
코드를 작성할 때 비즈니스 로직과 UI로직을 한 클래스가 담당하지 않도록 하여야 한다는 피드백을 받았습니다. 해당 피드백을 받고 저의 코드를 보니 아래와 같이 우승자 리스트를 찾는 비즈니스 로직과 해당 결과를 출력하는 UI로직이 한개의 클래스에 모여 있는 것을 확인할 수 있었습니다.

📌 이번 3주차 미션을 수행할 때는 지난주 과제 제출 이후 공부하였던 [MVC패턴](https://velog.io/@seongwon97/MVC-%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80) 을 적용하도록 노력하여 비느지스 로직과 UI로직을 분리해보고자 합니다.

```java
public class PrintUtils {
    public static void printFinalResult(List<Car> carList) {
    
          ...      
    }

    private static ArrayList<String> findWinnerList(List<Car> carList) {
		
        ...        
    }
}
```



# 다른 지원자의 코드를 보고 찾은 개선해야할 점
## 1. MVC 패턴을 사용해보자

![](https://images.velog.io/images/seongwon97/post/3c6a1b5d-4c82-4604-b3e2-a0b4215786e9/image.png)

우테코 프리패스 과정을 진행하며 보다 나은 코드와 코드의 구조를 만들고자 다른 지원자들의 코드 리뷰를 하며 제가 작성한 코드와 비교를 해봤습니다.
다른 지원자들의 제출 코드들을 보니 많은 지원자들이 MVC패턴을 적용하여 과제를 진행한 것을 확인할 수 있었습니다.
MVC패턴은 이전에 스프링 프레임워크 학습을 하며 사용을 하였으나 스프링 프로젝트 외의 다른 프로젝트들에 적용을 할 생각을 하지 못하였습니다.

MVC패턴을 적용하여 기능별로 코드를 분리해 가독성과 코드의 재사용을 증가시켜보고자 합니다.

> MVC패턴 정리자료 : https://velog.io/@seongwon97/MVC-%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80


## 2. 클래스를 더욱 분리해보자
앞서 말했듯이 2주차 과제의 연습 목표가 **클래스 분리**인 만큼 제 나름대로 과제를 진행하며 클래스를 분리하였습니다. 하지만 피드백을 받고 다른 지원자들의 제출 코드를 보니 "이런 것까지 클래스 분리를 한다고??😵"라는 생각이 들 정도로 클래스를 더욱 나눌 수 있다는 것을 배운 것 같습니다.

또한 클래스를 많이 분리한 코드를 보니 가독성이 더 좋다는 것을 느꼈습니다.

3주차 과제는 마지막 과제인 만큼 클래스를 최대한 분리하여 코드를 작성해보고자 합니다.

## 3. 일급 컬렉션을 사용해보자
[jayjaehunchoi](https://github.com/jayjaehunchoi) 님의 과제 제출 코드와 [woowafreecourse_study](https://github.com/jayjaehunchoi/woowafreecourse_study) 자료를 보며 일급 컬렉션이라는 용어를 처음 접하게 되었습니다.

일급 컬렉션 내용도 3주차 과제를 진행하며 적용할 수 있는 부분이 있으면 적용하며 보다 좋은 코드를 작성하기 위해 노력하고자 합니다.

> 일급 컬렉션 정리 자료: https://velog.io/@seongwon97/%EC%9D%BC%EA%B8%89-%EC%BB%AC%EB%A0%89%EC%85%98%EC%9D%B4%EB%9E%80

