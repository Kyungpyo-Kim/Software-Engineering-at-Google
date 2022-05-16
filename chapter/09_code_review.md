# Code Review

> Code Review: a process in which code is reviewed by someone other
> than author, often before the introduction of that code into a codebase.

* Benefits of code review
  * bug fix
  * code correctness
  * comprehension
  * consistency
  * **subtle effects** ...

## Code Review Flow
* 구글에서는 "precommit review" 를 하여, commit 을 하기도 전부터 code review 를 진행한다.
* LGTM (Looks Good To Me): a tag showing consent of another engineer

#### Google's Code Review Flow
1. 코드 작성자는 코드를 작성하고 코드가 바뀐 부분과 이에 대한 설명을 code review tool 에 올린다.
   * patch: snapshot of the code changes
2. 코드 작성자는 self-review 를 수행하고 다른 reviewer 들에게 code review 를 요청한다.
3. Reviewer 는 review 를 수행한다.
   * ~~ 수정해라
   * ~~ 확인해봐라 (information) 
4. 코드 작성자는 reviewer 의 코멘트에 대응하며, 3, 4 과정을 반복한다.
5. Reviewer 는 코드가 충분히 만족스러우면 LGTM 표시를 한다.
   * 구글에서는 1개의 LGTM 만 있으면 되지만, 보통은 reviewer 모두의 LGTM 을 필요로 함
6. Approval 을 통해 Commit 을 수행한다.

---
* Code Is a Liability (채무)
```
* 코드는 갚아야하는 빛이다.
* 코드를 처음부터 짜야 한다면 잘못 된 것이다.
* 코드의 중복을 피해야 한다.
* code review 를 하면서 project decision 을 다시 확인하는 일은 없도록 해야 한다.
```
---

## How Code Review Works as Google

#### Three Things for a Commit Approval
* approval from one of the code owners -> ownership 이 중요
  * maintainence
  * technical debt
* correctness and comprehension check (LGTM)
* readability
  * language's style and best practices
    ```bash
    python -c "import this"

    """
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

* code owner 의 경우 1개의 LGTM 를 받으면 Approval 의 조건을 만족할 수 있다.
* ownership 을 통해 scale up 을 효과적으로 관리한다.

## Code Review Benefits

## Code Review Best Pratices

## Types of Code Reviews

## Conclusion

## TL;DRs
