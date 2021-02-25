---
title: "테스트 주도 개발(TDD): 01. 다중 통화를 지원하는 Money 객체"
excerpt: "테스트 주도 개발(저자 켄트백) 01장"
date: 2021-02-25
---

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

```
**$5 x 2 = $10**
amount를 private로 만들기
..
```

중간에 문제점이 있으면 할일 목록에 적어놓고 계속 진행할 것

- 왜냐면 지금 우리는 실패하는 테스트가 목표고 최대한 빨리 초록 막대를 봐야하므로

## 한번에 하나씩 정복하기

> 1. 작은 테스트를 하나 추가
2. 모든 테스트를 실행해서 테스트가 실패하는 것을 확인
3. 조금 수정
4. 모든 테스트를 실행해서 테스트가 성공하는 것을 확인
5. 중복 제거를 위해 리팩토링

위 🚩주기 5단계🚩를 목표로 할일 목록을 바탕으로 간단한 곱셈 예시를 작성하여 고쳐나감

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

- 목표:가장 빠르게 초록 막대를 만들 것
- 문제: 컴파일 안됨
- 현재 가장 쉬운 최소 작업 ⇒ 컴파일만 가능하게 변경

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

- 목표:가장 빠르게 초록 막대를 만들 것
- 문제: 테스트 실패함 → 예상 값: 10, 실제 값: 0
- 현재 가장 쉬운 최소 작업 ⇒ amount 값에 직접 10을 초기화

```java
int amount = 10;
```

### STEP 4

🚩 주기 4. 모든 테스트를 실행해서 테스트가 성공하는 것을 확인

![%E1%84%83%E1%85%A1%E1%84%8C%E1%85%AE%E1%86%BC%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%AA%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%8C%E1%85%B5%E1%84%8B%E1%85%AF%E1%86%AB%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AB%20Money%20%E1%84%80%E1%85%A2%E1%86%A8%E1%84%8E%E1%85%A6%20a6f59bf38d9543a3829c10c67b5d4050/Untitled.png](%E1%84%83%E1%85%A1%E1%84%8C%E1%85%AE%E1%86%BC%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%AA%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%8C%E1%85%B5%E1%84%8B%E1%85%AF%E1%86%AB%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AB%20Money%20%E1%84%80%E1%85%A2%E1%86%A8%E1%84%8E%E1%85%A6%20a6f59bf38d9543a3829c10c67b5d4050/Untitled.png)

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

5 * 2 에서 5 값을 어디서 받아올까?  `Dollar(5);`에서 → 생성자 구현

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

최종 완성
