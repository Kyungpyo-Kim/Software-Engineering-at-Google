# Chapter 21 - Dependency Management


> 이번장에서는 Dependency Management가 얼마나 어려운지, 고려해야될 것들, 해결 방법, 한계 등에 대해 설명하고, 구글에서는 Dependency를 어떻게 관리하는지를 설명한다.

</br>


Dependency Management에서는 Depenedency로 엮여있는 package, network, library의 개발이 언제되는지, 무엇이 수정되는지 등에 관여할 수 없기 때문에, 소프트웨어 엔지니어링에서 어려운 문제 중 하나입니다. 

</br>

Ex)   
1. Dependency의 버전이 올라가면 우리의 코드베이스는 반영을 어떻게 할 것인지?  

2. Dependency 변화에 의해 우리 코드베이스를 변경한다면, 그에 따른 version은 어떻게 정의 또는 설명할 것인지

3. Dependency에 어떤 변화가 있을때, 그것이 우리 Dependency에 어떤 영향을 미칠지

4. Dependency가 변경됐을 때, 그 변화가 우리 코드베이스에 많은 영향을 끼친다면 우리가 직접 그것을 개발해야 되는건지

</br>

---

</br>

## Why Is Dependency Management So Difficult?

</br>

### Conflicting Requirements and Diamond Dependencies

![diamond dependency](/images/chapter21-image1.png)

설명 : 

내가 개발하는 코드 libuser 라이브러리가 liba, libb 라이브러리에   디펜던시를 갖고 있을 때,  
liba, libb가 공통으로 갖고 있는 dependency인 libbase는 같은 버전을 사용하고 있어야 된다.

liba, libb는 각각 다른 라이브러리이므로 개발속도가 다를 수 있고,  
만약 각 라이브러리가 사용하는 libbase의 버전이 다를 경우, Confilct가 날 수 있다.

- libuser : 개발한 코드
- liba, libb : 개발한 코드가 가지고 있는 Dependency
- libbase :  liba, libb가 가지고 있는 Dependency

</br>

Diamond Dependecy를 통해 알 수 있는 Dependency Management가 어려운 이유는  
다음과 같다.

1. 하나의 Dependency만 관리하면 되는 것이 아니다.  
모든 Dependency에 대해 관리를 해야하며,어떤 특정 Dependency를 업그레이드 했을 때,   
다른 Dependency들에 영향을 줄 수 있기 때문에, 함께 업그레이드 해야되는지 알아야 한다.  

    ex) Dependency libarary A와 B가 있을 때, 둘 다 같은 Dependency를 갖고 있을 때,  
    A만 업그레이드 하면, B에서 문제가 발생할 수 있음.


2. 모든 Dependency에 상호 호환 가능한 Dependency를 분석하고 찾는것이 어렵다.

3. Dependency가 계속해서 증가한다면, Dependency들을 관리하는 것이 어려워진다.


</br>

---

</br>

## Importing Dependencies

개발을 할 때, 밑바닥부터 개발을 시작하는 것보다 필요한 기능이 이미 구현된 라이브러리를 가져오는 것이 시간과 비용 측면에서 더 나을 수 도 있어 그것을 사용해야 되는 상황들이 있다.

그럴 때 다음과 같은 것들을 고려해야한다.

</br>

### Compatibility Promises

+ 얼마나 많은 라이브러리에 호환이 되는가?
+ 해당 Dependency가 업그레이드 됐을 때, 다른 Dependency들에 어떤 영향을 미치는가?
+ 릴리즈 버전이 얼마나 지원되는가?

</br>

### Considerations When Importing

Dependency를 추가하는 것이 프로그래밍 할 때는 리소스가 적지만,  
 소프트웨어 엔지니어링을 한다고 했을 때, 나중에 더 많은 리소스가 들 수 있기 때문에  
 고려해야될 것들이 있다.

 </br>

구글에서는 다음과 같은 것들을 고려한 후 Dependency를 추가한다고 한다.

+ 프로젝트에 실행할 수 있는 테스트가 있습니까?

+ 그 테스트를 통과합니까?

+ 누가 그 의존성을 제공하고 있습니까? 평판이 전부는 아니지만 조사할 가치가 있습니다.

+ 프로젝트가 지향하는 호환성은 무엇입니까?
+ 프로젝트에서 지원될 것으로 예상되는 용도에 대해 자세히 설명합니까?
+ 프로젝트는 얼마나 인기가 있습니까?
+ 우리는 얼마나 오랫동안 프로젝트에 의존할 것입니까?
+ 프로젝트는 얼마나 자주 중대한 변경을 수행합니까?
여기에 내부적으로 초점을 맞춘 짧은 질문을 추가하십시오.
+ Google 내에서 해당 기능을 구현하는 것이 얼마나 복잡합니까?
+ 이 종속성을 최신 상태로 유지하려면 어떤 인센티브가 필요합니까?
+ 누가 업그레이드를 수행합니까? 

+ Google 내에서 해당 기능을 구현하는 것이 얼마나 복잡합니까?
+ 이 종속성을 최신 상태로 유지하려면 어떤 인센티브가 필요합니까?
+ 누가 업그레이드를 수행합니까?
+ 업그레이드를 수행하는 것이 얼마나 어려울 것으로 예상합니까?

</br>

--- 

</br>

## How Google Handles Importing Dependencies

구글에서는 대다수의 Dependency를 내부적으로 개발한다고 한다.  
그 이유는 코드베이스에 대한 어느정도 가시성이 주어진다면, 종속성을 추가했을때 발생하는 위험을 관리하기 훨씬 쉽고, Dependency 변화가 어떤 영향을 끼칠지 알게 되면 종속성 관리는 큰 문제가 안된다고 생각하기 때문이라고 한다.

</br>

---

</br>

## Dependency Management, In Theory

</br>

### Nothing Changes (aka The Static Dependency Model)

Dependency의 보안이 문제가 되지 않고, 버그가 없고, Dependency가 바뀌지 않는 조건이라는 것이 증명돼었을 때, API변경, 소프트웨어 동작 변경, 아무것도 변경하지 않는 방법을 말함.  

Nothing Change 모델은 애초에 가정했던 보안문제가 없고, 버그가 없고, Dependency가 바뀌지 않는 조건들이 잘못된 것이 드러나면 모든 Dependency Management를 다시 해야된다는 것이 단점이다.

</br>

### Semantic Versioning

Sementic Versioning은 소수로 구분된 3개의 정수를 사용해 일부 Dependency(특히 라이브러리)에 대한 버전 번호를 나타내어 Dependency Management를 하는 방식

3개의 정수는 각각 주, 부, 패치 버전을 나타내며,

주버전은 기존 공개된 API와 호환이 불가능하게 변경된 경우를 의미하고,  
부버전은 기존 공개된 API에 단순히 사용 기능을 추가한 것을 의미하고,  
패치버전은 기존 공개된 API에서 사용에 영향을 미치지 않는 작은 변경 또는 버그 수정을 의미한다.

</br>

Ex1)   
    2.x.x 버전과 1.x.x 버전의 차이는 1.x.x에서 쓰는 API가 2.x.x에서는 사용이 불가능한 수정이 있음.


Ex2)   
    1.5.x 버전과 1.4.x 버전의 차이는 1.4.x에서는 1.5.x에서 제공하는 특정 API 기능을 사용할 수 없음.
  

Ex3)
    1.4.0 버전과 1.4.1 버전의 차이는 1.4.0에서 발생하던 버그를 1.4.1에서는 수정된 것을 의미.

</br>

### Bundled Distribution Models

Bundled Distribution Model은 Dependency로 가지고 있는 라이브러리 묶음에서 호환될 수 있는 모든 버전들을 나누어 여러 버전으로 배포하는 방식이다.

</br>

### Live at Head

Live at Head 방식은 구글에서 사용하는 방식이다.  
Dependency를 특정한 버전으로 나누는 게 아니라, 어떤 Head(관리자)가 해당 Dependency를  
관리하고,  그것을 사용하는 사람들에게 에러가 생기지 않는지 모두 테스트하며 Dependency를 관리  
하는 방식을 말한다.  

이 방식은 다른 방식보다 종속성 문제 해결을 쉽게 하고,  

위에서 설명한 것과 같이 코드베이스에 대한 어느정도 가시성을 줌으로써, 종속성을 추가했을때 발생하는 위험을 관리하기 훨씬 쉽고, Dependency 변화가 어떤 영향을 끼칠지 알게 되면 종속성 관리를 더 쉽게 할 수 있다고 한다.

</br>

---

</br>

## The Limitations of SemVer

Sementic Version을 사용하는 경우 다음과 같은 제한 사항들이 있다고 한다.


+ Sementic Version을 너무 세분화 하게 되면, 라이브러리 설치를 하는 사람이 Dependency를 설치할 때 확인해야 될 것들이 많고, 그에 따른 리소스가 증가 할 수 있다.

+ Version을 매길 때, 세분화하거나, 비약하는 경우 버전 관리가 어려워 질 수 있다.

+ 버전을 관리하는 사람의 실수가 있을 수 있습니다. 변경이 어떤 변화를 주었는지를 볼 수 없기 때문에 종속성 문제를 해결하기 쉽지 않습니다.

+ 실제로 부버전, 패치버전을 올렸을 때, API가 호환이 안되는 잠재적 문제가 껴있을 수 있다. 이것을 검증할 방법이 없다.


</br>

---

</br>

## Dependency Management with Infinite Resources

### Exporting Dependencies

</br>

---

</br>

## Conclusion

</br>

---

</br>

## TL;DRs