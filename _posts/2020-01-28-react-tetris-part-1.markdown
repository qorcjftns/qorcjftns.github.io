---
layout: post
type:   posts
title:  "[1] React 기반 테트리스 만들기 - 설계"
date:   "2020-01-28"
categories:
  - "React Tetris 튜토리얼"
tags:
  - react
  - game
  - development
  - tutorial
  - react-tetris
---

## 머릿말
개발은 무작정 시작해서는 안된다. 처음부터 많은 설계를 한 후에 실질적인 구현에 들어가야 나중에 고생하지 않는다. 다만, 실제로 개인 프로젝트를 하는 사람들 중에 제대로된 설계를 하고 구현하는 사람이 얼마나 될까?

머리로는 알고 있지만 몸이 따르지 않는, 그저 본능적으로 이끌려서 개발하게 되는 것이 현실이다.

**필자도 마찬가지다.**

하지만, 현재 필자는 블로그를 작성중이기 때문에 처음부터 어느정도의 설계를 하고 시작해야 할 것이다. 

매우 귀찮은 일임에 틀림없다. 몸이 따르지 않는다. 본능적으로 코드를 작성하고 싶다. 하지만 그럴 수는 없다...

블로그를 시작하면서 올린 첫번째 [공지사항](/공지사항/2020/01/27/notice-introduction)에도 적어뒀지만 이 블로그는 최대한 깔끔한 구조의 글을 작성 하고 싶고, 그러기 위해서는 충분한 설계부터 한 후에 구현을 하는것이 옳은 일일 것이다.

이 글을 읽고있는 여러분도 매우 이 심정에 공감하리라 믿는다. 개발을 처음 하는 초심자부터 이미 능력이 출중한 능력자까지 모두...

머릿말에 잡념이 너무 많이 들어간 듯 하다. 이제 본 프로젝트에 대해서 설명해 보도록 하자.


## 프로젝트 설명
본 프로젝트는 제목 그대로 <code>React</code>를 사용해서 간단한 Tetris 게임을 만드는 튜토리얼이다.

기본적으로 웹에서 돌아가도록 설계되어 있으며, 가능하면 별도로 <code>react-native</code>로 변환해서 실행해보는 추가 프로젝트도 진행해볼 수 있으면 좋을 것 같다.

### 프로젝트 환경
* Node.js (version: v12.14.1)
* npm (version: 6.13.6)
* react (version: 16.12.0)
* create-react-app (version: 3.3.0)
* 필자 OS: macOS Catalina (10.15.3)

<code>Node.js</code>의 버전은 우선 현 시점의 LTS 버전인 <code>12.14.1</code>을 사용하기로 한다. <code>npm</code>의 경우도 마찬가지 이유로 <code>6.13</code> 버전을 사용한다.

또한 <code>create-react-app</code>는 현재 최신 버전인 <code>3.3.0</code>으로 유지하고, 그에 따라서 <code>react</code>는 <code>16.12.0</code>으로 사용하도록 하자.

OS는 사실 어떠한 OS에서 실행해도 되겠지만, 간간히 나오는 <code>bash</code> 스크립트들의 몇몇 부분의 차이점이 있을 것이기 때문에 미리 명시하고 시작핟도록 한다.

## 프로젝트 환경 설정

머릿말에서 열심히 설계부터 어쩌구 해뒀으면서 프로젝트 환경 설정을 이 시점에 하는 것도 웃기긴 하지만, 우선은 개발 환경부터 세팅하도록 하자.

우선 눈앞에 보이는 환경이 존재해야 프로젝트의 틀을 정하고 설계를 제대로 할 수 있을 것이다...

라는건 사실 변명이기도 하고, 사실 조금이라도 뭔가 실제 개발에 관련된 것이 보여야 재미가 붙을 것 같아서 그렇다.

### 1. Node.js / npm 설치
[https://nodejs.org/en/download/](https://nodejs.org/en/download/)

npm의 경우에는 node.js를 설치하면 알아서 따라서 설치되게 된다.

### 2. create-react-app 설치
{% highlight bash %}
$ npm install -g create-react-app
{% endhighlight %}

### 3. create-react-app 실행
{% highlight bash %}
$ create-react-app react-tetris
$ cd react-tetris
{% endhighlight %}


## 다음 포스팅
다음 포스티에서는 프로젝트의 구조 및 설계를 시작하도록 할 것이다.

설계 관련 포스팅은 되도록이면 2개 포스티 이내로 맞출 예정이며, 가능하면 1개 포스팅 이내로 맞추고 싶다...


<!--
Code Highlight
{% highlight ruby %}
def foo
  puts 'foo'
end
{% endhighlight %}
-->