---
layout: laravel
title: Laravel
subtitle: Prologue
    
---

캐시
설정하기
드라이버 사전 준비사항
캐시 사용법
캐시 인스턴스 획득하기
캐시에서 아이템 찾기
캐시에 아이템 저장하기
캐시에서 아이템 삭제하기
캐시 헬퍼 함수
캐시 태그
태그가 추가된 캐시 아이템 저장하기
태그로 캐시 아이템 엑세스하기
태그가 추가된 캐시 아이템 삭제하기
사용자 정의 캐시 드라이버 추가하기
드라이버 작성하기
드라이버 등록하기
이벤트

## 설정하기
라라벨은 다양한 캐시 시스템을 위해서 풍부한 표현이 가능한 통합 API를 제공합니다. 캐시 설정은 config/cache.php 파일에 있습니다. 이 파일에 전체 애플리케이션에서 기본적으로 사용할 캐시 드라이버를 지정할 수 있습니다. 라라벨은 Memcached 나 Redis 와 같은 인기 있는 백엔드 캐시 시스템을 지원합니다.

캐시 설정 파일은 다양한 여러 옵션을 제공하고 있습니다. 각각의 옵션에 설명이 있으니 참고하시기 바랍니다. 라라벨의 기본 캐시 드라이버는 시리얼라이즈된 데이타를 파일 시스템에 저장하는 file 캐시 드라이버로 설정되어 있습니다. 보다 큰 응용프로그램은 Memcached 나 Redis 와 같이 보다 견고한 캐시 드라이버를 사용하기를 권장합니다. 동일한 드라이버를 위한 다수의 캐시 설정을 구성할 수도 있습니다.


## 드라이버 사전 준비사항
데이터베이스
database 캐시 드라이버를 사용할 때에는, 캐시 정보를 담아둘 테이블을 구성할 필요가 있습니다. 다음과 같이 테이블을 구성하는 Scheme를 확인하십시오.

```php
Schema::create('cache', function ($table) {
    $table->string('key')->unique();
    $table->text('value');
    $table->integer('expiration');
});
```

{tip} 적절한 스키마 마이그레이션을 생성하기 위해 php artisan cache:tableArtisan 명령어를 사용할 수도 있습니다.

## Memcached
Memcached 드라이버를 사용하려면 Memcached PECL 패키지가 설치되어 있어야 합니다. config/cache.php 설정 파일안에서 Memcache 서버 목록을 나열하면 됩니다.

```php
'memcached' => [
    [
        'host' => '127.0.0.1',
        'port' => 11211,
        'weight' => 100
    ],
],
```

또한 host 옵션으로 UNIX 소켓을 설정할 수 있습니다. 이 경우 port 옵션은 0 으로 구성해야 합니다.

```php
'memcached' => [
    [
        'host' => '/var/run/memcached/memcached.sock',
        'port' => 0,
        'weight' => 100
    ],
],
```

## Redis
라라벨에서 Redis 캐스를 사용하기 전에, 컴포저를 통해서 predis/predis 패키지(~1.0)를 설치하거나, PECL을 통해서 PhpRedis PHP Extension을 설치해야 합니다.

Redis 설정과 관련된 보다 자세한 사항은 라라벨 Redis 문서를 참고하십시오.


캐시 사용법

캐시 인스턴스 획득하기
Illuminate\Contracts\Cache\Factory 와 Illuminate\Contracts\Cache\Repository contracts는 라라벨의 캐시 서비스에 대한 엑세스를 제공합니다. Factory contract 는 애플리케이션에서 정의하고 있는 모든 캐시 드라이버를 제공합니다. Repository 는 일반적으로 cache 설정 파일에서 기본으로 설정된 캐시 드라이버의 구현체입니다.

하지만 언제라도 Cache 파사드를 사용 할 수 있습니다. Cache 파사드는 라라벨의 캐시 contract 를 활용하여 캐시에 대한 편리하고 간결한 엑세스를 제공합니다:

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Support\Facades\Cache;

class UserController extends Controller
{
    /**
     * Show a list of all users of the application.
     *
     * @return Response
     */
    public function index()
    {
        $value = Cache::get('key');

        //
    }
}
```

## 여러 개의 캐시 Store 사용하기
Cache 파사드를 사용하면서 store 메소드를 통해서 여러 개의 캐시 Store 에 엑세스 할 수 있습니다. store 메소드에 전달되는 키는 cache 설정 파일에서 stores 배열에 들어 있는 store 중 하나여야 합니다.

```php
$value = Cache::store('file')->get('foo');

Cache::store('redis')->put('bar', 'baz', 10);
```

## 캐시에서 아이템 조회하기
Cache 파사드에서 get 메소드를 사용하여 캐시에 들어 있는 아이템을 찾을 수 있습니다. 아이템이 캐시에 없다면, null 이 반환될 것입니다. 아이템이 존재하지 않을 때 기본값을 반환할 수 있도록 get 메소드의 두번째 인자로 전달할 수 있습니다.

```php
$value = Cache::get('key');

$value = Cache::get('key', 'default');
```

기본값을 클로저로 전달할 수도 있습니다. 캐시에 지정한 아이템이 존재하지 않을 경우에 클로저의 반환값이 결과값이 됩니다. 클로저를 전달하면 데이터베이스와 외부 서비스로부터 기본값을 획득할 수 있습니다.

```php
$value = Cache::get('key', function () {
    return DB::table(...)->get();
});
```

## 아이템이 존재하는지 확인하기
has 메소드는 캐시에 아이템이 존재하는지 확인하는데 사용됩니다. 만약 값이 null 이거나 false라면 이 메소드는 false를 반환합니다:

```php
if (Cache::has('key')) {
    //
}
```

## 증가 / 감소
increment 와 decrement 메소드는 캐시에 들어 있는 정수형 아이템의 값을 변경하는데 사용합니다. 두 메소드는 모두 해당 아이템의 값을 얼마나 증가 또는 감소 시킬지 결정하는 두 번째 인자를 선택적으로 전달할 수 있습니다.

```php
Cache::increment('key');
Cache::increment('key', $amount);
Cache::decrement('key');
Cache::decrement('key', $amount);
```

## 조회 & 저장
때로는 캐시로부터 아이템을 찾는 것 뿐만 아니라, 찾으려는 아이템이 존재하지 않는 경우 기본값을 저장하고자 할 수도 있습니다. 예를 들어 캐시에서 모든 사용자를 찾으려 할 때 아무 것도 없다면, 데이터베이스로부터 정보를 획득하여 캐시에 추가하고자 할 수도 있습니다. 이 경우에 Cache::remember 메소드를 사용하면 됩니다.

```php
$value = Cache::remember('users', $minutes, function () {
    return DB::table('users')->get();
});
```

캐시에 아이템이 존재하지 않는 경우, remember 메소드에 전달된 클로저 가 실행되어 캐시에 값을 저장하게 됩니다.

어떤 아이템을 조회하고 이를 영원히 기억하도록 rememberForever 메소드를 사용할 수 있습니다:

```php
$value = Cache::rememberForever('users', function() {
    return DB::table('users')->get();
});
```

## 조회 & 삭제하기
캐시에서 아이템을 찾은 뒤에 해당 아이템을 삭제할 필요가 있는 경우에는 pull 메소드를 사용하면 됩니다. get 메소드와 마찬가지로, 캐시에서 아이템이 존재하지 않는 경우에는 null 이 반환됩니다.

```php
$value = Cache::pull('key');
```

## 캐시에 아이템 저장하기
Cache 파사드의 put 메소드를 사용하여 캐시에 아이템을 저장할 수 있습니다. 캐시에 아이템을 저장할 때에는, 몇 분 동안 저장해야 할지 지정해야 합니다.

```php
Cache::put('key', 'value', $minutes);
```

몇 분동안 저장해야 할지 정수형의 숫자를 전달하는 대신에, 캐시된 아이템의 유효 기간을 나타내는 DateTime 인스턴스를 전달할 수 있습니다.

```php
$expiresAt = now()->addMinutes(10);

Cache::put('key', 'value', $expiresAt);
```

## 존재하지 않으면 저장하기
add 메소드는 캐시에 아이템이 이미 존재하지 않는 경우에만 아이템을 저장합니다. 캐시에 아이템이 잘 저장되었다면 메소드가 true를 반환할 것입니다. 그렇지 않은 경우 false 를 반환할 것입니다.

```php
Cache::add('key', 'value', $minutes);
```

## 아이템들을 영구적으로 저장하기
forever 메소드는 캐시에 아이템을 영구적으로 저장하고자 할 때 사용됩니다. 이 아이템들은 자동으로 만료되지 않기 때문에, forget 메소드를 사용하여 캐시에서 수동으로 삭제해주어야만 합니다.

```php
Cache::forever('key', 'value');
```

{tip} 만약 Memcached 드라이버를 사용중이라면, "영구적으로" 저장된 아이템들은 캐시의 사이즈 제한에 도달한 경우 제거될 것입니다.


## 캐시에서 아이템 삭제하기
forget 메소드를 사용하여 캐시에서 아이템을 삭제할 수 있습니다:

```php
Cache::forget('key');
```

flush 메소드를 사용하면 전체 캐시를 삭제합니다.

```php
Cache::flush();
```

{note} 모든 캐시를 비우는 것은 캐시에서 모든 항목이 제거된다는 것을 의미합니다. 애플리케이션의 다른 부분에서 공유하는 캐시를 제거할 때에 주의하십시오.


## 캐시 헬퍼 함수
Cache 파사드나 캐시 contract를 사용하는것에 더하여, 글로벌 cache 함수를 사용하여 캐시에서 데이터를 조회하거나 저장할 수 있습니다. cache 함수에 인자가 하나의 문자열일 때는, 주어진 키에 대한 값을 반환합니다: $value = cache('key');

키와 값으로된 배열과 유효시간을 함수에 전달하면, 지정된 기간동안 캐시에 값을 저장하고 있습니다:

```php
cache(['key' => 'value'], $minutes);

cache(['key' => 'value'], now()->addSeconds(10));
```

{tip} 글로벌 cache 함수 호출을 테스팅할 때에는, 파사드 테스팅을 참고하여 Cache::shouldReceive메소드를 사용할 수 있습니다.


## 캐시 태그
{note} 캐시 태그는 file과 database 드라이버를 사용하는 경우 지원되지 않습니다. 또한 "forever"로 저장되는 캐시에 많은 태그를 사용하면 기존 레코드를 자동으로 삭제하는 memcached 와 같은 드라이버에서 최상의 성능을 낼 것입니다.


## 태그가 추가된 캐시 아이템 저장하기
캐시 태그는 캐시에 있는 관련된 아이템들을 태그할 수 있도록 해주고, 주어진 태그가 지정된 전체 캐시 항목을 삭제할 수도 있게 해줍니다. 여러분은 순서대로 태그 이름의 배열을 전달하여 태그가 추가된 캐시 아이템에 엑세스할 수 있습니다. 예를 들어, 태그가 지정된 캐시에 엑세스하여, 값을 저장(put) 해 보겠습니다.

```php
Cache::tags(['people', 'artists'])->put('John', $john, $minutes);

Cache::tags(['people', 'authors'])->put('Anne', $anne, $minutes);
```

## 태그된 캐시 아이템 엑세스하기
태그가 지정된 캐시 아이템을 찾으려면 tags 메소드에 저장된 것과 동일한 순서로 태그의 목록을 전달하고, 찾고자 하는 키와 함께 get 메소드를 호출하면 됩니다.

```php
$john = Cache::tags(['people', 'artists'])->get('John');

$anne = Cache::tags(['people', 'authors'])->get('Anne');
```

## 태그된 캐시 아이템 제거하기
태그 또는 태그 목록이 지정된 아이템을 모두 지울 수 있습니다. 예를 들어 다음 코드는 people 또는 authors 또는 둘 모두에 태그가 지정된 모든 캐시된 항목이 삭제됩니다. 그러므로 "Anne"과 "John" 두 아이템은 캐시에서 제거됩니다:

```php
Cache::tags(['people', 'authors'])->flush();
```

한편, 다음 코드에서는 authors 태그된 캐시만 삭제되기 때문에 Anne은 삭제되지만 John는 남아 있습니다:

```php
Cache::tags('authors')->flush();
```

## 사용자 정의 캐시 드라이버 추가하기

드라이버 작성하기
여러분의 고유한 캐시 드라이버를 생성하기 위해서는 먼저 Illuminate\Contracts\Cache\Store contract 을 구현할 필요가 있습니다. 따라서 생성하려는 MongoDB 캐시는 다음과 같은 형태가 될 것입니다.

```php
<?php

namespace App\Extensions;

use Illuminate\Contracts\Cache\Store;

class MongoStore implements Store
{
    public function get($key) {}
    public function many(array $keys);
    public function put($key, $value, $minutes) {}
    public function putMany(array $values, $minutes);
    public function increment($key, $value = 1) {}
    public function decrement($key, $value = 1) {}
    public function forever($key, $value) {}
    public function forget($key) {}
    public function flush() {}
    public function getPrefix() {}
}
```

이제 MongoDB 연결을 사용하여 각각의 메소드를 구현해야 합니다. 각각의 메소드가 어떻게 구현되는가에 대한 예시로, 프레임워크 소스 코드 상에서 Illuminate\Cache\MemcachedStore 를 살펴보도록 하겠습니다. 구현을 완료하면 사용자 정의 드라이버를 등록할 수 있습니다.

```php
Cache::extend('mongo', function ($app) {
    return Cache::repository(new MongoStore);
});
```

{tip} 만약 여러분이 구성한 캐시 드라이버 코드를 어디에 놓아둘지 고민된다면, app 디렉토리 안에 Extensions 네임스페이스를 만들 수도 있습니다. 하지만, 라라벨은 엄격한 애플리케이션 구조를 가지고 있지 않기 때문에, 어느 곳이든 여러분이 설정하고자 하는 곳에 코드를 둘 수 있다는 점을 기억하세요.


## 드라이버 등록하기
사용자 정의 캐시 드라이버를 라라벨에 등록하기 위해서, Cache 파사드의 extend 메소드를 사용할 것입니다. Cache::extend 호출은 라라벨 애플리케이션 설치시 제공되는 App\Providers\AppServiceProvider의 boot 메소드를 사용하거나, extension 을 제공하는 고유한 서비스 프로바이더를 생성해서 쓸 수 있습니다. - 프로바이더를 config/app.php 파일 안에 있는 프로바이더 배열에 등록하는 것을 잊지 마십시오:

```php
<?php

namespace App\Providers;

use App\Extensions\MongoStore;
use Illuminate\Support\Facades\Cache;
use Illuminate\Support\ServiceProvider;

class CacheServiceProvider extends ServiceProvider
{
    /**
     * Perform post-registration booting of services.
     *
     * @return void
     */
    public function boot()
    {
        Cache::extend('mongo', function ($app) {
            return Cache::repository(new MongoStore);
        });
    }

    /**
     * Register bindings in the container.
     *
     * @return void
     */
    public function register()
    {
        //
    }
}
```

extend 메소드에 전달되는 첫 번째 인자는 드라이버의 이름입니다. 이 이름은 config/cache.php 설정 파일의 driver 옵션에 해당합니다. 두 번째 인자는 Illuminate\Cache\Repository 인스턴스를 반환하는 클로저가 됩니다. 클로저에는 $app 인스턴스가 전달되는데, 이 인자는 서비스 컨테이너의 인스턴스입니다.

extension이 등록되고 나면, config/cache.php 설정 파일의 driver 옵션을 추가한 extension의 이름으로 변경하면 됩니다.


## 이벤트
캐시가 동작할 때에 특정한 코드를 실행하기 위해서는 캐시에 의해 실행되는 이벤트 리스너를 등록해야 합니다. 일반적으로 이벤트 리스너에 대한 코드는 EventServiceProvider안에 구성합니다.

```php
/**
 * The event listener mappings for the application.
 *
 * @var array
 */
protected $listen = [
    'Illuminate\Cache\Events\CacheHit' => [
        'App\Listeners\LogCacheHit',
    ],

    'Illuminate\Cache\Events\CacheMissed' => [
        'App\Listeners\LogCacheMissed',
    ],

    'Illuminate\Cache\Events\KeyForgotten' => [
        'App\Listeners\LogKeyForgotten',
    ],

    'Illuminate\Cache\Events\KeyWritten' => [
        'App\Listeners\LogKeyWritten',
    ],
];
```
