# 일급 컬렉션(First Class Collection)이란?
일급 컬렉션을 간단하게 설명하면 **Collection**을 **Wrapping**하면서 **그 외의 다른 멤버 변수가 없는 상태**를 일급 컬렉션이라고 합니다.

예시를 들면 다음과 같습니다.

```java
List<Car> carList = new ArrayList<>();

for (String carName : carArr) {
    carList.add(new Car(carName));
}
```
다음과 같은 collection을 아래와 같이 **wrapping**하는 것이 일급 컬렉션입니다.
```java
public class Cars {
    // 멤버 변수가 하나밖에 없다는게 중요!!
    private List<Car> carList;

    public Cars(List<Car> carList) {
        this.carList = carList;
    }
}
```

> 일급 컬렉션은 **소트윅스 앤솔로지**의 [객체지향 생활체조 파트의 규칙 8](https://developerfarm.wordpress.com/2012/02/01/object_calisthenics_/) 에서 언급이 되었다고 합니다.
>> **규칙 8: 일급 콜렉션 사용**
이 규칙의 적용은 간단하다.
콜렉션을 포함한 클래스는 반드시 다른 멤버 변수가 없어야 한다.
각 콜렉션은 그 자체로 포장돼 있으므로 이제 콜렉션과 관련된 동작은 근거지가 마련된셈이다.
필터가 이 새 클래스의 일부가 됨을 알 수 있다.
필터는 또한 스스로 함수 객체가 될 수 있다.
또한 새 클래스는 두 그룹을 같이 묶는다든가 그룹의 각 원소에 규칙을 적용하는 등의 동작을 처리할 수 있다.
이는 인스턴스 변수에 대한 규칙의 확실한 확장이지만 그 자체를 위해서도 중요하다.
콜렉션은 실로 매우 유용한 원시 타입이다.
많은 동작이 있지만 후임 프로그래머나 유지보수 담당자에 의미적 의도나 단초는 거의 없다.

# 일급 컬렉션을 왜 사용할까?
일급 컬렉션을 사용하면서 얻는 이점은 다음과 같습니다.
1. 비지니스에 종속적인 자료구조
2. Collection의 불변성을 보장
3. 상태와 행위를 한 곳에서 관리
4. 이름이 있는 컬렉션

### 1. 비지니스에 종속적인 자료구조
우테코 프리코스 2주차 과제의 자동차의 객체를 예시로 들면 `중복된 이름이 있으면 안된다.`, `이름은 5자리 이하의 영어 이름이어야 한다.`와 같은 조건이 있습니다.

해당 조건들을 서비스 메서드에서 구현을 한다면 자동차 객체리스트가 들어간 모든 장소에서 해당 자동차 리스트에 대한 검증 코드가 들어가야 하는 문제점이 발생합니다.

이러한 문제는 아래의 코드와 같이 **일급 컬렉션을 만들어 특정 조건으로 만들 수 있는 새로운 자료구조를 생성**하면 해결할 수 있습니다.

```java
public class Cars {
    // 멤버 변수가 하나밖에 없다는게 중요!!
    private List<Car> carList;

    public Cars(List<Car> carList) {
        validateCarName(carList);
        validateDuplicateName(carList);
        this.carList = carList;
    }
    
    ...
 
}
```

### 2. Collection의 불변성을 보장
Java의 final은 불변을 만들어주는 것이 아니라 재할당만을 금지합니다. 그리하여 `final List<Car> carList;`와 같이 만들어도 carList은 재할당만 불가능 할 뿐이지 `add`, `remove`와 같은 메서드로 내용을 변경할 수는 있습니다.

Java에서 final로 만들 수 없는 **불변 객체는 일급 컬렉션과 래퍼 클래스의 방법으로 해결해야합니다.**

```java
public class Cars {
    // 멤버 변수가 하나밖에 없다는게 중요!!
    private List<Car> carList;

    public Cars(List<Car> carList) {
        this.carList = carList;
    }
    
    public int getCarListSize() {
        return carList.size();
    }
}
```
일급 컬렉션을 만든 위의 코드를 보면 carList는 생성된 이후 `getter`, `setter`가 따로 없어 내부 값의 변경이 불가능 한 것을 확인할 수 있습니다.

> ※ 우테코 2기의 티거님의 [일급 컬렉션을 사용하는 이유](https://tecoble.techcourse.co.kr/post/2020-05-08-First-Class-Collection/) 의 정리 글을 보니 불변성을 보장하는 일급 컬렉션을 만들려면 `getter`, `setter`가 없는 클래스를 생성하거나 생성자를 만들 때 `unmodifiableList`를 사용하여 `return Collections.unmodifiableList(carList);`와 같은 코드를 작성해야하는 것을 확인했습니다.
즉, **일급 컬렉션을 만들 때는 컬렉션 값을 그대로 반환하는 기능은 두지 않고 가공된 값을 반환하거나 `unmodifiableList`를 사용하여 불변을 보장해야한다!**

### 3. 상태와 행위를 한 곳에서 관리
일급 컬렉션은 값과 로직이 함께 존재하여 외부에서의 **중복된 메서드의 생성**과 같은 문제를 해결해줍니다.

예를 들어 1,2,3,4학년의 정보를 하나의 List로 관리하고 각 학년의 평균 수강 학점을 구하려 한다면 각 학년 정보별로 중복된 여러 메서드를 생성할 수 있을 것입니다.

그러면 코드의 길이가 길어지고 중복 코드가 발생하며 가독성 또한 떨어지게 될 것입니다.

이러한 문제는 일급 컬렉션을 생성하고 해당 메서드들을 일급 컬렉션 내에 만들어두고 외부에서는 호출을 하여 사용하도록 프로그래밍하면 **상태와 행위를 한 곳에서 관리**할 수 있습니다.

### 4. 이름이 있는 컬렉션
대학교 1,2,3,4학년의 정보를 List로 관리한다면
```java
List<Student> freshman = new ArrayList<>();
List<Student> sophomore = new ArrayList<>();
List<Student> junior = new ArrayList<>();
List<Student> senior = new ArrayList<>();
```
와 같이 만들 수 있습니다. 하지만 해당 코드는 개인이 프로젝트를 진행하였을 때는 문제가 없으나 거대한 팀 단위의 개발을 하게 된다면 다음과 같은 문제가 발생할 수 있습니다.
1. 해당 객체들을 검색을 할 때 변수명으로만 검색을 할 수 있어 변수명을 모를 시 검색을 하기 어렵다
   2.단순히 변수명에 불과하여 의미 부여가 어렵다.

이러한 문제는 각각의 학년을 일급 컬렉션으로 만들어 관리를 한다면 해당 컬렉션을 기반으로 검색을 하면 되기에 해결할 수 있습니다.

```java
Freshman freshman = new Freshman(~~);
Sophomore sophomore = new Sophomore(~~);
Junior junior = new Junior(~~);
Senior senior = new Senior(~~);
```

<hr>

※ 해당 내용은 [일급 컬렉션 (First Class Collection)의 소개와 써야할 이유](https://jojoldu.tistory.com/412) 를 기반으로 작성하였습니다.


# Reference
- https://jojoldu.tistory.com/412
- https://tecoble.techcourse.co.kr/post/2020-05-08-First-Class-Collection/
- https://brainbackdoor.tistory.com/140
