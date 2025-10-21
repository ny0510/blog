---
title: 'MacOS에서 Touch ID로 sudo 인증하기'
date: 2023-12-11T13:15:46+09:00
comments: true
categories: ['강좌']
tags:
  - macOS
---

MacOS를 사용하다 보면 터미널에서 sudo를 사용할 때마다 매번 비밀번호를 입력해 주어야 하는 번거로움이 있습니다.

물론 비밀번호 입력 과정을 아예 생략하고 sudo를 사용할 수도 있지만 이렇게 해버리면 보안이 취약해지는 단점이 있습니다.

이를 해결하고자 비밀번호 입력보다 더 편리하고 보안 측면에서도 안전한 Touch ID를 사용하여 sudo를 인증하는 방법을 알아보겠습니다.

## Homebrew로 설치 (권장)

만약 Homebrew가 설치되어 있다면 아래 명령어 두 줄로 간단하게 설치가 가능합니다.

```sh
brew install artginzburg/tap/sudo-touchid
sudo brew services start sudo-touchid
```

## 수동 설치

Homebrew가 설치되어 있지 않거나 사용할 수 없는 경우에는 수동으로 설치할 수 있습니다.

아래 명령어를 입력하면 설치 스크립트를 다운로드하고 자동화된 수동 설치를 진행합니다.

```sh
curl -sL git.io/sudo-touchid | sh
```

## 사용법

설치가 완료되면 `sudo` 명령어를 사용할 때 인증 과정을 비밀번호 대신 Touch ID로 대체할 수 있습니다.

Touch ID를 사용할 수 없는 경우 `Use password...` 버튼을 눌러 비밀번호로 인증할 수 있습니다.

![sudo 인증에 Touch ID 사용](./images/sudo-touchid.png)

## 제거

Homebrew로 설치한 경우 아래 명령어를 입력하여 제거할 수 있습니다.

```sh
sudo brew services stop sudo-touchid
brew remove sudo-touchid
```

수동 설치한 경우 아래 명령어를 입력하여 제거할 수 있습니다.

```sh
sudo-touchid --disable
```

## 추가 정보

추가로 상단에 말한 `sudo` 명령어의 인증 과정을 완전히 생략하는 방법은 아래와 같습니다.

`/etc/sudoers` 파일을 수정하여 비밀번호 인증 과정을 생략할 수 있습니다.

```sh
sudo sed -i '' 's/%admin ALL=(ALL) ALL/%admin ALL=(ALL) NOPASSWD: ALL/' /etc/sudoers
```
