# `connect-mongo` 사용해서 세션을 MongoDB에 저장하는 방법

## 1. connect-mongo 설치

```
npm i connect-mongo
```

## 2. connect-mongo 사용

```javascript
import mongoStore from 'connect-mongo';

// Mongo database의 URL을 가지고 있는 configuration object를 만들어야한다.

app.use(
  session({
    secret: 'Hello!',
    resave: false,
    saveUninitialized: false, // 세션을 수정할 때만 세션을 DB에 저장하고 쿠키를 넘겨주는 것
    store: MongoStore.create({ mongoUrl: 'mongodb://127.0.0.1:27017/wetube' }),
  })
);
```

- saveUninitialized: true
  세션을 즉시 Store에 저장
- saveUninitialized: false
  세션을 수정할 때만 세션을 DB에 저장하고 쿠키를 넘겨주게 되는 것.  
   `userController`에서 session을 수정하는 부분이 session을 초기화하는 부분으로 초기화하는 순간에 session을 Store에 저장하고 쿠키를 전달한다.  
  즉, backend가 로그인한 사용자에게만 쿠키를 주도록 설정되는 것이다.

```javascript
  // session 초기화
  req.session.loggedIn = true;
  req.session.user = user;`
```

- mongoDB에서 session collections 이 생긴 것을 확인 할 수 있다.

  > show collections  
  > db.sessions.find()

  session정보가 MongoDB에 저장되어 있기 때문에 재시작해도 세션 정보를 가지고 있다!
