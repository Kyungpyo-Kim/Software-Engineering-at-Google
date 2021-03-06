# What IS Software Engineering?
### 소프트웨어 엔지니어링 = 프로그래밍?
- 소프트웨어 엔지니어링과 프로그래밍은 같지 않다.

- 프로그래밍은 소프트웨어 엔지니어링에 포함하는 부분이며 중요하게 다뤄지는 부분임
	- 프로그래밍 = 새로운 소프트웨어를 생성하는 방법(개발)
	- 소프트웨어 엔지니어링 = 새로운 소프트웨어를 생성하고 유지보수 하는 방법(개발, 수정, 유지보수)
- 소프트웨어 엔지니어링과 프로그래밍 사이의 중요한 세 가지 차이점
	1. 시간
	2. 규모
	3. 트레이드 오프
 ---
 ## 시간
 ![](https://blog.kakaocdn.net/dn/2HCix/btrfXxInTdV/hdMGvqqKzHddMNq2tI7zCk/img.png)
- 코드의 기대 수명은 얼마인가?
	- 임시로 작성된 **몇 분** 동안만 사용하는 코드
		- 프로그래밍을 처음 접하는 사람이 공부하기 위한 코드 등
	 	- OS, 라이브러리, 하드웨어, 프로그래밍 언어의 새로운 버전에 적용할 필요가 없음
		 	- 투자하는 시간이 적음
	- **몇 년** 동안 사용하는 코드
		- Google의 검색 코드, Linux 커널 코드 등 기대 수명이 사실상 무한대인 코드
	 	- OS, 라이브러리 등이 업데이트 될 경우 이에 맞춰서 업데이트를 해주어야 함.
		 	- 오랜 기간 투자 해야 함 
	 	- 또한, 첫 번째 대규모 업그레이드를 성공적으로 수행하는것 뿐만 아니라 앞으로의 대규모 업데이트에서도 안정적으로 최신 상태를 유지할 수 있는 지점에 도달해야함
		 	- 장기적인 지속 가능성의 핵심
	- ...

- 프로그래밍에 시간이라는 개념이 추가되면서 고려해야할 대상이 늘어남.

## 규모
-  Software Engineering : The multiperson development of multiversion programs
- 소프트웨어를 개발할때에는 여러 팀 인원이 참가하여 프로그램을 만드므로 프로그램의 확장성 또한 생각하여 시스템을 설계해야함
- 즉, 소프트웨어 엔지니어링은 규모확장에 대해서도 고민해야함.

## 하이럼의 법칙
```
With a sufficient number of users of an API, 
it does not matter what you promise in the contract: 
all observable behaviors of your system will be depended on by somebody.
```
- API를 사용하는 유저가 충분히 많아지면 내가 만든 의도와 상관없이 사용자들의 의도대로 API가 작동된다.
- API의 원래의 의도에는 벗어나는 버그 여도 많은 사용자가 접한 이상 이를 무작정 수정하면 안되고,  버그를 위한 하위 호환성 까지 지킬 수 있어야 한다.
 
## 변화 없음을 목표로 삼지 않는 이유
- 대부분의 프로젝트는 기본 기술의 변화에 노출 되어 있다. 대부분의 프로그래밍 언어와 런타임은 자주, 많이 변경되고 있다. 높은 안정성을 보이는 C역시도 때떄로 사용자에게 영향을 줄 수 있는 새로운 기능을 지원하도록 변경이 되고 있기 때문에, 변화 하지 않는것은 상당히 위험한 도박이다.
- 프로젝트가 의존하는 모든 기술에는 위험 요소가 포함되어 있다. 이를 막기 위해서는 결국 프로그래밍에 변화를 주어야 한다.

## 규모와 효율성
- Google의 생산 시스템과 같이 규모가 큰 프로젝트에서는 규모에 더 큰 비중을 투자하게 된다.  규모가 커지게 된다면 자연적으로 무언가를 변경하는데 막대한 비용이 들게 되고 또, 자연스럽게 프로젝트가 연기될 가능성이 있다. 하지만 시간이 지나게 된다면 비용은 초선형적으로 증가하게 되어 작업을 더 이상 확장할 수 없게 된다.
	- 프로젝트 범위가 두 배로 확장되면 노동량도 두 배로 확장되는가? 인적 자원은 충분한가?
	- 인적 자원 뿐만 아닌 컴퓨터 리소스 역시 충분히 확장 가능한가?
	- 전체 빌드를 진행하는데 걸리는 시간 역시 두 배로 증가하는가?
	- 리포지토리를 새로 풀 하는데 시간이 충분한가?
- 이러한 의미에서 조직에서 관리하는 프로젝트는 전체 비용 및 리소스 소비 측면에서 확장이 가능하여야 한다.

## 의사 결정
- **프로그래밍 하는 방법**을 이해 하고, 유지 관리하는 **소프트웨어의 수명**을 이해하고, 규모가 커짐에 따라 **소프트웨어를 유지 관리하는 방법**을 이해하게 된다면 가장 마지막으로 남은 것은 **올바른 결정**을 내리는 것이다.
- "내가 그렇게 말했기 때문에"는 올바른 결정을 내리는데 가장 큰 방해 요소가 된다. 결정이 잘못된것이라 생각해도 합당한 이유가 있는 결정은 이해해 주고 합의해 줄 수 있어야한다.
- 합당한 이유를 만드는데 가장 중요한것 중 하나는 비용이며 이는 금전적인 문제만을 이야기 하지않고 아래의 것들을 모두 포함하는 비용을 말한다.
	1. 금전적 비용
	2. 리소스 비용(CPU 작업 시간 등)
	3. 인건비
	4. 거래 비용(해당 결정을 내리는데 드는 비용)
	5. 기회 비용(해당 결정을 내리지 않을 경우 얻을 수 있는 이익)
	6. 사회적 비용(해당 결정을 내리게 될 경우 사회에 미치는 영향)
		- 역사적으로 볼때 사회적 비용 문제는 무시되기 쉬었으나, 구글과 같이 수십억 인구가 사용하는 프로그램에서는 이 또한 무시할 수 없는 큰 비용으로 다가온다.
- 엔지니어링을 할 때 결정은 다음과 같이 말할 수 있어야 한다.
	```
	우리는 (법적 요구 사항 또는 고객 요구 사항) 을 준수해야 하기 때문에 이 일을 하고 있습니다.
	우리는 현재 그 동안의 경험에 기초하여 당시에 볼 수 있는 최선의 선택을 하였기 때문에 이 작업을 수행하고 있습니다.
	```
	~~- "내가 그렇게 말했기 때문에 우리는 이것을 하고 있습니다"~~

- 데이터의 무게를 측정할 때 일반적인 두 가지의 시나리오
	1. 관련된 모든 비용은 측정 가능하거나 최소한 추정이 가능하다.
		- N개의 CPU를 절약하기 위해 2주 동안 엔지니어가 작업을 필요로 한다.
	2. 일부 수량은 미묘하거나 측정 방법을 모른다.
		- 이 작업을 수행하는데 얼마나 걸릴지 알 수 없다.
	- 첫 번째 시나리오의 경우 결정을 내리는데 어려움이 전혀 없다. 위에서 말한 비용들을 토대로 어떤 것이 이득인지 선택을 하면 된다.
	- 두 번째 시나리오의 경우 결정을 내리는데 많은 어려움이 든다. 가장 좋은 방법은 결정에 대해 동일한 우선 순위를 두고 신중하게 신중히 다시 보는 것이다.
- 결정을 재검토하고 실수를 저지르는 것에 대한 이점은 이를 진행한 사람이 실수를 인정할 수 있는 능력을 배울 수 있다는 것이다. 데이터 기반의 사회에서 완전히 새로운 데이터가 들어오거나 가정이 바뀌게 된다면 결정의 오류는 발생 할 수 밖에 없다. 하지만 장기적 관점에서 볼 때 이는 더 이상 중요한 문제가 아닌 하나의 사소한 실수가 되게 된다. 이때 결정권자가 실수를 인정하고 빠르게 수정을 진행하면 반대로 더 좋은 결정을 내릴 수 있을 것이다. 

## 소프트웨어 엔지니어링 VS 프로그래밍
1. 프로그래밍이 소프트웨어 엔지니어링보다 나쁜가?
2. 수백 명으로 구성된 팀이 10년 동안 지속될 것으로 예상되는 프로젝트가 두 사람이 만든 한 달 동안만 유효한 프로그램보다 더 가치 있는 프로그램인가?
- "소프트웨어 엔지니어링"과 "프로그래밍"은 확실하게 구별되는 것이 중요하다. 이 두 영역의 차이의 대부분은 **시간 경과**에 따른 코드 관리, **규모**에 대한 시간의 영향, 이러한 아이디어에 직면한 **의사 결정**에서 비롯한다.
