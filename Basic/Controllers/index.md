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

컨트롤러

소개
기본적인 컨트롤서
    컨트롤러 정의
    컨트롤러 & 네임스페이스
    단일 동작 컨트롤러
컨트롤러 미들웨어
리소스 컨트롤러
    Resource 라우트의 일부만 지정하기
    리소스 라우트 이름 지정하기
    리소스 라우트 파라미터 이름 지정하기
    리소스 URI의 지역화(다국어 동사처리)
    Resource 컨트롤러 라우트에 추가하기
의존성 주입 & 컨트롤러
라우트 캐싱

# 소개
---
애플리케이션의 요청에 대한 모든 처리 로직을 하나의 routes.php 파일에 정의하는 것 보다 별도의 컨트롤러 클래스를 통해서 구성할 수도 있습니다. 컨트롤러는 클래스를 구성하여 HTTP 요청에 대한 그룹을 지정합니다. 컨트롤러는 app/Http/Controllers 디렉토리에 저장 됩니다.

