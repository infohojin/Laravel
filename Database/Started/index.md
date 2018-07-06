---
layout: laravel
title: Laravel
subtitle: Prologue
    
---

데이터베이스 : 시작하기
소개하기
설정하기
읽기 & 쓰기 커넥션
여러개의 데이터베이스 커넥션 사용하기
Raw SQL 쿼리 실행하기
쿼리 이벤트 리스닝
데이터베이스 트랙잭션

## 소개하기
---
라라벨은 직접 raw SQL을 사용할 때에도, 쿼리 빌더를 사용하거나 그리고 Eloquent ORM을 사용할 때에도 데이터베이스를 처리하는 것이 쉽도록 해줍니다. 현재 라라벨은 4가지 데이터베이스를 지원하고 있습니다:

* MySQL
* PostgreSQL
* SQLite
* SQL Server

## 설정하기
애플리케이션을 위한 데이터베이스 설정 파일은 config/database.php에 있습니다. 이 파일에서 모든 데이터베이스 커넥션에 대한 설정을 정의하고 기본적으로 사용할 커넥션을 지정할 수 있습니다. 이 파일에서는 지원하는 대부분의 데이터베이스 예제가 들어 있습니다.

기본적으로 라라벨의 샘플 환경 설정은 여러분의 로컬 머신에서 라라벨 개발 환경을 구축할 수 있는 편리한 방법을 제공하는 라라벨 홈스테드에 맞춰져 있습니다. 물론 로컬 데이터베이스에 맞게 변경하시면 됩니다.

## SQLite 설정하기
touch database/database.sqlite 와 같은 명령어를 사용하여 SQLite 데이터베이스를 새롭게 생성한 뒤에, 데이터베이스의 절대 경로를 사용하여 새롭게 생성된 데이터베이스를 지정하는 환경 설정을 손쉽게 추가할 수 있습니다:

```php
DB_CONNECTION=sqlite
DB_DATABASE=/absolute/path/to/database.sqlite
```

### 읽기 & 쓰기 커넥션
SELECT문에서 사용하는 데이터베이스와 INSERT, UPDATE 그리고 DELETE문을 사용하는 데이터 베이스에 다른 연결을 사용할고 싶은 경우도 있습니다. 라라벨은 이를 쉽게 할 수 있습니다. Raw 쿼리를 사용하든, 쿼리 빌더 또는 Eloquent ORM 을 사용하든 적절한 연결들을 사용할 수 있습니다.

다음은 어떻게 read / write 커넥션을 설정하는지에 대한 예제 입니다:

```php
'mysql' => [
    'read' => [
        'host' => ['192.168.1.1'],
    ],
    'write' => [
        'host' => ['196.168.1.2'],
    ],
    'sticky'    => true,
    'driver'    => 'mysql',
    'database'  => 'database',
    'username'  => 'root',
    'password'  => '',
    'charset'   => 'utf8mb4',
    'collation' => 'utf8mb4_unicode_ci',
    'prefix'    => '',
],
```

설정 배열에 read, write 그리고 sticky 세개의 키가 추가 된것을 참고하십시오. read 와 write 키는 host라는 하나의 키를 포함하는 배열 값입니다: read 와 write 의 나머지 데이터베이스 옵션 값은 기본 mysql 배열에서 합쳐(merge)집니다.

따라서 메인 배열에서 재정의하고자하는 값들을 read와 write 배열에 입력하기만 하면 됩니다. 위의 경우에서는 192.168.1.1는 "read(읽기용)" 커넥션에 대한 호스트로 사용되고, 192.168.1.2는 "write(쓰기용)" 커넥션에 대한 호스트로 사용될 것입니다. 메인 mysql설정 배열에 포함된 데이터베이스 연결정보, 프리픽스, 캐릭터 셋 등 다른 모든 옵션들은 양쪽연결에서 모두 공유될 것입니다.

## sticky 옵션
---
sticky 옵션은 현재 request-요청사이클 에서 데이터베이스에 기록된 레코드를 바로 읽을 수 있도록 하는 있어도 되고 없어도 되는 값입니다. sticky 옵션이 활성화 되어 있고 현재 request-요청사이클에서 데이터베이스에 "쓰기" 작업을 수행한 경우 이 뒤에 "읽기"작업은 "쓰기"에서 사용한 커넥션으르 사용합니다. 이렇게 되면, request-요청사이클 동안에 작성된 모든 데이터를 동일한 request-요청 중에서는 바로 데이터베이스에서 읽을 수 있습니다. 여러분의 애플리케이션에서 이러한 동작이 필요한지에 대해서 결정하는 건 여러분의 선택에 따라 달라질 수 있습니다.


## 여러개의 데이터베이스 커넥션 사용하기
여러개의 커넥션을 사용하는 경우, DB 파사드의 connection 메소드를 통해 각각의 커넥션에 액세스 할 수 있습니다. connection 메소드에 전달되는 name은 config/database.php 설정 파일에 나열되어 있는 이름이어야 합니다:

```php
$users = DB::connection('foo')->select(...);
```

또한 커넥션 인스턴스에서 getPdo 메소드를 사용하여 PDO 인스턴스로 액세스 할 수 있습니다:

```php
$pdo = DB::connection()->getPdo();
```
