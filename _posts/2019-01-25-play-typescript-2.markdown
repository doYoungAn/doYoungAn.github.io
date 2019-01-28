---
layout: post
title: 타입스크립트에서 사용하는 기본타입
data: 2019-01-16
description: 타입스크립트를 체험해봅니다.
img: ./play-typescript-1/logo.jpg
tags: [typescript]
author: Do Young An
---

## 들어가기 앞서
---
타입스크립트를 사용하기 위해선 기본적으로 [typescript](https://www.npmjs.com/package/typescript)를 다운받아야 됩니다.  
npm 글로벌로 설치하는 것도 좋지만  
공부하려고 테스트 하는 것이니깐 로컬로 받아봅니다.

1. 
``` bash
touch study-types
```

2. 
``` bash
cd study-types
```

3. 
``` bash
npm init
```

4. 
```bash
npm install -S typescript
```

- 위의 순서를 따라하면 로컬로 타입스크립트를 설치 할 수 있습니다.
- 해당 폴더 안에서 `npx tsc` 명령으로 받은 타입스크립트를 사용할 수 있습니다.

## 연습
---
- `example.ts` 파일을 생성합니다.

``` typescript
const age: number = 20;
```

- `npx tsc example.ts` 명령을 실행합니다. 

- 컴파일이 완료되면 `example.js` 자바스크립트 파일이 생성됩니다.

``` javascript
var age = 20;
```

- .ts 파일이 .js로 변환되었습니다.
- 큰 차이점은 number 타입이 없어지고 변수 선언이 const => var 로 바뀌었습니다.
- 간단하게 명령 하나로 타입스크립트를 작성해 자바스크립트로 컴파일 하였습니다.

#### 타입이 다르게 값을 설정한다면?

``` typescript
const age: number = "20";
```

<img src="./../assets/img/play-typescript-2/error_compile.png" />  
"20" 이라는 값은 number 타입이 아니라고 에러는 보여줍니다.



## String
---
문자열을 의미합니다. 타입스크립트에서는 문자열은 `""` 혹은 `''`으로 표현합니다.  
특수한 경우 백틱도 쓸 수 있습니다.

``` typescript
let name: string = 'Do Young';
let hoddy: string = "soccer";
let age: number = 20;
let helloTxt: string = `Hello, I am ${name}, My hoddy is ${hoddy}, i am ${age} year old`;
```
<br/>
``` javascript
var name = 'Do Young';
var hoddy = "soccer";
var age = 20;
/*
 * 백틱으로 다른 변수를 할당하면 + 로 스트링을 만들어 줍니다.
 * + 로 이어주는 것보다 가독성이 훨씬 증가하였습니다.
*/
var helloTxt = "Hello, I am " + name + ", My hoddy is " + hoddy + ", i am " + age + " year old";
```

## Number
---

## Boolean
---

## Array
---

## Object
---

## Function
---

## Any
---

## Void
---


