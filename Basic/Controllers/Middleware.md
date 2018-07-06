---
layout: laravel
title: Laravel
subtitle: Basic
cate:
    name: "기본사항"
    link: "/Laravel/Basic"

submenus:
    -
        name: 기본사항
        link: /Laravel/Basic
    -
        name: 라우팅
        link: /Laravel/Basic/Routing/Basic
    -
        name: 미들웨어
        link: /Laravel/Basic/Middleware
    -
        name: CSRF 보호
        link: /Laravel/Basic/CSRF
    -
        name: 컨트롤러
        link: /Laravel/Basic/Controllers
    -
        name: Requests-요청
        link: /Laravel/Basic/Requests
    -
        name: Responses-응답
        link: /Laravel/Basic/Responses
    -
        name: Views-뷰
        link: /Laravel/Basic/Views
    -
        name: URL 생성
        link: /Laravel/Basic/url
    -
        name: 세션
        link: /Laravel/Basic/session
    -
        name: Validation-유효성검사
        link: /Laravel/Basic/Validation
    -
        name: 에러처리
        link: /Laravel/Basic/Error
    -
        name: 로깅
        link: /Laravel/Basic/Logging
---

## 컨트롤러 미들웨어
---
미들웨어는 다음과 같이 컨트롤러 라우트에 지정할 수 있습니다.

```php
Route::get('profile', 'UserController@show')->middleware('auth');
```

하지만 보다 편리한 방법은 컨트롤러의 생성자에서 미들웨어를 지정하는 것입니다. 컨트롤러의 생성자에서 middleware 메소드를 사용하여 여러분은 손쉽게 컨트롤러에서 사용할 미들웨어를 지정할 수 있습니다. 컨트롤러의 몇몇 메소드에서만 제한하여 미들웨어를 지정할 수도 있습니다.

```php
class UserController extends Controller
{
    /**
     * Instantiate a new controller instance.
     *
     * @return void
     */
    public function __construct()
    {
        $this->middleware('auth');

        $this->middleware('log')->only('index');

        $this->middleware('subscribed')->except('store');
    }
}
```

컨트롤러를 사용하면 Closure를 사용하여 미들웨어를 등록 할 수 있습니다. 이는 전체 미들웨어 클래스를 정의하지 않고 단일 컨트롤러에 대한 미들웨어를 정의하는 편리한 방법을 제공합니다:

```php
$this->middleware(function ($request, $next) {
    // ...

    return $next($request);
});
```

{tip} 컨트롤러 액션의 하위 집합에 미들웨어를 할당 할 수 있습니다. 그러나 컨트롤러가 너무 커질 수 있음을 인지하여야 합니다. 대신 컨트롤러를 여러 개의 작은 컨트롤러로 나누는 것을 고려하세요.


