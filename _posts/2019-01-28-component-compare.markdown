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

# 프로젝트 요약
---
1. 하나의 css 파일을 React, Vue, Angular가 공유합니다.
2. JSON 배열을 GET으로 받아옵니다.
    - React, Vue 는 `axios`를 사용하고 Angular는 `@angular/http`를 사용합니다.
    - `axios`는 promise로 기반이고 `@angular/http`는 rxjs 기반입니다.
3. 부모 컴포넌트에서 배열의 데이터를 표현하기 위해 자식 컴포넌트를 사용합니다.

* Promise와 rxjs에 대한 이해가 없더라고 이 포스트를 보는데 문제는 없습니다. 만약 더 알고 싶은 분은 [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), [rxjs](http://reactivex.io/)를 참조하세요.

# github
---
해당 프로젝트는 [react-vue-angular](https://github.com/doYoungAn/react-vue-angular)에서 확인 할 수 있습니다.

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

웹팩을 사용하여 각각 엘리먼트 마다 React, Vue, Angular를 사용했습니다.

# React
---
### 1. 폴더 구조  

``` bash
|   components
|    |  item.jsx
|   App.jsx
|   main.js
```

### 2. 데이터 받기  

``` javascript
App.jsx

/*
 * 라이프 사이클의 첫번째인 componentWillMount에서 axios get으로 json 배열을 가져옵니다.
 * 가져온 json 배열을 items 변수에 넣어줍니다.
*/
import axios from 'axios';
...
constructor(prop) {
    super(prop);
    this.state = {
        items: []
    }
}

componentWillMount() {
    axios.get('/static/posts.json').then((res) => {
        this.setState({ items: res.data });
    }).catch((err) => {

    });
}
```

### 3. 자식컴포넌트에서 데이터 받을 준비하기 

``` javascript

/*
 * 리액트는 딱히 부모로 받을 데이터에 대한 설정은 안해도 됩니다.
 * this.props 로 접근 할 수 있습니다.
*/
constructor(prop) {
    super(prop);
}
```

### 4. 자식 컴포넌트 등록하기

``` jsx
App.jsx

/*
 * 컴포넌트를 불러온 이름 그대로 사용하면 됩니다.
 * 통상적으로 export default를 사용하기 때문에 
 * import 로 불러올때 이름을 잘 지어주는게 좋습니다.
 * <Item /> 이런 식으로 사용 가능합니다.
*/
import Item from './components/item.jsx';
```

### 5. 자식 컴포넌트 사용하기

``` jsx
App.jsx

/*
 * items배열을 map을 사용하여 원소 갯수 만큼 Item 컴포넌트를 리턴합니다.
 * item={item} 으로 Item 컴포넌트에 각각 원소를 넣어준다.
 * index는 배열의 index이다.
*/
render() {
    return (
        <div class="wrapper">
            <h1>React</h1>
            { this.state.items.map((item, index) => {
                return ( <Item item={item} /> )
            }) }
        </div>
    );
}
```

# Vue
---
### 1. 폴더 구조 

``` bash
|   components
|    |   Item.vue
|   App.vue
|   main.js
```

### 2. 데이터 받기  

``` javascript
App.vue

/*
 * 라이프 사이클의 첫번째인 created에서 axios get으로 json 배열을 가져옵니다.
 * 가져온 json 배열을 items 변수에 넣어줍니다.
*/
...
import axios from 'axios';
...
data() {
    return {
        items: []
    }
},
created() {
    axios.get('/static/posts.json').then((res) => {
        this.$data.items = res.data;
    }).catch((err) => {

    });
}
```

### 3. 자식컴포넌트에서 데이터 받을 준비하기

``` javascript
item.vue

/*
 * 부모로 부터 받을 변수는 props 안에 배열로 선언합니다.
 * 해당 값은 :item="" 형식으로 받을 수 있습니다.
*/
export default {
    props: ['item']
}
```

### 4. 자식 컴포넌트 등록하기

``` javascript
App.vue

/*
 * components 안에 불러온 컴포넌트를 넣어줍니다.
 * 키값을 넣지 않으면 키값은 컴포넌트 이름의 string가 됩니다.
 * 예를들어 아래는 'Item': Item 과 같습니다.
 * 컴포넌트를 사용할때는 키값을 태그로 사용합니다. <Item />
*/
import Item from './components/item.vue';
...
components: {
    Item
}
...
```

### 5. 자식 컴포넌트 사용하기

``` html
App.vue

<!--
    v-for 디렉티브를 써서 Item 컴포넌트를 items 배열안의 원소만큼 생성한다.
    :item="item" 으로 Item 컴포넌트에 각각 원소를 넣어준다.
    index는 배열의 index이다.
-->
<template>
    <div class="wrapper">
        <h1>Vue</h1>
        <Item v-for="(item, index) of items" :item="item" v-bind:key="index" />
    </div>
</template>
```

# Angular
---
### 1. 폴더 구조  

``` bash
|   app
|    |  app.component.html
|    |  app.component.ts
|    |  app.module.ts
|   components
|    |  item
|    |   | item.component.html
|    |   | item.component.ts
|    |  component.module.ts
|   main.ts
|   zone.ts
```

### 2. 데이터 받기 

``` typescript
app.component.ts

/*
 * 라이프 사이클 첫번쨰인 ngOnInit 에서 http get으로 json을 가져옵니다.
 * 가져온 json을 items 변수에 넣어줍니다.
*/
...
import { map } from 'rxjs/operators';
...
public items: any[] = [];
...
ngOnInit() {
    this.http.get('/static/posts.json')
        .pipe(map(res => res.json()))
        .subscribe((res) => {
            this.items = res;
        });
}
```

### 3. 자식컴포넌트에서 데이터 받을 준비하기

``` typescript
item.component.ts

/*
 * 부모로 부터 받을 변수는 Input 데코레이터로 설정합니다.
 * 해당 값은 [item]="" 이런 형식으로 받을 수 있습니다.
*/
import { Component, Input } from '@angular/core';
...
@Input() item: any;
...
```

### 4. 자식 컴포넌트 등록하기
* component 모듈에 item 컴포넌트를 등록합니다.

``` typescript
component.module.ts

import { NgModule } from '@angular/core';
import { ItemComponent } from './item/item.component';

@NgModule({
    /*
     * 해당 모듈에 등록하는 컴포넌트들의 배열입니다.
     * Item 컴포넌트를 사용하기 위해서는 먼저 모듈에 등록해야됩니다.
    */
    declarations: [ItemComponent],
    /*
     * 다른 모듈에서 해당 모듈을 쓸때 접근 가능한 컴포넌트들의 배열입니다.
     * Item 컴포넌트를 다른 모듈에서 사용하고 싶다면 exports 배열에 넣어줍니다.
     * 통상적으로 컴포넌트는 declarations와 exports 두군데에 넣어줍니다.
    */
    exports: [ItemComponent]
})
export class ComponentModule {}
```

* App 컴포넌트가 등록된 모듈에서 component 모듈을 불러옵니다.

``` typescript
App.module.ts

import { NgModule } from '@angular/core';
...
import { ComponentModule } from './../components/component.module';
import { AppComponent } from './app.component';

@NgModule({
    // component 모듈을 임폴트 하면 component 모듈이 exports 하는 것을 사용 할 수 있습니다.
    imports: [
        ...
        ComponentModule
        ...
    ],
    declarations: [AppComponent],
    bootstrap: [AppComponent]
})
export class AppModule {}
```

* 태그 이름 

``` typescript
item.component.ts

/*
 * React나 Vue와는 달리 컴포넌트를 만들때 태그 이름도 정해줍니다.
 * selector 값이 태그가 됩니다.
 * <Item></Item> 이런 식으로 사용합니다.
*/
import { Component } from '@angular/core';
@Component({
    selector: 'Item',
})
```

### 5. 자식컴포넌트 사용하기

``` html
app.component.html

<!--
    *ngFor 디렉티브를 써서 Item 컴포넌트를 items 배열안의 원소만큼 생성한다.
    [item]="item" 으로 Item 컴포넌트에 각각 원소를 넣어준다.
    index는 배열의 index이다.
-->
<div class="wrapper">
    <h1>Angular</h1>
    <item *ngFor="let item of items; let i = index" [item]="item"></item>
</div>
```


# 요약
----

 구분 | React | Vue | Angular
:----|:-----:|:------:|:--------:
컴포넌트 등록| 컴포넌트를 import 만 해주면 된다. | 컴포넌트를 import 하고 쓰려는 컴포넌트에 등록만 해주면 된다.| 컴포넌트를 모듈에 등록하고 사용하려는 컴포넌트가 있는 모듈에 등록해주어야 된다.
for문 사용 | 배열의 map을 사용한다. | v-for 디렉티브를 사용한다. | *ngFor 디렉티브를 사용한다.
데이터 주입 | item={item} | :item="item" | [item]="item"
자식컴포넌트의 데이터 받기 | X | props: ['item'] | @Input() item: any;

컴포넌트 등록하는 방법은 리액트와 뷰가 상당히 닮은 모습을 보입니다. 상대적으로 앵
귤러는 복잡한 단계를 거쳐야 합니다. 컴포넌트를 만들고 모듈에 등록하고 해당 모듈을 다른 모듈에 등록해야 됩니다. 이는 코드를 봤을때 이 컴포넌트는 어디서 왔는지 확인하기가 어렵습니다. 컴포넌트 단에서 확인이 불가능하고 모듈 단에서 확인해야되니깐요. 
이것만 보면 앵귤러가 다른 두개에 비해 떨어진다고 생각 할수 있습니다.
하지만 프로젝트가 커지면 만들어진 컴포넌트는 많아집니다. 
예를 들어 페이지가 20개의 컴포넌트를 사용한다고 볼때 리액트랑 뷰는 20개를 모두 임폴트 해야됩니다. 물론 글로벌등록 등 다른 방법도 있지만 기본적으론 20개를 임폴트 해야됩니다.
하지만 앵귤러는 20개의 컴포넌트가 등록된 모듈만 가져오면 됩니다.
이러한 점에서 앵귤러는 shared 모듈이라는 개념이 발달했습니다. 자주 쓰이는 것들은 하나의 모듈에 담아두는 것이지요. 기본적으로 shared 모듈만 등록하면 기본적인 기능을 가질 수 있습니다.
3개 모두 훌룽햔 프론트 엔드 프레임워크 혹은 라이브러리 입니다.