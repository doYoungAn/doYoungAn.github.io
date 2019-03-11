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

### 2. 스토어
스토어는 하나의 객체로 이루어져있습니다.  
reducer.js 안에 initalState 로 정의되어 있으며
export 되지 않아서 다른 곳에서 접근 불가합니다.

``` javascript
reducer.js

const initialState = {
    count: 0,
    owner: {
        name: '',
        age: 0
    },
    users: ['a', 'b', 'c']
}
```

### 3. 리듀서
리듀서는 하나의 함수로 이루어져있습니다.  
상태와 액션을 파라미터로 받습니다.  
만약 액션에 payload가 있다면 payload 값으로 상태 값을 변화시킵니다. 
함수의 리턴값으로 상태값을 반환합니다.

``` javascript
reducer.js

export function reducers(state = initialState, action) {
    switch (action.type) {
        case INCREMENT:
            return {
                ...state,
                count: state.count + 1
            }
        case DECREMENT:
            return {
                ...state,
                count: state.count - 1
            }
        case ADDUSER:
            return {
                ...state,
                users: [...state.users, action.payload]
            }
        case DELETEUSER:
            state.users.splice(action.payload, 1);
            const users = state.users;
            return Object.assign({}, state, {
                users: users
            });
        default:
            return state;
    }
}
```

### 4. 액션
상태를 변화시키는 행동들이 정의되어있습니다.
const 스트링 값으로 액션의 타입을 정해줍니다.(변화시킬 수 잆습니다.)
함수 리턴시 주는 payload값이 상태를 변화시키는 값들입니다.

``` javascript
actions.js

export const INCREMENT = 'INCREMENT';
export const DECREMENT = 'DECREMENT';
export const ADDUSER = 'ADDUSER';
export const DELETEUSER = 'DELETEUSER';

export function increment() {
    return {
        type: INCREMENT
    }
}

export function decrement() {
    return {
        type: DECREMENT
    }
}

export function addUser(user) {
    return {
        type: ADDUSER,
        payload: user
    }
}

export function deleteUser(index) {
    return {
        type: DELETEUSER,
        payload: index
    }
}
```

### 5. 리덕스 등록
리액트에서 리덕스를 사용하려면 등록을 해주어야됩니다.  

``` javascript
main.js

import React from 'react';
import ReactDOM from 'react-dom';
import App from './App.jsx';
import { createStore } from 'redux';
import { Provider } from 'react-redux';
import reducers from './store/reducers';

const store = createStore(reducers);
const el = document.getElementById('react-root');

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    el);
```

### 6. 컴포넌트에서 리덕스 사용하기
리액트에서는 스토어 값을 prop에 대입해 사용 가능합니다.

``` javascript
App.jsx

import React, { Component } from 'react';
import { connect } from 'react-redux';
import { syntaxHighlight } from './../utils';
import { increment, decrement, addUser, deleteUser } from './store/actions';

class App extends Component {

    constructor(prop) {
        super(prop);
        this.state = {
            user: ''
        }
    }

    handleChange(event) {
        this.setState({ user: event.target.value });
    }

    handleSubmit(event) {
        event.preventDefault();
        this.props.onAddUser(this.state.user);
        this.setState({ user: '' });
    }

    syntaxHighlight(json) {
        return syntaxHighlight(json);
    }

    render() {
        return (
            ...
        );
    }
}

// 스토어의 값을 prop에 매칭시켜줍니다.
let mapToStateToProps = (state) => ({
    all: state,
    count: state.count,
    users: state.users
});

// 스토어의 변화를 일으키는 액션들을 prop에 매칭시켜줍니다.
let mapToDispatchToProps = (dispatch) => {
    return {
        onIncrement: () => dispatch(increment()),
        onDecrement: () => dispatch(decrement()),
        onAddUser: (user) => dispatch(addUser(user)),
        onDeleteUser: (index) => dispatch(deleteUser(index))
    }
}

App = connect(mapToStateToProps, mapToDispatchToProps)(App);

export default App;
```

# Vue
---

### 0. vuex
Vue에서 리덕스 개념으로 만들어진 라이브러리입니다.
해당 라이브러리의 도움을 받아 리덕스를 사용합니다.
[vuex](https://vuex.vuejs.org/)는 루트 뷰 인스턴스 만들때 넣어줍니다.

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

### 6. 컴포넌트에서 리덕스 사용하기

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
### 0. Ngrx
앵귤러에서 리덕스 개념으로 만들어진 라이브러리입니다.  
해당 라이브러리의 도움을 받아 리덕스를 사용합니다.
[Ngrx](https://ngrx.io/)는 rxjs기반으로 만들어졌습니다.


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

### 2. 스토어
스토어는 하나의 객체로 이루어져있습니다.  
reducer.ts 안에 initialState 로 정의되어있으며 
export 되지 않아서 다른 곳에서 접근이 불가합니다.

``` typescript
reducer.ts

const initialState = {
    count: 0,
    owner: {
        name: '',
        age: 0
    },
    users: ['a', 'b', 'c']
}
```

### 3. 리듀서
리듀서는 하나의 함수로 이루어져있습니다.  
상태와 액션을 파라미터로 받습니다.
만약 액션에 payload가 있다면 payload값으로 상태 값을 변화시킵니다.  
함수의 리턴값으로 상태값을 반환합니다.

``` typescript
reducer.ts

export function reducers(state = initialState, action): any {
    switch (action.type) {
        case ActionTypes.Increment:
            return {
                ...state,
                count: state.count + 1
            };
        case ActionTypes.Decrement:
            return {
                ...state,
                count: state.count - 1
            };
        case ActionTypes.AddUser:
            return {
                ...state,
                users: [...state.users, action.payload]
            };
        case ActionTypes.DeleteUser:
            state.users.splice(action.payload, 1);
            return state;
        default:
            return state;
    }
}
```

### 4. 액션
상태를 변화시키는 행동들이 정의되어있습니다.  
ActionTypes enum으로 액션에 대한 타입을 정의해줍니다.  
해당 타입이 리듀서의 스위치에서 케이스를 판단합니다.
클래스 생성시 받는 payload는 상태를 변화시키는 값들을 받습니다.  

``` typescript
action.ts

import { Action } from '@ngrx/store';

export enum ActionTypes {
    Increment = '[main] Increment',
    Decrement = '[main] Decrement',
    AddUser = '[main] AddUser',
    DeleteUser = '[main] DeleteUser'
}

export class Increment implements Action {
    readonly type = ActionTypes.Increment;
}

export class Decrement implements Action {
    readonly type = ActionTypes.Decrement;
}

export class AddUser implements Action {
    readonly type = ActionTypes.AddUser;
    constructor(public payload: string) {}
}

export class DeleteUser implements Action {
    readonly type = ActionTypes.DeleteUser;
    constructor(public payload: number) {}
}
```

### 5. 리듀서 등록
앵귤러는 만든 리듀서를 사용하기 위해서는 module에 등록을 해야됩니다.
ngrx와 리덕스를 가져와서 모듈에 등록해줍니다.

``` typescript
app.module.ts

import { StoreModule } from '@ngrx/store';
import { reducers } from './../store/reducer';

@NgModule({
    imports: [
        // 리덕스 등록하는 키값으로 사용할때 불러옵니다.
        StoreModule.forRoot({ main: reducers })
    ]
})
export class AppModule {}
```

### 6. 컴포넌트에서 리덕스 사용하기

``` typescript
app.component.ts
import { Component, OnInit } from '@angular/core';
import { Store, select } from '@ngrx/store';
import { Observable } from 'rxjs';
import { syntaxHighlight } from './../../utils';
// 만들어둔 액션을 가져옵니다.
import { Increment, Decrement, AddUser, DeleteUser } from './../store/actions';

@Component({
    selector: 'angular-root',
    templateUrl: './app.component.html',
})
export class AppComponent implements OnInit {

    // 옵저버블 변수에는 통상적으로 $를 붙여줍니다.
    public all$: Observable<any>;
    public user: string = '';

    constructor(
        private store: Store<any>
    ) {
        // main 키값으로 등록한 리듀서를 가져옵니다.
        this.all$ = store.pipe(select('main'));
    }

    ngOnInit() {}

    public increment(): void {
        /*
            스토어 디스패치로 액션을 보내주면
            리듀서 함수에 들어가서 스위치문에서
            케이스에 따라 상태를 업데이트 합니다.
        */
        this.store.dispatch(new Increment());
    }

    public decrement(): void {
        this.store.dispatch(new Decrement());
    }

    public addUser(): void {
        this.store.dispatch(new AddUser(this.user));
        this.user = '';
    }

    public deleteUser(index: number): void {
        this.store.dispatch(new DeleteUser(index));
    }

    public syntaxHighlight(json: any) {
        return syntaxHighlight(json);
    }
}
```

# 결론
---

1. 공통점
- React, Vue, Angular 모두 스토어 객체를 만들어 export 를 하지 않습니다. (다른 곳에서 접근 불가능)
- 액션의 파리미터 값으로 스토어의 값을 변화시킵니다.

2. 다른점
- Vue는 action과 mutation으로 이루어져 있습니다.
- Vue는 $store로 리듀서를 임폴트 하지 않아도 상속 받아서 사용 가능합니다.
- Angular은 rxjs기반으로 스토어 값을 구독으로 받아옵니다.
- React는 스토어 값과 디스패치를 매핑할 수 있어 컴포넌트 로직과 스토어 관련 로직을 분리하기 쉽습니다.

