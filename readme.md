# 2022-09-05

1. PWA React파일 생성
-  npx create-react-app my-app --template cra-template-pwa

2. React 실행(미리보기)
- npm start

3. Component 및 폴더 생성
- Navbar.js

4. 폰트, Color, 이미지 설정
- google fonts : Nanum Gothic Coding
- Color Tool : main #ffb300, light #ffe54c dark #c68400 
- background Image : https://unsplash.com/photos/BIk2ANMmNz4

5. React-Router-dom 설치
- npm install react-router-dom@6
- index.js 파일 수정 (BrouserRouter로 App감싸기)
```index.js
    <BrowserRouter>
      <App />
    </BrowserRouter>
```

# 2022-09-06

1. React-Router 중 Navigate()이용
```Navbar.js
import { useNavigate } from 'react-router-dom';

export default function Navbar(){
    let navigate = useNavigate();
    
    return (
        <div onClick={()=>{navigate('/경로')}}></div>
    )
}
```

2. CSS분할
- 페이지가 많아질수록 CSS가 많아져 관리하기 어려워
각 Component, Page별 CSS 파일을 만들고 각 js 파일에 import하여 사용하였다.

3. mypage 생성
- float, flex 이용하여 mypage 생성

4. modal창 생성
- 버튼을 누르면 다음 form으로 넘어가는 레이아웃을 만들고 있다.
- 버튼을 누를때마다 db에 전송하는 방법 말고 한번에 전송하는 방법을 고안 중
- 버튼을 누를때 화면만 바뀌는 방법을 고안 중 

# 2022-09-11

1. onClick, useState사용
- useState를 사용하여 modal창 띄우기

2. props 사용
- props 사용하여 useState 초기화 

3. 방생성 modal창 수정
- 모바일처럼 방이름/참여인원/참가날짜 등을 하나의 질문당 하나의 페이지로 넘기게 만들고 싶었지만, 구글 폼 양식을 확인해보니 섹션별로 나누어져 있었다.
한페이지에 정보를 모두 담는 방향으로 변경
UI 

4. server 생성
- 새 폴더 생성 후 vscode로 열기
- npm init 하여 package.json 파일 만들기
(폴더명 등 내용확인 및 yes를 눌러야 함, 한번에 하려면 npm init -y 입력)
- npm install express로 express라이브러리 설치
- server.js파일 생성

```server.js
const express = require('express');
const path = require('path');
const app = express();

app.listen(8080, function () {
  console.log('listening on 8080');
});
```

-nodemon server.js 으로 서버 미리보기 시작

5. react 프로젝트 와 연결
- npm run build 로 리액트 빌드하여 index.html 생성
- server.js파일
``` html 파일 연동
// 특정 폴더안의 파일 사용하기 위해 연결
app.use(express.static(path.join(__dirname, 'wonderfulday/build')));

app.get('/', function (요청, 응답) {
  응답.sendFile(path.join(__dirname, 'wonderfulday/build/index.html'));
});

// 리액트라우터 사용 시 연결 **가장 하단에 배치**
app.get('*', function (요청, 응답) {
  응답.sendFile(path.join(__dirname, '/wonderfulday/build/index.html'));
});

```

6. DB생성
- Mongodb 계정 로그인
- Mongodb 프로젝트 생성
- DB접속용 id/pw생성
- IP지정
- Browse Collections
- add my own data로 생성!

7. DB 연결 (ajax요청)
// 리액트와 nodejs간 ajax요청
// array, object를 json으로 변경
```server.js
app.use(express.json());
```
// 다른 도메인 주소끼리 ajax 요청 주고받을 때 사용
- npm install cors
```server.js
var cors = require('cors');
app.use(cors());
```

- mongodb 라이브러리 설치 : npm install mongodb 
```server.js
const MongoClient = require('mongodb').MongoClient;

var db;
MongoClient.connect('db접속url', { useUnifiedTopology: true }, function (에러, client) {
  if (에러) return console.log(에러);
  db = client.db('wonderfulday'); // database이름
  
// 서버띄우는 코드 여기로 이동
  app.listen(8080, function () {
    console.log('listening on 8080');
  });
});
```

**  db접속 url
- db에서 connect> connect your application
- 내가 설치한 node.js 버전과 동일한 버전 적용
```
접속 URL 복붙하실 때 mongodb+srv://디비계정아이디:디비계정패스워드@cluster0-qaxa3.mongodb.net/데이터베이스이름?retryWrites=true&w=majority
```

7. body-parser 사용
- form 태그 기능 개발 시 post요청으로 서버에 데이터 전송하고 싶으면
** 2021년 이후로 body-parser 라이브러리가 express에 포함되있어 추가로 npm 설치 필요없음
```server.js
app.use(express.urlencoded({extended: true})) 
```
8. form 태그 대신 ajax사용
=> form 은 새로고침 되기 때문!!
- axios 외부 라이브러리 설치
npm install axios

8. 서버 나누기
- 리액트 페이지가 업데이트 될때마다 build하는게 불편해서 서버를 나눴다.
- react는 3001, nodejs는 8001
- react의 package.json
```json
// 3001포트 사용
  "scripts": {
    "start": "set PORT=3001 && react-scripts start", 
    } 
// 8001서버와 연결    
  "proxy": "http://localhost:8001/"
```
- node.js
```js
    app.listen(8001, function () {
      console.log('listening on 8001');
    });
```

# 2022-09-13

1. map 사용시 html 코드 주의점
-map(()=>{
    여기에 바로 html 코드를 넣으면 안되고
    return (
        꼭 여기에 넣어야함 !!!
    )
})

** 해당 문제 때문인데 proxy 문제인 줄 알고 계속 해맸다..

# 2022-09-14

1. 테이블 태그 사용
- ul>li 로 작성했던 것들을 table태그를 활용하여 수정하였다.
- td:nth-child(n번째열선택) 와 rowspan(세로열병합) 적용

2. ajax, params 이용하여 db에서 불러오기
- null 이 나온다 왜그럴까...

# 2022-09-15

1. find, findOne ({ _id: 요청.params.id })
- _id 를 찾을 경우 null 이 나왔다
- 다른 property를 입력하면 정상 출력된다
- server.js에 ObejctId를 import 후 { _id: new ObjectId(요청.params.id) }로 형식을 맞춰주니 성공하였다.

2. D-day 계산
1) server.js에서 db에 생성시 new Date를 사용하여 Date 형식의 날짜를 db 입력할 수 있다. 
- 대신 axios로 react에 들어올땐 string으로 오기 때문에 new Date()로 바꿔줘야 한다.
- 2022-09-15 포맷으로 가져오고 싶을 경우  
```js
new Date(props.roomsummary.시작일)
                .toISOString()
                .substring(0, 10) //앞에서 10자리 가져오기
```

2) string으로 날짜 입력하는 경우
```js
function dDay(enddate) {
  const now = new Date();
  const today =
    now.getFullYear() +
    '-' +
    (now.getMonth() + 1 > 9 ? now.getMonth() + 1 : '0' + (now.getMonth() + 1)) +
    '-' +
    (now.getDate() > 9 ? now.getDate() : '0' + now.getDate());
  const startDate = new Date(`${today}T00:00:00+0900`); // T00:00:00+0900 한국시간
  const endDate = new Date(`${enddate}T00:00:00+0900`);
  const timeGap = endDate.getTime() - startDate.getTime();
  const dday = Math.floor(timeGap / (1000 * 60 * 60 * 24)); // ms * 초 * 분 * 시간 = 하루
  return dday;
```

3) new Date()
- new Date().getMonth() : 0부터 시작하므로 +1을 해준다
- getDate() : 날짜
- getDay() : 요일을 숫자로 나타낸 것, 일요일은 0 월요일은 1 

3. 반응형 웹페이지 (미디어쿼리)
- 나는 아이폰12미니, 아이패드 에어, 삼성노트북 15.6인치를 가지고 있다.
- 아이폰12미니는 아이폰6와 가로 길이가 거의 비슷하다.
- 크롬 개발자도구 기준 소형휴대폰 320px / 아이폰6 가로는 375px / 아이패드 에어는 820px / 일반 노트북은 1024px
- 부트스트랩에서 사용하는 분기점 중 난 768px / 992px / 1200px
=> 작성계획
- 크롬개발자도구에서 320px 화면 기준으로 만들기 (내 아이폰 화면 충분)
- 미디어 쿼리 분기점 1) 768px 이상이면 태블릿 사이즈로 키워주세요 (아이패드 에어 충분)
- 미디어 쿼리 분기점 2) 1024px 이상이면 PC사이즈로 키워주세요 (아이패드 가로 길이라고 한다)

2개의 분기점을 사용해 3가지 사이즈로 만들 계획


