---
title: "테스트 주도 개발(TDD): 04장 정리"
excerpt: "04장 프라이버시. 테스트를 좀 더 명시적이게 변경하고 테스트와 코드의 결합도를 낮추기"
date: 2021-03-09
---


📢 배우게 될 것 요약

- 테스트를 좀 더 명시적으로 변경
- 테스트와 코드의 결합도를 낮추려는 노력
- TDD를 하면서 적극적으로 관리해야할 위험 요소

## 시작하기

> 👍마음에 드는 구절  
> "테스트가 조금 더 많은 이야기를 해줄 수 있도록 만들자."

이번 장에서는 assert문에서 조금 마음에 안들던 부분을 바꾼다.

## 기존에 작성한 테스트의 아쉬운 점

Dollar.times()가 하는 일

- Dollar객체에 인자 값 만큼 곱한 값을 갖는 Dollar를 반환

```java
Dollar five = new Dollar(5);
Dollar product = five.times(2);
assertEquals(10, product.amount);
product = five.times(3);
assertEquals(15, product.amount);
```

하지만 기존 테스트는 이 `Dollar.times()`가 하는 일을 정확하게 말하지 않고있음

## 🚩 STEP 1

> 수정 1. 객체로 값 비교하게 변경

앞 장에서 `equlas()`를 오버라이드 하여 amount 값과 상수의 비교가 아닌 객체끼리 비교가 가능해졌으므로 이 부분을 변경함

```java
Dollar five = new Dollar(5);
Dollar product = five.times(2);
assertEquals(new Dollar(10), product.amount);
product = five.times(3);
assertEquals(new Dollar(15), product.amount);
```

## 🚩 STEP 2

> 수정 2. 변수 product 제거

`product.amount`도 `five.times()`로 변경하고, 필요 없어진 `product` 변수를 제거함

```java
Dollar five = new Dollar(5);
assertEquals(new Dollar(10), five.times(2);
assertEquals(new Dollar(15), five.times(3));
```

이제 테스트는 우리의 의도를 더 명확하게 이야기해줌

## 🚩 STEP 3

> 수정 3. 변수  amount를 private하게 변경

이제 Dollar 객체의 amount인스턴스 변수를 사용하는 코드는 테스트 코드에서도 없으므로 변수를 private하게 변경할 수 있게 됨

```java
class Dollar {
    private int amount;
...
```

## 변경한 테스트에서 아쉬운 점

이렇게 테스트를 보완했지만 위험한 상황을 만들었다는 점을 알아야 함

- 만약 `equlas` 테스트가 정확히 작동한다는 검증에 실패하면, 곱하기 테스트 역시 정확하게 작동한다는 것을 검증하는 것이 실패하게 됨
    - 💡이것은 TDD를 하면서 적극적으로 관리해야 할 위험 요소임

## 마치기

> 우리는 완벽함을 위해 노력하지 않는다.  
> 모든 것을 두 번 말함으로써(코드1 + 테스트1) 자신감을 가지고 전진할 수 있을만큼만 결함의 정도를 낮추기를 희망할 뿐이다.

때때로 우리의 추론이 맞지 않아서 결함이 손가락 사이로 빠져나가는 수가 있다.

그럴 때면 테스트를 어떻게 작성해야 했는지에 대한 교훈을 얻고 다시 앞으로 나아간다.

🎈한 것🎈

- 오직 테스트를 향상시키기 위해서만 개발된 기능을 사용함
- 두 테스트가 동시에 실패하면 망한다는 사실을 인식하게 됨
- 위험 요소가 있음에도 계속 진행했음
- 테스트와 코드 사이의 결합도를 낮췄음
    - 그러기 위해 테스트하는 객체의 새 기능을 사용함
