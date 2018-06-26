---
layout: post
title:  "React eject"
date:   2018-06-01 12:57:00
author: 이상현
categories: React
---

# eject 개념
## 단순히 말해서 react 프로젝트 폴더에 android, ios폴더가 생긴다
<pre>
	react-native init 으로 프로젝트를 생성하면 자동으로 eject된 프로젝트가 생성된다.
	create-react-native-app(crna)로 프로젝트를 생성하면 eject되지 않은 프로젝트가 생성된다.
</pre>
## eject의 필요성
<pre>
	npm라이브러리 중에서도 eject를 해야 사용이 가능한 라이브러리가 있다.
	eject를 통해서 native 코드를 사용해야지 휴대폰의 특정 기능을 사용할 수 있는 모양이다.
	예를 들어.. 내장 db(realm, sqlite)등이 그러하다.
	하지만 eject를 하고나면 영속적으로 eject가 되는 것이므로 expo를 사용하지 못하게 된다.
	왜냐하면 expo는 순수히 js만 다루기 때문에, native코드인 자바는 처리할 수 없다.
</pre>
<img src="{{ site.baseurl }}/assets/postImages/20180601/eject1.jpg"> <br>
<img src="{{ site.baseurl }}/assets/postImages/20180601/eject2.jpg"> <br>
<pre>
	eject를 시도하면 위 이미지에는 없지만 먼저 eject를 정말로 할 것인지, 말 것인지 묻는다.
	이후 세가지 사항을 묻는데 native에서 어떻게 보여질지? 무슨 이름으로 파일을 실행할지? 등을 묻는 것 같다.
	이후 eject가 성공되었다고 뜨고, 그 다음에 보여졌던 오류는 뭐때문에 발생한지는 잘 모르겠다.
</pre>
## eject 이후
<img src="{{ site.baseurl }}/assets/postImages/20180601/eject3.jpg"> <br>
<pre>
	eject이후에는 expo로 프로젝트를 시작해보려고 해도 로딩만 뜨고 가만히 멈춘다..
	많이 바뀌는 것이 없을 줄 알았는데 위와 같이 바뀐 사항들이 많았다.
</pre>

# android studio에서 open
## sdk 설치
<img src="{{ site.baseurl }}/assets/postImages/20180601/android.jpg"> <br>
<pre>
	고맙게도 설치안되어있던 sdk를 자동으로 설치하라고 띄워준다.
	gradle을 아직 잘 모르기때문에 필요하다면 공부해야할 것 같다.
	gradle말고 maven으로 할 수 있는 방법은 없을까?
</pre>

## avd에서 실행
<img src="{{ site.baseurl }}/assets/postImages/20180601/android1.jpg"> <br>
<img src="{{ site.baseurl }}/assets/postImages/20180601/android2.jpg"> <br>
<pre>
	2가지 에러가 발생. 먼저 usb debugging을 enable
	[스택오버플로우 참고]("https://stackoverflow.com/questions/44446523/unable-to-load-script-from-assets-index-android-bundle-on-windows?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa")
</pre>

```sh
react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res

adb ...
```
<pre>
	위를 해도 여전이 에러가 발생함.
</pre>
