---
layout: post
title: 앵귤러 src 파일 트리 구조
date: 2017-10-15
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: angular2-logo.jpg # Add image post (optional)
tags: [Angular2]
author: An Do Young # Add name author (optional)
---

# 앵귤러 src 파일 트리 구조
---
## 기본적인 angular cli 로 생성한 앵귤러 프로젝트
<img src="./../assets/img/angular-trees/angular-src-file-tree.png">

### app 폴더
1. app.component.css 파일
    - 해당 파일을 비어있는 상태이다.
    - 해당 컴포넌트에서 필요로 하는 스타일들이 정의되는 파일이다.
2. app.component.html 파일
3. app.component.spec.ts 파일
4. app.component.ts 파일
    - 컴포넌트가 정의되는 파일이다.
    - 앵귤러 프로젝트를 생성한다면 해당 컴포넌트가 메인 컴포넌트의 역할을 한다.

``` javascript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'app';
}
```
5. app.module.ts 파일
    - 모듈을 생성할때는 기본적으로 @angular/core에 있는 NgModule를 import 하여 생성한다.
     - 생성한 모듈은 export를 이용하여 밖으로 내보낸다.

``` javascript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
### assets 폴더
1. .gitKeep 파일
    - 해당 파일의 내용은 비어있는 상태이다.
    - git은 기본적으로 빈 폴더를 트래킹 하지 않는다. 
    - 폴더 안에 파일이 생성되거나 수정되어야 트래킹 하기 시작한다.
    - 해당 파일이 있으면 빈 폴더라고 깃이 트래킹을 한다.
    - 비슷한 파일로는 .gitignore 파일이 있다.
    - example
        - 앵귤러 프로젝트에서 폴더를 생성한다.
        - <img src="./../assets/img/angular-trees/angular-add-folder.png">
        - git status 명령으로 트래킹을한다.
        - <img src="./../assets/img/angular-trees/angular-git-status-no-change.png">
        - 폴더 안에 .gitKeep 파일을 생성한다.
        - <img src="./../assets/img/angular-trees/angular-add-folder-gitkeep.png">
        - git status 명령으로 트래킹을 한다.
        - <img src="./../assets/img/angular-trees/angular-git-status-change.png">
2. 해당 프로젝트에서 필요한 이미지 파일 등이 들어간다.

### environments 폴더
### favicon.icon 파일
### index.html 파일
``` html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>MyApp</title>
  <base href="/">

  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
</head>
<body>
  <app-root></app-root>
</body>
</html>
```
싱글 애플리케이션이 가지고 있는 단하나의 index.html 파일이다. angular를 통해 만든 컴포넌트 들은 결국 `<app-root></app-root>`태그가 변회하여 보여진다.
### main.ts 파일
### polyfills.ts 파일
### styles.css 파일
``` css
/* You can add global styles to this file, and also import other style files */
```
해당 프로젝트의 글로벌 스타일 시트 파일이다. 다른 라이브러리의 스타일 시트를 import 하거나 자주 쓰이는 스타일들이 정의되는 파일이다.
### test.ts 파일
### tsconfig.app.json 파일
### tsconfig.spec.json 파일
### typing.d.ts 파일

