# `express-session` 사용해서 세션 처리하는 방법.

## 1. express-session 설치

```
npm i express-session
```

## 2. express-session 사용

```javascript
import session from 'express-session';

app.use(
  session({
    secret: 'Hello!',
    resave: true,
    saveUninitialized: true,
  })
);
```

### session 미들웨어의 역할:

브라우저가 우리의 backend와 상호작용 할 때마다 session이라는 미들웨어가 브라우저에 cookie를 전송.  
(cookie는 bakend가 너의 브라우저에 주는 정보-여기서는 sessionID)  
매번 backend에 request할 때, 브라우저가 알아서 request에 cookie정보를 덧붙여준다.

✅ session 확인하기

- sessionStore: session을 저장하는 곳. 서버가 재시작되면 sessionStore 내용 사라짐.

```javascript
app.use((req, res, next) => {
  req.sessionStore.all((error, sessions) => {
    console.log(sessions);
    next();
  });
});
```

Output

```plaint
[Object: null prototype] {
  _A0aS64OqQdfa1XjchDA7mefRU6w2rsA: {
    cookie: { originalMaxAge: null, expires: null, httpOnly: true, path: '/' }
  }
}
```

만약 2개의 브라우저(chrome, brave)에서 백엔드에 접근하면(현재 localhost:4000) Sesison은 2개가 출력된다.

## 3. 로그인 시 Session 정보를 저장

### sample

```javascript
// session에 정보 추가

req.session.loggedIn = true;
req.session.user = user;
```

### controllers/userController.js

```javascript
export const postLogin = async (req, res) => {
  const { username, password } = req.body;
  const pageTitle = 'Login';
  // user check
  const user = await User.findOne({ username });
  if (!user) {
    return res.status(400).render('login', {
      pageTitle,
      errorMessage: 'An account with this username dose not exists.',
    });
  }

  // password check
  const isVaildPassword = await bcrypt.compare(password, user.password);
  if (!isVaildPassword) {
    return res.status(400).render('login', {
      pageTitle,
      errorMessage: 'Wrong password',
    });
  }

  // session에 정보 추가
  req.session.loggedIn = true;
  req.session.user = user;

  return res.redirect('/');
};
```

## 4-1. base.pug 템플릿에서 Session 정보를 알 수 있는 방법

`req.session.loggedIn` 에 접근해서 로그인 됐는지를 알고 싶을 때

### 💩 Error Code

```pug
if !req.session.loggedIn
    li
        a(href="/join") Join
    li
        a(href="/login") Login
```

### Correct Code

```javascript
req.locals.sexy = 'you!';
req.locals.siteName = 'Wetube';
```

```pug
h1 Who is sexy? #{sexy}
```

pug, Express는 서로 `req.locals`를 공유하고 있어 프로퍼티 이름만으로 접근가능하다.
req.locals object는 pug template에서 전역으로 접근가능(global)

## 4-2. base.pug 템플릿에서 로그인 여부 확인하기

1. 로그인 성공하면
   req.session에 다음과 같이 로그인정보 추가

```javascript
req.session.loggedIn = true;
req.session.user = user;
```

2. res.locals object 에 req.session 정보 넣기

```javascript
export const localsMiddleware = (req, res, next) => {
  res.locals.loggedIn = Boolean(req.session.loggedIn);
  res.locals.siteName = 'Wetube';
  res.locals.loggedInUser = req.session.user;
  next();
};
```

3. pug template에서 사용하기

```pug
  if loggedIn
      li
          a(href="/logout") Log out
      li
          a(href="/my-profile") #{loggedInUser.name}의 Profile
  else
      li
          a(href="/join") Join
      li
          a(href="/login") Login
```
