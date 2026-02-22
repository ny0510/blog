---
title: 'macOS에서 SMB로 Time Machine 백업하기'
date: 2026-02-23T00:22:29+09:00
comments: true
categories: ['강좌']
summary: ''
tags:
  - macOS
  - Time Machine
  - SMB
---

SMB 모듈 중 fruit를 사용하면 `fruit:time machine = yes` 설정을 통해 간단하게 SMB 서버를 Time Machine 백업 대상으로 사용할 수 있다. 하지만 이러한 방식을 사용하면 필자의 경우 백업이 정상적으로 이루어지는 경우보다 도중에 실패해버리는 경우가 더 많았다.

따라서 약간의 꼼수를 사용할 것이다. 네트워크 드라이브에 sparsebundle 이미지를 만든 다음 Time Machine이 해당 이미지를 백업 디스크로 사용하도록 하는 것이다. 이런 방식을 사용하면 macOS에서 공식적으로 지원하는 네트워크 드라이브가 아니더라도 Time Machine 백업 디스크로 사용할 수 있다.

## Time Machine 백업 이미지 만들기

먼저 네트워크 드라이브에 sparsebundle 이미지를 만들어야 한다. 터미널에서 다음 명령어를 입력한다. 용량의 경우에는 자신의 기기 용량에 맞게 적절히 조절하면 된다. 애플에서 공식적으로 권장하는 백업 디스크 용량은 기기 용량의 2배라고 한다.

명령어 실행 전 네트워크 드라이브의 원하는 경로 안에서 실행해도 좋고 명령어 실행 후에 Finder에서 sparsebundle 이미지를 네트워크 드라이브로 옮겨도 좋다.

```bash
sudo hdiutil create -size 512g -type SPARSEBUNDLE -fs "HFS+J" TimeMachine.sparsebundle
```

네트워크 드라이브에 있는 sparsebundle 이미지를 더블클릭하여 마운트 해준다. 네트워크 속도에 따라 마운트하는 데 시간이 약간 소요될 수 있다.

## Time Machine 백업 디스크로 설정하기

지금 이 상태로는 Time Machine이 백업 디스크로 sparsebundle 이미지를 인식하지 못한다. 아래 명령어를 입력해 주어야 한다.

만약 sparsebundle 이미지의 파일명을 변경하였다면 아래 명령어에서 `/Volumes/TimeMachine` 부분을 알맞게 수정해주어야 한다.

```bash
sudo defaults write com.apple.systempreferences TMShowUnsupportedNetworkVolumes 1
sudo tmutil setdestination /Volumes/TimeMachine
```

이제 Time Machine 환경설정에 들어가보면 네트워크 드라이브에 있는 sparsebundle 이미지가 백업 디스크로 설정되어 있는 것을 확인할 수 있다. 이제부터는 일반적인 Time Machine 백업과 동일하게 사용할 수 있다.
