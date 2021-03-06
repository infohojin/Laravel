---
layout: laravel
title: Laravel
subtitle: Prologue
    
---

Compiling Assets (Laravel Mix)
Assets 컴파일 하기 (라라벨 Mix)
Introduction
소개하기
Installation & Setup
설치하기 & 설정하기
Running Mix
Mix 실행하기
Working With Stylesheets
스타일시트 작업하기
Less
Less
Sass
Sass
Stylus
Stylus
PostCSS
PostCSS
Plain CSS
일반적인 CSS
URL Processing
Source Maps
소스 맵
Working With JavaScript
자바스크립트 작업하기
Vendor Extraction
Vendor 분할
React
React
Vanilla JS
Vanilla JS
Custom Webpack Configuration
커스텀 Webpack 설정
Copying Files & Directories
파일 & 디렉토리 복사
Versioning / Cache Busting
버전 관리 / 캐시 갱신
Browsersync Reloading
Browsersync 리로딩
Environment Variables
환경변수
Notifications
Notifications-알림

Introduction

## 소개하기
Laravel Mix provides a fluent API for defining Webpack build steps for your Laravel application using several common CSS and JavaScript pre-processors. Through simple method chaining, you can fluently define your asset pipeline. For example:

라라벨 Mix(믹스)는 여러분의 라라벨 애플리케이션에 몇가지 공통적인 CSS 및 자바스크립트 전처리를 위한 Webpack 빌드를 위한 편리한 API를 제공합니다. 간단한 메소드 체이닝을 통해서, 간편하게 asset 파이프라인을 정의할 수 있습니다. 예를 들어:

```php
mix.js('resources/assets/js/app.js', 'public/js')
   .sass('resources/assets/sass/app.scss', 'public/css');
```

If you've ever been confused and overwhelmed about getting started with Webpack and asset compilation, you will love Laravel Mix. However, you are not required to use it while developing your application. Of course, you are free to use any asset pipeline tool you wish, or even none at all.

만약 여러분이 Webpack 과 asset 컴파일에서 혼란스럽고 부담을 느끼고 있다면, 라라벨 Mix 를 좋아하게 될 것입니다. 하지만 애플리케이션을 개발할 때 라라벨 Mix가 꼭 필요한 건 아닙니다. 당연하게도, 원하는 그 어떤 asset pipeline 툴을 사용해도, 또 사용하지 않아도 괜찮습니다.


Installation & Setup
## 설치하기 & 설정하기
Installing Node
Node 설치하기
Before triggering Mix, you must first ensure that Node.js and NPM are installed on your machine.

Mix를 사용하기전 여러분의 작업환경에 Node.js와 NPM이 설치되어있는지 확인하십시오.

```
node -v
npm -v
```

By default, Laravel Homestead includes everything you need; however, if you aren't using Vagrant, then you can easily install the latest version of Node and NPM using simple graphical installers from their download page.

기본적으로 라라벨의 홈스테드는 여러분이 필요로하는 모든것을 포함하고 있습니다. 하지만 Vagrant를 사용하지 않으신다면 Node.js 다운로드 페이지에서 간단한 화면 기반의 인스톨러를 사용하여 Node 와 NPM의 최신 버전을 손쉽게 설치할 수 있습니다.

Laravel Mix
## 라라벨 Mix
The only remaining step is to install Laravel Mix. Within a fresh installation of Laravel, you'll find a package.json file in the root of your directory structure. The default package.json file includes everything you need to get started. Think of this like your composer.json file, except it defines Node dependencies instead of PHP. You may install the dependencies it references by running:

이제 남은 과정은 라라벨 Mix를 설치하는 것 뿐입니다. 라라벨을 새롭게 설치하고나면 디렉토리 구조의 루트 폴더에 위치한 package.json 파일을 볼 수 있습니다. 이 기본적인 package.json 파일은 여러분이 필요한 모든것들이 들어 있습니다. 이 파일은 PHP 대신 Node 의존 패키지를 정의 한다는것을 빼고는 여러분의 composer.json과 동일합니다. 아래의 명령어로 의존 패키지들을 설치할 수 있습니다.

```
npm install
```

Running Mix
##Mix 실행하기
Mix is a configuration layer on top of Webpack, so to run your Mix tasks you only need to execute one of the NPM scripts that is included with the default Laravel package.json file:

Mix 는 Webpack을 기반으로 하는 설정 레이어로, Mix 작업을 실행하려면 라라벨에 기본적으로 포함되어 있는 package.json 파일과 함께 NPM 스크립트를 실행하기만 하면 됩니다:

```
// Run all Mix tasks...
npm run dev

// Run all Mix tasks and minify output...
npm run production
```

Watching Assets For Changes
## Assets의 변경사항 감시하기
The npm run watch command will continue running in your terminal and watch all relevant files for changes. Webpack will then automatically recompile your assets when it detects a change:

npm run watch 명령어는 터미널에서 계속 실행되면서 모든 관련된 파일의 변경사항을 감시합니다. Webpack은 변경사항이 감지되면 자동으로 이를 다시 컴파일 합니다:

npm run watch
You may find that in certain environments Webpack isn't updating when your files change. If this is the case on your system, consider using the watch-poll command:

구동환경에 따라서 파일의 변경사항을 발생해도 Webpack이 업데이트 되지 않게 할 수도 있습니다. 사용하는 시스템이 이 경우라면, watch-poll 명령어 사용을 고려하십시오:

npm run watch-poll

Working With Stylesheets
## 스타일시트 작업하기
The webpack.mix.js file is your entry point for all asset compilation. Think of it as a light configuration wrapper around Webpack. Mix tasks can be chained together to define exactly how your assets should be compiled.

webpack.mix.js 파일은 모든 asset 컴파일에 관한 내용을 담고 있습니다. 이 파일은 Webpack을 랩핑한 가벼운 설정이라고 볼 수 있습니다. Mix 작업은 assets 들이 어떻게 컴파일하는지 체이닝 형태로 정의되어 있습니다.


Less
Less
The less method may be used to compile Less into CSS. Let's compile our primary app.less file to public/css/app.css.

less 메소드는 Less를 CSS로 컴파일하는데 사용됩니다. app.less파일을public/css/app.css` 로 컴파일 해보겠습니다.

mix.less('resources/assets/less/app.less', 'public/css');
Multiple calls to the less method may be used to compile multiple files:

여러개의 파일을 컴파일 하기 위해서 less 메소드를 여러번 호출할 수도 있습니다:

mix.less('resources/assets/less/app.less', 'public/css')
   .less('resources/assets/less/admin.less', 'public/css');
If you wish to customize the file name of the compiled CSS, you may pass a full file path as the second argument to the less method:

컴파일된 CSS 파일의 이름을 변경하고 싶다면, less 메소드의 두번째 인자로 전체 경로를 전달하면 됩니다:

mix.less('resources/assets/less/app.less', 'public/stylesheets/styles.css');
If you need to override the underlying Less plug-in options, you may pass an object as the third argument to mix.less():

기본적인 Less 플러그인 옵션을 오버라이드 하려면, mix.less() 메소드의 세번째 인자로 이를 전달할 수 있습니다:

mix.less('resources/assets/less/app.less', 'public/css', {
    strictMath: true
});

Sass
Sass
The sass method allows you to compile Sass into CSS. You may use the method like so:

sass 메소드는 Sass파일을 CSS 파일로 컴파일해줍니다. 이 메소드는 다음과 같이 사용할 수 있습니다:

mix.sass('resources/assets/sass/app.scss', 'public/css');
Again, like the less method, you may compile multiple Sass files into their own respective CSS files and even customize the output directory of the resulting CSS:

less 메소드처럼, 여러개의 Sass 파일을 대응하는 CSS파일로 컴파일 할 수 있으며, CSS 결과 파일의 위치를 커스터마이징 할 수 있습니다:

mix.sass('resources/assets/sass/app.sass', 'public/css')
   .sass('resources/assets/sass/admin.sass', 'public/css/admin');
Additional Node-Sass plug-in options may be provided as the third argument:

추가적인 Node-Sass 플러그인 옵션은 세번째 인자로 전달하면 됩니다:

mix.sass('resources/assets/sass/app.sass', 'public/css', {
    precision: 5
});

Stylus
## Stylus
Similar to Less and Sass, the stylus method allows you to compile Stylus into CSS:

Less 와 Sass 의 경우와 비슷하게 stylus 메소드는 Stylus를 CSS로 컴파일 하는데 사용합니다:

mix.stylus('resources/assets/stylus/app.styl', 'public/css');
You may also install additional Stylus plug-ins, such as Rupture. First, install the plug-in in question through NPM (npm install rupture) and then require it in your call to mix.stylus():

Rupture와 같은 추가적인 Stylus 플러그인을 설치할 수도 있습니다. 먼저 (npm install rupture)을 통해서 필요한 플러그인을 설치한다음, mix.stylus() 메소드에서 이를 require 하십시오:

mix.stylus('resources/assets/stylus/app.styl', 'public/css', {
    use: [
        require('rupture')()
    ]
});

PostCSS
## PostCSS
PostCSS, a powerful tool for transforming your CSS, is included with Laravel Mix out of the box. By default, Mix leverages the popular Autoprefixer plug-in to automatically apply all necessary CSS3 vendor prefixes. However, you're free to add any additional plug-ins that are appropriate for your application. First, install the desired plug-in through NPM and then reference it in your webpack.mix.js file:

PostCSS는 추가적인 설치 없이도, 라라벨 Mix에 기본적으로 포함되어 사용할 수 있는 CSS 변환툴입니다. 기본적으로 Mix는 널리 사용되는 Autoprefixer 플러그인을 사용하여 필요한 모든 CSS3 벤더 prefix를 자동으로 적용합니다. 애플리케이션에 적합한 플러그인을 추가할 수도 있습니다. 먼저 NPM을 통해서 사용하고자 하는 플러그인을 설치한다음 webpack.mix.js 파일에서 참조할 수 있도록 하십시오.

mix.sass('resources/assets/sass/app.scss', 'public/css')
   .options({
        postCss: [
            require('postcss-css-variables')()
        ]
   });

Plain CSS
## 일반적인 CSS
If you would just like to concatenate some plain CSS stylesheets into a single file, you may use the styles method.

일반적인 CSS 스타일시트 파일들을 하나의 파일로 연결해서 붙이려면 styles 메소드를 사용하면 됩니다.

mix.styles([
    'public/css/vendor/normalize.css',
    'public/css/vendor/videojs.css'
], 'public/css/all.css');

URL Processing
## URL 프로세싱
Because Laravel Mix is built on top of Webpack, it's important to understand a few Webpack concepts. For CSS compilation, Webpack will rewrite and optimize any url() calls within your stylesheets. While this might initially sound strange, it's an incredibly powerful piece of functionality. Imagine that we want to compile Sass that includes a relative URL to an image:

라라벨 Mix는 Webpack을 기반으로 하여 구성되었기 때문에, Webpack의 개념을 이해하고 있는 것이 중요합니다. CSS 컴파일에서 Webpack은 스타일 시트 안에서 url()호출을 재작성하고 최적화합니다. 처음에는 이상해보일수도 있지만, 이것은 매우 강력한 기능입니다. 특정 이미지의 상대경로를 포함하고 있는 Sass를 컴파일하려고 한다고 생각해보겠습니다:

.example {
    background: url('../images/example.png');
}
{note} Absolute paths for any given url() will be excluded from URL-rewriting. For example, url('/images/thing.png') or url('http://example.com/images/thing.png') won't be modified.

{note} url()에 주어진 절대 경로들은 URL-재작성에서 제외됩니다. 예를 들어, url('/images/thing.png') 와 url('http://example.com/images/thing.png') 는 수정되지 않습니다.

By default, Laravel Mix and Webpack will find example.png, copy it to your public/images folder, and then rewrite the url() within your generated stylesheet. As such, your compiled CSS will be:

기본적으로, 라라벨 Mix와 Webpack은 example.png 파일을 찾아 이를 public/images 폴더에 복사하고, 생성된 스타일 시트 안에서 url()을 재작성합니다. 그 결과, 컴파일된 CSS는 다음과 같습니다:

.example {
  background: url(/images/example.png?d41d8cd98f00b204e9800998ecf8427e);
}
As useful as this feature may be, it's possible that your existing folder structure is already configured in a way you like. If this is the case, you may disable url() rewriting like so:

이 기능이 유용할 수 있지만, 이미 기존폴더가 존재할 수도 있습니다. 이런경우에는 url()의 재작성 동작을 다음처럼 비활성화 할 수 있습니다:

mix.sass('resources/assets/app/app.scss', 'public/css')
   .options({
      processCssUrls: false
   });
With this addition to your webpack.mix.js file, Mix will no longer match any url() or copy assets to your public directory. In other words, the compiled CSS will look just like how you originally typed it:

webpack.mix.js파일에 재작성 동작을 비활성화하도록 추가하면, Mix는 더이상 url()을 위한 asset을 public 디렉토리로 복사하지 않습니다. 다시말해서 컴파일된 CSS는 원래 입력한것과 같이 보여집니다:

.example {
    background: url("../images/thing.png");
}

Source Maps
## 소스 맵
Though disabled by default, source maps may be activated by calling the mix.sourceMaps() method in your webpack.mix.js file. Though it comes with a compile/performance cost, this will provide extra debugging information to your browser's developer tools when using compiled assets.

기본적으로는 비활성화 되어 있지만, webpack.mix.js 파일에서 mix.sourceMaps()을 호출하여 소스 맵을 활성화 시킬 수 있습니다. 컴파일하는데 성능상의 비용이 들지만, 컴파일 된 asset을 사용하면 브라우저의 개발자 도구에서 추가적인 디버깅 정보를 확인할 수 있습니다.

mix.js('resources/assets/js/app.js', 'public/js')
   .sourceMaps();

Working With JavaScript
## 자바스크립트 작업하기
Mix provides several features to help you work with your JavaScript files, such as compiling ECMAScript 2015, module bundling, minification, and concatenating plain JavaScript files. Even better, this all works seamlessly, without requiring an ounce of custom configuration:

Mix는 자바스크립트 작업을 하는데 도움이 될만한 몇가지 기능을 제공하는데, ECMAScript 2015 컴파일, 모듈 번들링, minification, 그리고 자바스크립트 파일 concatenating-연결등입니다. 더 나아가 이 모든 기능은 여러가지 설정을 할 필요없이 원활하게 동작합니다:

mix.js('resources/assets/js/app.js', 'public/js');
With this single line of code, you may now take advantage of:

이 한줄의 코드로 다음의 기능들을 취할 수 있습니다:

ES2015 syntax.
Modules
Compilation of .vue files.
Minification for production environments.

Vendor Extraction
Vendor 분할
One potential downside to bundling all application-specific JavaScript with your vendor libraries is that it makes long-term caching more difficult. For example, a single update to your application code will force the browser to re-download all of your vendor libraries even if they haven't changed.

모든 어플리케이셔녈 자바스크립트를 벤더 라이브러리와 함께 번들링 하는 것은 캐시를 하는데 있어서 잠재적으로 불리한 점입니다. 예를 들어 애플리케이션 코드를 한번 업데이트 하면 브라우저는 벤더 라이브러리가 변경되지 않았더라도, 전부다 다시 다운로드 받아야 합니다.

If you intend to make frequent updates to your application's JavaScript, you should consider extracting all of your vendor libraries into their own file. This way, a change to your application code will not affect the caching of your large vendor.js file. Mix's extract method makes this a breeze:

애플리케이션의 자바스크립트를 자주 업데이트 한다면, 벤더 라이브러리를 별도로 구성하는 것을 고려해야 합니다 이렇게 하면 애플리케이션의 코드가 변경되더라도 vendor.js 파일의 캐싱에는 영향을 주지 않습니다. Mix의 extract 메소드는 다음과 같이 처리합니다:

mix.js('resources/assets/js/app.js', 'public/js')
   .extract(['vue'])
The extract method accepts an array of all libraries or modules that you wish to extract into a vendor.js file. Using the above snippet as an example, Mix will generate the following files:

extract 메소드는 vendor.js 파일로 별도 구성(추출)하고자 하는 라이브러리 혹은 모듈의 배열을 전달 받습니다. 위의 예제를 살펴보자면 Mix는 다음과 같은 파일을 생성합니다:

public/js/manifest.js: The Webpack manifest runtime
public/js/vendor.js: Your vendor libraries
public/js/app.js: Your application code
To avoid JavaScript errors, be sure to load these files in the proper order:

자바 스크립트 오류를 ​​방지하려면 적절한 순서로 다음 파일들을 로드해야합니다:

<script src="/js/manifest.js"></script>
<script src="/js/vendor.js"></script>
<script src="/js/app.js"></script>

React
## React
Mix can automatically install the Babel plug-ins necessary for React support. To get started, replace your mix.js() call with mix.react():

Mix는 React 지원이 필요한경우 자동으로 Babel 플러그인을 설치합니다. 이렇게 하기 위해서는 mix.js() 호출을 mix.react()으로 변경하십시오:

mix.react('resources/assets/js/app.jsx', 'public/js');
Behind the scenes, Mix will download and include the appropriate babel-preset-react Babel plug-in.

이렇게 하면 백그라운드에서 Mix는 babel-preset-react Babel 플러그인을 다운로드 해서 인클루드 합니다.


Vanilla JS
Vanilla JS
Similar to combining stylesheets with mix.styles(), you may also combine and minify any number of JavaScript files with the scripts() method:

스타일시트 파일들을 mix.styles()를 통해서 합치는 것과 비슷하게, scripts() 메소드를 사용하여 바닐라 자바스크립트 파일들도 하나로 합치고 minify를 적용할 수 있습니다:

mix.scripts([
    'public/js/admin.js',
    'public/js/dashboard.js'
], 'public/js/all.js');
This option is particularly useful for legacy projects where you don't require Webpack compilation for your JavaScript.

이 옵션은 자바스크립트를 위한 Webpack 컴파일이 필요 없는 레거시 프로젝트에 특별히 유용합니다.

{tip} A slight variation of mix.scripts() is mix.babel(). Its method signature is identical to scripts; however, the concatenated file will receive Babel compilation, which translates any ES2015 code to vanilla JavaScript that all browsers will understand.

{tip} mix.babel()은 mix.scripts()에서 약간의 변형입니다. 메소드 사용법은 scripts 와 동일하지만 하나로 연결된 결과 파일의 내용은 ES2015코드를 모든 브라우저에서 해석이 가능한 바닐라 자바스크립트로 코드를 변환하는 Babel 컴파일 결과물이 됩니다.


Custom Webpack Configuration
커스텀 Webpack 설정
Behind the scenes, Laravel Mix references a pre-configured webpack.config.js file to get you up and running as quickly as possible. Occasionally, you may need to manually modify this file. You might have a special loader or plug-in that needs to be referenced, or maybe you prefer to use Stylus instead of Sass. In such instances, you have two choices:

애플리케이션의 뒤에서 라라벨 Mix 는 미리 설정된 webpack.config.js 파일을 참조하여 가능한 빠르게 실행되도록 합니다. 경우에 따라서 이 파일을 직접 수정해야 할 수도 있습니다. 참조해야할 특정 로더 또는 플러그인이 있거나 아니면 Sass 대신 Stylus를 사용할 수 있습니다. 이러한 경우라면 두가지 선택 사항이 있습니다:

Merging Custom Configuration
커스텀 설정 Merging(병합)-합치기
Mix provides a useful webpackConfig method that allows you to merge any short Webpack configuration overrides. This is a particularly appealing choice, as it doesn't require you to copy and maintain your own copy of the webpack.config.js file. The webpackConfig method accepts an object, which should contain any Webpack-specific configuration that you wish to apply.

Mix는 간한한 Webpack설정을 오버라딩해서 병합하는데 사용할 수 있는 webpackConfig 메소드를 제공합니다. 이 경우 별도의 webpack.config.js 파일을 복사하여 유지할 필요가 없어서 매력적인 선택입니다. webpackConfig 메소드는 적용하려는 Webpack 설정을 포함하는 객체를 전달받습니다.

mix.webpackConfig({
    resolve: {
        modules: [
            path.resolve(__dirname, 'vendor/laravel/spark/resources/assets/js')
        ]
    }
});
Custom Configuration Files
커스텀 설정 파일
If you would like to completely customize your Webpack configuration, copy the node_modules/laravel-mix/setup/webpack.config.js file to your project's root directory. Next, point all of the --config references in your package.json file to the newly copied configuration file. If you choose to take this approach to customization, any future upstream updates to Mix's webpack.config.js must be manually merged into your customized file.

Webpack 설정을 완전히 커스텀하게 지정하려면 node_modules/laravel-mix/setup/webpack.config.js 파일을 프로젝트의 루트 디렉토리로 복사하십시오. 그다음에 package.json 파일의 --config 참조를 새로 복사한 설정 파일로 지정하도록 합니다. 이 경우에는 추후 업데이트되는 Mix 의 webpack.config.js 내역을 수동으로 커스텀 파일에 병합해야 합니다.


Copying Files & Directories
파일 & 디렉토리 복사하기
The copy method may be used to copy files and directories to new locations. This can be useful when a particular asset within your node_modules directory needs to be relocated to your public folder.

copy 메소드는 파일들과 디렉토리를 새로운 위치에 복사하는데 사용됩니다. 이는 node_modules 디렉토리 내의 특정 asset을 public 폴더로 재배치해야 할 때 유용합니다.

mix.copy('node_modules/foo/bar.css', 'public/css/bar.css');
When copying a directory, the copy method will flatten the directory's structure. To maintain the directory's original structure, you should use the copyDirectory method instead:

디렉토리를 복사할 때, copy 메소드는 디렉토리 하위 구조를 하나의 형태로 구성합니다(flat 하게 만듭니다). 대신에 디렉토리의 원래 구조를 유지하려면 copyDirectory 메소드를 사용하십시오:

mix.copyDirectory('assets/img', 'public/img');

Versioning / Cache Busting
버전 관리 / 캐시 갱신
Many developers suffix their compiled assets with a timestamp or unique token to force browsers to load the fresh assets instead of serving stale copies of the code. Mix can handle this for you using the version method.

많은 개발자들이 기본 코드 대신 새로운 assets 을 강제로 로드하게끔 하기 위해서 컴파일된 assets 에 timestamp 또는 고유한 토큰을 접미사로 추가합니다. Mix는 이러한 문제를 처리하기 위해 version 메소드를 사용합니다.

The version method will automatically append a unique hash to the filenames of all compiled files, allowing for more convenient cache busting:

version 메소드는 자동으로 컴파일된 파일이름 뒤에 고유한 hash 를 덧붙여, 편리하게 캐시를 날릴 수 있도록 합니다:

mix.js('resources/assets/js/app.js', 'public/js')
   .version();
After generating the versioned file, you won't know the exact file name. So, you should use Laravel's global mix function within your views to load the appropriately hashed asset. The mix function will automatically determine the current name of the hashed file:

버전이 지정된 파일이 생성되면, 여러분은 정확한 파일 이름을 알 수가 없습니다. 따라서 뷰-views에서 라라벨의 글로벌 mix 헬퍼를 사용하여 해시값이 붙은 asset 을 로딩할 수 있습니다. mix 함수는 자동으로 해시값이 붙어 있는 현재의 파일이름을 결정합니다:

<link rel="stylesheet" href="{{ mix('/css/app.css') }}">
Because versioned files are usually unnecessary in development, you may instruct the versioning process to only run during npm run production:

개발중에는 버저닝된 파일이 항상 필요하지는 않기 때문에, npm run production 일때만 버저닝 프로세스가 동작하도록 지시할 수 있습니다:

mix.js('resources/assets/js/app.js', 'public/js');

if (mix.inProduction()) {
    mix.version();
}

Browsersync Reloading
Browsersync 리로딩
BrowserSync can automatically monitor your files for changes, and inject your changes into the browser without requiring a manual refresh. You may enable support by calling the mix.browserSync() method:

BrowserSync는 파일의 변경사항을 감시하고 있다가, 수동으로 페이지를 다시 로드하지 않아도 자동으로 변경 사항을 브라우저에 반영합니다. mix.browserSync() 메소드를 호출하여 이 지원사항을 활성화 시킬 수 있습니다:

mix.browserSync('my-domain.test');

// Or...

// https://browsersync.io/docs/options
mix.browserSync({
    proxy: 'my-domain.test'
});
You may pass either a string (proxy) or object (BrowserSync settings) to this method. Next, start Webpack's dev server using the npm run watch command. Now, when you modify a script or PHP file, watch as the browser instantly refreshes the page to reflect your changes.

이 메소드에는 (프록시) 또는 (BrowserSync 설정)등을 전달할 수도 있습니다. 그런 다음, npm run watch 명령을 사용하여 Webpack의 dev 서버를 시작하십시오. 이제 스크립트나 PHP 파일을 수정하게되면 브라우저가 즉시 페이지를 새로 고침하여 변경 사항을 반영합니다.


Environment Variables
환경 변수
You may inject environment variables into Mix by prefixing a key in your .env file with MIX_:

.env 파일에 MIX_ 로 시작하는 키를 사용하면 환경변수를 Mix에 지정할 수 있습니다:

MIX_SENTRY_DSN_PUBLIC=http://example.com
After the variable has been defined in your .env file, you may access via the process.env object. If the value changes while you are running a watch task, you will need to restart the task:

.env 파일에 변수를 정의한 후에는, process.env 객체를 통해서 이 값을 엑세스 할 수 있습니다. watch 작업 중에는 이 값을 변경한다면, 재시작을 필요로 합니다:

process.env.MIX_SENTRY_DSN_PUBLIC

Notifications
Notification-알림
When available, Mix will automatically display OS notifications for each bundle. This will give you instant feedback, as to whether the compilation was successful or not. However, there may be instances when you'd prefer to disable these notifications. One such example might be triggering Mix on your production server. Notifications may be deactivated, via the disableNotifications method.

가능한 경우에, Mix 는 자동으로 번들링을 마칠 때 OS 알림을 표시합니다. 이렇게 하면 컴파일이 성공했는지, 그렇지 않은지 즉각적인 피드백을 얻을 수 있습니다. 만약 이러한 알림을 비활성화 시키고자 할 수도 있는데, 프로덕션 서버에서 Mix를 사용할 때가 그렇습니다. disableNotifications 메소드를 사용하면 알림 기능을 비활성화 시킬 수 있습니다.

mix.disableNotifications();