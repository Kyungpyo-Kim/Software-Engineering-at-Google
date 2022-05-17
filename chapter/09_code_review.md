# Code Review

> Code Review: a process in which code is reviewed by someone other
> than author, often before the introduction of that code into a codebase.

- Benefits of code review
  - bug fix
  - code correctness
  - comprehension
  - consistency
  - **subtle effects** ...

## Code Review Flow

- 구글에서는 "precommit review" 를 하여, commit 을 하기도 전부터 code review 를 진행한다.
- LGTM (Looks Good To Me): a tag showing consent of another engineer

#### Google's Code Review Flow

1. 코드 작성자는 코드를 작성하고 코드가 바뀐 부분과 이에 대한 설명을 code review tool 에 올린다.
   - patch: snapshot of the code changes
2. 코드 작성자는 self-review 를 수행하고 다른 reviewer 들에게 code review 를 요청한다.
3. Reviewer 는 review 를 수행한다.
   - ~~ 수정해라
   - ~~ 확인해봐라 (information)
4. 코드 작성자는 reviewer 의 코멘트에 대응하며, 3, 4 과정을 반복한다.
5. Reviewer 는 코드가 충분히 만족스러우면 LGTM 표시를 한다.
   - 구글에서는 1개의 LGTM 만 있으면 되지만, 보통은 reviewer 모두의 LGTM 을 필요로 함
6. Approval 을 통해 Commit 을 수행한다.

---

- Code Is a Liability (채무)

```
* 코드는 갚아야하는 빛이다.
* 코드를 처음부터 짜야 한다면 잘못 된 것이다.
* 코드의 중복을 피해야 한다.
* code review 를 하면서 project decision 을 다시 확인하는 일은 없도록 해야 한다.
```

---

## How Code Review Works as Google

#### Three Things for a Commit Approval

- approval from one of the code owners -> ownership 이 중요
  - maintainence
  - technical debt
- correctness and comprehension check (LGTM)
- readability

  - language's style and best practices

    ```bash
    python -c "import this"

    """python
    The Zen of Python, by Tim Peters

    Beautiful is better than ugly.
    Explicit is better than implicit.
    Simple is better than complex.
    Complex is better than complicated.
    Flat is better than nested.
    Sparse is better than dense.
    Readability counts.
    Special cases aren't special enough to break the rules.
    Although practicality beats purity.
    Errors should never pass silently.
    Unless explicitly silenced.
    In the face of ambiguity, refuse the temptation to guess.
    There should be one-- and preferably only one --obvious way to do it.
    Although that way may not be obvious at first unless you're Dutch.
    Now is better than never.
    Although never is often better than *right* now.
    If the implementation is hard to explain, it's a bad idea.
    If the implementation is easy to explain, it may be a good idea.
    Namespaces are one honking great idea -- let's do more of those!
    """
    ```

- code owner 의 경우 1개의 LGTM 를 받으면 Approval 의 조건을 만족할 수 있다.
- ownership 을 통해 scale up 을 효과적으로 관리한다.

## Code Review Benefits

구글에서는 창의적의고 새로운 것에 대한 빠른 적응을 위해서 관료적이고 형식적인 절차를 지양하지만, code review 에 대해서는 예외적으로 필수적인 과정으로 적용하고 있다. Code review 로 인한 속도의 지연, 많은 비용의 발생을 감수하고도 아래와 같은 장점들로 인해 code review 를 필수적으로 도입하고 있다.

1. Code Correctness
  * 지금 잘 만든 코드는 나중에 덜 고치게 된다. (오히려 속도 면에서 더 효과적...)
  * 초기에 문제를 해결하는 것이 좋다.
  * Correctness 는 "code patch scale" 뿐만 아니라 **"codebase scale"** 에서도 따져봐야 한다.

2. Comprehension of Code
  * 모두가 이해할 수 있는 코드를 만들어야 한다.
  * 코드는 한번 쓰여지지만 여러번 읽힌다.

3. Code Consistency
  * 유지보수가 용이하게 일관성 있는 코드를 생산해야 한다.
  * 쉽고 간결하게 작성해야 한다.

4. Psychological and Cultural Benefits (some subtle effects...)
  * 개발자들이 코드는 개발자의 것이 아닌 모두의 것이라는 인식을 주게 된다.
  * 코드에 대한 자유로운 비판을 가능하게 한다.
  * 최선을 다하게 만든다.

5. Knowledge Sharing


## Code Review Best Pratices
Code review 의 도입은 항상 쉽지 않지만 Google 에서는 최대한 간결하고 긴밀하게 그리고 scale-up 할수 있도록 노력하고 있다.

1. Be Polite and Professional
  * trust and repect
  * 인간적으로, 상식적으로...
  * 겸손과 배려
  * 상처 받지 않게...

2. Write Small Changes
  * Google's small: less than 200 lines
  * changes in a single file: 빠른 리뷰 가능

3. Write Good Change Descriptions
  * the worst descriptions: Bug fix, new feature, code update

4. Keep Reviewers to a Minimum
  * 한명이면 충분하다.
  * 많아질수록 참여가 줄어들 수 있다.

5. Automate Where Possible
  * tool 을 사용하다. 뒤에 나온다.


## Types of Code Reviews
code review 의 수준은 다 다를 수 있다. 각기 맞는 수준, 포인트의 code review 가 필요하다.

1. Greenfield Code (새로 짠) Reviews
  * generally and simply solve a real problem
  * 구글에서는 design 에서 많은 노력을 기울이기 때문에 design 측면에서의 code review 는 적절하지 않다.

2. Behavrioral Changes, Improvements, and Optimizations
  * 꼭 필요한가?
  * CI tool 을 이용하여 codebase 에 영향을 미치지 않는지 체크한다.

3. Bug Fixes and Rollbacks

4. Refactorings and Large-Scale Changes
  * Large-Scale Changes: 코드를 자동으로 refactoring 해준다. chapter 22에서 다룰 예정

## Conclusion
* 코드리뷰의 중요성, 이점
* 민첩한 코드리뷰
* Tools