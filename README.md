# **:zap: React-Electron 기본 템플릿**
![다운로드](https://user-images.githubusercontent.com/95972251/191018713-30dfef7d-fab7-406d-a7c5-a6c8ff72840f.png)

## :tada: npx create-react-app을 이용해 react-electron이라는 React 프로젝트를 만듭니다.

```bash
npx create-react-app react-electron
```
## :rocket: 프로젝트 폴더로 이동후 vscode(visual studio code)를 실행합니다.

```bash
cd react-electron code .
```

- vscode의 터미널을 열고 yarn start로 프로젝트를 실행하여 프로젝트가 잘 작동하는지 확인합니다.

```bash
yarn start
```
## :heavy_plus_sign: 프로젝트에 일렉트론 패키지를 추가합니다.

- electron 패키지는 개발시에만 사용하므로 --dev 옵션을 사용합니다.
```bash
yarn add --dev electron
```
- package.json에서 electron 관련 설정을 추가합니다.

```bash
"main": "src/main.js" --> 일렉트론의 entry point를 main.js로 설정
"electron": "electron ." --> 일렉트론 실행 스크립트
{ ... "main": "src/main.js", 
  ... "scripts": { 
       ... "electron": "electron ." 
       } 
  ... 
}
```

## :memo: src/main.js를 다음과 같이 만듭니다.

- 이때 리액트 화면을 일렉트론에서 보여주기 위해서, win.loadFile을 win.loadURL("http://localhost:3000")로 변경합니다.

```bash
const { app, BrowserWindow } = require('electron') 
const path = require('path') 
function createWindow () { 
  const win = new BrowserWindow({ 
    width: 800, 
    height: 600, 
    webPreferences: { 
      nodeIntegration: true,
      contextIsolation : false
    } 
  }) 
  win.loadURL("http://localhost:3000")
} 
app.whenReady().then(() => { 
  createWindow() 
}) 
app.on('window-all-closed', function () { 
  if (process.platform !== 'darwin') app.quit() 
})
```

## :heavy_plus_sign: Electron과 React를 같이 실행하기 위해 다음 패키지를 추가합니다.

- concurrently : 일렉트론과 리액트 프로세스를 동시에 실행하기 위해 사용합니다.
- wait-on : 프로세스 동시 수행시 한개의 프로세스가 완료되기를 기다리다 완료된 후 다음 프로세스를 수행하게 만들어 줍니다.

```bash
yarn add --dev concurrently wait-on
```

## :memo: package.json에 스크립트를 변경합니다.
- start 스크립트 수행시 concurrently를 이용해 react와 electron을 같이 시작하게 합니다.
- electron 스크립트는 react가 완료되길 기다린 후 실행하게 합니다.
```bash
"scripts": {
    "start": "concurrently \"yarn react-scripts start\" \"yarn electron\" ",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "electron": "wait-on http://localhost:3000 && electron ."
},
```

- 루트 폴더에 .env 파일을 만들고, 다음 내용을 추가합니다.
```bash
BROWSER=none
```

## **:paperclip: 출처**
- 출처 : https://codegear.tistory.com/36
