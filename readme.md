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

# 2022-09-22
- html에서 글자 띄어쓰기는 &nbsp;

# 2022-09-29
1. sass 도입
2. state로 collapse 기능 만들기
- console.log()를 해보면 값이 바뀌었는데, 화면에는 바뀌지가 않는다..
=> 바뀌지 않은게 아니라 반영이 안된것, 다른 페이지 들어갔다 오면 바뀌어져 있음
*** state가 array라서 copy를 잘못해서 그런것!
*** state변경 함수는 state 값이 기존값과 동일하면 변경되지 않는다.
```js
  let [show, setShow] = useState([false, false]);
  
<tr
onClick={() => {
  let copy = [...show]; 
//array, object는 reference data type => copy해서 쓰자!
// 이거를 let copy = show;라고해서 array값이 아니라 array의 참조값이 복사된것!
  copy[0] = !copy[0];
  setShow(copy);
}}
>
  
```

# 2022-09-30
- 레이아웃 디자인 구상이 오래걸려서 이러다 마크업만 하다 지칠 것 같아 잠시 마이페이지는 멈추고 로그인 기능을 만들어야겠다.

- export default
1) 코딩앙마
```js
- export default function 작명() {
    return ();
}
```

2) 코딩애플
```js
function 작명() {
    return ();
}
export default 작명;
```

3) export vs export default
- export 내보낼수있는게 여러개인 경우
- export default 1개만 있는경우

# 2022-10-01

1. 개념
- 회원인증방법 3가지
1) 세션 베이스드 어쎈디케이션(session-based)
- 어떤 사람이 로그인 함 → 서버는 메모리에 저장하고 쿠키(브라우저에 몰래 저장할수 있는 긴 문자열(세션아이디))를 발행함 → 로그인 후 사용자가 마이페이지 보여달라고 요청(쿠키데이터 전송, 자동으로 몰래 보내짐) → 서버는 로그인했는지 판단(세션데이터에서 아이디를 갖고 찾음) → 마이페이지를 유저에게 보내줌
- 자바스크립트 라이브러리 들이 알아서 해줌, 흐름만 이해

- 서버메모리에 로그인 상태를 저장

2) token-based(JWT)
- 로그인 시 서버에 전송 → 서버는 JSON웹 토큰(암호와된 긴 문자열)을 발행해줌 → 브라우저는 쿠키나 로컬스토리지 같은데 저장함 → 유저가 마이페이지 보여달라고 요청(웹토큰을 헤더에 포함시켜 전송) → 서버는 웹토큰이 유효기간 안지났는지 검증 후 마이페이지 보내줌

- 서버에 로그인 상태를 저장할 필요가 없음, 웹토큰(유통기한 있는 열쇠) 유효한지 아닌지 판단 => REST 원칙(stateless해야한다)

3) Open Authentication(OAuth)
- 구글의 프로필 정보를 가져옴

- 로그인 시 구글계정정보를 저쪽에 제공하는걸 동의? → 이메일, 이름, 성별 등 정보를 서버에 전달 → 서버에서 계정만들거나 세션만들거나 JWT발급하거나 등등..

- 서버는 비밀번호를 다룰 일이 없으나,,, 사이트가 없어지면 사용할수없음 ㅎ


2. 실습
- 로그인 기능 구현 관련 라이브러리 설치 후 import
- npm install passport passport-local express-session 3개설치
(로그인, 로그인 검증, 세션생성)
```js
const passport = require('passport');
const LocalStrategy = require('passport-local').Strategy;
const session = require('express-session');

app.use(session({secret : '비밀코드', resave : true, saveUninitialized: false}));
app.use(passport.initialize());
app.use(passport.session()); 
```

** 미들웨어 (app.use(어쩌구)) : 모든 요청과 응답 중간에 어쩌구가 실행됨.

serializeUser() 
- 유저의 id 데이터를 바탕으로 세션데이터를 만들어주고
- 그 세션데이터의 아이디를 쿠키로 만들어서 사용자의 브라우저로 보내줍니다. 

- react 에서 fontawesome 사용하기
1) SVG 코어 추가
 모든 유틸리티가 포함 된 핵심 패키지를 설치
https://fontawesome.com/v6/docs/web/use-with/react/
- npm i --save @fortawesome/fontawesome-svg-core

2) 아이콘 패키지 추가
- # Free icons styles
npm i --save @fortawesome/free-solid-svg-icons
npm i --save @fortawesome/free-regular-svg-icons

3) React 컴포넌트 추가
- npm i --save @fortawesome/react-fontawesome@latest

4) import 
- import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
- import { faUser } from '@fortawesome/free-solid-svg-icons';
** 주의 !!
```
// fontawesome 홈페이지에 react 버전
<FontAwesomeIcon icon="fa-solid fa-user" />

// 실제 사용시 카멜케이스로 변경 및 {} 이용!!
<FontAwesomeIcon icon={fa-user} />
```
5) form태그
- form 태그 중 button submit 사용시 버튼에 onClick시 axios요청 구문을 따로 쓰지 않아도 됨.
- 서버에 post 요청 시 구문 작성만 하면 됨.

6) -passport의 flash message  찾아보기

# 2022-10-02
- React 에서 form 태그르 쓰지 않는 이유: 새로고침 되기 때문 !! 잊지말자.
- ajax 요청으로 대체 : name태그를 사용하는게 아니라 onChange 시 e.target.value를 변수나 스테이트에 담아서 전송!

- JSX내에서는 if문 불가능. 삼항연산자는 가능!&&도 가능!
- Id, 닉네임을 db에서 중복된 값이 있는지 확인하고 state를 이용해 경고문 보여주기

# 2022-10-03
1. 회원가입 기능 보완하기
- 아이디, 비밀번호 등 입력시 공백 및 특수문자 등을 체크하는 기능을 추가하려고 한다.
- javascript 정규식이라는 것을 찾았다.

## 1. 정규표현식
```js
    const regex = /[^a-zA-Z0-9]/g; // 영문자제외 [^제외할문자] / g는 글로벌
    regex.test(user.id) // user.id는 onChange((e)=>{e.target.value})로 들어온 문자열 //제외시킨 특수문자 있으면 true
```

# 2022-10-04

- id, 닉네임, pw의 state를 한 state 변수에 담으니 헷갈려서 object 형으로 변경하였다.
- object 복사 시 { ... } 스프레드 문법 이용 // 깊은복사

- 문제1 : e.target.value의 값이 1타자씩 늦게 인식됨
-> 해결: useEffect((),[state])적용
useEffect()를 적용 해 user의 state가 들어온 뒤 글자수를 세었다. (그 전에는 1타자 늦게 입력되었다.)

- 문제2 :  처음 페이지 로드 시에는 경고문구 없으나, 커서 입력 후 또는 값 입력후에 값이 없으면 경고창을 띄우고싶다.
-> 난관: input 값의 길이가 0 일때도 경고문구가 나오게 만들어서, 초기 상태에도 경고창이 나왔다. 
-> 해결: 또 다른 state를 만들어 input태그에 포커스가 해지될때와(onBlur()) onChange시에 경고문구가 나오게 수정하였다.
문제를 해결하니 더 재미있다.
-> 계획: DOM 이벤트(마우스, 포커스 등등) 찾아보기

# 2022-10-08

1. 반응형레이아웃으로 수정
1) 320px화면에서 작성 // 아이폰5
2) min-width : 768px // 아이패드(세로)
3) min-width : 1024px // 아이패드(가로)
4) min-width : 1200px // PC


# 2022-10-12

- 회원가입 페이지 반응형으로 수정
- 디자인이 너무 오래걸린다. 부트스트랩을 쓸까..?

# 2022-10-13

- 불필요한 css 파일 삭제 및 중복 사용 css 통합
- 로그인시 로컬스토리지에 세션정보 저장
- 세션아이디를 저장하고 싶은데 db에서 찾은 아이디만 나온다..

# 2022-10-19

- server에서 redirect 시 페이지 이동이 안됨.
- react에서 useNavigate 사용.

# 2022-10-27
- 로그인시 세션정보를 state에 담아 로그인시에만 navbar에 마이페이지/로그아웃 버튼을 보여주고 싶었다.
- 모든 페이지에서 user가 확인될 수 있도록 redux를 활용하고자 함.

** redux 왜쓰는가?
- 모든 컴포넌트들이 store에 있는 state를 사용할 수 있음(공유)
- props안써도됨


1. redux 설치
- react와 react-dom이 18버전 이상일 것
- npm install @reduxjs/toolkit react-redux 으로 설치
- store.js파일 생성
```js
import { configureStore } from '@reduxjs/toolkit'

export default configureStore({
  reducer: { }
}) 
```

-index.js수정
```js
import { Provider } from "react-redux";
import store from './store.js'

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <Provider store={store}>
      <BrowserRouter>
        <App />
      </BrowserRouter>
    </Provider>
  </React.StrictMode>
); 
```
