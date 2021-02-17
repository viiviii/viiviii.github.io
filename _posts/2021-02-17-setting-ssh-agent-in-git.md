---
title: "ssh-agent 설정하여 비밀번호 최초 1회만 입력하기"
date: 2021-02-17
---

# ssh-agent 설정하기

ssh는 (일반적으로) 로그인 세션동안 실행되는데 나같은 경우 계정을 여러개 쓰기 위해 `id_ed25512` 파일이 없이 `id_ed25519_username` 패턴으로 생성했기 때문에 git 명령을 할 때 마다 비밀번호를 입력해줘야된다.

[SSH Agent Explained](https://smallstep.com/blog/ssh-agent-explained/)

매번 비밀번호를 입력하지 않기 위해 ssh-agent 설정을 하기로 한다

---

### 1. ssh-agent 실행

```
$ eval $(ssh-agent -s)
Agent pid 3254
```

### 2. ssh-add로 추가

```
$ ssh-add ~/.ssh/id_ed25519_*username*
Could not open a connection to your authentication agent.
```

### (선택사항) 조금 더 간단하게 한줄로 쓰기

하지만 매번 저렇게 쓰는건 귀찮으므로 한줄로 쓰기로 한다

```bash
eval $(ssh-agent -s) && ssh-add ~/.ssh/id_ed25519_username
```

난 연결됐다는 로그도 보고싶어서 테스트 명령어`ssh -T`도 추가하였음 (체크 딜레이 존재)

```bash
eval $(ssh-agent -s) && ssh-add ~/.ssh/id_ed25519_username && ssh -T git@github.com
```

### (선택사항) git alias로 추가해서 쓰기

[ssh-agent란?](https://www.notion.so/ssh-agent-cf25174ceba94ea98a406c19260409ed)
