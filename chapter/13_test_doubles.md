# 테스트 대역

단위 테스트는 개발자의 생산성을 높여주고 코드의 결함을 줄여주는 핵심 Tool 이다.
단순한 코드라면, 단위 테스트 작성이 어렵지 않지만 복잡한 코드일 경우 단위 테스트 작성이 쉽지가 않다.
테스트 대역은 실제 구현 대신 사용할 수 있는 객체나 함수를 말한다. 마치,주연 배우 대신 스턴트맨을 이용해 촬용하는 것과 마찬가지이다. 

## 테스트 대역이 소프트웨어 개발에 미치는 영향
테스트 대역을 사용하면 소프트웨어 개발 시 절충이 필요한 복잡한 문제가 몇 가지 생긴다. 
***테스트 용이성(testability)***
**테스트 대역을 사용하려면** 코드 베이스가 **테스트하기 쉽도록 설계**되어있어야 한다. 그래야 테스트에서 실제 구현을 테스트 대역으로 교채할 수 있다. 예컨대 데이터 베이스를 호출하는 코드라면 실제 데이터 베이스 대신 테스트 대역을 사용해도 괜찮을 만큼 유연해야한다.

***적용 가능성(applicability)***
테스트 대역을 **잘못 활용할 경우**, 오히려 복잡하고 효율도 나쁜 테스트가 될 수 있다. 특히, 커다란 코드 베이스 곳곳에서 테스트 대역을 잘못 사용한다면 이런 단점이 극대회 되어 **생산성을 크게 떨어트리게 된다**. 커다란 코드베이스는 테스트 대역을 활용하기 보다는 실제 구현에 목적을 두는 것이 바람직해보인다.

***충실성(fidelity)***
충실성은 **테스트 대역**이 실제 구현의 행위와 **얼마나 유사하냐**를 말한다. 테스트 대역이 실제 구현과 전혀 다르게 동작한다면, 테스트들은 별다른 가치를 만들어내지 못할 것이다. 

## 테스트 대역 기본 개념
***테스트 대역의 예***

 

       class PaymentProcessor {
        private CreditCardService creditCardService;
        ...
        boolean makePayment(CreditCard creditCard, Money amount) {
        if (creditCard.isExpired()) { return false; }
        boolean success =
        creditCardService.chargeCreditCard(creditCard, amount);
        return success;
        }
    }
makePayment 메소드를 테스트 하고자 한다.

    class TestDoubleCreditCardService implements CreditCardService {
    @Override
    public boolean chargeCreditCard(CreditCard creditCard, Money amount) {
    return true;
	    }
    }
테스트 클래스의 chargeCreditCard 메소드를 이용해 makePayment메소드를 검증 할 수 있다.

    @Test public void cardIsExpired_returnFalse() {
    boolean success = paymentProcessor.makePayment(EXPIRED_CARD, AMOUNT);
    assertThat(success).isFalse();
    }
이때 테스트 대역은 절대 원래 클래스 PaymentProcessor에 접근하지 않기 때문에 안전하게 메소드를 검증할 수 있다. 
***이어주기(Seams)***
단위 테스트를 고려해 짜인 코드를 우리는 테스트 하기 쉽다(testable)이라고 한다. 그리고 **이어주기(Seams)**란 제품 코드 차원에서 테스트하기 쉽게끔 만들어주는 걸 뜻한다. 즉, **제품 단에서 의존 대상을 다른 대상으로 교체할 수 있도록** 하면 된다.
대표적인 이어주기로는 의존성 주입(DI)이 있다. 의존성 주입을 활용하는 클래스는 의존성을 내부에서 생성하지 않고 외부로 부터 전달 받는다.

    class PaymentProcessor {
    private CreditCardService creditCardService;
    PaymentProcessor(CreditCardService creditCardService) {
    this.creditCardService = creditCardService;
	    }
    ...
    }

CreditCardService는 생성자에서 인스턴스를 직접 생성하지 않고 인수로 건네 받는다.
따라서 적절한 CreditCardService 인스턴스를 생성할 책임은 생성자를 호출하는 쪽에 있다. 제품 코드에서는 외부 서버와 통신하는 CreditCardService을 건냄으로서 테스트 대역을 적용할 수 있게 된 것이다.

    PaymentProcessor paymentProcessor =
    new PaymentProcessor(new TestDoubleCreditCardService());
다음과 같이 생성자에 테스트 대역을 직접 주입한다. 파이썬과 자바 스크립트 같은 언어는 의존성 주입시 주의가 필요하다. 왜냐하면 실제 구현중에서 당장 활용할 수 없는 함수나 메서드를 테스트 대역이 덮어 쓸 수 있기 때문이다. 

테스트하기 쉬운 코드를 만드려면 반드시 초기 설계에 반영해야한다. 시간이 지날 수록 코드베이스를 수정하기 어려워지기 때문이다.
***모의객체프레임워크(Mocking Frameworks)***
모의 객체 프레임워크는 테스트 대역을 쉽게 만들어주는 라이브러리이다. 즉, 객체를 테스트 대역으로 대체할 수 있게 해준다. 여기서 모의 객체란 구체적인 동작 방식을 테스트가 지정해줄 수 있는 테스트 대역을 말한다. 모의 객체 프레임 워크를 사용하면 테스트 대역이 필요할 때마다 새로운 클래스를 정의하지 않아도 됨으로 자질구레한 코드 사용을 줄일 수 있다.

    class PaymentProcessorTest {
    ...
    PaymentProcessor paymentProcessor;
    //단 한줄로 CrediCardService의 테스트 대역을 생성
    @Mock CreditCardService mockCreditCardService;
    @Before public void setUp() {
    // 테스트 대역을 대상 시스템에 건냄
    paymentProcessor = new PaymentProcessor(mockCreditCardService);
    }
    @Test public void chargeCreditCardFails_returnFalse() {
    // 테스트 대역에 특정한 행위를 지시함
    // ChargeCreditCard()메서드를 호출하면 무조건 false를 반환하도록함
    // 메서드 인자로 “any()” 를 지정했ㅎ는데, 이는 입력이 뭐든 상관하지 말라는 뜻
    when(mockCreditCardService.chargeCreditCard(any(), any())
    .thenReturn(false);
    boolean success = paymentProcessor.makePayment(CREDIT_CARD, AMOUNT);
    assertThat(success).isFalse();
    	}
    }
구글은 자바에서는 Mockito, C++ 은 googlemock, 파이썬에는 unittest.mock을 사용한다.
하지만, 모의 객체 프레임 워크를 과용하면 코드를 유지보수하기가 어려워진다.
## 테스트 대역 활용 기법
테스트 대역을 활용하는 대표적인 기법은 크게 세가지이다.
***속이기(Faking)***
가짜 객체는 제품코드로는 적합하지 않지만, 실제 구현과 비슷하게 동작하도록 가볍게 구현한 대역이다.

    // 가짜 객체는 쉽게 생성 가능하다.
    AuthorizationService fakeAuthorizationService =
    new FakeAuthorizationService();
    AccessManager accessManager = new AccessManager(fakeAuthorizationService):
    // 모르는 사용자의 ID 접근을 불허합니다.
    assertFalse(accessManager.userHasAccess(USER_ID));
    // 사용자 ID를 인증 서비스에 등록한 다음에 접근을 허용합니다.
    fakeAuthorizationService.addAuthorizedUser(new User(USER_ID));
    assertThat(accessManager.userHasAccess(USER_ID)).isTrue();

가짜 객체는 실제 객체의 지금 구현된 행위 뿐만 아니라 앞으로 구현될 행위들까지 비슷하게 흉내낼 수 있어야하기때문에 구현 난이도가 매우 높습니다.
***뭉개기(Stubbing)***
뭉개기는 원래 없던 행위를 부여하는 것을 말한다. 예컨대 대상 함수가 반환값을 지정한다고 하면, 이를 반환값 뭉개기라고 한다. 
아래 코드는 뭉개기 과정을 보여주는 예로써, Mockito 프레임 워크가 제공하는 when,theReturn,lookupUser메소드가 어떻게 동작해야하는 지를 설명한다.

    // 모의 객체 프레임 워크로 생성한 테스트 대역을 건낸다.
    AccessManager accessManager = new AccessManager(mockAuthorizationService):
    // USER_ID에 해당하는 사용자를 찾지 못하면(null을 반환) 접근을 불허한다.
    when(mockAuthorizationService.lookupUser(USER_ID)).thenReturn(null);
    assertThat(accessManager.userHasAccess(USER_ID)).isFalse();
    // null이 아니면 접근할 수 있도록 한다.
    when(mockAuthorizationService.lookupUser(USER_ID)).thenReturn(USER);
    assertThat(accessManager.userHasAccess(USER_ID)).isTrue();
***상호작용테스트(Interation Test)***
상호작용테스트란 대상 함수를 실제로 호출하지 않고도 그 함수가 어떻게 호출되었는지를 검증하는 기법이다. 예를들어, 함수가 전혀 호출되지 않거나, 너무 많이 호출되거나, 잘못된 인수와 함께 호출된다면 실패하는 경우가 있습니다.
아래 코드는 Mockito프레임 워크가 제공하는 verify() 메소드를 이용해 lookupUser()가 기대한 방식대로 호출되는지를 테스트 하는 과정이다.

    // 모의 객체 프레임 워크로 생성한 테스트 대역을 건넨다.
    AccessManager accessManager = new AccessManager(mockAuthorizationService);
    accessManager.userHasAccess(USER_ID);
    //  accessManager.userHasAccess(USER_ID)가 
    // mockAuthorizationService.lookupUser(USER_ID)을 호출하지 않았다면
    // 테스트 실패
    verify(mockAuthorizationService).lookupUser(USER_ID);
모의 객체 프레임 워크를 이용해서 함수가 몇번 호출되고 어떤 인자를 받았는지 추적합니다. 상호 작용 테스트도 모의객체라고(mock)이라고 불리긴 합니다. 
## 실제 구현 방식의 테스트
구글은 모의객체 프레임 워크를 선호하는 **모의객체 중심테스트**보다 **실제 구현을 선호하는 고전적 테스트(classic test)**를 위주로 사용하고 있다. 즉, 제품 코드가 사용하는 것과 똑같은 구현체를 사용해 테스트를 진행하고 있다.
단위 테스트들에 테스트 대역을 너무 많이 사용한다면 통합 시스템 개발속도가 현저히 느려질 것이며 무시할 경우 버그가 생길 경우가 많다.
실제 구현을 이용한 테스트는 실제 구현에 버그가 있다면 실패할 것이다. 이는 굉장히 설득력 있는 실패결과이다.

***실제 구현을 사용할지 결정하기***
구조가 간단하다면 실제 구현을 사용하는게 좋다. 예를들면, 값 객체와 같은 금액, 날짜, 주소 등 컬렉션 클래스가 대표적이다.
이보다 복잡한 코드라면 실제 구현을 사용하는게 비현실적일 때가 많다. **실제 구현과 단위 테스트의 트레이드 오프**를 잘 이해하고 사용하는 것이 중요하다.

***실행시간***
속도는 단위 테스트는 가장 중요한 요소이다. **실제 구현의 수행시간이 오래 걸릴 경우에는 테스트 대역이 유용할 수 있다.**
또한, 실제 구현을 이용하면 빌드도 오래 걸린다. 실제 구현은 테스트를 위해 구현이 참조하는 코드들까지 빌드해야 하기 때문이다. 빌드 도구인 Bazel은 이전 빌드 결과를 캐시해주는데 이처럼 확장성이 뛰어난 빌드 시스템을 이용하면 실제 구현에서 도움을 얻을 수 있다.
***결정성***
같은 버전의 시스템을 대상으로 실험하면 **언제나 똑같은 결과를 내주는 테스트**를 **결정적인** 테스트라고 한다. 이와 **반대**인 경우 **비결정적** 테스트라고 한다.
테스트에서 비결정성은 불규칙한 결과로 이어진다. 이는 테스트 자체가 문제일 수도 있지만, 대상 시스템이 사용하는 다른 코드(의존성)이 원흉일 수도 있다. 만약, **비결정성이 빈번하게 나온다면 테스트 대역을 이용해** 의존성을 줄이는 방향으로 테스트하는 것이 옳을 수도 있다.
실제 구현은 테스트 대역보다 훨씬 복잡하기 때문에 비결정성의 여지가 다분하다. 예를들어 실제 구현에서 멀티스레드를 이용하고 스레드 수행 순서가 대상 시스템의 결과에 영향을 준다면 테스트가 가끔씩 실패할 수 있다.
***의존성 생성***
실제 구현을 이용하려면 의존 대상들도 모두 생성해야한다. 에를들어 객체하나를 생성하려면 의존성 트리에 등장하는 모든 객체가 필요하다. 이에 반해 **테스트 대역은 다른 객체를 그다지 사용하지 않는다.** 
예를들어 

    Foo foo = new Foo(new A(new B(new C()), new D()), new E(), ..., new Z());
다음과 같이 객체 하나를 생성하는데 여러 객체가 필요하다고 하자. 이때 단 하나의 생성자만 변경되도 전부 수정되야한다. 
하지만, 모의객체 프레임 워크를 이용해 테스트 대역을 생성한다면

    @Mock Foo mockFoo;
굉장히 편하게 생성할 수 있다.

## 속이기(가짜 객체)
실제 구현을 사용할 수 없을 때는 가짜 객체가 최선일 경우가 많다. 가짜 객체는 실제 구현과 비슷하게 동작하기 때문에 다른 테스트 대역들보다 우선적으로 고려되야한다. 이때 가짜 객체는 실제 구현과 최대한 비슷해야한다.

    // FileSystem interface 인터페이스를 구현하는 가짜 객체입니다.
    // 실제 구현도 이와 동일한 인터페이스를 이용합니다.
    public class FakeFileSystem implements FileSystem {
    // 파일의 이름과 파일 내용의 매핑 정보를 저장합니다.
    // 테스트에서 디스크에 입력하지 않도록 파일들을 메모리에 저장합니다.
    private Map<String, String> files = new HashMap<>();
    @Override
    public void writeFile(String fileName, String contents) {
    // 파일과 해당 파일의 내용을 맵에 추가합니다.
    files.add(fileName, contents);
    }
    @Override
    public String readFile(String fileName) {
    String contents = files.get(fileName);
    // 실제 구현은 파일을 찾을 수 없을 때 예외를 던져줍니다.
    // 가짜객체 또한 예외를 던져줍니다.
    if (contents == null) { throw new FileNotFoundException(fileName); }
    return contents;
    	}
    }
위와 같이 **실제 객체가이 예외를 던져준다면, 가짜객체또한 예외를 던져주어야한다**.

***가짜객체가 중요한 이유***
가짜객체는 실제 객체를 사용할 때의 모든 단점들을 제거하고 API테스트를 훌륭하게 마칠 수 있도록 해준다.

***가짜객체를 생성해야 할 때***
가짜 객체는 실제 구현과 비슷하게 동작하기 때문에 만들기 어렵다. 또한, 실제 객체가 변경될 때마다 가짜 객체 또한 변경되야함으로 유지보수가 필요하다. 따라서 이러한 **가짜객체 생성과 유지보수**는 **실제 구현을 담당하는 팀이 맡아야한다.**
팀이 가짜객체를 만들려면 유지보수 비용을 잘 판단해야한다. 유지 보수할 가짜 객체수를 줄이려면 진짜 객체를 사용하기 힘든 코드를 가짜로 만들면 된다.
또한, 프로그래밍 언어별로 가짜 객체를 위한 라이브러리를 생성해두는 것도 하나의 방법이다.

***가짜객체의 충실성***
**가짜 객체를 사용하는 핵심은 '충실성'**에 있다. 100% 충실하게 만들기가 어렵더라도 가짜 객체는 **실제 구현의 API 설명에 가능한 충실하게 동작**해야한다. 어떤 데이터를 전달하든 가짜 객체는 실제 구현과 동일한 결과를 돌려주고 상태변화도 똑같이 시뮬레이션 되야한다.

> 예를들어 database.save(itemID)라는  API가 있고 이 API는 주어진 ID가 있을 때는 리턴하고 없다면 실패하는 기능을 한다고 하자. 이때 **가짜객체는 각상황에 대해 똑같이 작동해야한다.**

**중요한 점은 가짜 객체가 기능은 충실해야하지만, 그 값까지 똑같을 필요는 없다**는 것이다. **실제 객체가 어떤 상황에서 해쉬값을 내뱉**는다고 해서, **가짜객체**가 똑같은 해쉬를 내뱉을 필요는 없다. 그냥 **그 상황에서 아무 값이나 내뱉**기만 하면된다.
하지만, **이런 특성 때문에 지연 시간과 같이 제약조건을 검증**하려면 **실제 구현**만이 방법이다.(가짜 객체는 제약조건까지 똑같을 수 없기 때문에)

***가짜객체도 테스트가 필요하다***
가짜 객체를 만들기 위해서는
1. 해당 **API를 감싸는 클래스**를 만들고 이 클래스를 통해서만 API가 호출되도록 한다.
2. 인터페이스는 똑같지만 실제 **API를 사용하지 않는 클래스**를 준비한다.
이 클래스가 가짜 객체이다.  이렇게 하면 실제 API중 **내가 필요한 API만 실제로 구현**하고 테스트 하면 됨으로 훨씬 경제적이다. 

## 뭉개기(Stubbing)
뭉개기는 원래 없는 행위를 테스트가 함수에 덧씌우는 방법이다. 테스트에서 실제 구현을 대체할 수 있는 쉽고 빠른 방법 중 하나이다. 
***뭉개기의 위험성***
뭉개기는 쉬워서 실제 구현을 이용하기 어려울 때마다 자주 사용되지만, 그만큼 위험하다.
1. **불명확해진다.**
뭉개기를 이용하려면 대상 함수에 행위를 덧씌우는 코드를 추가로 작성해야한다. 이 추가코드로 인해 처음 보는 사람은 이해하기 어려울 것이다.
뭉개기 때문에 다른 파일의 코드를 봐야한다면 문제가 있는 것이다.
2. **깨지기 쉬워진다.**
뭉개기를 이용하면 시스템 내부 구현방식이 테스트에 영향을 주게 되고 이는 내부 코드가 변경되면 뭉개기도 변경되야함을 뜻한다.
3. **테스트 효과가 감소한다.**
뭉개기로 행위를 뭉개버리면 실제 구현과 똑같이 동작하는지 보장할 수가 없다. 예를들어


when(stubCalculator.add(1, 2)).thenReturn(3);
위와 같이 **1,2를 받는 add()를 무조건 3을 리턴하도록 뭉개버린다면 문제가 생긴다**.
또한, 뭉갤 경우 해당 값을 저장할 수 있는 방법이 없다. 가짜 객체의 경우 멤버 변수에 저장해 다시 불러오면 되지만 뭉개버리면 일회성으로 사용되고 끝이다.

    @Test public void creditCardIsCharged() {
    // 테스트 대역 전달
    paymentProcessor =
    new PaymentProcessor(mockCreditCardServer, mockTransactionProcessor);
    // 테스트 대역들이 함수들을 뭉갠다.
    when(mockCreditCardServer.isServerAvailable()).thenReturn(true);
    when(mockTransactionProcessor.beginTransaction()).thenReturn(transaction);
    when(mockCreditCardServer.initTransaction(transaction)).thenReturn(true);
    when(mockCreditCardServer.pay(transaction, creditCard, 500))
    .thenReturn(false);
    when(mockTransactionProcessor.endTransaction()).thenReturn(true);
    // 테스트를 위해 시스템을 호출한다.
    paymentProcessor.processPayment(creditCard, Money.dollars(500));
    //실제로 Pay()가 작동한건지 알 수 없다. 그저 Pay()가 호출된 것만 알 수 있다.
    verify(mockCreditCardServer).pay(transaction, creditCard, 500);
    }
위와 같이 과**도하게 뭉게버릴 경우** 실제 함수의 **정상 작동 여부를 전혀 판별할 수 없게 된다**. 위와 같은 경우 가짜 객체를 이용해 함수가 정상 작동하는 지 판별하도록 리팩토링 할 수 있을 것이다.

***뭉개기가 적합한 경우***
뭉개기는 실제 구현을 대체하기 보다는 특정 함수가 특정값을 반환하도록 하여 **시스템을** **내가 원하는 상태로 변경**하려 할 때 적합하다. 
분명한 목적을 표기한다면 뭉개기는 분명 쉽게 원하는 효과를 얻을 수 있을 것이다.

## 상호작용 테스트(Interaction Test)
**상호작용 테스트**는 대**상 함수를 호출하지 않으면서 함수가 어떻게 호출**되는지 검증하는 기법이다.

***상호작용 테스트보다 상태 테스트를 우선시하자***
상호작용 테스트 보다는 **상태 테스트를 이용하는 것**이 좋다. **상태 테스트**란 대상 **시스템을 호출**하여 **올바른 값을 반환**하는지, 혹은 대상 **시스템의 상태가 옳바르게 변경**됐는지를 검증하는 테스트를 말한다.

    //상태 테스트
    @Test public void sortNumbers() {
    NumberSorter numberSorter = new NumberSorter(quicksort, bubbleSort);
    // 대상 시스템 호출.
    List sortedList = numberSorter.sortNumbers(newList(3, 1, 2));
    // 반환된 리스트가 정렬되어 있는지 검증
    // 결과가 맞다면 어떤 알고리즘을 사용했는지 알 필요가 없다.
    assertThat(sortedList).isEqualTo(newList(1, 2, 3));
    }
**즉, 상태 테스트는 위와 같이 결과만을 가지고 판별하는 것이다.**

    @Test public void sortNumbers_quicksortIsUsed() {
    // 테스트 대역을 건넨다.
    NumberSorter numberSorter =
    new NumberSorter(mockQuicksort, mockBubbleSort);
    // 시스템을 호출한다.
    numberSorter.sortNumbers(newList(3, 1, 2));
    // numberSorter.sortNumbers() 가 퀵 소트 방식을 이용했는지 검사한다.
    // 만약, 퀵 소트 방식을 이용하지 않는다면 테스트는 실패할 것이다.
    verify(mockQuicksort).sort(newList(3, 1, 2));
    }
**상호작용 테스트는 이와 달리 함수의 호출이 옳바른지 확인하게 된다. 하지만, 그 결과는 알 수 없다.** 구글은 오랜 경험을 통해 상호작용 테스트의 문제점을 파악했다.
1. 함수가 호출되면 결과도 맞을 것이라고 가정해야한다.
2. 구조가 변경되면 테스트 방식 또한 바뀌어야한다.

***상호작용 테스트가 적합한 경우***
1.  **실제 구현이나 가짜 객체를 이용할 수 없을 때.** 이럴 때를 대비해 상호 작용 테스트를 통해 **특정함수가 호출되는지 확인할 수 있다. 이를 통해 시스템이 기대한대로 동작한다는 결론**을 내릴 수 있다.
2.  **함수 호출 횟수나 호출 순서가 달라지면 결과가 바뀌는것이 명백할 경우**

***상호작용 테스트 모범 사례***
1. **상태변경함수 경우에만 상호작용 테스트를 고려하자**

> **상태 변경 함수**
> 함수가 시스템을 변경합니다. (e.g. sendEmail(), saveRecord()) 
> **상태 유지**
> 시스템을 변경하지 않습니다. (e.g. getUser(), findResult())

상태 유지 함수의 **상호작용을 테스트** 한다면 상호작용 방식이 바뀔 때마다 테스트를 수정해야한다. 하지만, 이와달리 **상태를 변경하는 함수라면 어디서 상태가 변경되는지를 손쉽게 확인할 수 있다.**

2. **너무 상세한 테스트는 피하자**
어떤 함수가 **어떤 인수를 받아 호출되는지 너무 세세하게 검증하면 안된다**. 다양한 테스트 환경을 포괄하지 못할 것이기 때문이다.

## 핵심 정리
1. 테스트 대역보다는 되도록 실제 구현을 사용하는 것이 좋다.
2. 테스트에서 실제 구현을 사용할 수 없을 땐 가짜 객체가 최선일 때가 많다.
3. 뭉개기(스텁)을 과용하면 테스트가 불명확해진다.
4. 상호작용 테스트는 되도록 피하는게 좋다.

