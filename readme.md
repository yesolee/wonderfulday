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
