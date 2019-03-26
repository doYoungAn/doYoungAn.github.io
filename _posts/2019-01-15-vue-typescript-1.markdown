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

## 타입스크립트로 작성
---
