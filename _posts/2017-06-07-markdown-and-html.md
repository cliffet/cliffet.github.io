---
layout: post
title: Jekyll & github를 이용한 블로그 준비 과정
category: dev
tags: [misc]
---

## blog 템플릿 fork

[type-theme](https://github.com/rohanchandra/type-theme) 프로젝트 fork  
그리고 fork된 github 프로젝트 Settings에서 프로젝트 이름을 [github계정명].github.io로 변경  

## 로컬 개발 머신
```
* fork된 프로젝트 clone  
* ruby 설치  
* gem install jekyll  
* bundle install  
```

## 로컬 테스트
 jekyll serve  
 http://localhost:4000  

## 포스팅 작업
 포스트 추가  
 로컬 테스트  
 commit & push (or sync)  
 
## bourbon 설치
 gem install bourbon  
 bourbon install --path _sass  
 
