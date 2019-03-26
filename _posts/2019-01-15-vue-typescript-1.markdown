---
layout: post
title: vue를 typescript로 써보기 1
data: 2019-01-15
description: 
img: ./vue-typescript-1/logo.jpg
tags: [vue, typescript]
author: Do Young An
---

## Vue의 타입스크립트 지원
---

vue는 2.5 버전 이상부터 타입스크립트를 지원하기 시작했습니다.  
해당 사항은 [vue typescript](https://kr.vuejs.org/v2/guide/typescript.html) 이 부분을 참고하면 됩니다.

## 기본적인 뷰 컴포넌트
---

``` html
<template>
    <div>{% raw %}
        {{ message }}{% endraw %}
    </div>
</template>

<script>
export default {
    data() {
        return {
            text: ''
        }
    }
}
</script>

<style>

</style>
```
.vue 파일 안에 template 와 script 와 style 부분이 존재합니다.  
script 부분에서 export default로 방출시켜서 다른 컴포넌트에서  
import XXX from './..'; 이런 식으로 불러와 사용합니다. 

## vue-class-component
---
vue의 타입스크립트 공식 라이브러리입니다.  
클래스 기반의 컴포넌트 작성을 도와줍니다.  
자세한 내용은 [vue-class-component](https://www.npmjs.com/package/vue-class-component)를 참조하세요.  

해당 포스트 작성 기준 트렌드를 살펴보면 많은 사람들이 사용 중인것을 볼 수 있습니다.

<img src="./../assets/img/vue-typescript-1/trend.png" />