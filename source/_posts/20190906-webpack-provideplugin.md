---
title: webpack-provideplugin
categories:
  - front-end
  - electron
  - module-bundlers
tags:
  - Module-Bundlers
date: 2019-09-06 10:59:29
thumbnail:
---
### 자동 모듈 로드
  - 일렉트론에서 lodash를 매번 import해서 사용하게 되었는데 자동으로 모듈이 로드되도록 개선 필요
<!--more-->



### ProvidePlugin
  - webpack 설정으로 해결
    - 경로 : .electron-vue/webpack.renderer.config.js
  - plugins 항목중 new webpack.ProvidePlugin(...) 내 객체로 추가하면 된다.
  ```js
    plugins: [
      ...
      new webpack.ProvidePlugin({
        '_': 'lodash'
      })
      ... 
    ],
  ```



### Related Posts
 - https://webpack.js.org/plugins/provide-plugin/
---
