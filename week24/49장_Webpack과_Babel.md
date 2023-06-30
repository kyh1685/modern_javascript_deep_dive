# 49장 Webpack과 Babel

🔖 오늘 읽은 범위 : 900 ~ 917p

---

### 😃 책에서 정리하고 싶은 내용을 써보세요

ES6와 ES.NEXT의 최신 ECMAScript 사양을 사용하여 프로젝트를 진행하려면 최신 사양으로 작성된 코드를 구형 브라우저에서 잘 동작시키기 위한 개발 환경을 구축하는 것이 필요하다.

또한 대부분의 프로젝트가 모듈을 사용하므로 모듈 로더로 필요하다. ES6 모듈은 대부분의 모던 브라우저에서 사용할 수 있지만 아직까지는 별도의 모듈 로더를 사용하는 것이 일반적이다.

## 49.1 Babel

Babel은 최신 사양의 소스코드를 구형 브라우저에서도 동작하는 소스코드로 변환(트랜스파일링)할 수 있다.

### 49.1.1 Babel 설치

```jsx
npm install ---save-dev @babel/core @babel/cli
```

### 49.1.2 Babel 프리셋 설치와 babel.config.json 설정 파일 작성

Babel을 사용하려면 @babel/preset-env를 설치해야 한다. @babel/preset-env는 함께 사용돼야 하는 Babel 플러그인을 모아둔 것으로 Babel 프리셋이라고 부른다.

@babel/preset-env는 필요한 플러그인들을 프로젝트 지원 환경에 맞춰 동적으로 결정해준다. 만약 프로젝트 지원 환경 설정 작업을 생략하면 기본값으로 설정된다.

```jsx
npm install ---save-dev @babel/preset-env
```

@babel/preset-env까지 설치되면 프로젝트 루트 폴더에 babel.config.json 설정 파일을 생성하고 아래와 같이 작성한다. 설치한 @babel/preset-env를 사용하겠다는 의미이다.

```jsx
{
  "presets": ["@babel/preset-env"]
}
```

### 49.1.3 트랜스파일링

Babel을 사용하여 소스코드를 트랜스파일링 하기 위해 package.json에 script를 추가한다.

```jsx
"scripts": {
    "build": "babel src/js -w -d dist/js"
}
```

위 명령어는 scr/js 폴더(타깃 폴더)에 있는 모든 자바스크립트 파일들을 트랜스파일링한 후, 결과물을 dist/js 폴더에 저장한다.

<aside>
💡 **사용한 옵션의 의미**

- -w : 타깃 폴더에 있는 모든 자바스크립트 파일들의 변경을 감지하여 자동으로 트랜스파일링한다.(—watch 옵션의 축약형)
- -d : 트랜스파일링된 결과물이 저장될 폴더를 지정한다. 만약 지정된 폴더가 존재하지 않으면 자동 생성한다.(—out-dir 옵션의 축약형)
</aside>

### 49.1.4 Babel 플러그인 설치

설치가 필요한 Babel 플러그인은 Babel 홈페이지에서 검색할 수 있다. 만약 현재 제안 단계에 있는 사양을 트랜스파일링하기위해 플러그인을 설치해야 한다면, 검색창에 제안 사양의 이름을 입력하면 해당 플러그인을 검색하고 설치할 수 있다. 

```jsx
{
  "presets": ["@babel/preset-env"],
  "plugins": ["@babel/plugin-proposal-class-properties"]
}
```

설치한 플러그인은 위와 같이 babel.config.json 설정 파일에 추가해야 한다.

### 49.1.5 브라우저에서 모듈 로딩 테스트

## 49.2 Webpack

Webpack은 의존 관계에 있는 자바스크립트, CSS, 이미지 등의 리소스들을 하나(또는 여러개)의 파일로 번들링하는 모듈 번들러다.

Webpack을 사용하면 의존 모듈이 하나의 파일로 번들링되므로 별도의 모듈 로더가 필요없다. 그리고 여러 개의 자바스크립트 파일을 하나로 번들링하므로 HTML 파일에서 script 태그로 여러 개의 자바스크립트 파일을 로드해야 하는 번거로움도 사라진다.

### 49.2.1 Wabpack 설치

```jsx
npm install --save-dev webpack webpack-cli
```

### 49.2.2 babel-loader 설치

Webpack이 모듈을 번들링할 때 Babel을 사용하여 트랜스파일링하도록 babel-loader를 설치한다.

```jsx
npm install --save-dev babel-loader
```

package.json script에서 Babel을 사용하는 대신 Webpack을 실행하도록 수정한다.

```jsx
"scripts": {
    "build": "webpack -w"
}
```

### 49.2.3 webpack.config.js 설정 파일 작성

webpack.config.js는 Webpack이 실행될 때 참조하는 설정 파일이다. 프로젝트 루트 폴더에 webpack.config.js 파일을 생성하고 아래와 같이 작성한다.

```jsx
const path = require('path');

module.exports = {
  // entry file
  // https://webpack.js.org/configuration/entry-context/#entry
  entry: './src/js/main.js',
  // 번들링된 js 파일의 이름(filename)과 저장될 경로(path)를 지정
  // https://webpack.js.org/configuration/output/#outputpath
  // https://webpack.js.org/configuration/output/#outputfilename
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'js/bundle.js'
  },
  // https://webpack.js.org/configuration/module
  module: {
    rules: [
      {
        test: /\.js$/,
        include: [
          path.resolve(__dirname, 'src/js')
        ],
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env'],
            plugins: ['@babel/plugin-proposal-class-properties']
          }
        }
      }
    ]
  },
  devtool: 'source-map',
  // https://webpack.js.org/configuration/mode
  mode: 'development'
};
```

이제 Webpack을 실행하면 트랜스파일링은 Babel이 수행하고 번들링은 Webpack이 수행한다. 

Webpack을 실행한 결과로 dist/js 폴더에 bundle.js가 생성된다. 이 파일은 main.js, lib.js 모듈이 하나로 번들링된 결과물이다. index.html을 아래와 같이 수정하고 브라우저에 실행해보면 정상적으로 동작하는 걸 확인할 수 있다.

```jsx
<!DOCTYPE html>
<html>
<body>
  <script src="./dist/js/bundle.js"></script>
</body>
</html>
```

### 49.2.4 babel-polyfill 설치

Babel을 사용하여 트랜스파일링해도 브라우저가 지원하지 않는 코드가 남아 있을 수 있다. 예를 들어, ES6에서 추가된 Promise, Object.assign, Array.from은 ES5 사양으로 트랜스파일링해도 대체할 기능이 없기 때문에 트랜스파일링되지 못하고 그대로 남는다. 이러한 객체나 메서드를 사용하기 위해서는 babel-polyfill을 설치해야 한다.

```jsx
npm install @babel/polyfill
```

babel-polyfill은 개발 환경에서만 사용하는 것이 아니라 실제 운영 환경에서도 사용해야 한다. 따라서 개발용 의존성(devDependencies)으로 설치하는 —save-dev 옵션을 지정하지 않는다.

ES6의 import를 사용하는 경우에는 진입점의 선두에서 먼저 폴리필을 로드하도록 한다.

```jsx
// src/js/main.js
import "@babel/polyfill";
import { pi, power, Foo } from './lib';
...
```

Wabpack을 사용한다면 위 방법 대신 webpack.config.js 파일의 entry 배열에 폴리필을 추가한다.

```jsx
const path = require('path');

module.exports = {
  // entry file
  // https://webpack.js.org/configuration/entry-context/#entry
  entry: ['@babel/polyfill', './src/js/main.js'],
...
```
