---
title: "테스트 주도 개발(TDD): 02장 정리"
excerpt: "02장 타락한 객체. 01장의 구현에서 새로운 테스트 케이스를 추가하여 잘못된 구현을 올바르게 리팩토링 및 문제를 나누어 해결하기"
date: 2021-02-27
---

📢 배우게 될 것 요약  
- 1장의 구현에서 테스트 케이스를 추가하여 Dollar 객체의 부작용을 확인
- Dollar 객체의 부작용 리팩토링

## 시작하기

> 우리의 목적은 "작동하는 깔끔한 코드를 얻는 것"이다

하지만 이것은 대체로 어려움

⇒ 그렇다면 나누어서 정복하자

- 일단 '작동하는'에 해당하는 문제를 먼저 해결
- 그러고 나서 '깔끔한 코드' 부분을 해결

### 일반적인 TDD 주기

1. 테스트를 작성한다
2. 실행 가능하게 만든다
3. 올바르게 만든다

위 두 개의 주제는 같은 맥락인 것 같음

✅ 문제를 나누어서 해결하기

## Dollor 객체의 부작용

앞의 01장에서 Dollor 연산을 기능을 만들었지만, 아래와 같이 `time(3)`을 넣을 경우엔 실패함

```java
@Test
void testMultiplication() {
    Dollar five = new Dollar(5);
    five.times(2);
    assertEquals(10, five.amount);
    five.times(3); // 추가
    assertEquals(15, five.amount); // 추가
}
```

인스턴스 변수인 `amount` 값이 계속 변경되기 때문인데

```java
void times(int multiplier) {
    amount *= multiplier;
}
```

책에선 이 문제를 `times()`에서 새로운 달러 객체를 반환하는 방법으로 진행함

```java
Dollar times(int multiplier) {
    return new Dollar(amount * multiplier);
}
```

```java
@Test
void testMultiplication() {
    Dollar five = new Dollar(5);
    Dollar product = five.times(2);
    assertEquals(10, product.amount);
    product.times(3);
    assertEquals(15, product.amount);
}	
```

몇 개의 예제를 진행하면서 "*어이고 1 x 3을 1+1+1 순서로 하는 것 같네*"라고 생각했는데 이번 장에서 설명을 해주었음

## 최대한 초록 막대를 보기 위한 세 가지 전략 중 두 가지

1. 가짜로 구현하기: 상수를 반환하게 만들고 진짜 코드를 얻을 때 까지 단계적으로 바꾸어감
2. 명백한 구현 사용하기: 실제 구현을 입력
3. ~~3장에서 보게 될 삼각측량~~

켄트벡은 실무에서 TDD를 사용할 때 이 두 방법을 번갈아 가며 사용함

내가 뭘 입력해야 할 지 알 때 → 명백한 구현 사용

- 명백한 구현이 정말 명백한 사실이 맞는지 테스트를 한번씩 실행해야 함
- 테스트가 실패할 경우 `가짜로 구현하기` 방법을 사용하여 올바른 코드로 리팩토링
- 다시 위 단계를 반복

## 마치기

- 느낌을 테스트로 변환하는 것은 TDD의 일반적 주제다
    - 하나의 Dollar 객체에 곱하기를 두 번(`times(2)`, `times(3)`) 수행해본 것
- 이런 작업을 오래 할 수록 테스트로 담아내는 것에 점점 익숙해진다
- 이걸 익숙해질 수록 설계 논의를 더 재미있게 할 수 있다
    - 시스템이 어떤 식으로 동작해야 하는지 논의할 수 있음
    - 올바른 행위에 대한 결정을 내린 후에, 그 행위를 얻어낼 수 있는 최상의 방법에 대해 이야기할 수 있음
