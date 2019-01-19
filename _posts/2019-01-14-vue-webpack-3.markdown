---
layout: post
title: vue, webpack 프로젝트 생성기 3
data: 2019-01-14
description: 
img: ./vue-webpack-2/logo.jpeg
tags: [vue, webpack]
author: Do Young An
---

## 이전포스트
----------------------------------------------------
이전 포스트까지 완료했으면 vue를 빌드하고 dev-server로 테스트 하고   
정적파일들을 복사해서 사용할 수 있게 되었습니다.  
[vue,webpack프로젝트2](https://doyoungan.github.io/vue-webpack-2/)  

## 12. webpack.config 분할하기
-----------------------------------------------------
개발해서 빌드할때는 두가지로 나눌 수 있습니다. (development, production)   
`development` 일때는 소스의 확인이 편해야되고 난독화 등을 하면 안되겠죠   
`production` 일때는 반대로 난독화를 해야되며 파일을 최대한 작게 만들게 됩니다.  
해당 모드에 따른 webpack의 설정이 다를 수 있습니다.
그러면 한가지 방법은 webpack의 설정파일을 `development`, `production` 두개를 만들면 어떨까요?  
물론 이것도 한가지 방법이지만 두개의 파일에 곂치는 부분이 있을겁니다.  
예를들어

``` javascript
...
    rules: [
        ...
            test: /\.vue$/,
            loader: 'vue-loader'
        ...
    ]
...
```

vue-loader는 vue를 쓰기 위해서는 필수적으로 들어가야 됩니다. 결국 두개의 설정 파일에 들어가야겠죠


이 방법보단 곂치는 부분을 한곳에 모을 순 없을까요?  
이것을 해결해 주는 것이 `webpack-merge` 입니다.

이제 3개의 설정 파일을 만들어 보겠습니다. 기존의 webpack.config.js는 삭제합니다.
1. webpack.base.js  
`development`, `production` 에서 공통으로 사용되는 설정 값들이 있습니다.

``` javascript
module.exports = {
    entry: {
        main: './src/main.js'
    },
    module: {
        rules: [
            {
                test: /\.vue$/,
                loader: 'vue-loader'
            },
            {
                test: /\.css$/,
                loader: ['vue-style-loader', 'css-loader']
            }
        ]
    }
};
```

2. webpack.config.dev.js  
`development`에서 사용되는 설정 값들이 있습니다.

``` javascript
const webpack = require('webpack');
const merge = require('webpack-merge');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const baseConfig = require('./webpack.config.base');

/*
 * webpack.config.base 를 거져와서 머지합니다.
* rules이라는 값이 없어도 baseConfig에 존재한다.
*/
module.exports = merge(baseConfig, {
    // 
    mode: 'development',
    // webpack-dev-server에 대한 설정입니다.
    devServer: {
        hot: true,
        host: '0.0.0.0',
        port: 8080,
        historyApiFallBack: true,
        publicPath: '/'
    },
    plugins: [
        // 핫 리로딩을 해주는 플러그인입니다.
        new webpack.HotModuleReplacementPlugin(),
        new HtmlWebpackPlugin({
            template: './public/index.html'
        })
    ]
});
```

3. webpack.config.prod.js  
`production` 에서 사용되는 설정 값들이 있습니다.


``` javascript
const merge = require('webpack-merge');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const baseConfig = require('./webpack.config.base');

/*
 * webpack.config.base 를 거져와서 머지합니다.
 * rules이라는 값이 없어도 baseConfig에 존재한다.
*/
module.exports = merge(baseConfig, {
    mode: 'production',
    // 빌드한 결과 파일들의 이름을 설정합니다.
    output: {
        filename: '[name].[hash].min.js',
        chunkFilename: '[name].[hash].min.js'
    },
    plugins: [
        /*
        * html-webpack-plugin 의 설정이 development보다 추가되었습니다.
        */
        new HtmlWebpackPlugin({
            filename: 'index.html',
            template: './public/index.html',
            inject: true,
            minify: {
                removeComments: true,
                collapseInlineTagWhitespace: true,
                collapseWhitespace: true
            }
        })
    ]
});
```

이제 3개의 웹팩 설정파일이 생겼습니다.
만약 공통으로 쓰이는 설정이라면 `webpack.config.base`를 수정하면 됩니다.  
development에서 쓰이는 설정이라면 `webpack.config.dev`를 수정하면 됩니다.  
production에서 쓰이는 설정이라면 `webpack.config.prod`를 수정하면 됩니다.

플러그인들의 자세한 옵션을 확인하려면  
[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)
[webpack-merge](https://github.com/survivejs/webpack-merge)
[html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin)
해당 사이트를 방문해주세요.

## 13. npm script 수정하기
--------------------------------------------------------
webpack은 기본값으로 webpack.config.js파일을 읽습니다.  
하지만 webpack.config.js는 지워지고  
webpack.config.base.js, webpack.config.dev.js, webpack.config.prod.js 세개의 파일이 생겼습니다.  
해당 파일을 읽을 수 있게 npm 스크립트를 수정합니다.

``` json
package.json
"script": {
    "dev": "webpack-dev-server --config webpack.config.dev.js",
    "build": "webpack --config webpack.config.prod.js"
}
```

--config 옵션으로 해당 설정파일을 넣어줍니다.

## 14. 폰트 혹은 svg 파일
---
웹 프로젝트를 할때는 이미지도 필요하지만 폰트나 svg 파일을 사용할때가 있습니다.  
이때 사용하는 것이 `url-loader`입니다.

``` javascript
webpack.config.base.js
...
    rules: [
        ...
            {
                test: /\.(woff|woff2|eot|ttf|svg)(\?[a-z0-9=.]+)?$/,
                loader: 'url-loader',
                options: {
                limit: 100000,
                name: '[name].[ext]',
                outputPath: '/assets/fonts'
                }
            }
        ...
    ]
...
```


## 15. dist 폴더
---
빌드할때마다 dist 폴더안에 js, css, html 등의 파일들이 생성됩니다.  
코드를 고치고 빌드하면 이전 버전의 dist는 필요 없어질 수 있습니다.  
이를 위해 사용하는 것이 `clean-webpack-plugin`입니다.

``` javascript
webpack.config.base.js
...
    plugins: [
        ...
            /*
             * webpack.config.base.js 가 실행할때마다 dist 폴더를 삭제합니다.
             * 해당 설정을 dev, prod 가 공유함으로 dev, prod 모두 할때마다 dist 폴더가 삭제됩니다.
            */
            new ClearWebpackPlugin(['dist'], {
                root: process.cwd(),
                verbose: true,
                dry: false
            })
        ...
    ]
...
```

## 16. router 사용하기
---
이제 webpack에 대한 설정은 거의 끝났습니다.
Vue는 SPA(single page application)이므로 하나의 index.html을 갖습니다.
기본적인 라우팅의 이동은 주소를 이동하면서 해당 주소에 있는 파일을 보여주게 됩니다.
Vue에서 라우팅을 사용하려면 `vue-router`가 필요합니다.

- ./src/router.js

``` javascript
import Vue from 'vue';
import Router from 'vue-router';

Vue.use(Router);

const router = new Router({
    mode: 'history',
    routes: [
        // 프로젝트에서 사용하는 라우트등이 등록됩니다.
    ]
});

export default router;
```

- ./src/main.js

``` javascript
import router from './router';

new Vue({
    router,
    el: '#app'
})
```

## 17. 마무리
---
이제 기본적은 vue 프로젝트 보일러 플레이트가 완료되었습니다.
vue에 대한 내용보단 webpack에 대한 내용이 더 많았습니다.
vue-cli로 프로젝트를 구성하는 것보단 하나씩 해보는 것이
뷰가 어떻게 컴파일 되고 어떤 파일들이 들어가는지 확실히 알 수 있습니다.
개발할때 큰 장점이 됩니다. 개발자가 커스텀을 할 수 있는 부분이 많아지죠

샘플 프로젝트는 [vue-webpack](https://github.com/doYoungAn/vue-webpack-1) 을 참조하세요.
