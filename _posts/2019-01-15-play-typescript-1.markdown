---
layout: post
title: 타입스크립트와 친해지기
data: 2019-01-16
description: 타입스크립트의 정의를 알아봅니다.
img: ./play-typescript-1/logo.jpg
tags: [typescript]
author: Do Young An
---

## 1. 타입스크립트(Typescript) 란 무엇인가?
---
[공식홈페이지](https://www.typescriptlang.org/)를 방문해보면 첫 화면에 이러한 문장이 있습니다.  

>Typescript is a typed superset of javascript that compiles to plain javascript

간단히 해석하면 타입스크립트는 자바스크립트의 슈퍼셋입니다.  
슈퍼셋? 의 뜻을 알기 위해서는 중학생때 배운 집합이라는 개념을 떠올려야됩니다.  
집합에는 합집합, 교집합, 부분집합 등의 개념등이 있지요. 이중에 슈퍼셋은 `상위집합`을 뜻합니다.

결국 이미지로 보면  
<img src="./../assets/img/play-typescript-1/superset.png" />

타입스크립트가 es5, es6를 감싸고 있습니다.  
es5, es6는 ECMAScript5,6 을 뜻하고 있습니다. 간단하게 자바스크립트라고 보면 됩니다.  

이를 토대로 생각해보면 1가지 결론을 생각할 수 있습니다.  
현재 쓰는 자바스크립트 그대로 타입스크립트를 사용할 수 있습니다. 타입스크립트가 상위집합이니깐요.  

## 2. 타입스크립트의 근황
---

타입스크립트는 Turbo Pascal, Delphi, 그리고 C#의 핵심 개발자인 Microsoft 직원 Anders Hejlsberg의 리드 하에 개발되었고, 2년간의 내부 개발을 거쳐 2012년 10월 1일 공개되었다.  
처음에는 그닥 주목받지 못하고 있다가 `AngularJS`가 `Angaulr2`(지금은 통상 Angular)로 넘어가면서 타입스크립트를 기본 언어로 채택하면서  
급속하게 발전하기 시작했습니다.  

<img src="./../assets/img/play-typescript-1/typescript-trend.png" />

타입스크립트가 발전하면서 SPA의 삼대장인 `react` `vue` `angular` 모두 타입스크립트를 쓸 수 있게 지원합니다.  
또한 `node`도 타입스크립트를 쓸 수 있으며  `express` 도 마찬가지 입니다. 


## 3. 타입스크립트의 유용한 점
---
간단한 예시를 보는게 제일 이해가 빠르다고 생각합니다.  

여기 간단한 자바스크립트 함수가 있습니다.

``` javascript
function add(a, b) {
    return a + b;
}

function getStudentName() {
    return obj.name;
}
```

위의 함수 2개 함수는 둘다 리턴값을 가지고 있습니다.  
하지만 차이점이 있지요. 그건 add라는 함수는 number을 리턴 한다고 생각 할 수 있지만  
getStudentName 함수는 obj.name을 리턴하고 있습니다. 이름이라 string라고 생각할 수는 있지만 추론일뿐 정확한 사실은 아니지요.

자바스크립트는 예전부터 계속 써온 언어입니다. 타입스크립트가 나오기 전에 이러한 점을 해결하는 방법은 있었죠.  
바로 `JSDoc`를 이용하는 방법입니다.  

``` javascript
/**
 * @param{Number} a
 * @param{Number} b
 * @return{Number} a + b
*/
function add(a, b) {
    return a + b;
}

/**
 * @return{String} 학생이름
*/
function getStudentName() {
    return obj.name;
}
```

각각의 함수마다 파라미터, 리턴값들의 타입과 설명을 적어두면 됩니다.  이러면 이 함수가 어떤 값을 받고 어떤 값을 리턴할지 알 수 있게 되지요.  
하지만 add('a', 'b')라고 해도 해당 함수를 실행하기 전까지는 잘 못되었다는 것을 알 길이 없습니다.

- 하지만 타입스크립트는 좀 더 나은 방법을 제시해줍니다.

``` typescript
function add(a: number, b: number): number {
    return a + b;
}

function getStudentName(): string {
    return obj.name;
}
```

파라미터와 리턴 값을 함수 안에 포함시킬 수 있게 해줍니다.  
add 라는 함수의 리턴이 number라는 것을 단번에 알 수 있습니다.  
getStudentName 함수의 리턴값이 string라는 것을 알 수 있습니다.  
게다가 add('a', 'b') 라고 하면 컴파일 당시에 에러라는 것을 알 수 있습니다.  

- 컴파일 단계에서 타입 에러를 잡을 수 있으면 버그 발생 확률이 아닌 것보단 크게 낮아집니다.