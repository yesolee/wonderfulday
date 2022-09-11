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
