---
layout: post
title: React를 Typescript로 써보기
description: cli를 사용하지 않고 프로젝트를 세팅합니다.
img: ./react-typescript/logo.png
tag: [react, typescript]
author: Do Young An
---

## 0. 설명
------------------------------------
create-react-app 을 이용하면 간편하게 리액트 프로젝트를 만들 수 있습니다.
하지만 세부적으로 프로젝트가 어떻게 구성되는지 모르고 지나가는 경우가 많습니다.


## 1. folder 생성
------------------------------------

프로젝트를 만들기 위한 폴더를 생성합니다.
```bash
mkdir react-typescript-sample
```

## 2. npm 초기화
------------------------------------
npm을 사용하는 프로젝트이므로 package.json 파일을 생성합니다.
```bash
npm init
```

## 3. devDependencies 설치
------------------------------------
프로젝트를 개발할때 필요한 라이브러리를 설치합니다.
1. 웹팩 관련 라이브러리
```bash
npm install -D webpack webpack-cli webpack-dev-server webpack-merge
```
2. 웹팩 플러그인과 로더 관련 라이브러리
```bash
npm install -D html-webpack-plugin ts-loader
```
3. 타입스크립트
```bash
npm install -D typescript
```
4. 타입 관련 라이브러리
```bash
npm install -D @types/react @types/react-dom
```


| 이름  | 설명  | 주소 |
|---|---|----|
| wepback   | 프로젝트 파일들을 번들링 해줍니다.  |  https://webpack.js.org |
| webpack-cli  | webpack의 command line interface  | https://webpack.js.org/api/cli |
| webpack-dev-server  | webpack으로 빌드한 파일을 로컬서버로 띄어줍니다. | https://webpack.js.org/configuration/dev-server |
| html-webpack-plugin  | webpack에서 index.html을 컨트롤 할 수 있게해줍니다.  | https://github.com/jantimon/html-webpack-plugin |
| ts-loader  |  webpack에 들어오는 타입스크립트 파일을 자바스크립트로 변환해줍니다. | https://github.com/TypeStrong/ts-loader |
| typescript  | 타입스크립트를 사용할 수 있게 해줍니다.  | https://www.typescriptlang.org |
| @types/react  | react의 타입파일이 들어있습니다.  | https://www.npmjs.com/package/@types/react |
| @types/react-dom  | react-dom의 타입파일이 들어있습니다. | https://www.npmjs.com/package/@types/react-dom |

## 4. dependencies 설치
------------------------------------
프로젝트를 빌드할때 실제 들어가는 라이브러리를 설치합니다.

```bash
npm install -S react react-dom
```

## 5. tsconfig.json 생성
------------------------------------
타입스크립트 사용할때 쓰는 설정 파일을 생성합니다.

1. 프로젝트 루트에서 해당 커맨드를 입력합니다.
```bash
npx tsc --init
```
2. 루트에 생성된 `tsconfig.json`을 수정합니다.
```json
tsconfig.json
{
    "complierOptions": {
        ...
        "jsx": "react", /* JSX 코드를 사용하기 위해 react로 해줍니다. */
        ...
        "noImplicitAny": false, /* any 타입의 에러를 발생 시키지 않습니다. */
        ...
        "strictNullChecks": false, /* null 값을 체크하지 않습니다. */
    }
}
```

## 6. 메인 컴포넌트(App.tsx) 생성
------------------------------------
리액트의 메인 컴포넌트를 생성합니다.

1. 프로젝트 루트에서 해당 커맨드를 입력합니다.
```bash
mkdir src
```
```bash
touch ./src/App.tsx
```

2. App.tsx

```typescript
import React from 'react';

export default class App extends React.Component {
    constructor(props) {
        super(props);
    }
    render(): JSX.Element {
        return (
            <div>
                Hello react for Typescript!!!
            </div>
        )
    }
}
```

## 7. 엔트리 파일(main.tsx) 생성
------------------------------------
웹팩에 들어가는 제일 처음 파일입니다.

1. 프로젝트 루트에서 해당 커맨드를 입력합니다.
```bash
touch ./src/main.tsx
```

2. main.tsx

```tsx
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

const el = document.getElementById('app');
ReactDOM.render(<App/>, el);
```

## 8. webpack 설정 파일 생성
------------------------------------
웹팩을 사용하기 위한 설정 파일들을 생성합니다.
여기서는 기본설정, 개발설정, 프로덕션설정 3가지로 나눕니다.

1. 프로젝트 루트에서 해당 커맨드를 입력합니다.
```bash
mkdir webpack
```
```bash
touch ./webpack/webpack.config.base.js
```
```bash
touch ./webpack/webpack.config.dev.js
```
```bash
touch ./webpack/webpack.config.prod.js
```

2. webpack.config.base.js

```javascript
const CONFIG = {
    entry: {
        /*
            webpack에 들어가는 첫번째 파일입니다. 
            main.tsx 안에서 import되는 모든 파일들이 따라서 webpack안으로 들어갑니다.
        */
        'main': './src/main.tsx'
    },
    module: {
        rules: [
            {
                /*
                    webpack안으로 들어오는 파일 중에서
                    .ts 혹은 .tsx에 대해서 ts-loader를 사용합니다.
                */
                test: /\.ts(x*)$/,
                loader: 'ts-loader'
            }
        ]
    },
    resolve: {
        extension: ['.ts', '.tsx', '.js']
    }
};

module.exports = CONFIG;
```

3. webpack.config.dev.js

```javascript
const merge = require('webpack-merge');
const BaseConfig = require('./webpack.config.base');
const HtmlWebpackPlugin = require('html-webpack-plugin');

const CONFIG = merge(BaseConfig, {
    plugins: [
        new HtmlWebpackPlugin({
            template: './public/index.html'
        })
    ]
});

module.exports = CONFIG;
```

4. webpack.config.prod.js

```javascript
const merge = require('webpack-merge');
const BaseConfig = require('./webpack.config.base');
const HtmlWebpackPlugin = require('html-webpack-plugin');

const CONFIG = merge(BaseConfig, {
    plugins: [
        new HtmlWebpackPlugin({
            template: './public/index.html'
        })
    ]
});

module.exports = CONFIG;
```



## 9. index.html 생성
------------------------------------
SPA에서 사용되는 하나의 html파일인 index.html을 생성합니다.

1. 프로젝트 루트에서 해당 커맨드를 입력합니다.
```bash
mkdir public
```
```bash
touch ./public/index.html
```

2. index.html

```html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <!-- react 가 랜더링 되는 엘리먼트입니다. -->
    <div id="app"></div>
</body>
</html>
```

## 10. npm script 생성
------------------------------------
생성한 webpack설정 파일을 쉽게 사용하기위해 스크립트를 등록합니다.

```json
...
"script": {
    "dev": "webpack-dev-server --config ./webpack/webpack.config.dev.js",
    "prod": "webpack --config ./webpack/webpack.config.prod.js"
}
...
```

해당 스크립트를 등록하면 `npm run dev` OR `npm run prod` 를 실행할 수 있습니다.  

만약 스크립트를 등록하지 않고  

`npx webpack-dev-server --config ./webpack/webpack.config.dev.js` OR  
`npx webpack --config ./webpack/webpack.config.prod.js` 를 사용해도 똑같지만 스크립트를 동록해서 사용하는게 개발속도를 향상시킵니다.


## 11. npm script 실행
------------------------------------

command에서 `npm run dev`