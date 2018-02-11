---
layout: post
title:  "2회차. 리액트 네이티브 날씨 어플만들기 (2/2) / 결과"
date:   2018-01-15 16:02:00
author: 이상현
categories: 로컬(모각코)
---

# 2회차. 리액트 네이티브 날씨 어플만들기 (2/2) / 결과

## 결과

## 1. flex box를 이용한 기본 프레임 제작

<img src="{{ site.baseurl }}/assets/postImages/20180113/loading.png"> <br>
UI작업은 항상 어렵게 느껴지지만 안드로이드스튜디오에서 UI작업을 했던 경험과 수 많은 디버깅 경험을 떠올려보면, 리액트를 통해서 했을때가 몇 배는 더 쉽게 느껴졌다. 하지만 애초에 간단한 프레임의 어플리케이션이라 그렇게 느껴지는지도 모르겠다.

## 2. 휴대폰의 gps를 통해 위치정보를 받아오기

<img src="{{ site.baseurl }}/assets/postImages/20180113/geolocation.png"> <br>
일반적인 웹 등의 자바스크립트에서 쓰이는 geolocation이 동일하게 사용되어 사용자 휴대폰의 gps기능을 이용하여 위도와 경도를 받아올 수 있었다. 물론 다른 추가정보들도 json형식으로 주어지지만 아래의 날씨정보를 얻어오는 API를 이용하기 위해서는 이 2개만 있어도 충분했다.

## 3. 위치정보를 이용하여 OpenWeatherMap의 [Weather API](http://openweathermap.org/api)를 사용하여 날씨정보를 받아오기

API를 사용하기 위해 회원가입을 하고 API키를 받았다. 이후 2에서 얻어온 위도와 경도를 주고 날씨정보를 json으로 받을 수 있었다.

## 4. 받아온 날씨정보를 어플에 적절히 사용하기

<img src="{{ site.baseurl }}/assets/postImages/20180113/cloud.png">

<img src="{{ site.baseurl }}/assets/postImages/20180113/mist.png"> <br>
날씨정보가 들어있는 json에서 날씨명(Sunny, Cloudy 등), 온도의 정보를 긁어와서 화면에 출력할 수 있었다.

## 5. 완성된 어플을 standalone app로 배포하기

<img src="{{ site.baseurl }}/assets/postImages/20180113/BuildingStandaloneApp.png">

<img src="{{ site.baseurl }}/assets/postImages/20180113/BuildCompleteLog.png"> <br>
완성된 리액트 네이티브 앱을 standalone app으로 배포하는 방법은 강의에 포함되지 않아 직접 expo의 doc을 읽고 진행하였다. 이는 Expo XDE에서는 제공하지 않는 기능으로 exp를 npm으로 설치하고, 프로젝트를 서버단에서 빌드하는 방식이었다. 물론 빌드하기전에 프로젝트의 app.json에 빌드에 대한 정보를 수정하여 입력해야 한다. 빌드가 완료되면 링크를 주는데 빌드된 어플을 다운로드 받을 수 있었다.

## 요약

이전에 노마드코더의 리액트로 웹서비스 만들기를 하면서 기본적인 리액트에 대하여 배웠었다. 해당 강의를 들으면서 프로젝트를 작성했던 깃허브의 레포 주소는 https://github.com/isseebx123/ReactMovieApp 이다. 여기서 가장 중요한 용어들로 기억이 남는게 있다면 상태인 *state* 와, 하나의 단위를 의미하는 *component*, 또 state를 가지지 않는 component 느낌의 *functional component* 등이 있었다. 이에 대한 개념이 본 강의에서 필수적이었으며(본 강의에서 이에대한 내용은 자세히 다루지 않았음), 한번더 복습하고 적용해볼 기회가 있어서 좋았다.
