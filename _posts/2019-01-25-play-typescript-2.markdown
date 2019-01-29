---
layout: post
title: 타입스크립트에서 사용하는 기본타입
data: 2019-01-27
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
숫자를 의미합니다.  이진수, 8진수, 10진수, 16진수 모두 number 타입으로 정의합니다.

``` typescript
let year: number = 2019;
let age: number = 20;
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```
<br/>
``` javascript
var year = 2019;
var age = 20;
var decimal = 6;
var hex = 0xf00d;
var binary = 10;
var octal = 484;
```

## Boolean
---
true 혹은 false 값을 가집니다.

``` typescript
let isDone: boolean = false;
let isFinished: boolean = true;
````
<br/>

``` javascript
var isDone = false;
var isFinished = true;
```

## Array
---
배열을 의미합니다. 다른 타입을 원소로 가집니다.  
배열을 표현하는 방법은 2가지가 있습니다.  

``` typescript
let list: number[] = [1, 2, 3];
let names: Array<string> = ['a', 'b', 'c'];
```
<br />
``` javascript
var list = [1, 2, 3];
var names = ['a', 'b', 'c'];
```

## Tuple
---
배열에 여러 타입을 지정해줍니다.  
파이썬의 튜플과는 달리 값은 자유롭게 변경 가능합니다.  
``` typescript
let x: [string, number];
x = ["hey", 1];
```
<br/>
``` javascript
var x;
x = ["hey", 1];
```

## Enum
---
C의 enum과 유사합니다.  
값을 설정하지 않으면 0부터 시작합니다.
``` typescript
enum OS { window, mac, linux }
let myCom: OS = OS.window;
console.log(myCom);
```
<br/>
``` javascript
var OS;
(function (OS) {
    OS[OS["window"] = 0] = "window";
    OS[OS["mac"] = 1] = "mac";
    OS[OS["linux"] = 2] = "linux";
})(OS || (OS = {}));
var myCom = OS.window;
console.log(myCom); // console log is 0
```
<br />
enum은 설정한 변수들과 값들로 구성된 객체를 만듭니다.
``` javascript
{
    '0': 'window',
    '1': 'mac',
    '2': 'linux',
    window: 0,
    mac: 1,
    linux: 2
}
```

## Any
---
모든 타입을 정의합니다.  
한마디로 보면 타입스크립트에서 타입을 정해주지 않는 것입니다.  

``` typescript
let age: any = 20;
let name: any = 'Do Young';
let finished: any = false;
let list: any = [1, 2, 3];
// 어떤 타입의 원소를 가질 수 있는 배열입니다.
let cars: any[] = [1, 'ford', false];
```
<br/>
``` javascript
var age = 20;
var name = 'Do Young';
var finished = false;
var list = [1, 2, 3];
var cars = [1, 'ford', false];
```

## Void
---
타입이 없는 상태입니다. 
`any`와 반대되는 개념이며 주로 리턴이 없는 함수에 쓰입니다.  
변수에 지정할 경우 undefined만을 가질 수 있습니다.

``` typescript
function warn(): void {
    console.log('This is my warning message');
}
```
<br />
``` javascript
function warn() {
    console.log('This is my warning message');
}
```


