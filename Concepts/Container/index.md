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

## 서비스 컨테이너

## 소개
라라벨의 서비스 컨테이너는 클래스의 의존성을 관리하고 의존성을 주입하는 강력한 도구 입니다. 의존성 주입이라는 멋진 말의 의미는 다음과 같습니다: 클래스간의 의존성은 클래스 생성될 때 또는 경우에 따라 "setter" 메소드에 의해서 "주입" 된다는 의미입니다.

간단한 예제를 들어 봅시다:

```php
<?php

namespace App\Http\Controllers;

use App\User;
use App\Repositories\UserRepository;
use App\Http\Controllers\Controller;

class UserController extends Controller
{
    /**
     * The user repository implementation.
     *
     * @var UserRepository
     */
    protected $users;

    /**
     * Create a new controller instance.
     *
     * @param  UserRepository  $users
     * @return void
     */
    public function __construct(UserRepository $users)
    {
        $this->users = $users;
    }

    /**
     * Show the profile for the given user.
     *
     * @param  int  $id
     * @return Response
     */
    public function show($id)
    {
        $user = $this->users->find($id);

        return view('user.profile', ['user' => $user]);
    }
}
```

이 예제에서 UserController는 데이터 소스로 부터 사용자를 조회할 필요가 있습니다. 따라서 우리는 사용자를 조회할 수 있는 서비스를 주입 할 것입니다. 여기에서는, UserRepository 가 Eloquent를 사용하여 데이터 베이스로 부터 사용자 정보를 조회합니다. repository 가 주입되었기 때문에, 원하는 경우 손쉽게 다른 구현 객체로 변경할 수 있습니다. 또한 애플리케이션을 테스트 할 때 손쉽게 "목킹" 하거나, 더미 UserRepository 구현체를 생성할 수도 있습니다.

라라벨 서비스 컨테이너를 깊이 이해하는 것은 강력하고 큰 애플리케이션을 구축 할 때나 라라벨 코어에 공헌하기 위해서 아주 중요한 부분입니다.


