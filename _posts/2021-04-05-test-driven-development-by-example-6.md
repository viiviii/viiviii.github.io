---
title: "테스트 주도 개발(TDD): 06장 정리"
excerpt: "[06장 돌아온 '모두를 위한 평등'] 단계적으로 중복 제거하기"
date: 2021-04-05
---

📢 배우게 될 것 요약

- 5장에서 발생한 중복을 단계적으로 제거하는 방법
- TDD가 부족한 상황에서 리팩토링을 만나게 될 때

## 시작하기

이전 장에서 테스트를 빨리 통과하기 위해 많은 코드의 중복이 발생했음

Dollar와 Franc 클래스의 중복을 어떻게 제거할 수 있을까?

![고려하는 두 가지 방법 구조](/assets/images/post/2021-04-05-test-driven-development-by-example-6_1.jpeg)
- `방법 A`:우리가 만든 클래스 중 하나가 다른 클래스를 상속받게 하는 것
    - 저자가 해봤는데 거의 어떤 코드도 구원하지 못했다고 함
- `방법 B` 두 클래스의 공통 상위 클래스를 찾아내기
    - 잘 작동하므로 이 방법으로 코드의 중복을 제거할 것임

## 목표

> 상위 클래스 Money를 만들어 공통의 equals 코드를 갖게하자

## STEP 1

🚩 `Money` 클래스 작성 후 상속받기

```java
class Money {

}
	
class Dollar extends Money {
    private int amount;
    ...	
}
```

## STEP 2

🚩 테스트 실행

![테스트 성공하는 실행 결과 스크린샷](/assets/images/post/2021-04-05-test-driven-development-by-example-6_2.png)

## STEP 3

🚩 `amount` 인스턴스 변수를 `Money`로 이동

- 하위 클래스에서도 변수를 볼 수 있도록 `private`에서 `protected`로 변경

```java
class Money {
    protected int amount;
}
	
class Dollar extends Money {
    ...
}
```

## STEP 4

🚩 `Dollar` 클래스의 `equals()`에서 임시변수 타입을 `Money` 클래스로 변경 

```java
class Dollar extends Money {

    @Override
    public boolean equals(Object obj) {
        Money dollar = (Money) obj;
        return amount == dollar.amount;
    }
  ...
}
```

## STEP 5

🚩 테스트 실행

![테스트 성공하는 실행 결과 스크린샷](/assets/images/post/2021-04-05-test-driven-development-by-example-6_2.png)

## STEP 6

🚩 `equals()` 임시 변수 이름 변경

- 좀 더 원활한 의사소통을 위해서

**Dollar**

```java
class Dollar extends Money {

    @Override
    public boolean equals(Object obj) {
        Money money = (Money) obj;
        return amount == money.amount;
    }
  ...
}
```

## STEP 7

🚩`Dollar` 클래스의 `equals()`를 `Money`로 이동

```java
class Money {
    protected int amount;

    @Override
    public boolean equals(Object obj) {
        Money money = (Money) obj;
        return amount == money.amount;
    }	
}
	
class Dollar extends Money {

    Dollar(int amount) {
        this.amount = amount;
    }
		
    Dollar times(int multiplier) {
        return new Dollar(amount * multiplier);
    }
}
```

---

➡️  여기부터는 `Franc`에서 `equals()` 을 제거하기 위한 단계이며 위의 비슷한 일을 함

## STEP 8

🚩 빠져있던 `Franc`의 `equals()` 테스트 코드 추가

- 기존의 `testEquality()` 테스트는 `Franc`끼리의 비교에 대해서는 다루지 않았음
- 그래서 우리는 코드를 변경하기 전에 **애초에 있어야 했던 테스트**를 먼저 작성

```java
@Test
void testEquality() {
    assertTrue(new Dollar(5).equals(new Dollar(5)));
    assertFalse(new Dollar(5).equals(new Dollar(6)));
    assertTrue(new Franc(5).equals(new Franc(5)));
    assertFalse(new Franc(5).equals(new Franc(6)));
}
```

종종 적절한 테스트를 갖지 못한 코드에서 TDD를 해야 하는 경우가 있을것인데,

충분한 테스트가 없다면 지원 테스트가 갖춰지지 않은 리팩토링을 만나게 될 수 밖에 없음

⇒ 리팩토링하면서 실수했는데도 불구하고 테스트가 여전히 통과할 수도 있는 것

> 📌   
> 있으면 좋을 것 같은 테스트를 작성하라

그렇게 하지 않으면 결국에는
→  리팩토링 하다가 뭔가 깨트림 

→ 리팩토링에 대한 안좋은 느낌을 갖게됨 

→ 리팩토링을 덜 하게되는 악순환 발생

## STEP 9

🚩 `Franc`가 `Money`를 상속받게 하고 `amount` 제거

- `Dollar`의 STEP 1, 3과 동일함

```java
class Franc extends Money {
    ...
}
```

## STEP 10

🚩 `Franc`의 `equals()` 고쳐보기

- `Franc.equals()`는 `Money.equals()`와 거의 비슷해보임
    - 이 두 부분을 완전히 똑같이 만들 수 있다면 프로그램의 의미를 변화시키지 않고도 `Franc`의 `equals()`를 지울 수 있게 됨
- `Dollar`의 STEP 4, 6처럼 임시변수의 타입을 `Money`로 바꾸고 이름도 `money`로 바꿔본다

```java
class Money {
    protected int amount;

    @Override
    public boolean equals(Object obj) {
      Money money = (Money) obj;
      return amount == money.amount;
    }	
}

class Franc extends Money {
    @Override
    public boolean equals(Object obj) {
		    Money money = (Money) obj;
		    return amount == money.amount;
    }
  ...
}
```

## STEP 11

🚩 `Franc`의 `equals()` 제거

- 위 단계에서 `equals()`가 다른 점이 없는 것을 확인했으므로 `Franc`의 불필요한 코드를 제거

```java
class Money {
    protected int amount;

    @Override
    public boolean equals(Object obj) {
        Money money = (Money) obj;
        return amount == money.amount;
    }	
}

class Franc extends Money {
    ...
}
```

## STEP 12

🚩 테스트 실행

![테스트 성공하는 실행 결과 스크린샷](/assets/images/post/2021-04-05-test-driven-development-by-example-6_2.png)

## 마치기

이번 챕터에서는 어찌보면 한번에 할 수 있었던 일을 단계적으로 쪼개서 테스트를 밟아가는걸 보게됐고,

TDD가 부족한 코드를 만나면 내가 먼저 있으면 좋을 것 같은 테스트를 추가하라는걸 배우게 됐음 

### 느낀점?(미래의 나에게..)

나였으면 `equals()` 변경 과정인 STEP 4, 5, 6에서 4, 6을 한번에 하고 테스트를 돌렸을 것 같은데 왜 순서를 이렇게 했지?라고 곰곰히 생각해보니 만약 테스트가 통과 안되면 변수명을 다시 원상복귀 해야되는 액션이 추가되는거니 (4)에서 (5) 테스트를 돌려 초록 막대를 보고 (6)을 진행하는게 좋은 것 같다


🎈한 것🎈

- 공통된 코드를 첫 번째 클래스(Dollar)에서 상위 클래스(Money)로 `단계적`으로 옮겼음
- 두 번째 클래스(Franc)도 Money의 하위 클래스로 만듦
- 불필요한 구현을 제거하기 전에 두 equals() 구현을 일치시켰음
