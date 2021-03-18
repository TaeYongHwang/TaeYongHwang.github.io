---
title: "Express.js 개요"
date: 2020-10-31
toc: true
toc_sticky: true
categories: Tool
---

# Express.js 프레임워크
- Node.js 상에서 구동되는 javascript 프레임워크
- MVC 디자인 패턴을 사용한다.

```
├── app.js # 실행 주체 
├── package.json # NPM 패키지 관리
├── public # css 등 저장
├── models # Model에 대응
├── routes # Controller에 대응
└── views  # View에 대응
```

## MVC (Model-View-Controller) 디자인 패턴
- Controller -> Model -> View 순서로 사용자 요청이 처리된다.
- 각 분류별로 모아서 관리
- Controller : 유저의 진입점(URI)으로, 어떤 일을 처리할지에 대한 분기
- Model: Controller에서 요청한 job에 대한 실제 처리 로직
- View: 처리가 끝난 후 유저가 보게되는 화면 (모델과의 의존성이 존재함)

## Test
- jest, supertest 등으로 테스트 진행 가능 (git pre-commit hook 이용)
- postman으로 테스트 진행해 jenkins에 넣어 원격 서버에서 pull 당길 때마다 테스트 진행 가능

## 사용 시 불편했던 점
- **Dynamically typed language**(javascript) 사용으로 유지, 보수가 힘들었다.
- npm 라이브러리 다운 시, typing이 안돼있는 라이브리의 경우 해석이 더 오래걸렸다. (jsDoc를 잘 작성해놔야 할듯)



> Reference   
> [Express 공식 사이트](https://expressjs.com/)   
> [DOCS](http://expressjs.com/en/4x/api.html)