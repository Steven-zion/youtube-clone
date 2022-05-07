https://docs.github.com/en/developers/apps/building-oauth-apps/authorizing-oauth-apps

# Settings

1. github.com/settings/apps 접속
2. OAuth Apps 클릭
3. Register a new application 클릭
   - Application name, Homepage URL, Authorization callback URL 기입
   - Register application 클릭해서 생성

# 로그인 과정

## flow

### Step 1.

    1-1. 사용자를 Github로 보낸다.
    1-2. 사용자는 Github로 이메일, 패스워드 등을 넣고 로그인한다.
    1-3. 그리고 우리에게 정보를 공유하는 것을 승인한다.

### Step 2.

    2-1. Github에서 사용자를 `code`과 함께 우리 웹사이트로 돌려보낸다.

### Step 3.

    3-1. 우리는 `access token`으로 Github API를 사용해 user 정보를 가져온다.

## flow settings

### Step 1. Request a user's GitHub identity

> GET https://github.com/login/oauth/authorize  
> 위의 URL과 필요한 Params을 넣어서 GET

#### login.pug

```javascript
    a(href="https://github.com/login/oauth/authorize?client_id={받은 client ID}") Continue with Github &rarr;
```

### Step 2. Users are redirected back to your site by GitHub

- Github에서 준 `code`를 `access token`으로 바꿔줘야한다.  
  (client_id, client_secret, code 값 필요)

> POST https://github.com/login/oauth/access_token  
> 위의 URL과 필요한 Params을 넣어서 POST  
> Params (client_id, client_secret, code)
>
> - client_secret: OAuth application settings에서 Client secret key 생성

### Step 3. Use the access token to access the API

> GET https://api.github.com/user
> Authorization: token {OAUTH-TOKEN}

# 고려사항

## 일반로그인 계정과 Github 계정이 동일하다면?

- DB에 동일한 email이 있다면 로그인 시켜줄 건지?
- 통합 관리할 것인가?
