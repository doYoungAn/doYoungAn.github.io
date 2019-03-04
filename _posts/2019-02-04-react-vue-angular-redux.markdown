---
layout: post
title: 리액트 뷰 앵귤러에서 리덕스 사용 비교
data: 2019-01-28
description: 리액스에서 사용하던 리덕스개념을 뷰와 앵귤러에도 사용 할 수 있습니다. 어떻게 사용하는지 비교해봅니다.
img: ./component-compare/logo.jpg
tags: [react, vue, angular]
author: Do Young An
---

#
---
SPA로 만들어지는 프로젝트들이 커지면서 데이터를 관리해야되는 이슈가 생겼습니다.  
처음에는 React에서 Redux라는 개념이 생겨나면서 스토어로 데이터를 관리하기 시작했습니다. 
해당 개념이 다른 SPA인 angular과 vue까지 넘어오게 되었습니다.

이 포스트에서는 React, Vue, angular가 어떻게 Redux개념을 사용하는지 알아보겠습니다.

# 프로젝트 요약
---
1. 각각 줄마다 React, Vue, Angular를 사용합니다.

# github
---
해당 프로젝트는 [react-vue-angular-redux](https://github.com/doYoungAn/react-vue-angular-redux)에서 확인 할 수 있습니다.

# React
---
### 1. 폴더구조

``` bash
|   store
|    |  actions.js
|    |  reducers.js
|   App.jsx
|   main.js
```

# Vue
---
### 1. 폴더구조

``` bash
|   store
|    |   actions.js
|    |   getters.js
|    |   index.js
|    |   mutations.js
|   App.vue
|   main.js
```

### 2. 스토어
스토어는 하나의 객체로 이루어져 있습니다.
스토어는 index.js안에 있으며 직접 접근할 수 없습니다.

``` javascript
index.js
import Vue from 'vue';
import Vuex from 'vuex';
import * as getters from './getters';
import * as mutations from './mutations';
import * as actions from './actions';

Vue.use(Vuex);

// state 객체가 실제 데이터를 저장하는 스토어입니다.
const state = {
    count: 0,
    owner: {
        name: '',
        age: 20
    },
    users: ['a', 'b', 'c']
}

const store = new Vuex.Store({
    state,
    // 스토어 이외에 게터, 액션, 뮤테이션등을 가져옵니다.
    getters,
    actions,
    mutations
});

export default store;
```

### 3. getter
스토어의 여러 데이터중 원하는 부분을 골라서 가져올 수 있습니다.

``` javascript
export const all = (state) => {
    return state;
}

export const count = (state) => {
    // index.js의 state객체에서 count 프로퍼티를 가져옵니다.
    return state.count;
}

export const owner = (state) => {
    // index.js의 state객체에서 owner 프로퍼티를 가져옵니다.
    return state.owner;
}

export const users = (state) => {
    // index.js의 state객체에서 user 프로퍼티를 가져옵니다.
    return state.users;
}
```

### 4. actions
스토어의 상태를 바꾸는 행동들이 모여있습니다.
React나 Angular는 action을 스위치문 안에서 스토어의 상태를 바꾸지만
Vue의 특징은 액션이 스토어의 행동을 직접 바꾸는게 아니라
commit 이라는 함수로 mutations을 발동 시켜 스토어의 상태를 바꿉니다.
유용한점은 액션 안에서 api를 부르기 쉽습니다.

``` javascript
actions.js
export const increment = ({ commit }) => {
    // 커밋을 하면 뮤테이션의 increment가 발동합니다.
    commit('increment');
}

export const decrement = ({ commit }) => {
    commit('decrement');
}

export const addUser = (context, user) => {
    context.commit('addUser', user);
}

export const deleteUser = (context, index) => {
    context.commit('deleteUser', index);
}
```

### 5. mutations
스토어의 상태를 바꿉니다.
간단한 값의 변경이라면 굳이 액션을 쓰지 말고 
컴포넌트에서 직접 커밋을 부를 수 있습니다.

``` javascript
export const increment = (state) => {
    // state는 index.js의 state입니다.
    state.count++;
}

export const decrement = (state) => {
    state.count--;
}

export const addUser = (state, user) => {
    state.users.push(user);
}

export const deleteUser = (state, index) => {
    state.users.splice(index, 1);
}
```

### 6. App.vue

``` javascript
App.vue
...
increment() {
    this.$store.dispatch('increment');
},
decrement() {
    this.$store.dispatch('decrement');
},
addUser() {
    this.$store.dispatch('addUser', this.$data.user);
    this.$data.user = '';
},
deleteUser(index) {
    this.$store.dispatch('deleteUser', index);
}
...
```
example)
1. (App.vue)        increment 라는 디스패치를 보냅니다.
2. (actions.js)     increment 라는 액션이 실행합니다.
3. (actions.js)     increment 액션 안에서 increment 커밋을 보냅니다. 
4. (mutations.js)   뮤테이션 안에 있는 increment가 실행합니다.
5. (index.js)       state.count에 1을 더합니다.

# Angular
---
### 1. 폴더구조
``` bash
|   app
|    |  app.component.html`
|    |  app.component.ts
|    |  app.module.ts
|   store
|    |  actions.ts
|    |  reducer.ts
|   main.ts
|   zone.ts
```

