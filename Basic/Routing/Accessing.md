---
layout: laravel
title: Laravel
subtitle: Routing
cate:
    name: "라우팅"
    link: "/Laravel/Basic/Routing"

submenus:
    -
        name: 라우팅
        link: /Laravel/Basic/Routing
    -
        name: 기본적인 라우팅
        link: /Laravel/Basic/Routing/Basic
    -
        name: 라우트 파라미터
        link: /Laravel/Basic/Routing/Parameters
    -
        name: 이름이 지정된 라우트
        link: /Laravel/Basic/Routing/Named
    -
        name: 라우트 그룹
        link: /Laravel/Basic/Routing/Groups
    -
        name: 라우트 모델 바인딩
        link: /Laravel/Basic/Routing/ModelBinding
    -
        name: Rate 제한
        link: /Laravel/Basic/Routing/RateLimiting
    -
        name: Form 메소드 Spoofing-속이기
        link: /Laravel/Basic/Routing/FormMethod
    -
        name: 현재 라우트에 엑세스하기
        link: /Laravel/Basic/Routing/Accessing
---

## 현재 라우트에 엑세스하기
---

Route 파사드의 current, currentRouteName 그리고 currentRouteAction 메소드를 사용하여, 유입된 request-요청을 처리하는 라우트에 대한 정보에 엑세스 할 수 있습니다:

```php
$route = Route::current();

$name = Route::currentRouteName();

$action = Route::currentRouteAction();
```

모든 메소드를 확인하고자 한다면 Route 파사드 뒤에서 동작하는 클래스 와 Route 인스턴스 API 문서를 참고하십시오.