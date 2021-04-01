---
title: "Mac 터미널 세팅 및 커스텀(Homebrew, oh-my-zsh, iTerm2)"
excerpt: "Homebrew, oh-my-zsh, iTerm2 설치 및 커스텀에 대해"
date: 2021-04-01
---

# 시작하기

큰 분류로 아래의 3가지를 설치하며 내게 필요한 옵션을 커스텀하는 포스팅

- Homebrew: MacOS 패키지 관리 시스템. 패키지 설치, 삭제 등을 하기 위함
- oh-my-zsh: zsh에서 가장 많이 사용되는 플러그인 프레임워크
    - 테마 변경
    - 하이라이트 기능 설치
- iTerm2: 맥 기본 터미널 대신 사용 가능한 몇몇 기능이 추가된 터미널

# 🚩 Homebrew 설치

아래의 명령어를 터미널에서 입력하여 설치 시작

- [brew.sh에서도 복사 가능](https://brew.sh/)

### 설치

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

설치가 완료되고 아래와 같은 안내 로그가 뜨면 해당 명령어도 복붙하여 진행함

- brew로 사용할 수 있게 환경설정 하는 단계 같음

```
==> Next steps:
- Add Homebrew to your PATH in /Users/{유저}/.zprofile:
    echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/{유저}/.zprofile
    eval "$(/opt/homebrew/bin/brew shellenv)"
- Run `brew help` to get started
- Further documentation: 
    https://docs.brew.sh
```

### 설치 확인

아래의 명령어가 잘 실행되면 완료

```bash
brew help
```

# 🚩oh-my-zsh 설치

> ZSH에서 가장 널리 사용되는 플러그인 프레임워크
많은 내장 플러그인 및 테마가 제공됨 [원글](http://choesin.com/zsh-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EB%A9%B0-%EC%99%9C-bash-%EB%8C%80%EC%8B%A0-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%95%BC%ED%95%A9%EB%8B%88%EA%B9%8C)

맥에는 zsh이 이미 설치되어 있었고 기본으로 설정되어 있는 상태였기 때문에 바로 oh-my-zsh 설치함

- [ohmyz.sh에서도 복사 가능](https://ohmyz.sh/#install)

### 설치

```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## zsh 테마 변경 및 설정

### 기본 테마 확인

```bash
➜  ~ echo $ZSH_THEME
robbyrussel
```

### 테마 변경

일단 어떤게 있는지 몰라서 나는 테마 `random`으로 설정해봄

```bash
vi ~/.zshrc
```

빨간색 박스친 부분(ZSH_THEME="...")을 `random`으로 변경

![vi로 열은 .zshrc 파일 내용 중 ZSH_THEME 뒷 부분을 변경해야된다는 스크린샷](/assets/images/post/2021-04-01-my-mac-terminal-settings_1.png)

### 터미널에 적용

```bash
source ~/.zshrc
```

실행할 때 마다 아래와 같이 테마가 변경됨

![랜덤 테마 적용이 완료된 스크린샷](/assets/images/post/2021-04-01-my-mac-terminal-settings_2.png)

## zsh-syntax-highlighting 설치

> 터미널 명령어를 색상 및 밑줄 등으로 꾸며줌

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
```

설치방법

- [수동설치(위에서 내가 한 방법)](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md#in-your-zshrc)
- [oh-my-zsh등 플러그인 매니저를 사용한 설치 방법](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md#oh-my-zsh)

⚠️ zsh-syntax-highlighting에서는 수동 설치를 권장한다고 README에 적혀있음

### (선택) path highlighting 옵션 끄기

유효한 경로일 경우 아래 이미지처럼 밑줄(underline)로 표시되는 편리한 기능이지만 난 싫어서 끄고자 함

![밑줄 highlighting 기능이 켜져있는 스크린샷](/assets/images/post/2021-04-01-my-mac-terminal-settings_3.png)

vim으로 .zshrc 파일에 아래 코드 추가

```bash
(( ${+ZSH_HIGHLIGHT_STYLES} )) || typeset -A ZSH_HIGHLIGHT_STYLES
ZSH_HIGHLIGHT_STYLES[path]=none
ZSH_HIGHLIGHT_STYLES[path_prefix]=none
```

완료

![밑줄 highlighting 기능이 꺼진 스크린샷](/assets/images/post/2021-04-01-my-mac-terminal-settings_4.png)

- [원본 링크: how do I disabled the underline?](https://github.com/zsh-users/zsh-syntax-highlighting/issues/573)

# 🚩 iTerm2 설치

맥에서 기본 터미널 대신 쓸 수 있는건데 회사에서 쓰던 맥에서 iTerm2를 사용했기 때문에 설치

[여러 기능들](https://iterm2.com/features.html)이 있다고 하지만 아직 써본적은 없다

## 다운로드

[https://iterm2.com/downloads.html](https://iterm2.com/downloads.html)

## 테마 설정

[https://iterm2colorschemes.com/](https://iterm2colorschemes.com/) 

- 위 링크에서 마음에 드는 테마 선택
- 테마를 선택하여 클릭하면 페이지로 이동하는데 해당 url을 복사하여 터미널을 통해 다운로드하는 방식

[Iterm Themes - Color Schemes and Themes for Iterm2](https://iterm2colorschemes.com/)

### 테마 다운로드 및 설정

```bash
curl -LO "여기에 테마 링크에서 복사한 주소 복붙"
```

![맥에서의 Preferences 설정 메뉴 위치 스크린샷](/assets/images/post/2021-04-01-my-mac-terminal-settings_5.png)

iTerm2 → Preferences → Profiles → Colors 이동

오른쪽 하단의 `Color Presets...`에서 다운받은 파일 import

![Preferences의 profiles - colors 탭 스크린샷](/assets/images/post/2021-04-01-my-mac-terminal-settings_6.png)

import 후 다시 `Color Presets...`를 클릭하면 해당 테마가 추가되어 있으니 선택하여 적용

# (선택) 셸 사용자 이름 수정

맥북은 이름이 CHMacui-MacBookAir와 같이 길게 나오므로 이걸 수정함

시스템 환경설정 → 공유에서 컴퓨터 이름 수정

![시스템 환경설정 - 공유 메뉴 스크린샷](/assets/images/post/2021-04-01-my-mac-terminal-settings_7.png)

변경 완료

![사용자 이름이 변경된 터미널 스크린샷](/assets/images/post/2021-04-01-my-mac-terminal-settings_8.png)

# (선택) 랜덤 이모지 넣기

참고한 코드

```bash
prompt_context() { 
  # Custom (Random emoji) 
  emojis=("🍒" "🔥" "🍑" "👑" "🥑" "🐸" "🧀" "🦄" "🌈" "🪐" "🚀" "🌏" "🌹" "🐝" "🍕" "🌙" "🏀" "🧃" "🍓")
  RAND_EMOJI_N=$(( $RANDOM % ${#emojis[@]} + 1)) 
  prompt_segment black default 
}
```

나는 awesomepanda 테마를 사용중이므로 해당 테마를 복사하여 수정하였음

```bash
cd ~/.oh-my-zsh/themes/
cp awesomepanda.zsh-theme awesomepanda-custom.zsh-theme
vim awesomepanda-custom.zsh-theme
```

아래 코드 붙여넣기

```bash
# for custom
random_emoji() { 
  local emojis=("🍒" "👒" "🍑" "🥑" "🧀" "🪐" "🚀" "🌏" "🐝" "🍕" "🌙" "🏀" "🍄" "🍓" "🌭" "👻" "🐋" "🕊" "🌵" "🌱" "🐚" "🍍")
  local RAND_EMOJI_N=$(( $RANDOM % ${#emojis[@]} + 1 )) 
  echo ${emojis[$RAND_EMOJI_N]}
}
```

그리고 vim로 파일 내용 보면 딱 이부분 바꾸면 되겠구나 하고 찾을 수 있는데 그부분을 `$(random_emoji)`로 변경

![함수 추가 및 변경한 theme 파일 스크린샷](/assets/images/post/2021-04-01-my-mac-terminal-settings_9.png)

변경 내용 적용

```bash
source ~/.zshrc
```

before

![랜덤이모지 커스텀 전 스크린샷](/assets/images/post/2021-04-01-my-mac-terminal-settings_10.png)
after

![랜덤이모지 커스텀 적용된 스크린샷](/assets/images/post/2021-04-01-my-mac-terminal-settings_11.png)

---

# 참고링크

- [iTerm2로 터미널 커스텀하기](https://ooeunz.tistory.com/21)
- [oh-my-zsh 테마 변경 및 설정 (alias, agnoster 멀티라인, 사용자명 숨김처리)](https://medium.com/@joshuaxavier/how-to-customise-your-command-prompt-to-include-an-emoji-647e1f3e4027)
- [mac의-터미널-테마적용을-위한-zsh-그리고-oh-my-zs](https://blog.pigno.se/post/156761593073/mac%EC%9D%98-%ED%84%B0%EB%AF%B8%EB%84%90-%ED%85%8C%EB%A7%88%EC%A0%81%EC%9A%A9%EC%9D%84-%EC%9C%84%ED%95%9C-zsh-%EA%B7%B8%EB%A6%AC%EA%B3%A0-oh-my-zsh)
- [How to customise your command prompt to include an emoji](https://medium.com/@joshuaxavier/how-to-customise-your-command-prompt-to-include-an-emoji-647e1f3e4027)
- [내 맥북 셋팅 - 맥북 이름 바꾸기(터미널에서 이름 예쁘게 보는 법(?))](https://m.blog.naver.com/PostView.nhn?blogId=dhdh6190&logNo=220626073970&proxyReferer=https:%2F%2Fwww.google.com%2F)
- [Oh My ZSH+ iTerm2로 터미널을 더 강력하게](https://medium.com/harrythegreat/oh-my-zsh-iterm2%EB%A1%9C-%ED%84%B0%EB%AF%B8%EB%84%90%EC%9D%84-%EB%8D%94-%EA%B0%95%EB%A0%A5%ED%95%98%EA%B2%8C-a105f2c01bec)
