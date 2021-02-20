---
title: "Git bash alias 설정하기"
excerpt: "Git bash alias를 vi나 명령어로 설증하는 방법"
date: 2021-02-18
---


# git bash alias 설정하기

## 첫번째 방법: 에디터로 추가하기

`/Git/etc`로 이동(Git 설치한 곳)

```bash
$ vi bash.bashrc
```

원하는 alias 추가

```
$ alias alias_name='commands'
```
> 👀  
> vi에서 맨 끝 라인으로 이동하고 싶다면 명령모드에서 `G$`

재실행

```
$ source bash.bashrc
```

완료

![vi 설정 예시 이미지](/assets/images/post/2021-02-18-add-git-alias-1.png)

## 두번째 방법: 명령어로 한 번에 추가하기

```
$ echo alias alias_name='commands' >> bash.bashrc
```

혹시 eval 같은 명령어때문에 '가 들어가야 할 경우

```
$ echo alias alias_name=\''commands'\' >> bash.bashrc
```

재실행

```
$ source bash.bashrc
```

완료

![bash에서 명령어 한 줄로 설정하는 예시 이미지](/assets/images/post/2021-02-18-add-git-alias-2.png)
