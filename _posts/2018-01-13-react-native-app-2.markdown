---
layout: post
title:  "리액트 네이티브 날씨 어플만들기 (2/2)"
date:   2018-01-13 01:18:00
author: 이상현
categories: 개인공부
---

# 리액트 네이티브 날씨 어플만들기 (2/2)

## 개요

클론코딩, 라이브코딩으로 유명한 노마드코더의 [인프런](https://www.inflearn.com/course/%EB%A6%AC%EC%95%A1%ED%8A%B8-%EB%84%A4%EC%9D%B4%ED%8B%B0%EB%B8%8C/)강의를 수강하면서 빠른 속도로 리액트 네이티브 앱을 만들어본다. 리액트 네이티브 앱 제작시에 사용하는 개발도구는 이를 쉽게 작업하도록 도와주는 [Expo XDE](https://expo.io/)를 이용하여 실시간으로 빠르게 코드의 결과를 확인하면서 구현을 진행한다.

프로젝트는 깃허브의 [레포지터리](https://github.com/isseebx123/ReactWeatherApp)로 관리한다. 마지막으로 앱 배포의 경우 리액트의 특성상 ios와 android 모두 가능하지만, 본 개발자는 안드로이드 폰을 이용하므로 안드로이드로 standalone app을 배포하여 설치하고 실행해본다.

---

## 1. flex box를 이용한 기본 프레임 제작

<img src="{{ site.baseurl }}/assets/postimages/20180113/loading.png">

## 2. 휴대폰의 gps를 통해 위치정보를 받아오기

> ![frame](/assets/postimages/20180113/geolocation.png)

## 3. 위치정보를 이용하여 OpenWeatherMap의 [Weather API](http://openweathermap.org/api)를 사용하여 날씨정보를 받아오기

## 4. 받아온 날씨정보를 어플에 적절히 사용하기

> ![frame](/assets/postimages/20180113/cloud.png)
>
> ![frame](/assets/postimages/20180113/mist.png)

## 5. 완성된 어플을 standalone app로 배포하기

> ![frame](/assets/postimages/20180113/BuildingStandaloneApp.png)
>
> ![frame](/assets/postimages/20180113/BuildCompleteLog.png)