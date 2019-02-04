---
layout: post
title: 리액트 뷰 앵귤러 컴포넌트 사용 비교
data: 2019-01-28
description: 리액스, 뷰, 앵귤러에서 각각 컴포넌트를 어떻게 사용하는지 살펴보고 장단점을 비교해봅니다.
img: ./component-compare/logo.jpg
tags: [react, vue, angular]
author: Do Young An
---

#
---
싱글 웹페이지 어플리케이션 하면 현 시점에서 React, Vue, Angular 세가지가 제일 유명하고 많이 쓰고 있습니다.  
자세히 들어가면 그 중에서도 React가 가장 많은 사용을 보여주고 있습니다.  
만약 프론트엔드를 희망한다면 위의 3개 중에 하나는 반드시 접하게 됩니다.  

3개의 가장 공통점을 든다면 컴포넌트 기반 개발 방법입니다.  
예를 들어 많이 쓰이는 버튼에 디자인을 넣어서 하나의 컴포넌트로 만들어 두고 계속 재사용 하게 됩니다.  

이 포스트에서는 React, Vue, Angular가 어떻게 컴포넌트를 사용하는지 알아보겠습니다.

# 준비
---
1. 하나의 css 파일을 React, Vue, Angular가 공유합니다.
2. JSON 배열을 받아와서 리스트를 출력합니다.
    - React, Vue 는 `axios`를 사용하고 Angular는 `@angular/http`를 사용합니다.
    - `axios`는 promise로 기반이고 `@angular/http`는 rxjs 기반입니다.

* Promise와 rxjs에 대한 이해가 없더라고 이 포스트를 보는데 문제는 없습니다. 만약 더 알고 싶은 분은 [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), [rxjs](http://reactivex.io/)를 참조하세요.

# index.html
---

``` html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>리액트 뷰 앵귤러</title>
    <link rel="stylesheet" href="./css/main.css" />
</head>
<body>
    <div class="spa-item">
        <!-- 리액트의 루트 엘리먼트 -->
        <div id="react-root"></div>
    </div>
    <div class="spa-item">
        <!-- 뷰의 루트 엘리먼트 -->
        <div id="vue-root"></div>
    </div>
    <div class="spa-item">
        <!-- 앵귤러의 루트 엘리먼트 -->
        <angular-root></angular-root>
    </div>
</body>
</html>
```

# React
---

# Vue
---

# Angular
---

# github
---
해당 프로젝트는 [react-vue-angular](https://github.com/doYoungAn/react-vue-angular)에서 확인 할 수 있습니다.