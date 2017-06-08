---
layout: post
title: Jekyll & github를 이용한 블로그 준비 과정
category: dev
tags: [misc]
---

## github pages 생성하기
깃허브 페이지 생성과정은 [여기](https://help.github.com/categories/customizing-github-pages/)를 참조

## 로컬 테스트
```sh
jekyll serve  
```
 http://localhost:4000  

## 포스팅 작업
 포스트 추가  
 로컬 테스트  
 commit & push (or sync)  
 
## 부록
### 태그 기능 추가하기
bourbon 설치  
```sh
gem install bourbon  
bourbon install --path _sass  
``` 
[tag](http://codinfox.github.io/blog/)


### jekyll minima theme
[github](https://github.com/jekyll/minima)
[demo](https://jekyll.github.io/inima/)
[jekyll](https://jekyllrb.com/)

### github markdown cheatsheet
<https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet>