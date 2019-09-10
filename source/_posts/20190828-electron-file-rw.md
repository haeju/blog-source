---
title: Electron에서의 파일처리
categories:
  - Projects
  - Seed
tags:
  - Electron
  - fileRead
  - fileWrite
  - FS
  - nodeIntegration
  - vue  
date: 2019-08-28 14:10:36
thumbnail:
---

### ipc 통신
  electron에서의 파일처리는 node.js에서 제공하는 fs(file system) 모듈로 이용가능 하다.
  단, Renderer Process에서 사용하기 위해서는 아래와 같이 "nodeIntegration: true"로 설정되어야 한다. 
  바로 사용해도 무방하지만 fs(file system)은 가급적 MainProcess에서 처리하는걸로 하였으며 이에 ipc 통신을 통해 처리되도록 기능 추가.
<!-- more -->

### 이벤트 등록
  Main Process에서 src/main/ipc/fs.js 작성하면 Wrapper모듈을 ipc 이벤트로 등록.
{% asset_img post1.png 이벤트등록 %}


### prototype 지정
  Renderer Process에서 플러그인 src/renderer/plugins/ipc.js 내에 prototype지정.
{% asset_img post2.png prototype지정 %}


### 사용
  vue나 store에서 사용.
{% asset_img post3.png 사용 %}

- 파일 쓰기
  - this.$writeFile('파일경로', '데이타', ['옵션'])
  - 각 파라미터는 fs.writeFile와 동일
  - Promise로 리턴되므로 async await를 사용
- 파일 읽기
  - this.$readFile('파일경로', ['옵션'])
  - 각 파라미터는 fs.readFile와 동일
  - Promise로 리턴되므로 async await를 사용

### Related Posts
---
