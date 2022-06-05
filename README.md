# JinyERP version 1.0
지니ERP는 다양한 업무기능들을 모듈화 하여 설치 사용할 수 있습니다.

## 설치하기
지니ERP는 PHP언어와 라라벨 프레임워크 기반으로 제작되었습니다. 따라서, PHP 언어와 컴포저, 마리아DB가 설치되어 있어야 합니다. 

* php 8.0 이상
* composer
* MariaDB
* nodejs

### 1. 라라벨 설치
컴포저를 이용하여 라라벨 프레임워크를 설치합니다. 다음과 같이 콘솔에서 명령을 입력하시면 라라벨이 설치됩니다.
```
$ composer create-project --prefer-dist laravel/laravel jinyerp
```

#### TailwindCSS 설정
지니ERP는 UI를 구성하기 위하여 tailwindCSS를 기본으로 사용합니다. 이를 위하여 간단환 설정이 필요합니다.
> 라라벨 [테일윈드](https://tailwindcss.com/docs/guides/laravel) 설치

npm을 통하여 테일윈드 패키지를 설지합니다.
```
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init
```
> npm을 사용하기 위해서는 nodejs가 설치되어 있어야 합니다.

라라벨 mix를 통하여 asset을 컴파일 합니다. 다음과 같이 webpack.mix.js를 수정합니다. 파일명 및 위치 : /webpack.mix.js
```
mix.js("resources/js/app.js", "public/js")
  .postCss("resources/css/app.css", "public/css", [
    require("tailwindcss"),
  ]);
.browserSync("http://localhost:8000");
tailwind.config.js 파일을 수정합니다. content 배열을 추가합니다.

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

#### 라이브와이어 설치
지니ERP는 최신 라이브와이어를 통하여 UI 컴포넌트 구성과 AJAX 통신을 처리합니다. 다음과 같이 명령을 입력하여 라이브와이어 컴포넌트를 설치합니다. 
```
$ composer require livewire/livewire
```

#### 라라벨 모듈
지니ERP는 각각의 기능을 모듈로 관리합니다.

```
$ composer require jiny/modules
$ php artisan vendor:publish --provider="Nwidart\Modules\LaravelModulesServiceProvider"
```
config/module.php 설정파일 변경
오토로드 변경
```
{
  "autoload": {
    "psr-4": {
      "App\\": "app/",
      "Modules\\": "Modules/",
      "Database\\Factories\\": "database/factories/",
      "Database\\Seeders\\": "database/seeders/"
  }

}
```

```
$ composer dump-autoload
```

### 3. 지니UI 설치
