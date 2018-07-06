---
layout: laravel
title: Laravel
subtitle: Concepts
cate:
    name: "서비스 컨테이너"
    link: "/Laravel/Concepts"

submenus:
    -
        name: 설계컨셉
        link: /Laravel/Concepts

    -
        name: 서비스 컨테이너
        link: /Laravel/Concepts/Container

    -
        name: 바인딩
        link: /Laravel/Concepts/Container/Binding
    -
        name: 의존성 해결
        link: /Laravel/Concepts/Container/Resolving

    -
        name: 이벤트
        link: /Laravel/Concepts/Container/Events
    -
        name: PSR-11
        link: /Laravel/Concepts/Container/PSR-11
---

## 바인딩
---

### 기본적인 바인딩
대부분의 서비스 컨테이너 바인딩들은 서비스 프로바이더 내에서 등록됩니다. 따라서 이러한 모든 예제들은 해당 컨텍스트에서 컨테이너를 사용하는 데모가 될것입니다.

{tip} 특정 인터페이스에 대한 의존성이 없을 때에는 컨테이너에 클래스를 바인딩 할 필요는 없습니다. 이러한 객체들은 리플랙션에 의해서 자동으로 의존성이 해결되기 때문에, 컨테이너가 각각의 객체들이 어떻게 생성될지 알 필요는 없습니다.

### 간단한 바인딩
서비스 프로바이더 안에서는 항상 $this->app 속성을 통해서 컨테이너 인스턴스에 접근 할 수 있습니다. 또한 bind 메소드를 사용하여 클래스나 인터페이스 이름에 대한 의존성을 우리가 원하는 클래스의 인스턴스를 반환하는 Closure를 등록하여 바인딩 할 수 있습니다.

```
$this->app->bind('HelpSpot\API', function ($app) {
    return new HelpSpot\API($app->make('HttpClient'));
});
```

클로저에서 컨테이너 자신을 인자로 전달 받고 있다는 것에 유의하십시오. 이를 통해서 연결된 객체의 의존성 문제를 위해서 컨테이너 자신을 사용할 수 있습니다.

## 싱글톤으로 바인딩하기
---
singleton 메소드로 클래스나 인터페이스를 바인딩 하면 컨테이너는 한번만 해당 의존성을 해결합니다. 싱글톤 바인딩으로 의존성이 해결되면, 컨테이너의 다른 부분에서 호출 될 때 동일한 객체 인스턴스가 반환될 것입니다.

```
$this->app->singleton('HelpSpot\API', function ($app) {
    return new HelpSpot\API($app->make('HttpClient'));
});
```

## 인스턴스를 바인딩하기
---
instance 메소드를 사용하여 이미 존재하는 객체의 인스턴스를 컨테이너에 바인딩 할 수 있습니다. 이후 컨테이너에서 호출이 될때는 매번 주어진 인스턴스가 반환됩니다.

```php
$api = new HelpSpot\API(new HttpClient);

$this->app->instance('HelpSpot\API', $api);
```

### 기본 타입 바인딩

때로는, 클래스가 주입되는 클래스들을 받아 들일 수도 있지만, 정수형과 같은 기본타입의 값들을 주입할 필요가 있을 수도 있습니다. 여러분은 손쉽게 문맥에 따라 조건적 바인딩을 통해서 클래스가 필요한 값을 주입할 수 있습니다:

```php
$this->app->when('App\Http\Controllers\UserController')
          ->needs('$variableName')
          ->give($value);
```

### 인터페이스에 구현객체 바인딩하기

서비스 컨테이너의 강력한 기능중 하나는 주어진 구현 객체에 인터페이스를 바인딩 할 수 있다는 것입니다. 예를 들어 EventPusher 인터페이스와 RedisEventPusher 구현이 있다고 가정해 보겠습니다. 이 인터페이스를 구현한 RedisEventPusher 객채를 구성한 뒤에 이 객체를 다음과 같이 서비스 컨테이너에 등록할 수 있습니다:

```php
$this->app->bind(
    'App\Contracts\EventPusher',
    'App\Services\RedisEventPusher'
);
```

이것은 구문은 EventPusher 인터페이스의 구현 객체가 필요할 때 컨테이너가 RedisEventPusher 을 주입해준다는 것을 말합니다. 이제 우리는 EventPusher 인터페이스에 대한 타입을 생성자에 지정하면 어디에서라도 서비스 컨테이너가 의존성을 주입해줍니다.

```php
use App\Contracts\EventPusher;

/**
 * Create a new class instance.
 *
 * @param  EventPusher  $pusher
 * @return void
 */
public function __construct(EventPusher $pusher)
{
    $this->pusher = $pusher;
}
```

### 문맥에 따른 조건적 바인딩
때때로 동일한 인터페이스에 대한 2가지 구현 객체가 있고, 각각의 클래스마다 다른 구현 객체를 전달하고자 할 수도 있습니다. 예를 들어 각각의 컨트롤러가 다른 Illuminate\Contracts\Filesystem\Filesystem contract 구현체에 의존한다면, 라라벨은 간단하고 유연한 인터페이스를 통해서 다음 행동을 정의합니다.

```php
use Illuminate\Support\Facades\Storage;
use App\Http\Controllers\PhotoController;
use App\Http\Controllers\VideoController;
use Illuminate\Contracts\Filesystem\Filesystem;

$this->app->when(PhotoController::class)
          ->needs(Filesystem::class)
          ->give(function () {
              return Storage::disk('local');
          });

$this->app->when(VideoController::class)
          ->needs(Filesystem::class)
          ->give(function () {
              return Storage::disk('s3');
          });
```

## 태깅
---
가끔은, 바인딩의 특정 "카테고리" 전체에 대한 의존성 해결을 해야 할 때도 있습니다. 예를 들어, 서로 다른 Report 인터페이스의 구현 객체를 포함하는 배열을 전달 받는 보고서 수집기를 개발하고 있다고 해봅시다. Report 구현 객체를 등록한 뒤에, tag 메소드를 사용하여 태그를 달 수 있습니다:

```php
$this->app->bind('SpeedReport', function () {
    //
});

$this->app->bind('MemoryReport', function () {
    //
});

$this->app->tag(['SpeedReport', 'MemoryReport'], 'reports');
```
서비스에 태그가 붙으면 tagged 메소드를 사용하여 손쉽게 의존성을 해결할 수 있습니다.

```php
$this->app->bind('ReportAggregator', function ($app) {
    return new ReportAggregator($app->tagged('reports'));
});
```

## 바인딩 확장
extend 메서드로 서비스의 의존성을 수정할 수 있습니다. 예를 들어, 서비스의 의존성이 해결되었을때, 서비스를 꾸미거나(decorate) 혹은 설정하는 위한 추가 코드를 실행할 수 있습니다.

```php
$this->app->extend(Service::class, function($service) {
    return new DecoratedService($service);
});
```

