# Chapter 12. Unit Testing

이장의 대부분은 단위 테스트 수행에 대해 아이디어와 어떻게 유지보수를 할 것인지에 대해 설명하고 있음.

</br>

<b>테스트</b>  
  구현된 코드가 내가 의도한 방향대로 동작하는지를 검증하는 과정을 말하고,  
  테스트를 작성한다는 것은 해당 기능 확인을 위한 코드를 작성하는 것을 말함.

<!-- 구글이 테스크를 분류하는 두 가지 주요축은 크기와 범위이다.

+ 크기 : 테스크에서 사용되는 리소스와 테스크가 할 수 있는 것을 의미함.
+ 범위 : 테스트가 타당한지 검사하기 위한 코드의 양을 의미함. -->

</br>

<b>단위 테스트</b>  
  일 클래스 또는 메서드같이 상대적으로 작은 범위의 테스트를 지칭할때 사용한다.  
단위 테스트의 크기는 작지만, 항상 그렇지는 않음.

</br>

<b>단위 테스트의 장점</b>
+ 큰 시스템을 이해할 필요없이, 단위 테스트만을 이해하면 되서 테스트에 집중할 수 있음.
+ 단위 테스트 일정 부분에 집중할 수 있기 때문에, 문제가 발생했을 때 무엇이 문제인지 빠르게 찾을 수 있음.
+ 단위 테스트들은 해당 부분이 어떤 동작을 의도했고, 어떤 동작을 수행하는지에 대한 예시나, 문서로 제공될 수 있는 장점이 있음.

</br>

<b> Google's Unit Testing</b>  
+ 구글에서는 단위 테스트의 크기를 작게하려고 노력하고, 개발자들은 단위 테스트를 자주 실행하여 즉각적인 피드백을 받음.  
+ 구글에서 실행하는 테스트들은 대부분 단위테스트이고, 구글에서는 80%의 단위테스트와 20%의 광범위 테스트를 하도록 권장하고 있음.
+ 구글에서는 보통 개발자들이 작업할 때 수천번의 단위테스트를 실행하기 때문에, 테스트 유지 보수에 많은 공을 들임.


</br>
</br>

---

</br>
</br>

## The Importance of Maintainability


다음과 같은 상황을 상상해보자.
> 재윤이는 뭔가 새로운 기능을 추가하려고 한다. 그것은 몇줄의 코드만 있으면 매우 빠르게 할 수 있는 것이다.  
하지만, 재윤이가 바꾼 것을 검증하려고 자동 테스트 시스템을 실행했고, 화면을 꽉 채우는 에러를 보게됐다.  
이제 재윤이는 그 에러를 하나하나 확인하는데 하루를 보낸다.  
<br/>
> 사실 재윤이가 짠 코드는 버그가 있진 않았지만, 코드 내부 구조에 일부 규칙에 맞지 않아서 발생한것이었다.
재윤이는 이제 뭘 해야될지 모른다. 그리고 에러를 해결하기 위해 코드를 막 수정하거나 추가한다.  
이런것들은 앞으로의 테스트들을 더 이해하기 어렵게 만들것이다. 
결국 재윤이는 금방할 수 있던 일을 몇시간 몇일동안 일을 하며 생산성이 없어지고, 의기소침 해진다. 

</br>
</br>

위의 사례에서 보았을 때, 테스트는 원래 의도했던 코드의 품질을 높인다거나, 생산성을 향상시킨다거나 하는 방향과는 반대의 결과를 가져왔다. 이런 케이스는 정말 많으며, 구글의 많은 엔지니어들도 매일 이런 일들과 싸운다.

완벽하게 이런 일이 안생기게 할 순 없겠지만, 구글 엔지니어들은 이런 문제를 완화시키기 위해 일련의 패턴과 관행을 개발하기 위해 노력했음. 그리고 이것들을 회사의 다른 사람들이 따를 수 있도록 장려함.

</br>
</br>

--- 

</br>
</br>


## Preventing Brittle Tests

<b>Preventing Brittle Test</b>  
   내가 수정한 것과 관련없는 에러가 발생하는 테스트,  
   Brittle Test 발생시 반드시 해당 파트를 담당하고 있는 엔지니어들이 문제를 진단하고 수정해야함.

</br>

    작은 코드양에 적은 엔지니어로 구성된 팀같은 경우 이런 불안정한 테스트들을 만들어 내는것이 문제가 안될거라고 생각할 수 있겠지만, 만약 계속해서 이런 불안정한 테스트들을 하는 경우 어디서 부터 문제가 발생한건지 샅샅이 조사해야 돼서 점점 더 많은 시간을 쓰게 됌.

    정말 구글같은 경우 작은 테스트를 진행한다고 해도, 수십만개의 sub 테스트들이 트리거 될 수 있고, 수십만개의 sub 테스트들이 트리거된 상황에서는 정말 많은 시간을 할애해야 되는 수가 있음.


구글은 보다 Brittle Test를 피할 수 있게하는 패턴, 관행들을 찾아냈음.

</br>
</br>

### Strive for Unchanging Tests

이상적인 테스트는 코드가 만들어진 이후 시스템의 요구 사항이 변경되지 않는 한 테스트를 바꿀 필요가 없어야 함.  

다음은 테스트가 변경될 수 있는 상황들을 분류한 것이다.
+ Pure refactorings    
    인터페이스, 시스템 동작 변환없이, 단순히 코드 구조를 리팩토링 하는 경우 시스템 테스트를 바꿀 필요는 없음.  
    <b>리팩토링이 시스템 동작에 영향을 주거나 하는 경우는 시스템 테스트를 바꿔야 함.</b>  
    
+ New features  
    새 기능을 추가했을 때, 기존 시스템 동작이 바뀌면 안됌.  
    <b>새롭게 추가한 기능에 대한 테스트만 작성하면 됌.</b>

+ Bug fixes  
    버그를 수정한다는 것은 기존 테스트에서 테스트케이스가 누락됐다는 것을 의미함.  
    <b>따라서, 기존 테스트는 바꾸지 않고, 테스트 케이스만 추가해줌.</b>

+ Behavior changes  
    <b>시스템 기존 동작이 변경되면, 기존 테스트를 바꿔야함.</b>  
    주의사항으로는 시스템 동작이 바뀌면 기존 동작을 사용하고 있던 사용자와 조율이 필요함.
    


</br>
</br>

### Test via Public APIs

<b>코드 테스트를 진행할 때는 구현 세부 내용이 아닌 실제로 사용자가 사용하는 API호출을 통해 테스트해라.</b>  
해당 테스트가 안된다면, 사용자한테도 영향을 줄 것이고, 테스트는 사용자에게 좋은 예제 또는 문서가 될 수 있음.

</br>
Case - 구현 코드

```
public void processTransaction(Transaction transaction) {
    if (isValid(transaction)) {
        saveToDatabase(transaction);
    }
}

private boolean isValid(Transaction t) {
    return t.getAmount() < t.getSender().getBalance();
}

private void saveToDatabase(Transaction t) {
    String s = t.getSender() + "," + t.getRecipient() + "," + t.getAmount();
    database.put(t.getId(), s);
}

public void setAccountBalance(String accountName, int balance) {
    // Write the balance to the database directly
}

public void getAccountBalance(String accountName) {
    // Read transactions from the database to determine the account balance
}
```

Case - Bad Test Case  
실제 사용자가 쓰는 API가 아님.
```
@Test
public void emptyAccountShouldNotBeValid() {
    assertThat(processor.isValid(newTransaction().setSender(EMPTY_ACCOUNT)))
    .isFalse();
}
@Test
public void shouldSaveSerializedData() {
    processor.saveToDatabase(newTransaction()
    .setId(123)
    .setSender("me")
    .setRecipient("you")
    .setAmount(100));
    assertThat(database.get(123)).isEqualTo("me,you,100");
}
```

Case - Good Test Case  
실제 사용자가 쓰는 API임.
```
@Test
public void shouldTransferFunds() {
    processor.setAccountBalance("me", 150);
    processor.setAccountBalance("you", 20);
    processor.processTransaction(newTransaction()
    .setSender("me")
    .setRecipient("you")
    .setAmount(100));
    assertThat(processor.getAccountBalance("me")).isEqualTo(50);
    assertThat(processor.getAccountBalance("you")).isEqualTo(120);
}
@Test
public void shouldNotPerformInvalidTransactions() {
    processor.setAccountBalance("me", 50);
    processor.setAccountBalance("you", 20);
    processor.processTransaction(newTransaction()
    .setSender("me")
    .setRecipient("you")
    .setAmount(100));
    assertThat(processor.getAccountBalance("me")).isEqualTo(50);
    assertThat(processor.getAccountBalance("you")).isEqualTo(20);
}
```

</br>
</br>

<b>테스트 단위 설정 가이드</b>

+ 만약 어떤 메서드나 클래스가 다른 클래스에서 사용되는 용도로만 존재한다면,  
해당 메서드를 테스트하는게 아니라 그 메서드를 사용하는 클래스를 통해 테스트한다.

+ 클래스 또는 메서드가 접근가능하고, 사용자가 해당 클래스, 메서드를 사용한다면,  
그것은 단위로써 테스트를 수행한다.

+ 클래스 또는 메서드가 접근가능하지 않더라도, 다른 클래스나 메서드에서 범용적으로 사용되는 기능을 제공한다면,  
단위 테스트를 수행한다.




</br>
</br>

### Test State, Not Interactions
시스템이 정상적으로 동작하는지 확인하는 방법은 <b>상호작용 테스트와 상태 테스트</b> 두가지가 있다.
상호작용 이후 시스템 상태에 대해 테스트하는 것이 가장 정확한 동작 상태를 테스트하는 방법이다.

</br>
</br>

--- 

</br>
</br>

## Writing Clear Tests

테스트 Fail은 엔지니어에게 버그에 대한 정보를 주기 때문에 좋다.
이 Fail이 나는 2가지 Case는 아래와 같다.

+ 테스트하는 시스템에 문제가 있는 경우
+ 테스트 자체에 문제가 있는 경우

</br>

Fail이 어떤 Case에 속하는지 빠르게 알기 위해서는 테스트 자체가 명확해야 함.  
테스트 자체가 명확해야 추후 변경사항에 대해 새로운 테스트를 작성하기가 수월 함.  
아래 내용은 테스트를 명확하게 작성하기 위한 방법들을 소개함.

</br>
</br>


### Make Your Tests Complete and Concise

무엇을 TEST하는것인지 직관적으로 보이게 해라.
+ 테스트하는 모듈에 직관적이지 않은 아규먼트들을 사용하지 말아라.
+ 아규먼트가 필요하다면 Helper Method를 구현해서 Test를 직관적으로 이해할 수 있도록 해라.

```
@Test
public void shouldPerformAddition() {
  Calculator calculator = new Calculator(new RoundingStrategy(),
  "unused", ENABLE_COSINE_FEATURE, 0.01, calculusEngine, false);
  int result = calculator.calculate(newTestCalculation());
  assertThat(result).isEqualTo(5); // Where did this number come from?
}
```

```
@Test
public void shouldPerformAddition() {
  Calculator calculator = newCalculator();
  int result = calculator.calculate(newCalculation(2, Operation.PLUS, 3));
  assertThat(result).isEqualTo(5);
}
```

### Test Behaviors, Not Methods
Behavior 테스트하는 방식을 사용해라.
Method 테스트 방식은 코드가 복잡해지면,   
테스트 또한 복잡해지고, 명확해지지 않는다.   

</br>
</br>

+ Method Driven Test  
  메서드가 실행됐을때 나오는 결과물들을 토대로 테스트를 진행하는 방식


+ Behavior Driven Test  
  메서드 내에서 수행되어야 되는 행동들을 테스트 하는 방식  
    
  </br>
  
  테스트 할 코드
  ```  
  public void displayTransactionResults(User user, Transaction transaction) {
    ui.showMessage("You bought a " + transaction.getItemName());
    if (user.getBalance() < LOW_BALANCE_THRESHOLD) {
      ui.showMessage("Warning: your balance is low!");
    }
  }
  ```
  

  
  method-driven test
  ```
  @Test
  public void testDisplayTransactionResults() {
    transactionProcessor.displayTransactionResults(
      newUserWithBalance(LOW_BALANCE_THRESHOLD.plus(dollars(2))),
      new Transaction("Some Item", dollars(3))
    );
    
    assertThat(ui.getText()).contains("You bought a Some Item");
    assertThat(ui.getText()).contains("your balance is low");
  }
  ```

  behavior-driven test
  
  ```
  @Test
  public void displayTransactionResults_showsItemName() {
    transactionProcessor.displayTransactionResults(
      new User(), new Transaction("Some Item")
    );

    assertThat(ui.getText()).contains("You bought a Some Item");
  }

  @Test
  public void displayTransactionResults_showsLowBalanceWarning() {
    transactionProcessor.displayTransactionResults(
      newUserWithBalance(LOW_BALANCE_THRESHOLD.plus(dollars(2))),
      new Transaction("Some Item", dollars(3))
    );

  assertThat(ui.getText()).contains("your balance is low");
  }

  ```

### Don’t Put Logic in Tests
테스트에 있어 연산자, 루프, if문과 같은 프로그래밍을 하지 말아라.  
테스트의 명확성을 떨어뜨린다.   

</br>

### Write Clear Failure Messages
Fail 메세지에는 테스트 이름과 동일한 정보와 원하는 결과, 실제 결과, 관련 매개변수를 포함해야 한다.


</br>
</br>

---

## Tests and Code Sharing: DAMP, Not DRY
Don't Repeat Yourself 방식에 따라 코드를 작성하면,  
한군데서만 코드를 수정하면 되기 때문에 코드관리는 수월하지만,   
사용자가 해당 코드를 이해하기 위해서 관련 체인을 따라가야되는 문제가 있음.

테스트를 작성할 때는 최대한 DAMP(Descriptive And Meaningful Phrases)  
중복이 되더라도 이해하기 편하게 만들어야 한다.
아래는 테스트를 DAMP하게 하는 방식들을 소개한 내용이다.

### Shared Values
테스트하는 모듈의 인자로 shared value를 사용하는 경우가 많다. 
이런 shared value를 사용할 때는 helper method를 사용해라.   
helper method를 사용하면 각 테스트에서 필요한 값을 명확하게 표현할 수 있음.

</br>

### Shared Setup
테스트를 위해 필요한 정보들을 한곳에서 초기화하는 setup 모듈을 만들어라.  
초기화 반복을 방지하여 테스트를 보다 명확하고 간결하게 만들 수 있다.

</br>

### Defining Test Infrastructure
테스트 인프라는 여러 테스트에 사용하는 테스트 코드를 제공하는 클래스와 비슷한 성격의 코드를 말함.
테스트 인프라를 만드는 것은 매우 신중해야 되고, 자체적인 테스트가 필요하지만,   
테스트를 쉽게 할 수 있게 만드는 좋은 방법임.



<!-- </br>
</br>

---

## Conclusion



</br>
</br>

---

</br>
</br>

## TL;DRs

</br>
</br>

---

</br>
</br>


</br>
</br>

---

</br>
</br> -->



### Reference

단위테스트 - https://mangkyu.tistory.com/143
구글 테스트 예제 - https://tttsss77.tistory.com/225


</br>
</br>

