---
layout: post
title: vue, webpack 프로젝트 생성기 3
data: 2019-01-14
description: 
img: vue_logo.jpeg
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

