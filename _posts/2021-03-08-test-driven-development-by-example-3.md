---
title: "테스트 주도 개발(TDD): 03장 정리"
excerpt: "[03장 모두를 위한 평등] Value Object Pattern, 삼각측량 전략 등"
date: 2021-03-08
---

📢 배우게 될 것 요약

- 값 객체 패턴 및 특징 3가지(새 객체 반환, 불변객체, equals() 구현)
- 삼각측량 전략($5 != $6)
- Dolloar 객체의 인스턴스 변수 amount를 private하게 변경

## 시작하기

> 어떤 정수에 1을 더했을 때 → a+1일 때  
> 우리는 a가 변할거라고 예상하기 보다는 a+1인 새로운 값을 갖게될 것을 예상한다.

하지만 일반적으로 객체는 우리 예상대로 작동하지 않음

- 어떤 계약 + 보상 ⇒ 계약 자체가 변함

## 값 객체 패턴(value object pattern)

> 지금의 Dollar 객체와 같이 "객체를 값처럼 쓸 수 있는 것"

값 객체에 대한 제약사항 중 하나

- 객체의 인스턴스 변수가 생성자를 통해 설정된 후에는 결코 변하지 않아야 함

### 값 객체가 암시하는 것

- 2장에서와 같이 `모든 연산은 새 객체를 반환`해야 함
- $5가 있을 때 영원히 $5임을 보장받을 수 있음 → `immutable`
- `equals()를 구현`해야 한다
    - 왜냐하면 $5는 항상 다른 $5와 같은 값이어야 하니까

## equals() 구현하기

`Dolloar(5)`가 같은지 비교하는 테스트 코드 작성

```java
@Test
void testEquality() {
    assertTrue(new Dollar(5).equals(new Dollar(5)));
}
```

빨간 막대를 보았으므로 빠르게 다음 단계로 넘어가기 위해 가짜로 구현하기

```java
@Override
public boolean equals(Object obj) {
    return true;
}
```

이제 여기까지 하면 초록막대가 뜨고 `return true` 부분을 구현하는 방식으로 변경할텐데

책에서는 삼각측량을 설명하기 위해 예제를 하나 더 추가함

### 삼각측량 전략

- 예제가 두 개 이상 있어야만 코드를 일반화할 수 있다
- 두번째 예제가 좀 더 일반적인 해를 필요로 할 때 그때 일반화한다

### 두번째 예제인 $5 != $6 추가

```java
@Test
void testEquality() {
    assertTrue(new Dollar(5).equals(new Dollar(5)));
    assertFalse(new Dollar(5).equals(new Dollar(6)));
}
```

### equals() 변경

추가된 예제까지 초록 막대를 보기 위해 `equals()` 변경

```java
@Override
public boolean equals(Object obj) {
    Dollar other = (Dollar) obj;
    return amount == other.amount;
}
```

- Dollar와 Dollar를 직접 비교할 수 있게 됨
- ⇒ 올바른 인스턴스 변수들이 그러듯 amount를 private하게 만들 수 있게 됨

## 마치기

켄트벡은 삼각측량 전략을 아래와 같이 사용

- 어떻게 리팩토링해야 하는지 전혀 감이 안올 때만 사용함
- 일반적인 해법이 보이면 그 방법대로 구현
    - 왜 한번에 끝낼 수 있는 있는데 테스트를 또 만들어서 하는가?
- 하지만 어떻게 할지 떠오르지 않을 때면, **삼각측량은 문제를 조금 다른 방향에서 생각해볼 기회를 제공함**
    - 지금 설계하는 프로그램이 어떤 변화 가능성을 지원해야 하는가?
        - 몇몇 부분을 변경시켜보면 답이 좀 더 명확해짐

🎈한 것🎈

- 디자인 패턴이 하나의 또 다른 오퍼레이션을 암시한다는걸 알게됨
- 오퍼레이션 테스트
- 오퍼레이션 간단하게 구현
- 바로 리팩토링 하는것이 아닌 예제를 추가하여 테스를 더 했음
- 두가지 예제를 모두 만족하도록 리팩토링 하였음
