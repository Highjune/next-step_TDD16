> PR 요청을 통해 review 받은 내용들을 정리했다.
> [PR 리스트 링크](https://github.com/Highjune/next-step_TDD16)

# 1. 자동차 경주 - 단위테스트

## PR

- 한 줄에 .하나씩 처리하면 가독성 올라감.
- if 문에서 중괄호 {} 생략하지 말기.
- 메서드명은 동사로 사용하는 것이 좋다.
- [인터페이스를 분리하여 테스트하기 좋은 메서드로 만들기](https://tecoble.techcourse.co.kr/post/2020-05-17-appropriate_method_for_test_by_interface/)
- 테스트 클래스는 모든 클래스와 1:1 대응?
    - 거의 모든 케이스가 그렇죠. 그러나 테스트가 어려운 케이스들이 있을꺼에요. View와 관련 케이스들

- 5개의 클래스가 있고, 각각의 테스트를 한다면 5개가 될 수 있겠지만 기능적인 부분을 테스트 한다면 여러 여러 테스트가 혼합될 수 있는데, 그럴 경우에는 테스트 클래스를 어떻게 작성?
    - 의존이 있는것들은 의존이 있는대로 작성하는게 일반적
    - 하지만 최대한 독립적으로 분리

- [static 과 관련된 좋은 글](https://honbabzone.com/java/java-static/)

- 멤버변수 나열 순서
    - 상수 -> 필드 -> 생성자 -> 정적메서드 -> 메서드
    - 일반적으로 상수가 객체 최상단에 위치하는 것이 관례.
    - 순서 예시
    ```
    상수
    필드(변수, public -> default -> protected -> private 순서)
    생성자
    정적메서드
    메서드(public -> default -> protected -> private 순서)
    ```

- final 로 불변유지할 수 있는지 살펴보기.
- 기본생성자가 없다면 초기값 지정은 굳이 하지 않아도 된다. 어차피 값을 부여받을 것이므로.

## 이론



# 2. 로또 - TDD
## PR
- 매개변수 최대 2개까지로 줄여보기
- xxxUtil같은 클래스 설계는 지양하라. 이름 자체가 말하듯 범용성을 내포하고 있기에 여러 책임이 같이 맞물리게되고, 특정 구현체들의 요구조건에 맞추다보면 코드가 난잡해지기 쉬운 객체가 된다. 해당 기능같은경우 최대한 JDK 기본라이브러리가 제공하는걸 사용하거나 자체적으로 구현 혹은 공통되는부분이 꽤 있다면 차라리 인터페이스와 구현체 혹은 열거타입을 활용할 수 있다. 그외에 커스텀 Util 클래스를 만들지 않는게 좋습니다 ㅎ
- 메서드 내에서 여러 분기점을 가지는 것은 좋지 않다.
    - 하나의 메서드가 책임져야 할 분기(조건)들이 너무 많아지기도하고, 차후 유지보수성도 떨어지므로.
    - 열거타입과 표준함수형 인터페이스 등을 활용해서 로직을 추상화 할 수 있음.

- 라이브러리 내장 예외가 발생할 경우, 다른 예외로 번역해서 혼동을 주기보다는 해당 예외(ArithmeticException)를 활용하되 메세지만 추가해주는 방법이 낫다.
    ```java
        private void validateDivine(int divineNumber) {
            if (divineNumber == 0) {
                throw new IllegalArgumentException("0으로 나눌 수는 없습니다.");
            }
        }
    ```
    - 위에서 원래 0으로 나눌 경우에 던져지는 예외는 ArithmeticException 이므로, 그대로 던지면 된다.

- 아래처럼 너무 극단적으로 메서드로 뺄 이유는 없다.
```java
private int toInt(String stringTobeInt) {
    return Integer.parseInt(stringTobeInt);
}
```

- [테스트 케이스 작성시 케이스 뽑는 팁](https://catsbi.oopy.io/7c084479-c9d0-44a1-acb9-f6b43a19e332)

- 단 한줄짜리라도 블럭{} 생략은 지양하자. 코드의 일관성과 가독성을 낮춥니다. IDE에 의해 자동생성된 코드도 예외대상이 아닙니다.
    ```java
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
    }
    ```
    - 위처럼 쓰지말고 아래처럼
    ```java
    @Override
    public boolean equals(Object o) {
        if (this == o) {
            return true;
        }
    }
    ```

- 자바에선 제네릭과 같이 특수한 경우가 아니라면 Wrappter타입보다 primitive 타입을 권장하 편이다.

- `트랜잭션 스크립트 패턴` 을 사용하면 테스트하기 힘들. 객체지향 패러다임인 SOLID를 지키기 힘든 코드
    - `도메인 모델 패턴`을 사용해야 한다. 

- README.md의 명세는 보통 개발자를 대상으로하는 명세로쓰이기에 테스트와 1:1으로 수렴할수록 좋다.
    - 아니면 depth를 나눠서 기능과 내부적인 에지케이스를 나눠서 명세로 작성하는 방법도 있다.

- 적당한 개행으로 가독성 높이기
    - 개념적 유사성에 따라 개행을 통해 구분을 짓기. 코드가 다 붙어있다보면 가독성이 낮아질 수 있다.

- 클래스명과 변수명을 다르게 작성해야 한다.
    - 아래처럼 하면 getLotto()가 클래스를 말하는건지 자료구조를 말하는건지 구분짓기 힘들다.
    ```java
    public class Lotto {

        private final List<LottoNumber> lotto;
        ...

    }
    ```

- Test에서 @DisplayName 작성시 `예외는 발생한다기보단 던진다(throw)`가 적절한 표현이다.

- 아래처럼 try catch 로 잡는 것이 맞는가? 아니면, Double.ParseDouble 자체에서 NumberFormatException 발생되는 것이랑은 어떤 차이가 있을까? 메시지를 담을 수 있는 부분?
    ```java
    private void isNumeric(String number) {
        try {
            Double.parseDouble(number);
        } catch (NumberFormatException e) {
            throw new NumberFormatException(ERROR_INPUT_MUST_BE_NUMBER_FORMAT);
        }
    }
    ```
    - 답변) 잘했다 못했다 평가는 기획서의 내용에 따라 달라지겠죠. 자바에서 try-catch가 딱히 문제되는 코드는 아닙니다. 다만 NumberformatException자체가 어떤 값을 변환하려 시도할 때 예외가 던져져는지에 대해 메세지를 다 제공할꺼라서 그 외에 추가적으로 넣어야 하는 메세지가 있거나, 해당 예외가 발생했을 때 기본 값으로 세팅해서 예외를 안던진다는 기획이 있거나 할 때는 적절하겠죠. 그게 아니라면 굳이 try-catch를 안하고 그냥 예외가 던져지면 던져지는대로 사후 처리를 하는게 맞구요.


- 자료형 선언시, Wrapper class 로 사용하는 건 어떤 경우인가요? 제네릭 이외에, 웹 개발에서 RequestDto 처럼 null 이 들어올 수 있는 경우는 Wrapper 클래스로 감쌌거든요. 다른 경우는 어떤 것들이 있을까요?
    - 제네릭사용이 대표적이고 컬렉션 프레임워크에서도 타입지정을 위해 사용됩니다. 그외에는 크게 필요한 경우는 못 본것 같습니다. null값이 필요한 경우 사용될 수 있긴 한데 거의 못봤고, Number등으로 상위타입으로 추상화를 통해 다른 객체들관의 호환성을 높힐수도 있긴한데 이 역시 거의 사용되지 않죠.
    - Wrapper 타입을 사용하는것은 꼭 필요한 경우(제네릭, Colleciton API 등)가 아니라면 지양해주세요 장점보단 단점이 많습니다.

- Static 메서드와 연관된 메서드들도 다 static으로 변환? 
    - ex) Store 클래스에서 밖에서 사용하는 함수 하나(order()) 를 static 으로 만드니, 그와 연관된 함수들(private까지)을 다 static 으로 만들게 되던데 이렇게 하는 것이 맞을까요? 아니면 static 으로 늘어나는 것이 부담되면 밖에서 호출하는 함수도 인스턴스 메서드로 변경하는 것이 맞을까요?, OutputView 같은 함수도 마찬가지입니다.
        - A) static이여도 상관없는 재사용성을 가지고 별도의 상태를 가지지 않는 메서드라면 상관 없습니다. 인스턴스 메서드로 만들되 싱글턴으로 관리할지, 아예 유틸리티 클래스로 갈지는 크게 차이가 없습니다.

- 가독성 문제. LottoApplicationMain 클래스에서 아래와 같이 작성했는데요.(한 라인 길이 무관)
    ```java
    WinningNumber winningNumber = new WinningNumber(Store.pickWinNumber(InputView.questionWinnerNumber()));
    //   그런데 그렇게 말고 아래와 같이 나눠서 하는 것이 가독성 측면에서 더 나을까요?

    String answer = InputView.questionWinnerNumber();
    Lotto lotto = Store.pickWinNumber(answer);
    WinningNumber winningNumber = new WinningNumber(lotto);
    ```
    - 답변) 한 라인에 작성하는게 더 가독성도 떨어질 뿐더러 디버깅도 어렵게 만들죠. 적절한 개행을 통해 구분짓는게 좋습니다. `매개변수 작성칸에 다른 메서드를 호출해 반환값을 그대로 넣어준다는 것의 장점은 코드라인이 줄어든다는 것을 제외하고는 없습니다.` 메서드 시그니처가 너무 명확해서 간혹가다 쓰는건 그럴 수 있지만 그 외에는 추천드리지 않습니다.

- 예외는 정말 예외 상황에서만 던져야 한다.

- 가변인수 사용 지양하기
    ```java
    public static Lotto of(Integer... numbers) {
        Set<LottoNumber> lottoTicket = new HashSet<>();
        for (Integer number : numbers) {
            lottoTicket.add(new LottoNumber(number));
        }
        return new Lotto(lottoTicket);
    }
    ```
    - 가변인수는 쓰기 전 한 번 더 고민해볼 필요가 있습니다. 변수가 하나만 전달되더라도 내부적으로 배열을 생성하는 작업이 필요하죠. 그래서 오버로딩을 통해 구현하거나 하는식으로 대체됩니다.

- public 메서드 생성시 항상 고려해야 한다.
    - 협업관점에서 public API는 어떠한 클라리언트가 접근할지를 다 고려해야 한다.


- map, enum을 같이 사용할거면 enumMap을 고려해보자.


- 디미터의 법칙
    - 네 이웃의 이웃을 알게 하지 마라.
    - . 으로 계속 연결해서 사용금지

- map 의 merge 활용하기
    - 원래의 코드
    ```java
    private static void putRankMap(int matchingCount, Map<Rank, Integer> rankMap) {
        if (matchingCount >= 3) {
            Rank rank = Rank.find(matchingCount);
            rankMap.put(rank, rankMap.getOrDefault(rank, 0) + 1);
    ...
    ```
    - merge 사용
    ```java
    private static void putRankMap(int matchingCount, Map<Rank, Integer> rankMap) {
        if (matchingCount >= 3) {
            Rank rank = Rank.find(matchingCount);
            rankMap.merge(rank, 1, Integer::sum);
    ...
    ```

- Stream API 안에서는 지역변수(local valule)에 영향을 주면 안된다.
    - local value는 jdk 과거버전에서는 무조건 final을 붙혀야 했고, 시간이 지나 지금은 final을 붙히지 않아도 되지만, 사실 final로 취급하기에 effective final value로 취급됩니다. 그래서 외부 변수에 값을 변경할 수 없는 것. 변경하기 위해서 접두어 Atomic- 변수를 사용해서 해결하는 것도 잘못된 것. AtomicInterger로 해서 에러가 발생안하는건 AtomicInterger 가 참조하고있는 주소값을 변경하는게 아니기 때문인데, 이 역시 외부 변수에 변조를 의도하기에 사용하지 않는게 좋다.
    - Stream API에서 사용되는(그렇지 않더라도) Lambda는 순수함수(pure function)이어야 한다. 즉 외부값에 영향을 주지 않아야 한다는 것이죠. 그렇기때문에 forEach보다는 제가 따로 코멘트 드렸던 내용을 권장드렸던 것.
    - forEach 와 peek 중간 연산자는 디버깅 및 출력 목적을 제외하고 사용하는 것은 사용목적과 어긋난다.
    - forEach안에 들어가는 수식은 람다식으로 람다식은 순수성을 사이드 이펙트가 없어야 하며, 그러기 위해 필연적으로 로직은 짧아야 한다. 그렇기에 로직이 길어지고 복잡해진다면 향상된 for문을 사용하거나 메서드 분리를 해줘야 한다.

- 필드 초기화를 해주는 경우는?
    1. Collection API를 사용해서 NPE 위험을 줄이기 위해서
    2. 기본값이 아닌 값을 모든 인스턴스 생성시 기본 초기값으로 지정해주기 위해서
    - 인스턴스 변수는 원시값의 경우는 굳이 초기화하지 않아도 된다.
        - 이미 정해져있기에 작성하지 않는다고 가독성이 낮아지진 않는다.
    

- 특정 계산방식같은 것은 여러 조건에 따라 변경되기 쉽다. 세법이나 등등
    - 이런 경우에는 외부주입을 통해 [전략패턴](https://victorydntmd.tistory.com/292)을 사용하면 더 좋다.


- 인터페이스에 하나의 메서드만 정의될 것 같으면 FunctionalInterface 어노테이션을 명시해서 추가적인 기능 정의를 컴파일 단계에서 막아줄 수 있다.

- 테스트를 분리하는 것은 검증문 단위가 아니라 말 그대로 단위.
    - a라는 메서드를 테스트할 때 조건이 같다면(1이냐 2냐가 아니라 유효하고 동일한 결과가 유추) 하나로 묶어도 무관.

- 의미있는 변수명 짓기. 의도한 바를 드러내자

- A라는 함수가 매개변수를 받는데, 그 매개변수들이 본인 자신의 인스턴스 변수라면 애초에 매개변수들을 받을 필요가 없다.

- 참조타입은 최대한 추상화된 타입을 사용하는 것이 좋다.

- 예외 발생시, 사용자가 무슨 값을 전달해줬는지 안내해주면 좋다.
    - 그런데 단순 자바 100% 어플리케이션인 경우에는 그렇다 하더라도, 웹 개발에서도 그러한가?

- 정적 팩토리 메서드 패턴에서 관례적으로 인자가 하나일 경우에는 of보단 from을 권장한다.

- enum 객체를 찾는 메서드에는 `평가의 기준을 상위 객체(Rank)가 아닌 하위 객체(Rank.THIRD, SECOND, FIRST, ...)으로 잡아야 한다.`
    - 상위객체(Rank)에서 조건식들을 직접평가하며 너는 이 조건에 맞으니 FIRST야! 이런식으로 하위 구현체를 언급하는게 아닌, 
    ```
    RANK: 하위구현체에게 FIRST야 이거 너랑 맞는 조건이니?
    FIRST: 아니요.
    RANK: SECOND야 이건 너랑 맞는 조건이니?
    SECOND: 네
    RANK: return SECOND
    ```
    즉, `책임사슬패턴`같은 것이다.
    - 보통 아래와 같이 많이들 구현한다.
    ```java
    Arrays.stream(values())
             .filter(rank -> rank.support(matchCount, isBonusBall))
             .findFirst()
             .orElse(MISS);
    ``` 
    - 즉, 인자로 받은 데이터들을 각각의 인스턴스(FIRST, SECOND, THRID)가 직접 평가하도록하고 응답에 따라 로직을 수행하는 것. 이처럼 구현하면 상위 객체에서는 어떤 조건에 어떤 구현체인지 알 필요 없고, 각각의 구현체들 역시 각각 자기에게 맞는 조건식을 작성해서 검사할 수 있게 된다.
    - 만약 Enum 객체를 찾아야 하는 조건들이 있다면? enum 필드에 조건을 추가해서 아래와 같이 작성하면 된다.
    ```java
    public enum Rank {

    FIRST((count, isBonus) -> count == 6, 6, 2_000_000_000),
    SECOND((count, isBonus) -> count == 5 && isBonus, 5, 30_000_000),
    THIRD((count, isBonus) -> count == 5 && !isBonus, 5, 1_500_000),
    FOURTH((count, isBonus) -> count == 4, 4, 50_000),
    FIFTH((count, isBonus) -> count == 3, 3, 5_000),
    MISS((count, isBonus) -> false, 0, 0);

    private final BiPredicate<Integer, Boolean> matcher;
    private final int matchingCount;
    private final int prize;

    Rank(BiPredicate<Integer, Boolean> matcher, int matchingCount, int prize) {
        this.matcher = matcher;
        this.matchingCount = matchingCount;
        this.prize = prize;
    }

    public static Rank find(MatchCount matchCount, boolean isBonus) {
        return Arrays.stream(values())
                .filter(rank -> rank.matcher.test(matchCount.value(), isBonus))
                .findFirst()
                .orElse(MISS);
    }

    .. 중략 ..
    ```

# 3. 사다리타기 - FP, OOP

- 방어적 복사
    ```java
    public List<Boolean> getLines() {
        return Collections.unmodifiableList(lines);
    }
    ```

- 리팩토링 대상?
    - 반복되는 구문이 3번 이상이라면 리팩토링 대상

- 공식문서
    - [Mokito](https://github.com/mockito/mockito/wiki)
    - [Junit5](https://junit.org/junit5/docs/current/user-guide/)


# 4. 수강신청 - 레거시 코드 리팩토링
- 딱히 없음.