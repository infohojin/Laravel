---
layout: laravel
title: Laravel
subtitle: Prologue
    
---

암호화
소개하기
설정하기
Encrypter 사용하기

## 소개하기
라라벨의 encrypter AES-256 과 AES-128 암호화를 제공하기 위해서 OpelSSL을 사용합니다. 라라벨에 내장된 암호화 기능은 매우 강력합니다. 따라서 여러분의 "고유한" 암호화 알고리즘을 구성하지 않는 것이 좋습니다. 라라벨의 모든 암호화된 값은 (MAC) 메세지 인증 코드를 사용하여 서명되고, 따라서 한번 암호화 되면 값을 변경할 수 없습니다.


## 설정하기
라라벨의 encrypter 를 사용하기 전에, config/app.php 설정 파일의 key 옵션을 지정해야만 합니다. 여러분은 php artisan key:generate 명령어를 사용하여 이 키를 생성해야 합니다. 이 아티즌 명령어는 키를 생성하는데 PHP의 안전한 랜덤 바이트 제너레이터를 사용합니다. 이 값이 확실하게 설정되지 않으면, 라라벨에 의해서 암호화된 값은 안전하지 않습니다.


## Encrypter 사용하기
하나의 값 암호화하기
encrypt 헬퍼함수를 사용하여 하나의 값을 암호화 할 수 있습니다. 모든 암호화된 값들은 OpenSSL 과 AES-256-CBC 알고리즘이 사용됩니다. 또한 모든 암호화된 값은 변경을 감지하기 위해서 (MAC) 을 기본으로한 서명이 적용됩니다.

```php
<?php

namespace App\Http\Controllers;

use App\User;
use Illuminate\Http\Request;
use App\Http\Controllers\Controller;

class UserController extends Controller
{
    /**
     * Store a secret message for the user.
     *
     * @param  Request  $request
     * @param  int  $id
     * @return Response
     */
    public function storeSecret(Request $request, $id)
    {
        $user = User::findOrFail($id);

        $user->fill([
            'secret' => encrypt($request->secret)
        ])->save();
    }
}
```

## Serialization 없이 암호화하기
암호화를 진행하는 동안 암호화된 값은 serialize를 통해서 전달되어 객체와 배열의 암호화를 가능하게 합니다. 따라서 PHP가 아닌 클라이언트에서 암호화된 값을 받으면 데이터를 unserialize 해야 합니다. 만약 serialize 없이 값을 암호화하고 이를 복호화하려면 Cyprt 파사드의 encryptString 과 decryptString 메소드를 하용하면 됩니다:

```php
use Illuminate\Support\Facades\Crypt;

$encrypted = Crypt::encryptString('Hello world.');

$decrypted = Crypt::decryptString($encrypted);
```

## 값 복호화 하기
decrypt 헬퍼 함수를 사용하여 특정 값을 복호화 할 수 있습니다. MAC 이 일치하지 않는 경우에는, 지정한 값이 정상적으로 복호화 되지 않으며 

```php
Illuminate\Contracts\Encryption\DecryptException이 발생할 것입니다.

use Illuminate\Contracts\Encryption\DecryptException;

try {
    $decrypted = decrypt($encryptedValue);
} catch (DecryptException $e) {
    //
}
```