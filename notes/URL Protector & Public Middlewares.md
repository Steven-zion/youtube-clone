- 로그인한 사용자만 `logout`, `edit` URL에 접근가능

  ```javascript
  export const protectorMiddleware = (req, res, next) => {
    if (req.session.loggedIn) {
      return next();
    } else {
      return res.redirect('/login');
    }
  };
  ```

  ```javascript
  userRouter.get('/logout', protectorMiddleware, logout);
  userRouter
    .route('/edit')
    .all(protectorMiddleware)
    .get(getEdit)
    .post(postEdit);
  ```

- 방문자만 `github`, `login`, `join` URL 접근가능

  ```javascript
  export const publicOnlyMiddleware = (req, res, next) => {
    if (!req.session.loggedIn) {
      return next();
    } else {
      return res.redirect('/');
    }
  };
  ```

  ```javascript
  userRouter.get('/github/start', publicOnlyMiddleware, startGithubLogin);
  userRouter.get('/github/finish', publicOnlyMiddleware, finishGithubLogin);

  rootRouter
    .route('/join')
    .all(publicOnlyMiddleware)
    .get(getJoin)
    .post(postJoin);
  rootRouter
    .route('/login')
    .all(publicOnlyMiddleware)
    .get(getLogin)
    .post(postLogin);
  ```
