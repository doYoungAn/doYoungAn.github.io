---
layout: post
title: vue, webpack 프로젝트 생성기 2
data: 2019-01-03
description: 
img: vue_logo.jpeg
tags: [vue, webpack]
author: Do Young An
---

## 이전 포스트
----------------------------------------------------
이전 포스트에서는 vue파일을 webpack으로 빌드해서 확인해 보는 것까지 했습니다.
혹시 이전글을 못보신 분들은 [vue,webpack프로젝트1](https://doyoungan.github.io/vue-webpack-1/)여기를 보고 와주세요.

## 8. index.html
---------------------------------------------------
7.번에서는 webpack으로 빌드한 자바스크립트를 확인해봤습니다.  
index.html파일에 자바스크립트를 import 해주면 끝이 납니다.  
하지만 개발하면서 파일을 수동으로 import 한다면 많은 시간 낭비일것입니다.  
이것을 도와주는 것이 webpack plugin중 `html-webpack-plugin` 입니다.  

- public 폴더 안에 index.html 파일 생성합니다.  

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
</body>
</html>
```
- 단지 id를 app이라고 가진 div 엘리만트만이 body안에 존재합니다.  

- webpack.config.js

``` javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');
...
    plugins: [
        ...
            new HtmlWebpackPlugin({
                // 템플릿으로 사용할 html파일의 주소를 입력합니다.
                template: './public/index.html'
            })
        ...
    ]
...
```

- 빌드
`npm run build`를 실행합니다.

``` bash
dist
 | index.html
 | main.js
```

dist 폴더 안에 index.html과 main.js 파일이 생깁니다.

- index.html 파일을 확인합니다.

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
<script type="text/javascript" src="main.js"></script></body>
</html>
```

body안에 div만 존재했는데 js가 추가되었습니다.   
이는 html-webpack-plugin이 작업해준것입니다.    
webpack으로 빌드한 js를 자동으로 추가해줍니다.   

- 이로서 수동으로 index.html을 생성할 필요가 없어졌습니다.


## 9. webpack dev server
---------------------------------------------------------------
이제 기본적으로 vue 파일을 빌드해 html에 추가하는 것까지 왔습니다.   
개발하다 보면 수시로 저장하고 확인 하게 될 것입니다.  
이때마다 빌드하고 확인하는 것은 큰 시간 낭비가 될 것입니다.   
이를 해결해 주는 것이 `webpack-dev-server`입니다.  

- 실행해보기
`npm run dev` 를 콘솔창에서 실행합니다.

``` bash
ℹ ｢wds｣: Project is running at http://localhost:8080/
ℹ ｢wds｣: webpack output is served from /
```

해당 주소로 접속해보면 index.html이 보여집니다.  
여러분들은 아주 간단하게 로컬 서버를 열었습니다.  
해당 상태에서 App.vue의 div 안의 텍스트를 바꿔서 저장해보세요.


## 10. css 적용
-----------------------------------------------------------------
이제 Vue 파일의 html과 javascript를 빌드할 수 있게되었습니다.  
한번 이제 스타일을 지정해봅시다.  

``` html
App.vue
...
<template>
    <div class="sample">
        vue webpack project
    </div>
</template>
...
<style>
.sample {
    background-color: green;
}
</style>
...
```

`npm run dev` 를 실행하면 오류가 나는 걸 볼 수 있습니다.

``` bash
You may need an appropriate loader to handle this file type.
|
|
> .sample {
|     background-color: green;
| }
```

해석해보면 이 파일타입에 적용할 로더가 필요하다고 하네요.  
이 오류를 해결하려면 2가지 로더가 필요합니다.

``` javascript
webpack.config.js
...
    rules: [
        ...
        {
            test: /\.css$/,
            loader: [
                /*
                 * 로더에는 배열로 값을 넣을 수 있습니다.
                 * 배열 순서대로 로더를 적용 시킵니다.
                 * vue-style-loader은 자바스크립트로 변환된 css를 동적으로 돔에 추가해줍니다.
                 * css-loader은 css를 자바스크립트로 바꾸어 모듈로 넣어줍니다.
                */
                'vue-style-loader',
                'css-loader'
            ]
        }
        ...
    ]
...
```

다시 `npm run dev`를 실행하면 div가 초록색으로 변하는 것을 볼 수 있습니다.   
이로써 간단하게 스타일 까지 적용 시켰습니다.


## 11. 이미지 적용하기
---------------------------------------------------------------
css까지 적용시켜봤으니 이제 이미지를 어떻게 불러올 수 있는지 알아봅시다.  
여기서 사용될 것이 `copy-webpack-plugin`입니다.

- src 폴더안에 assets 폴더를 생성하고 샘플 이미지를 넣어줍니다.
- webpack.config.js를 수정해줍니다.

``` javascript
const CopyWebpackPlugin = require('copy-webpack-plugin');
...
    plugins: [
        ...
            new CopyWebpackPlugin([
                {
                    /*
                     * from: 복사하려는 폴더 혹은 파일의 주소입니다.
                     * to: 복사되는 폴더 혹은 파일의 주소입니다.
                     * type: 'dir', 'file', 'template' 3가지 값을 가질 수 있습니다.
                     * 지금은 폴더 자체를 복사하기 떄문에 dir 값이 들어갔습니다.
                    */
                    from: 'src/assets',
                    to: 'assets',
                    toType: 'dir'
                }
            ])
        ...
    ]
...
```

수정이 끝나면 `npm run build`를 실행합니다.  
빌드가 끝나면 dist폴더안에 assets폴더가 있는 걸 확인할 수 있습니다.  
이미지 들은 `<img src="/assets/hello.png" alt="" />` 이런 형식으로 불러올 수 있습니다.

## 이로써 기본적인 Vue에 스타일과 이미지까지 적용시켰습니다.