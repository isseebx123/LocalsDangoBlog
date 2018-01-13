---
layout: post
title:  "리액트 네이티브 날씨 어플만들기 (1/2)"
date:   2018-01-11 02:10:00
author: 이상현
categories: 개인공부
---

# 리액트 네이티브 날씨 어플만들기 (1/2)

## 개요

클론코딩, 라이브코딩으로 유명한 노마드코더의 [인프런](https://www.inflearn.com/course/%EB%A6%AC%EC%95%A1%ED%8A%B8-%EB%84%A4%EC%9D%B4%ED%8B%B0%EB%B8%8C/)강의를 수강하면서 빠른 속도로 리액트 네이티브 앱을 만들어본다. 리액트 네이티브 앱 제작시에 사용하는 개발도구는 이를 쉽게 작업하도록 도와주는 [Expo XDE](https://expo.io/)를 이용하여 실시간으로 빠르게 코드의 결과를 확인하면서 구현을 진행한다.

프로젝트는 깃허브의 [레포지터리](https://github.com/isseebx123/ReactWeatherApp)로 관리한다. 마지막으로 앱 배포의 경우 리액트의 특성상 ios와 android 모두 가능하지만, 본 개발자는 안드로이드 폰을 이용하므로 안드로이드로 standalone app을 배포하여 설치하고 실행해본다.

---

## 1. Expo XDE 설치하기

> [Expo XDE](https://github.com/expo/xde/releases)를 해당 repo에서 release된 .AppImage를 다운로드 받는다.
>
> chmod a+x xde*.AppImage
>
> ./xde*.AppImage.

## 2. Android에서 Expo App 설치하기

> error 502 발생.
- echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
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