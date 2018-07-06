---
layout: laravel
title: Laravel
subtitle: Prologue
cate:
    name: "프롤로그"
    link: "/Laravel/Prologue"

submenus:
    -
        name: 프롤로그
        link: /Laravel/Prologue
    -
        name: 릴리즈노트
        link: /Laravel/Prologue/release
    -
        name: 업그레이트
        link: /Laravel/Prologue/upgrade
    -
        name: 공여
        link: /Laravel/Prologue/contribute
    -
        name: API
        link: https://laravel.com/api/5.6/
    
---

## 업그레이드 가이드
---
* 5.5에서 5.6.0 으로 업그레이드 하기

<br>

## 5.5에서 5.6.0 으로 업그레이드 하기
---

업그레이드 예상 시간 : 약 10-30분
{note} 가능한 모든 변경 내용을 기록하려고 했습니만, 변경 사항 중 일부는 프레임워크의 명확하지 않은 부분에서 이루어 지기 때문에 이중 일부가 실제 애플리케이션에 영향을 끼칠 수도 있습니다.

<br>

### PHP
---
라라벨 5.6은 PHP 7.1.3 이상을 필요로합니다.

<br>

### 의존성 업데이트
---
composer.json 파일에 있는 laravel/framework 의존성을 5.6.*으로 변경하고, fideloper/proxy 의존성을 ^4.0 으로 업데이트 합니다.

또한, 다양한 라라벨 추가 패키지들을 사용하고 있다면, 최신버전으로 업그레이드 해야 합니다:

Dusk (^3.0으로 업그레이드)
Passport (^5.0 으로 업그레이드)
Scout (^4.0 으로 업그레이드)
또한, 애플리케이션에서 사용하는 써드파티 패키지를 확인하고 라라벨 5.6를 지원하는 적절한 버전을 사용하고 있는지 확인하십시오.

<br>

### Symfony 4
---

라라벨에서 사용되는 모든 Symfony 컴포넌트가 ^4.0 리리즈 시리즈로 업그레이드되었습니다. 애플리케이션에서 Symfony 컴포넌트를 직접 다루는 경우 Symfony 변경 사항을 확인해야 합니다.

<br>

### PHPUnit
---

phpunit/phpunit의존성을 ^7.0 으로 업데이트 해야 합니다.

<br>

### 배열
---

Arr::wrap 메소드
null을 반환하던 Arr::wrap 메소드는 이제 빈 배열을 반환합니다.

<br>

## Artisan 명령어
---

optimize 명령어
이전 버전에서 deprecated 되었던 optimize 아티즌 명령어가 이제 제거되었습니다. OPCache를 포함한 PHP 자체의 최근의 개선으로 인해서 optimize 명령어는 더 이상 관련 성능상의 이점을 제공하지 않습니다. 따라서 따라서 composer.json 파일에서 scripts 옵션에서 php artisan optimize 를 제거 할 수 있습니다.

<br>

## Blade
---

HTML Entity 인코딩
이전 버전의 라라벨에서는, 블레이드 템플릿(그리고 e 헬퍼 메소드에서는) HTML 엔티티를 이중으로 인코딩 하지 않았습니다. 이는 htmlspecialchars 함수의 기본 동작이 아니었으며, 콘텐츠를 렌더링 할 때나, 자바스크립트 프레임워크에 JSON 데이터를 전달할 때 예기치 않은 동작을 일으킬 수 있습니다.

라라벨 5.6에서는 블레이드 템플릿과 e 헬퍼는 기본적으로 특수 문자를 이중으로 인코딩 합니다. 이로 인해서 이 기능의 기본 동작이 htmlspecialchars 함수 동작과 달라집니다. 이중 인코딩을 원하지 않고 이전의 동작을 유지하려면, Blade::withoutDoubleEncoding 메소드를 사용하면 됩니다:

```php
<?php

namespace App\Providers;

use Illuminate\Support\Facades\Blade;
use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Bootstrap any application services.
     *
     * @return void
     */
    public function boot()
    {
        Blade::withoutDoubleEncoding();
    }
}
```

<br>

## 캐시
---

tooManyAttempts 메소드를 사용한 속도 제한
사용되지 않는 $decayMinutes 파라미터가 메소드에서 제거되었습니다. 캐시를 직접 구현한 경우, 이 메소드의 오버라이딩 하였다면, 메소드 선언 부분에서 파라미터 선언부분을 제거해야합니다.

<br>

## 데이터베이스
---

Index Order Of Morph Columns
morphs 마이그레이션 메소드를 통해서 생성된 컬럼의 인덱스는 성능향상을 위해서 더는 인덱스를 생성하지 않게 되었습니다. 데이터베이스 마이그레이션 도중에 morphs 메소드를 사용하는 경우, 마이그레이션의 down 메소드를 실행하도록 시도하면 에러가 확인됩니다. 애플리케이션이 아직 개발중이라면, migrate:fresh 명령어를 사용하여 데이터베이스의 스키마를 새롭게 구성할 수 있습니다. 애플리케이션이 프로덕션 환경에 있다면, morphs 메소드에 명시적으로 인덱스 이름을 전달해야 합니다.

MigrationRepositoryInterface 메소드 추가
MigrationRepositoryInterface 에 getMigrationsBatches 메소드가 추가되었습니다. 인터페이스를 직접 구현한 경우라면, 이 메소드를 추가해야 합니다. 프레임워크에 구현된 기본사항을 참고하십시오.

<br>

## Eloquent
---

getDateFormat 메소드
getDateFormat 메소드는 이제 protected에서 public으로 변경되었습니다.

<br>

## 해싱
---

새로운 설정 파일
모든 해싱 설정은 이제 config/hashing.php 설정 파일에 지정됩니다. 애플리케이션에 기본 설정 파일을 복사해서 넣으십시오. Most likely, you should maintain the bcrypt driver as your default driver. However, argon is also supported.

<br>

## 헬퍼
---

헬퍼 함수
이전 버전의 라라벨에서는, 블레이드 템플릿(그리고 e 헬퍼 메소드에서는) HTML 엔티티를 이중으로 인코딩 하지 않았습니다. 이는 htmlspecialchars 함수의 기본 동작이 아니었으며, 콘텐츠를 렌더링 할 때나, 자바스크립트 프레임워크에 JSON 데이터를 전달할 때 예기치 않은 동작을 일으킬 수 있습니다.

라라벨 5.6에서는 블레이드 템플릿과 e 헬퍼는 기본적으로 특수 문자를 이중으로 인코딩 합니다. 이로 인해서 이 기능의 기본 동작이 htmlspecialchars 함수 동작과 달라집니다. 이중 인코딩을 원하지 않고 이전의 동작을 유지하려면, e 헬퍼 함수의 두번째 인자에 false를 전달하면 됩니다:

```php
<?php echo e($string, false); ?>
```

<br>

## 로깅
---

새로운 설정 파일
로깅과 관련된 모든 설정은 이제 config/logging.php 설정 파일에 지정됩니다. 애플리케이션에 기본 설정 파일을 복사해서 넣고, 필요한 설정을 구성하십시오.

config/app.php 설정 파일의 log 와 log_level 설정 옵션은 제거되었습니다.

configureMonologUsing 메소드
애플리케이션에서 Monolog 인스턴스를 커스터마이징 하기 위해서 configureMonologUsing 메소드를 사용했다면, custom 로그 채널을 생성하십시오. 커스텀 채널을 생성하기 위한 보다 자세한 내용은 로깅 문서를 참고하십시오.

<br>

## Log Writer 클래스
---

Illuminate\Log\Writer 클래스는 Illuminate\Log\Logger 으로 이름이 변경되었습니다. 애플리케이션의 클래스중 하나에서 이 클래스를 의존하고 있었다면, 새로운 이름으로 변경해야 합니다. 또는, 대신에, Psr\Log\LoggerInterface 인터페이스를 사용하는 것을 고려해보십시오.

Illuminate\Contracts\Logging\Log 인터페이스
이 인터페이스는 Psr\Log\LoggerInterface 인터페이스의 복제본과 같으므로, 제거되었습니다. 대신에 Psr\Log\LoggerInterface 인터페이스를 사용하십시오.

<br>

## 메일
---

withSwiftMessage 콜백
이전 버전의 라라벨에서는 withSwiftMessage 메소드를 호출하여 등록된 Swift 메세지를 커스터마이징 하는 콜백은 내용이 이미 인코딩되어 메세지에 추가된 다음에 호출되었습니다. 이 콜백은 이제 콘텐츠가 추가되기 전에 호출되므로, 필요한 경우 인코딩이나, 기타 메세지 옵션을 커스터마이징 할 수 있습니다.

<br>

##페이지네이션
---

Bootstrap 4
페이지네이터의 의해서 생성되는 페이지 링크는 이제 기본적으로 부트스트랩 4버전에 호환됩니다. 페이지네이터가 부트스트랩 3 링크를 생성하도록 하려면, AppServiceProvider 의 boot 메소드에서 Paginator::useBootstrapThree 메소드를 호출하십시오:

```php
<?php

namespace App\Providers;

use Illuminate\Pagination\Paginator;
use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Bootstrap any application services.
     *
     * @return void
     */
    public function boot()
    {
        Paginator::useBootstrapThree();
    }
}
```
<br>

## Resources
---
original 속성
resource responses-응답의 original 속성은 이제 JSON 문자열 / 배열 대신에 원래의 모델로 설정됩니다. 이를 통해서 테스팅 과정에서 response-응답의 모델을 확인하기가 수월해집니다.

<br>

## 라우팅
---

새롭게 생성된 모델 반환하기
새롭게 생성 된 Eloquent 모델을 라우트에서 직접 반환하게 되면 response-응답의 상태코드가 자동으로 200 대신 201 로 설정됩니다. 애플리케이션의 테스트에서 명시적으로 200 response-응답을 기대하는 경우에 해당 테스트가 201을 예상하도록 수정해야합니다.

신뢰할 수 있는 프록시 설정
Symfony HttpFoundation의 신뢰할 수 있는 프록시 기능이 변경되었기 때문에, 애플리케이션의 App\Http\Middleware\TrustProxies 미들웨어를 조금 수정해야합니다.

이전버전에는 배열이었던 $headers 속성은 이제, 몇가지의 서로 다는 유형의 값을 지정가능합니다. 예를 들어 모든 헤더를 신뢰하려면 $headers 속성을 다음처럼 수정하면 됩니다:

use Illuminate\Http\Request;

```php
/**
 * The headers that should be used to detect proxies.
 *
 * @var int
 */
protected $headers = Request::HEADER_X_FORWARDED_ALL;
```

사용가능한 $headers 값에 대한 보다 다세한 사항은, 신뢰가능한 프록시 문서를 참고하십시오.

<br>

## Validation-유효성 검사
---

ValidatesWhenResolved 인터페이스
$request->validate() 메소드와의 충돌을 피하기 위해서 ValidatesWhenResolved 인터페이스와 / trait-트레이트의 validate 메소드는 validateResolved 으로 이름이 변경되었습니다.

<br>

## 기타
---

또한 laravel/laravel GitHub repository GitHub 저장소에서 변경사항을 확인하는 것이 좋습니다. 이러한 변경사항이 꼭 필요하지는 않지만, 여러분의 애플리케이션을 이 변경사항들에 맞추어 항상 최신의 상태로 유지하고자 할 수도 있습니다. 변경사항 중 일부는 이 업그레이드 가이드에서 다루지만, 설정 파일이나, 설명의 변경같은 경우 일부는 문서에서 기술하지 않을 수도 있습니다. GitHub 에서 Diff 툴을 사용하여 변경사항을 보다 쉽게 확인하고, 필요한 업데이트를 적용할 수도 있습니다.