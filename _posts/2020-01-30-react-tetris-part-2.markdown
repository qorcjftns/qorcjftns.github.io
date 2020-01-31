---
layout: post
type:   posts
title:  "[2] React 기반 테트리스 만들기 - 설계 2편"
date:   "2020-01-30"
categories:
  - "React Tetris 튜토리얼"
tags:
  - react
  - game
  - development
  - tutorial
  - react-tetris
---

## 프로젝트 구조

이번 시간에는 테트리스 프로젝트의 기본 틀을 잡아보도록 하자. 기본적으로는 MVC 패턴을 따라가볼 예정이다. 이를 위해서 우선은 폴더 구조를 아래와 같이 만들어보자.

{% highlight bash %}
┍public
├src
│┖┬controller
│ ├model
│ ├view
...
{% endhighlight %}

정 말 단순한 구조의 파일 트리가 완성이 되었다. 이중 public 폴더와 src 폴더의 경우, <code>create-react-app</code>에서 자동으로 생성해주는 폴더이기 때문에 내부에 다양한 파일들이 미리 들어있지만, 우선은 신경쓰지 않고 넘어가도록 하자.

자동 생성된 파일의 자세한 설명을 원하는 경우, 추후 올라올 react 기초 강좌를 참고하거나 다른 다양한 블로그들을 검색하여 찾아보도록 하자.


### View
기본적으로는 웹 어플리케이션으로 제작이 되기 때문에 View 클래스들은 모두 근본적으로는 HTML로 이루어지게 될 것이다. 물론 우리는 <code>React</code>를 활용하기 때문에 <code>React.Component</code>와 <code>JSX</code>를 활용하게 될 것이다.

<code>React</code>의 기본 코드 구조상 가장 상위의 View는 <code>src/App.js</code>이다. 처음 프로젝트를 생성하면 이 <code>App.js</code>는 단순한 <code>React</code>의 로고를 띄워주는 함수만 가지고 있을 것이다.

우리는 이 코드를 변경하여 우리의 테트리스 코드를 만들어 줄 것이다.

자, 우선 우리에게 필요한 View 컴포넌트들을 트리형식으로 나열해보자.

{% highlight bash %}
App.js
┖┬GameView.js
 │┖┬NextView.js
 │ ├TetrisView.js
 │ ┕HoldView.js
 ├ScoreView.js
 ├HelpView.js
...
{% endhighlight %}

네이밍을 경우 최대한 직관적으로 하였지만, 혹시나 하는 마음에 하나하나 간단한 설명을 붙여보도록 하자.

* **App.js** - <code>React</code>의 기본 Root Element
* **GameView.js** - 게임의 기본적인 UI를 담당할 부분으로, 게임 현 상황을 유저에게 보여주는 담당을 한다.
* **NextView.js** - 다음에 사용될 블록을 미리 보여주는 부분이다. 게임 환경 설정에 따라서 0개 ~ 5개까지의 다음 블록들을 보여줄 수 있도록 할 예정이다.
* **TetrisView.js** - 현재 실제 게임 화면이다. 테트리스 공식 룰에 의하여 가로 10, 세로 20의 화면을 지원한다.
* **HoldView.js** - 현재 Hold해둔 테트리미노를 표기한다.
* **ScoreView.js** - 지금까지의 스코어를 표기하며, High Score 또한 표기해주도록 한다.
* **HelpView.js** - 게임의 컨트롤과 같은 기타 사항들을 표기해준다.

우선은 큰 틀은 위와 같고, 세세한 View들은 그때그떄 필요할 때 만들어주도록 할 것이다. 너무 많은 디테일까지 설계해두기에는 아직 어떠한 것이 필요할지 확실하지 않기 때문이다. 머릿속에 그려지는 부분들이 물론 있긴 하지만, 그때그때 구현해 주도록 하자.


### Model
Model 구조는 굉장히 단순하다. "Model"은 사실 테트리스에서 다양하게 만들 이유도 없기 때문에, 최대한 단순화 시키고 싶다.

{% highlight bash %}
BaseModel.js
┖┬TetriminoModel.js
 ├GameBoardModel.js
...
{% endhighlight %}

여기서의 BaseModel은 View같이 종속관계가 아닌 상속관계를 표현하였다. 정말 단순한 구조이지만, **View**는 설명하고 여기를 설명 안하면 차별이 되기 때문에 가볍게 설명하보도록 하겠다.

* **BaseModel.js** - 모든 Model 클래스의 기본형이다. 게임 모델 간의 충돌 체크를 해주는 부분까지 여기에 넣어보도록 하자.
* **TetriminoModel.js** - 단일 테트리미노 1개를 표현하는 모델이다. 개별 테트리미노 모양 정보 등을 가지고 있게 된다.
* **GameBoardModel.js** - 현재 테트리스 게임의 보드 상황을 표현하는 모델이다.

여기서 한가지 참고할 사항은, 모든 Model은 <code>Redux</code>를 통하여 Global하게 관리하여 줄 것이다. 일반적인 <code>React</code>는 모델들이 View에 종속적이라서 Props와 State를 통하여 변경을 해주는데, 이렇게 하면 Controller와의 연계를 구현하기가 매우 쉽지 않게 되기 때문이다.


### Controller
Controller는... 종속도, 상속도 아닌 각각의 별개의 객체를 두기로 한다. 게임이 실행되면 각각 독립적으로 실행되며, 각각의 Controller는 Model들에 접근하여 데이터를 수정하거나 View의 상태를 변화시키거나 하게 된다.

{% highlight bash %}
GameController.js
ScoreController.js
InputController.js
...
{% endhighlight %}

사실 많이 분류하지 않아도 될 것으로 예쌍한다. 테트리스 특성상 그닥 많은 컨트롤이 필요하지는 않다.

* **GameController.js** - 게임의 전반적인 로직을 담당한다. 테트리미노의 회전 로직, 테트리미노의 시간당 드롭 로직, 그리고 가장 핵심인 라인 삭제 로직을 다루게 된다.
* **ScoreController.js** - 게임의 스코어를 계산해서 ScoreView에 표시해주는 역할을 담당한다. <code>GameController</code>가 보드의 라인을 삭제할 시, 테트리스의 룰에 따라서 점수를 부여하게 된다
* **InputController.js** - 유저의 인풋을 관리해준다. 본 컨트롤러는 되도록이면 Model을 직접적으로 터치하지 않고 <code>GameController</code>의 메서드를 불러주는 방식으로 진행할 것이다.


## 마치며

자, 우선은 간단한 MVC 구조는 짜여졌다. 기본에 충실하여 최소한의 개수로 최대한의 효과를 볼 수 있도록 만들어 보았다. 물론, 더욱 깔끔하게 설계하는 방법이 있을 지도 모른다. 그런 것을 원한다면 다른 블로그로 가서 알아보도록 하자.

이번 프로그램 설계의 목표는 독자들에게 **설계의 중요성**에 대하여 알려주기 위함이 크다. 따라서, 설계의 효율보다는 설계를 어떻게든 한 후에 프로그램 개발에 들어가면 얼마나 일이 수월해지는지를 알려줄 수 있다면 그것만으로 만족한다.

다음 포스팅에서는 MVC 패턴 중 <code>Model</code> 파트를 작성해줄 것이다. Model을 먼저 작성하는 이유는 Model이 View나 Controller를 부르는 부분이 없이 독립적인 개체로서 존재하기 때문이다.



<!--
Code Highlight
{% highlight ruby %}
def foo
  puts 'foo'
end
{% endhighlight %}
-->