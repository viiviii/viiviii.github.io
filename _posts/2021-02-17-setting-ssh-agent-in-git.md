---
title: "ssh-agent 설정하여 비밀번호 최초 1회만 입력하기"
excerpt: "SSH 설정을 하면 git bash에서 명령어를 칠 때 마다 비밀번호를 입력해야 하는데 ssh-agent를 사용하여 비밀번호를 최초 1회만 입력하도록 설정"
date: 2021-02-17
---

# ssh-agent 설정하기

[SSH 설정하기](https://viiviii.github.io/using-multiple-github-accounts-with-ssh-key/)를 하고나면 git 명령을 할 때마다 비밀번호를 입력해야 됨

매번 비밀번호를 입력하는 것은 불편하므로 ssh-agent를 사용

> ssh-agent란?  
> 메모리에 ssh key와 certificates를 보관하는 SSH key manager

[SSH Agent에 대한 더 자세한 설명 링크](https://smallstep.com/blog/ssh-agent-explained/)

- 클라이언트가 서버에 연결 시 `public key` 제공
- 서버가 클라이언트에게 메세지에 `private key` 서명 요청
- 클라이언트는 ssh-agent에게 메세지에 서명하도록 요청하고 이걸 다시 서버에게 전달
- 서버는 클라이언트의 `public key`를 사용하여 서명 확인

대략 이런 흐름인데 우리가 서버에 요청할 때 마다 매번 비밀번호를 치는 것을 ssh-agent가 대신 해준다고 볼 수 있음


---

### 1. ssh-agent 실행

```
$ eval $(ssh-agent -s)
Agent pid 3254
```

### 2. ssh-add로 추가

* 여기서 `id_ed25519_username`는 생성한 파일명

```
$ ssh-add ~/.ssh/id_ed25519_username
Could not open a connection to your authentication agent.
```
  
Mac에서는 키 체인에 추가할 수 있음

```
$ ssh-add -K /path/to/private_key
```

### 🍎 Mac인 경우

Mac에서 ssh config 파일에 아래의 옵션만 추가하면 위 절차 없이 훨씬 간편하게 사용 가능

```
   AddKeysToAgent yes
   UseKeychain yes
```
  - 대략적인 설명
    - UseKeychain: 키체인에 암호 추가(생략하는 경우 사용 안함)
    - AddKeysToAgent: 인증 중에 사용되는 private key를 ssh-agent에 추가

### (선택사항) 조금 더 간단하게 한줄로 쓰기

하지만 매번 저렇게 쓰는게 귀찮다면 한 줄로 쓸 수 있음

```
$ eval $(ssh-agent -s) && ssh-add ~/.ssh/id_ed25519_username
```

### (선택사항) git alias로 추가해서 쓰기

[⚙ git bash alias 설정하기](https://viiviii.github.io/add-git-alias/)

---

# 참고링크

[Mac에서 키 체인 추가](https://stackoverflow.com/questions/21095054/ssh-key-still-asking-for-password-and-passphrase)

[github docs](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#adding-your-ssh-key-to-the-ssh-agent)

[open-ssh docs](http://www.openssh.com/txt/release-7.2)

---
