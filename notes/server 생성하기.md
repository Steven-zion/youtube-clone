```javascript
import express from 'express';
import morgan from 'morgan';

const PORT = 4000;

// 1. app 생성
const app = express();

// 2. app 설정
const home = (req, res) => {
  console.log('I will respond');
  return res.send('Hello');
};

const login = (req, res) => {
  return res.send('login');
};

app.use(morgan('dev'));
app.get('/', home);
app.get('/login', login);

// 3. app 외부로 개방
const handleListening = () =>
  console.log(`✅Server listening on port http://localhost:${PORT} 🚓`);

// 서버가 4000 포트를 listening 하고 있다.
app.listen(PORT, handleListening);
```
