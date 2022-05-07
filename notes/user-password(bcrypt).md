# `bcrypt` 사용해서 비밀번호 해싱하는 방법.

## 1. bcrypt 설치

```
npm i bcrypt
```

## 2. bcrypt 사용해서 비밀번호 해싱

회원가입 화면에서 새로운 User를 생성 하기 전에 비밀번호 해싱처리

- user.save() 호출할 때 마다 pre() 호출됨.  
  따라서, 불필요하게 비밀번호가 hashing 될 수 있으므로 방지할 수 있도록 처리해야함.

### models/User.js

```javascript
import bcrypt from 'bcrypt';

userSchema.pre('save', async function () {
  // this.isModified() : User(this)의 property가 수정되면 true
  if (this.isModified('password')) {
    this.password = await bcrypt.hash(this.password, 5);
  }
});
```

## 3. bcrypt 사용해서 입력받은 비밀번호 확인

로그인 할 때 비밀번호 확인

### controllers/userController.js

```javascript
import bcrypt from 'bcrypt';

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
  console.log('LOG USER IN! COMING SOON!🐵');
  return res.redirect('/');
};
```
