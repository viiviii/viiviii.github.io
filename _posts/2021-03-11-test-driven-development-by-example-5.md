---
title: "테스트 주도 개발(TDD): 05장 정리"
excerpt: "[05장 솔직히 말하자면] 큰 테스트를 더 작게 정복하기"
date: 2021-03-11
---

📢 배우게 될 것 요약

- 큰 테스트를 작게 나누어 해결하는 습관
- 주기 5단계의 중요성

## 시작하기

<pre>
$5 + 10CHF = $10(환율이 2:1일 경우)
<del>$5 x 2 = $10</del>
<del>amount를 private으로 만들기</del>
<del>Dollar 부작용?</del>
Money 반올림
<del>equals()</del>
hashCode()
...생략
</pre>

할일 목록 중 가장 흥미로워 보이는 첫번째 테스트에 어떤 식으로 접근하는게 좋을까

👉 `$5 + 10CHF = $10(환율이 2:1일 경우)`

문제점

- 너무 큰 발걸음이다.
- 작은 단계 하나로 구현하는 테스트를 작성해낼 수 있을지 확실치 않다.

⇒ 그렇다면 좀 더 작게 생각하기

`Dollar` 객체와 비슷하게 작동하는 `Franc`이라는 객체를 만든다면 단위가 섞인 덧셈 테스트를 작성하고 돌려보는데 더 가까워질 것이다.

## STEP 1

🚩 테스트 작성

- `Dollar` 테스트를 복사해서 `Franc`로 수정

```java
@Test
void testFrancMultiplication() {
    Franc five = new Franc(5);
    assertEquals(new Franc(10), five.times(2));
    assertEquals(new Franc(15), five.times(3));
}
```

4장에서 테스트를 단순하게 리팩토링하여 지금 작업이 더 쉬워졌음

## STEP2

🚩 컴파일 에러 수정

- 현재 `Franc` 객체가 없으므로 `Dollar` 객체 코드를 복사해서  `Franc`으로 수정
- 빠르게 초록막대를 보기 위해서

```java
class Franc {
    private int amount;

    Franc(int amount) {
        this.amount = amount;
    }
		
    Franc times(int multiplier) {
        return new Franc(amount * multiplier);
    }

    @Override
    public boolean equals(Object obj) {
        Franc other = (Franc) obj;
        return amount == other.amount;
    }
}
```

이 단계에서 고수들은 몇 가지가 의아할 수 있음

- 복붙을 통한 재사용?
- 추상화 없음?
- 깨끗한 설계 없음?

### 잊어선 안될 것✅

- 우리에겐 `주기`가 있다는 것
- 각 단계에는 서로 다른 목적이 있다는 것

    ⇒ 다른 스타일의 해법, 다른 미적 시각을 필요로 함

## STEP 3

🚩 테스트 실행하여 성공하는 것을 확인

![단위 테스트를 성공하는 이미지](/assets/images/post/2021-03-11-test-driven-development-by-example-5.md)

## 다시 보는 주기 5단계

1. 테스트 작성
2. 컴파일 되게 하기
3. 실패하는지 확인하기 위해 테스트 실행
4. 실행되게 만듦
5. 중복 제거

처음 네 단계는 빨리 진행해야 함

- 그러면 새 기능이 포함되더라도 잘 알고 있는 상태에 이를 수 있음

## 마치기

> 📌 적절한 시기에 적절한 설계를.  
> 돌아가게 만들고, 올바르게 만들어라.

앞에서 처음 네 단계를 빠르게 진행하라고 되어있지만, 주기의 다섯 번째 단계 없이는 앞의 네 단계도 제대로 되지 않는다.

이번 장에서는

- 주기 5단계 중 네 단계를 빠르게 진행하였고
- 중복이 많기 때문에 다음 테스트를 작성하기 전에 이것들을 제거해야 함(주기 다섯 번째 단계)

🎈한 것🎈

- 큰 테스트를 공략할 수 없으므로 진전을 나타낼 수 있는 작은 테스트를 만듦
- 중복을 만들고 조금 고쳐서 테스트를 작성
- 모델 코드까지 복붙으로 수정하여 테스트를 통과
- 중복이 사라지기 전에는 집에 가지 않겠다고 약속했음👻
