# JinyERP v1
`지니ERP`는 라라벨 기반의 업무처리 프레임워크 입니다. 다양한 업무 기능들을 모듈화 하여 개별로 설치 사용을 할 수 있습니다.


## 설치하기
지니ERP는 `PHP`언어 기반의 라라벨을 기본 바탕으로 두고 있습니다. 또한, 데이터베이스는 Mysql 또는 마리아DB를 사용합니다.
UI 빌드를 위하여 nodejs와 tailwind css 를 필요로 합니다.
* php 8.0 이상
* composer
* MariaDB
* nodejs

### 1. 라라벨 설치
컴포저를 이용하여 라라벨 프레임워크를 설치합니다. 다음과 같이 콘솔에서 명령을 입력하시면 라라벨이 설치됩니다.
```
$ composer create-project --prefer-dist laravel/laravel 프로젝트명
```

#### 2. 모듈 관리자 설치
`지니ERP`는 작은 개별 모듈들로 구성되어 동작을 합니다. 각각의 모듈을 설치관리하기 위한 관리자를 설치합니다.

```
$ composer require jiny/modules
$ php artisan vendor:publish --provider="Jiny\Modules\JinyModulesServiceProvider"
$ php artisan module:init
```

## UI 컴파일하기
`지니ERP`의 기본 UI는 [tailwind css](https://tailwindcss.com/docs/guides/laravel)를 이용합니다. 이를 위하여 nodejs를 통하여 tailwind를 설치하고, scss를 컴파일하여 설치를 합니다.

npm을 통하여 테일윈드 패키지를 설지합니다.
```
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init
```

라라벨 mix를 통하여 asset을 컴파일 합니다. 다음과 같이 webpack.mix.js를 수정합니다. 파일명 및 위치 : /webpack.mix.js
```
mix.js("resources/js/app.js", "public/js")
  .postCss("resources/css/app.css", "public/css", [
    require("tailwindcss"),
  ]);
.browserSync("http://localhost:8000");
```

tailwind.config.js 파일을 수정합니다. content 배열을 추가합니다.

```
module.exports = {
  content: [
    "./resources/**/*.blade.php",
    "./resources/**/*.js",
    "./resources/**/*.vue",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

/resources/css/app.css 파일을 수정합니다.

```
@tailwind base;
@tailwind components;
@tailwind utilities;
```
이제 npm을 통하여 설정한 tailwind를 컴파일하여 적용합니다.

```
npm run dev
```

## 실행

```
php artisan serve
```
