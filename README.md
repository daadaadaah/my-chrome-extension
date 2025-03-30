# Chrome Extension with Vite, React, and TypeScript

이 프로젝트는 Vite, React, TypeScript를 사용하여 Chrome 확장 프로그램을 개발하는 방법을 설명합니다.

## 목차

1. [프로젝트 설정](#프로젝트-설정)
2. [필요한 파일 구조](#필요한-파일-구조)
3. [개발 및 빌드](#개발-및-빌드)
4. [Chrome에 확장 프로그램 로드하기](#chrome에-확장-프로그램-로드하기)

## 프로젝트 설정

### 1. 프로젝트 폴더 생성 및 초기화

```bash
# 프로젝트 폴더 생성
mkdir my-chrome-extension
cd my-chrome-extension

# npm 초기화
npm init -y
```

### 2. 필요한 패키지 설치

```bash
npm install -D vite @vitejs/plugin-react @crxjs/vite-plugin react react-dom typescript @types/react @types/react-dom @types/chrome --legacy-peer-deps
```

### 3. TypeScript 설정 파일 생성

**tsconfig.json**
```json
{
  "compilerOptions": {
    "target": "ESNext",
    "useDefineForClassFields": true,
    "lib": ["DOM", "DOM.Iterable", "ESNext"],
    "allowJs": false,
    "skipLibCheck": true,
    "esModuleInterop": false,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "module": "ESNext",
    "moduleResolution": "Node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx"
  },
  "include": ["src"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

**tsconfig.node.json**
```json
{
  "compilerOptions": {
    "composite": true,
    "module": "ESNext",
    "moduleResolution": "Node",
    "allowSyntheticDefaultImports": true
  },
  "include": ["vite.config.ts"]
}
```

## 필요한 파일 구조

### 1. Vite 설정 파일

**vite.config.ts**
```typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import { crx } from '@crxjs/vite-plugin';
import manifest from './manifest.json';

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    react(),
    crx({ manifest }),
  ],
});
```

### 2. Chrome 확장 프로그램 Manifest 파일

**manifest.json**
```json
{
  "manifest_version": 3,
  "name": "My Chrome Extension",
  "version": "1.0.0",
  "description": "A Chrome extension built with Vite, React, and TypeScript",
  "action": {
    "default_popup": "index.html",
    "default_title": "My Chrome Extension"
  },
  "permissions": []
}
```

### 3. HTML 파일

**index.html**
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My Chrome Extension</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.tsx"></script>
  </body>
</html>
```

### 4. React 애플리케이션 파일

**src/main.tsx**
```tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import './index.css';

ReactDOM.createRoot(document.getElementById('root') as HTMLElement).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
);
```

**src/App.tsx**
```tsx
import React from 'react';

function App() {
  return (
    <div className="app">
      <h1>Hello World</h1>
    </div>
  );
}

export default App;
```

**src/index.css**
```css
body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.app {
  width: 300px;
  min-height: 200px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 20px;
  text-align: center;
}

h1 {
  color: #333;
}
```

### 5. package.json 스크립트 설정

package.json의 scripts 섹션을 다음과 같이 업데이트합니다:

```json
"scripts": {
  "dev": "vite",
  "build": "tsc && vite build",
  "preview": "vite preview"
}
```

## 개발 및 빌드

### 개발 서버 실행

```bash
npm run dev
```

### 확장 프로그램 빌드

```bash
npm run build
```

빌드가 완료되면 `dist` 디렉토리에 확장 프로그램 파일이 생성됩니다.

## Chrome에 확장 프로그램 로드하기
1. 아래 명령어를 통해 빌드가 완료되면 `dist` 디렉토리에 확장 프로그램 파일이 생성됩니다.
```bash
npm run build
```

2. Chrome 브라우저를 열고 주소 표시줄에 다음을 입력합니다: `chrome://extensions/`

3. 페이지의 오른쪽 상단에 있는 "개발자 모드" 토글 스위치를 켜고, 왼쪽 상단에 나타나는 "압축해제된 확장 프로그램을 로드합니다" 버튼을 클릭한 후 파일 탐색기에서 프로젝트의 `dist` 폴더를 선택하면, 확장 프로그램이 Chrome에 로드되고 확장 프로그램 목록에 나타납니다.

<img width="647" alt="Image" src="https://github.com/user-attachments/assets/39b36056-e95d-4482-81a8-bce418d1d447" />

4. Chrome 브라우저의 오른쪽 상단에 있는 확장 프로그램 아이콘을 클릭하고, 방금 추가한 "My Chrome Extension"을 클릭하면 팝업이 열리고 "Hello World" 메시지가 표시됩니다.

<img width="647" alt="Image" src="https://github.com/user-attachments/assets/b5e09c9a-825d-479b-8e47-02fe19f5133b" />


## 확장 프로그램 개발 및 업데이트

코드를 수정한 후에는 다시 빌드하고, Chrome 확장 프로그램 페이지에서 새로고침 버튼을 클릭하여 업데이트된 버전을 로드해야 합니다.

```bash
npm run build
```

이후 Chrome의 확장 프로그램 페이지에서 해당 확장 프로그램의 새로고침 아이콘을 클릭하여 업데이트합니다.