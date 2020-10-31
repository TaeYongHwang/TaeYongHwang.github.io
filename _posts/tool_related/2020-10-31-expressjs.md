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



> [Express 공식 사이트](https://expressjs.com/)