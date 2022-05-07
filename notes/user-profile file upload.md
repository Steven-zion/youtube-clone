# file upload

## Multer middleware

- 파일 업로드를 도와주는 미들웨어

### Install

```
npm i multer
```

### Usage

1. ✅ form을 multipart로 해줘야 한다.

   ```pug
   form(enctype = 'multipart/form-data');
   ```

2. middleware 생성
   사용자가 보낸 파일을 uploads폴더에 저장하도록 설정하는 미들웨어.

   ```javascript
   export const avatarUpload = multer({
     dest: 'uploads/avatars/',
     limits: {
       fileSize: 3000000,
     },
   });

   export const videoUpload = multer({
     dest: 'uploads/videos/',
     limits: {
       fileSize: 10000000,
     },
   });
   ```

3. middleware 사용
   multer middleware가 input으로 avatar파일을 받아서 파일을 uploads폴더에 저장한 다음, 그 파일정보를 postEdit에 전달해준다.

   ```javascript
   // user profile - avatar
   userRouter
     .route('/edit')
     .all(protectorMiddleware)
     .get(getEdit)
     .post(avatarUpload.single('avatar'), postEdit);

   // video
   videoRouter
     .route('/upload')
     .all(protectorMiddleware)
     .get(getUpload)
     .post(videoUpload.single('video'), postUpload);
   ```

   이렇게 하면 `avatar` file이 req.file에 들어감.

4. User 모델의 avatarUrl 업데이트 해주기

   - **DB에 파일을 저장하지 않고, 파일의 위치를 저장한다!!**

5. uploads 폴더를 브라우저가 볼 수 있도록 설정
   - static failes serving : 폴더 전체를 브라우저에 노출
   ```javascript
   // server.js 에 다음과 같이 추가
   app.use('/uploads', express.static('uploads'));
   ```

## 파일을 서버에 저장하면 좋지않음. 서버가 죽을 수도 있으니 다른 장소에 저장해야한다.
