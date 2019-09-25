# blog-source
깃헙IO 블로그 소소

# 설치 
```
> git init
> git clone https://github.com/letzgitit/blog-source.git
> npm install
```
# 서브 모듈 설치
  블로그 테마소스는 submodule로 관리하고 있으며 아래와 같이 수행해야 서버에서 데이터를 가져온다
``` 
> git submodule init
Submodule 'themes/blog-theme-next' (https://github.com/letzgitit/blog-theme-next) registered for path 'themes/blog-theme-next'
> git submodule update
Cloning into 'C:/workspace/blog/blog-source/themes/blog-theme-next'... 
```
