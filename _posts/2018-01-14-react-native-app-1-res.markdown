---
layout: post
title:  "1회차. 리액트 네이티브 날씨 어플만들기 (1/2) / 결과"
date:   2018-01-12 16:10:00
author: 이상현
categories: 로컬(모각코)
---

# 1회차. 리액트 네이티브 날씨 어플만들기 (1/2) / 결과

## 결과

## 1. Expo XDE 설치하기

> [Expo XDE](https://github.com/expo/xde/releases)를 해당 repo에서 release된 .AppImage를 다운로드 받는다.
>
> chmod a+x xde*.AppImage
>
> ./xde*.AppImage.

## 2. Android에서 Expo App 설치하기

> error 502 발생.
- echo fs.inotify.max_user_watches=524288
- sudo tee -a /etc/sysctl.conf && sudo sysctl -p
- [참조](https://forums.expo.io/t/packager-starts-then-stops-silently/3435/3)

## 3. Android Emulator 설치 (옵션)

> 안드로이드 스튜디오와 같이 가상 에뮬레이터를 만들어서 구동, Expo에서 구동을 통해 연동하여 확인할 수 있음.
- 1) [Genymotion](https://www.genymotion.com/fun-zone/) 설치
- 2) Virtual box 설치: 사이트에서 설치하는 것은 expo와 연동이 되지 않아, 소프트웨어 센터의 것을 설치받아 사용하였음.(구버전일 수도 있음)

## 4. [Snack](https://snack.expo.io/) 이용 (옵션)

> 에뮬레이터의 웹사이트 버전 같은 것임.

---

## 1. 프로젝트 생성

> Expo XDE에서 New project를 통해 프로젝트를 생성한다.

## 2. 안드로이드와 연동

> 설치한 Expo app에서 QR코드를 이용하여 XDE와 연결한다.
>
> 스마트폰을 좌우로 흔들면 메뉴가 나타난다.
>
> 메뉴에서 hot reload 기능을 켜서, 수정한 것만 업데이트 되도록하여 test의 속도를 높인다.

## 요약

아직 구현을 시작하기 전인데도 개발도구의 설치와 설정에 시간이 생각보다 많이 소요되었다. 처음에 윈도우 OS로 개발을 하려고 생각하고 위 작업들을 시도했지만 불편한 요소를 많이 느껴서, 우분투를 통해 개발을 진행하는 것으로 방향을 바꾸었다. 우분투에서 위 작업들을 해보니 적은 오류로 상당히 빠른 시간에 설치를 마칠 수 있어서 다행이었다.
