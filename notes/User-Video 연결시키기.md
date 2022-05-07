# User - Video model 연결시키기

## 1. Video Ownder 추가

- `mongoose.Schema.Types.ObjectId` : ObjectId 타입은 mongoose에서 가져온다.
- `ref: User` : `objectId`가 User model의 objectId를 가져오는 것.

```javascript
const videoSchema = new mongoose.Schema({
  // ... 생략
  owner: { type: mongoose.Schema.Types.ObjectId, required: true, ref: 'User' },
});
```

## 2. Video upload 할 때 사용자의 Id 전송

```javascript
const {
  user: { _id },
} = req.session;

await Video.create({
  title,
  description,
  fileUrl,
  owner: _id,
  hashtags: Video.formatHashtags(hashtags),
});
```

## 3. Video watch 화면에서 Edit/Delete Video 링크를 해당 owner에게만 보여주도록 수정

- `loggedInUser._id === video.owner`

## 4. populate - Video의 owner로 User데이터 가져오기

```javascript
// video.owner 에 ObjectId가 출력되는 것이 아니라
// user 정보가 출력된다.!!!!

const video = await Video.findById(id).populate('owner');
console.log(video);
```

#### Output

```javascript
{
  meta: { views: 0, rating: 0 },
  hashtags: [ '#bunny' ],
  _id: 6111d26720b6ed6e52754234,
  title: 'Bunny',
  description: 'Bunny is eating carrots!~~~',
  fileUrl: 'uploads/videos/20cb2960c0d52772dc1ed2c74e446feb',
  owner: {
    socialOnly: true,
    _id: 6111d17ec159606b0d940222,
    name: 'jieun lee',
    avatarUrl: 'https://avatars.githubusercontent.com/u/39111959?v=4',
    username: 'eazisilver',
    email: 'jieunlee.me@gmail.com',
    password: '$2b$05$VtKtYXIaSE.Wai5OJFxldeT5OnrKMMQfSnJq4i/6Dg3uAP0wld/wu',
    location: 'South Korea',
    __v: 0
  },
  createdAt: 2021-08-10T01:12:07.825Z,
  __v: 0
}
```
