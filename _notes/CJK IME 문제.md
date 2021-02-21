---
title: 'CJK IME 문제'
author: 'vlwkaos'
created: 2021-02-22:04:21:40
published: true
---

CJK(Chinese, Japanese, Korean)는 글자의 완성이라는 개념이 있어서 IME를 이용하여 이를 처리한다. 때문에 라틴계 언어만 염두하고 [[Zettlr 자동완성 버그|텍스트 에디터를 만든 경우]] 여러 문제가 발생한다. 

`keyup` 키보드 이벤트로 입력이 종료되는 시점에 자동완성 등 기능을 제공하려는 경우 CJK에서 제대로 작동하지 않는다. CJK의 `keyup`은 글자의 완성과는 별개로 일어나기 때문에 하나의 글자가 다 입력되지 않았음에도 `keyup`이 발생하기 때문에 이상해진다.

예전에는 `keycode 229`로 IME의 Composition의 시작을 알 수 있었다.

### 웹 표준 CJK Key event 대처

`isComposing`을 이용할 수 있다. [참조](https://www.fxsitecompat.dev/en-CA/docs/2018/keydown-and-keyup-events-are-now-fired-during-ime-composition/)

### 한글 vs 일어, 중국어

일본어나 중국어는 히라가나, 로마자 등을 우선 입력한 뒤 원하는 조합 문자를 선택하여 입력을 마친다. 반면 한글은 입력하는 순서대로 글자가 조합된다. (알파벳과 유사)



