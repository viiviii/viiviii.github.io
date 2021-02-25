---
title: "테스트 주도 개발(TDD): 01장 정리"
excerpt: "테스트 주도 개발(저자 켄트백) 01장 다중 통화를 지원하는 Money 객체"
date: 2021-02-25
---

📖 테스트 주도 개발

🤷‍♀️ 켄트 벡

📢 이장에서 배우게 될 것 요약

- 할일 목록 작성
- 외부에서 어떻게 보이길 원하는지를 코드로 표현
- 스텁 구현
- 빠르게 테스트 통과시키기
- 상수 → 변수로의 점진적인 일반화
- 새로운 일을 그때그때 처리하는 것이 아닌 목록에 추가하고 넘어가는 습관


## 할일 목록 작성하기

- `앞으로` 어떤 일을 해야 하는지
- `지금` 하는 일에 집중할 수 있게
- `언제` 일이 다 끝나는지

진행상황 체크 방법 → **진행중** ~~작업끝~~

## 시작하기

> 객체를 만들면서 시작하는게 아니라 테스트를 먼저 만들어야 함

테스트를 작성할 때는

- 오퍼레이션의 완벽한 인터페이스에 대해 상상해볼 것
- 우리는 오퍼레이션이 외부에서 어떤 식으로 보일지에 대한 이야기를 코드에 적고있는 것임

<pre>
<b>$5 x 2 = $10</b>
amount를 private로 만들기
..
</pre>

중간에 문제점이 있으면 할일 목록에 적어놓고 계속 진행할 것

- 왜냐면 지금 우리는 실패하는 테스트가 목표고 최대한 빨리 초록 막대를 봐야하므로

## 한번에 하나씩 정복하기

> 1. 작은 테스트를 하나 추가
> 2. 모든 테스트를 실행해서 테스트가 실패하는 것을 확인
> 3. 조금 수정
> 4. 모든 테스트를 실행해서 테스트가 성공하는 것을 확인
> 5. 중복 제거를 위해 리팩토링

위 🚩주기 5단계를 목표로 할일 목록을 바탕으로 간단한 곱셈 예시를 작성하여 고쳐나감

### STEP 1

🚩 주기 1. 작은 테스트를 하나 추가

- 뼈대 작성
- Dollar 객체 없음, 생성자 없음 등으로 당연히 컴파일 에러 뜸

```java
class tdd_01 {
    @Test
    void testMultiplication() {
        Dollar five = new Dollar(5);
        five.times(2);
        assertEquals(10, five.amount);
    }
}
```

### STEP 2

🚩 주기 2. 모든 테스트를 실행해서 테스트가 실패하는 것을 확인

- 문제: 컴파일 안됨
- 현재 가장 쉬운 최소 작업 ⇒ 컴파일만 가능하게 변경
- `times()` 스텁 구현(stub implementation): 메서드의 껍데기만 만들어두는 것

```java
class tdd_01 {
    @Test
    void testMultiplication() {
        Dollar five = new Dollar(5);
        five.times(2);
        assertEquals(10, five.amount);
    }
}

class Dollar {
    int amount;

    Dollar(int amount) {
        // TODO Auto-generated constructor stub
    }
	
    void times(int multiplier) {
        // TODO 
    }
}
```

### STEP 3

🚩 주기 3. 조금 수정

- 문제: 테스트 실패함 → 예상 값: 10, 실제 값: 0
- 현재 가장 쉬운 최소 작업 ⇒ amount 값에 직접 10을 초기화

```java
int amount = 10;
```

### STEP 4

🚩 주기 4. 모든 테스트를 실행해서 테스트가 성공하는 것을 확인

![테스트 돌려서 초록막대가 뜬 이미지](/assets/images/post/2021-02-25-test-driven-development-by-example-1.png)

### STEP 5

🚩 주기 5. 중복 제거를 위해 리팩토링

```java
int amount = 5 * 2;
```

```java
int amount;

void times(int multiplier) {
    amount = 5 * 2;
}

```

amount = 5 * 2 에서 5 값을 어디서 받아올까?  `Dollar(5);`에서 → 생성자 구현

```java
Dollar(int amount) {
    this.amount = amount;
}

void times(int multiplier) {
    amount = amount * 2;
}
```

```java
void times(int multiplier) {
    amount *= multiplier;
}
```

완료!🎈🎈🎈

최종 전체 코드

```java
class tdd_01 {
    @Test
    void testMultiplication() {
        Dollar five = new Dollar(5);
        five.times(2);
        assertEquals(10, five.amount);
    }
}

class Dollar {
    int amount;

    Dollar(int amount) {
        this.amount = amount;
    }

    void times(int multiplier) {
        amount *= multiplier;
    }
}
```

## 마치기

> 📌 one and only one  
> 필요한 것을 하되(one) 단 한번만(only one)하라

스티브 프리만은 테스트 코드와 코드 간의 문제는 중복이 아닌 의존성이라 말했음

- 의존성이 문제 그 자체라면 중복은 문제의 징후다
- 문제 자체는 남겨둔 채로 징후만을 제거하면 현실과는 달리 프로그램에서는 중복이 제거되면 의존성도 제거된다
    - 이게 바로 TDD 두번째 규칙이 존재하는 이유다(*두가지 규칙 중 2. 중복을 제거*)

⇒ 다음 테스트로 진행하기 전 중복을 제거하는건 오직 한가지(one and only one)의 코드 수정을 통해 다음 테스트도 통과되게 만들 가능성을 최대화 한 것이다.

> 📌 TDD의 핵심은 이런 작은 단계를 밟아야 한다는 것이 아니라,  
> 이런 작은 단계를 밟을 능력을 갖추어야 한다는 것이다.

만약 정말 작은 단계로 작업하는 방법을 배우면, 저절로 적절한 크기의 단계로 작업할 수 있게 될 것이다. 큰 단계로만 작업했다면, 더 작은 단계가 적절한 경우에 대해 결코 알지 못한다.
