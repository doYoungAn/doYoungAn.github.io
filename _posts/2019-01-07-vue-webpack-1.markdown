---
layout: post
title: vue, webpack 프로젝트 생성기 1
data: 2018-01-03
description: 
img: vue_logo.jpeg
tags: [vue]
author: Do Young An
---

# Vue 프로젝트 생성기
---------------------------------------------------
vue를 사용하기 위해서는 vue-cli를 사용하면 간단하게 프로젝트를 구성할 수 있다. <br/>
하지만 제가 vue를 공부하면서 느낀 점은 cli로 만든 프로젝트는 간단한 TODO 앱을 만들때는 유용하게 쓸 수 있
지만 개발자가 원하는 설정에 제한을 받는 느낌을 받았습니다. <br/>
cli를 통하면 webpack에 대한 설정들, 타입스크립트의 적용, scss의 사용등을 프로젝트 생성할때 명령 하나로 가
능하지만 이게 어떻게 해서 쓰이는 건지에 대한 이해도는 떨어졌습니다. <br/>
이로 인해 처음 빈폴더에서 vue 프로젝트를 생성해 보는 포스트를 작성해보려고 합니다. <br/>
- 해당 포스트에서 만드는 프로젝트는 깃헙에 올릴 예정이니 참고 해 주시기 바랍니다.
- os는 맥을 기준으로 합니다. 윈도우나 리눅스도 거의 같게 적용 가능합니다.
- webpack에 대한 이해가 있으면 이해하기 쉬우나 webpack을 몰라도 간단한 설명을 포함하고 있습니다.
- 포스트는 1, 2로 나누어서 진행 하겠습니다.

## 0. 들어가기 앞서서
------------------------------------------------------
vue-cli로 프로젝트를 생성해보자.

1. vue-cli를 글로벌로 설치해줍니다.
``` bash
npm install -g @vue/cli
```
2. 설치되었는지 확인해봅니다.
``` bash
vue --version
```
- 버전이 나온다면 제대로 설치 되었습니다. 현재 포스트의 cli 버전은 3.1.1입니다.
3. 프로젝트를 생성합니다.
``` bash
vue create hello-world
```
<img src="./../assets/img/complete-vue-cli.png" />
<br/>
설치가 끝나고 명령어가 나오네요 한번 해봅시다.
<img src="./../assets/img/vue-cli-serve.png" />
<br/>
무언가 작업이 끝나고 http://localhost:8080 으로 동작하는걸 알 수 있습니다.
<img src="./../assets/img/vue-cli-host.png" />
<br/>
해당 주소로 들어가니 vue 프로젝트가 실행중이네요. 완전 편하게 프로젝트를 생성했습니다.
4. 프로젝트 구성 살펴보기
<br/>
<img src="./../assets/img/vue-cli-files.png" width="200" />
<img src="./../assets/img/vue-cli-dist.png" width="200" />
<br/>
왼쪽의 이미지에서는 프로젝트의 파일들을 볼 수 있고 오른쪽 이미지에서는 실제 빌드된 파일들을 볼 수 있습니다.
js 파일이 없었는데 생겨버렸네요. 이는 빌드할때 vue파일을 읽어서 js를 만들어 줍니다. cli에서 알아서 하게 설정이 되어있습니다.
하지만 vue를 공부하면서 어떻게 js파일을 만드는지 또 누가 그 작업을 해주는지는 cli로 만든 프로젝트로는 알기가 어렵습니다.
물론 vue를 공부하면서 간단한 프로젝트 만들때는 꽤나 유용한 도구이지요.

- 해당 포스트에서는 빈폴더에서 vue 프로젝트를 만들어 보면서 vue를 공부해 보겠습니다. 


## 1. 폴더 생성 및 git의 사용
-------------------------------------------------------
1. 폴더를 생성합니다.
``` bash
mkdir vue-webpack
```
2. 만들 폴더에 접근해서 git을 초기화 합니다.
``` bash
git init
```
- .git 이라는 폴더가 생기면서 git을 사용할 수 있습니다.
3. .gitignore 생성
```
/node_modules
package-lock.json
/dist
```
- 해당 파일은 git 이 무시할 파일, 폴더를 명시합니다.

## 2. npm 프로젝트 생성
-------------------------------------------------------
1. 콘솔창으로 npm 프로젝트를 생성합니다.
``` bash
npm init
```
엔터를 계속 치면 해당 폴더에 package.json 파일이 생성됩니다.
``` json
{
  "name": "vue-webpack",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```
간략하게 package.json 파일은 서드파티 라이브러리들을 관리합니다.
라이브러리들은 `node_modules` 폴더 안에 설치됩니다.

## 3. npm 라이브러리 install
-------------------------------------------------------
-S 플래그는 package.json의 dependencies 에 명시된다.
-D 플래그는 package.json의 devDependencies 에 명시된다.
1. vue
``` bash
npm install -S vue
```
2. webpack
``` bash
npm install -D webpack-cli webpack webpack-dev-server
```
3. webpack-loader
``` bash
npm install -D vue-loader vue-style-loader css-loader
```

4. webpack-plugin
``` bash
npm install -D html-webpack-plugin copy-webpack-plugin
```

5. compiler
``` bash
npm install -D vue-template-compiler
```

package.json
``` json
...
"devDependencies": {
    "css-loader": "^2.1.0",
    "html-webpack-plugin": "^3.2.0",
    "vue-loader": "^15.5.1",
    "vue-style-loader": "^4.1.2",
    "vue-template-compiler": "^2.5.22",
    "webpack": "^4.28.4",
    "webpack-cli": "^3.2.1",
"webpack-dev-server": "^3.1.14"
},
"dependencies": {
    "vue": "^2.5.22"
}
...
```

## 4. npm script
------------------------------------------------------
webpack을 사용하기 위해 package.json안에 스크립트를 만든다.

``` json
...
"scripts": {
    "dev": "webpack-dev-server",
    "build": "webpack"
}
...
```

- 스크립트는 npm run `scripts` 으로 실행할 수 있다.
- 지금 npm run dev | npm run build 를 실행하면 에러가 나는 것을 볼 수 있는데 이는 webpack에 대한 설정파일이 없기 때문이다.
- webpack은 기본적으로 webpack.config.js 를 기본 설정으로 읽는다. 

## 5. webpack.config.js
-------------------------------------------------------
프로젝트 루트에 webpack.config.js 파일을 만든다.

``` javascript
module.exports = {
    /*
     * 웹팩에 넣을 파일들을 정의합니다.
     * Vue 프로젝트에서는 new Vue()가 있는 파일이 기본적으로 들어갑니다.
     * 루트 뷰가 들어가면서 루트 뷰가 임폴트 하는 모든 파일들이 따라서 들어갑니다.
     * key는 프로젝트 빌드할때 나오는 파일 이름이 됩니다.
     * value는 주소로 웹팩에 넣을 파일에 대한 주소가 들어갑니다.
     * 어떤 파일을 넣고 어떤 이름으로 빌드가 될지 결정하는 곳입니다.
    */
    entry: {
        main: './src/main.js'
    },
    module: {
        /*
         * 웹팩이 파일들을 읽을때 파일들에 어떤 처리를 할지 설정값들이 배열로 들어갑니다.
        */
        rules: []
    },
    /*
     * 웹팩에서 사용할 플러그인들이 배열로 들어갑니다.
    */
    plugins: []
};
```

프로젝트 루트에 main.js 파일을 생성합니다.

``` javascript
import Vue from 'vue';
```

`npm run build` 를 실행합니다.
- 작업이 완료되면 dist폴더가 생성됩니다.
- dist안에는 main.js 파일이 있습니다.
- 난독화되어있어서 읽기는 쉽지 않지만 vue 코드가 들어간걸 확인할 수 있습니다.
- 확인이 끝났으면 dist 폴더를 삭제합니다.

## 6. root 파일 생성하기
-------------------------------------------------------
1. ./src/App.vue
- .vue 파일은 html, javascript, css 가 한파일 안에 존재한다.
물론 angular처럼 나눠서 관리할 수 있지만
뷰는 한파일에 있는것을 권장한다.

``` html
<!-- 컴포넌트의 html -->
<template>
    <div>
        vue webpack project
    </div>
</template>

<!-- 컴포넌트의 javascript -->
<script>
export default {}
</script>

<!-- 컴포넌트의 css -->
<style></style>
```

2. index.js

``` javascript
import Vue from 'vue';
import App from './src/App.vue';

/*
 * el: #app                 html의 엘리먼트중 id가 app인 엘리먼트를 찾는다.
 * render: h => h(App)      찾은 엘리먼트를 App으로 렌더링한다.
*/
new Vue({
    el: '#app',
    render: h => h(App)
});
```


## 6. Vue 컴파일
-------------------------------------------------------
지금 상태에서 `npm run build` 를 실행하면 오류가 납니다.
오류가 나는 이유는 App.vue 파일을 어떻게 해야될지 웹팩이 모르기 떄문입니다.
- 웹팩이 App.vue 파일을 어떻게 처리해야 될지 설정을 해야됩니다.
- 여기서 아까 설치한 webpack-loader 들과 컴파일을 사용합니다.

``` javascript
webpack.config.js

const { VueLoaderPlugin } = require('vue-loader');
...
module: {
    rule: [
        /*
         * test는 어떠한 파일들을 찾겠다 입니다.
         * loader는 테스트로 찾은 파일들에 설정한 로더를 적용시키겠다 입니다.
         * 지금 설정값은 vue 파일들은 vue-loader을 적용시키겠다 입니다.
        */
        {
            test: /\.vue$/,
            loader: 'vue-loader'
        }
    ]
},
plugins: [
    /*
     * 부가적으로 vue-loader의 플러그인을 넣어줍니다.
    */
    new VueLoaderPlugin()
]
...
```

- vue-loader은 webpack에서 사용하는 로더일 뿐이고 vue-template-complier가 실질적으로 vue파일을 컴파일 합니다. vue-loader과 vue-template-complier을 같이 설치해야 됩니다.

이상태에서 `npm run build`를 실행하면 dist폴더가 생깁니다.

## 7. 빌드 파일 확인해보기
--------------------------------------------------------
dist 폴더안에 index.html 파일을 만듭니다.

``` html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div id="app"></div>
    <script src="./main.js"></srcipt>
</body>
</html>
```

index.html 파일을 실행합니다.
<img src="./../assets/img/vue-webpack-first.png" />

- index.html 안의 <div id="app"> 이라는 태그가 사라지고 App.vue의 내용으로 바뀌었습니다.

## 이로써 기본적으로 webpack을 사용해서 vue를 어떻게 사용하는지 알 수 있습니다.
