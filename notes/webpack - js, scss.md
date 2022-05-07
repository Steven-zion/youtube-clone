# install

```
npm i webpack webpack-cli -D
```

# Usage

### 1. `webpack.config.js` 파일 생성

- 이 파일은 굉장히 오래된 javascrpit코드만 이해할 수 있다.

### 2. `webpack.config.js` 필수 설정

1.  `entry`

    - 우리가 처리하고자 하는 파일들(구버전으로 변경하고자 하는 코드)
    - `entry : "./src/client"` : 서버가 아닌 브라우저에서만 동작할 파일들

2.  `output`

    - 변경한 파일의 이름과 그 파일을 저장할 경로
    - `filename: 'main.js'`
    - `path: 'path.resolve(__dirname, 'assets', 'js')'` => 절대경로

3.  `rules`

    #### **javascript**

    - 각각의 파일에 종류에 따라 어떤 파일로 전환할 것인지를 명시
    - `npm i -D babel-loader` 설치

    #### **scss**

    - scss 를 css로 변환
    - `npm i sass-loader sass webpack--save-dev` 설치
    - `npm i css-loader --save-dev` 설치
    - `npm i style-loader --save-dev` 설치
    - `use: ['style-loader', 'css-loader','sass-loader'],` 역순으로 기입
      (loader는 적힌 반대 순서로 작동하기 때문)
    - client/js/main.js 파일에 scss import
      ```javascript
      import '../scss/styles.scss';
      ```

    #### `mini-css-extract-plugin` css plugIn사용하기

    css를 추출해서 다른 파일로 분리시키는 플러그인
    이것을 사용하면 style-loader 설치할 필요X

    - install
      ```javascript
      npm i --save-dev mini-css-extract-plugin
      ```
    - usage

      ```javascript
      // webpack.config.js
      const MiniCssExtractPlugin = require('mini-css-extract-plugin');

      module.exports = {
        //...
        plugins: [new MiniCssExtractPlugin()],

        //...rules...
        use: [MiniCssExtractPlugin.loader, 'css-loader', 'sass-loader'],
      };
      ```

4.  mode 설정

    - 실행시키면 WARNING - mode option has not been set 경고가 뜬다.
    - 개발 중이면 development, 완성된 결과물을 원한다면 production
    - `mode : 'development'`

### 3. webpack 실행

- package.json 에서 스크립트 생성

  ```
   "assets" : "webpack --config webpack.config.js"
  ```

### 4. `assets/js/main.js` 에 접근할 수 있도록 설정

- static failes serving : 폴더 전체를 브라우저에 노출

```javascript
// server.js 에 다음과 같이 추가
app.use('/assets', express.static('assets'));
// app.use('/static', express.static('assets')); 이렇게 바꿔줘도 상관없다. 브라우저가 접근하는 경로일뿐
```

### 5. assest 폴더의 js, css를 `base.pug` 파일과 연결

5-1. `assets/js/main.js`

```javascript
// base.pug 하단에 추가
script((src = '/assets/js/main.js'));
```

5-2 `assets/css/styles.css`

```javascript
// base.pug 다음과 같이 추가
link((rel = 'stylesheet'), (href = '/assets/css/styles.css'));
```

### 정리

> src/client/js/main.js 에 최신의 세련된 코드를 작성.  
> 이 세련된 코드는 assets로 못생긴 JS코드로 변환되어 들어가고,  
> pug는 assets 폴더에서부터 파일들을 불러온다.

### 더 나은 개발환경 설정하기

- watch : true  
  refresh와 compile을 다시해준다.

  ```javascript
  // webpack.config.js
  watch: true,
  ```

- clean : true  
   output folder를 build를 시작하기 전에 clean해준다.

  ```javascript
  // webpack.config.js
  output: {
    clean: true,
  },
  ```
