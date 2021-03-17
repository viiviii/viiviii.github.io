---
title: "Git bash functions 등록하기"
excerpt: "functions를 등록하여 alias처럼 사용하기, alias에 인자 받고 싶을 때"
date: 2021-03-13
---

포스팅을 노션에서 내보내기 후 git 블로그로 옮겨서 쓰고 있는데 이 과정에서 몇 가지 TODO가 있다.

그중 제일 간단해 보이던 한 가지를 해결해봤다.

## 내가 하고 싶은 것

> 노션에서 내보내기 한 이미지를 git image 폴더로 이동하기

### 원하는 것

- 파일명 변경: 노션은 Untitled.png로 내보내기 때문에 원하는 파일명으로 변경 필요
- 폴더 이동: 바탕화면에서 git blog 해당 폴더로 이동

### 문제점

- `mv ...` 명령어를 alias로 등록하기엔  alias는 변경할 파일명을 인자로 받을 수 없음

## 파일명 변경하고 이동하기

그래서 `bashrc`에 function을 등록하여 사용하기로 함

```java
moveImage() { mv /c/파일위치/"$1" /c/이동할위치/"$2"; }
```

- `$1`, `$2`는 순서대로 인자에 들어온 값들을 뜻함

[👀 Git bash alias setting하는 방법](https://viiviii.github.io/add-git-alias/) 

### 사용

```java
$ moveImage test1.jpg test2.jpg
```

## Git push까지 자동으로 하기

나는 `ssh`를 쓰고 있으므로 ssh alias까지 포함하였음 

```bash
pushImage() {
    target="/c/DevWorkspace/viiviii.github.io/assets/images/post/";
    mv ~/Desktop/$1 $target$2;
    cd $target;
    ssh-viiviii;
    git pull;
    git add $2;
    git commit -m "Add post image $2";
    git push;
}
```

- 실제로 사용한 건 한 줄이지만 가독성을 위해 정렬함
- 여기서 `git commit -am` 으로 add와 commit을 동시에 하고 싶었지만 `-a` 옵션은 Untracked 상태의 파일은 제외하길래 따로 하였음

### 추가로: 첫 번째 인자 생략하는 function 추가하기

항상 노션에서 이미지는 Untitled.png로 떨어지기 때문에(1장일 때) 첫 번째 인자 생략하기

```bash
pushUntitledImage() { pushImage Untitled.png $1; }
```

🎈완료🎈

최종으로 사용하는 이미지

![최종적으로 bash에서 실제 사용하는 이미지](/assets/images/post/2021-03-13-add-git-bash-functions_1.png)

## ⚠주의점

`{ mv..` 이 부분에서 { 뒤에 띄어쓰기가 없으면 내 기준 syntax 에러났음

![vi에서 띄어쓰기를 잘못하여 bash에서 에러나는 이미지](/assets/images/post/2021-03-13-add-git-bash-functions_2.png)


## 추가로 해야 할 것

- [ ]  이미지 여러 개일 경우 처리하는 함수 필요함
- [ ]  압축 파일을 풀지 않고 압축한 상태에서 바로 push 할 수 있는지
