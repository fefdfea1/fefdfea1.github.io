---
emoji: 🎯
title: Next JS의 SSR CSR 설정 및 차이점
date: '2024-05-26 18:10:00'
author: fefdfea
tags: Next _Js Javascript
categories: Next_JS
---

## 🙄 SSR & CSR에 대하여

이번 포스팅은 Next JS의 SSR 및 CSR에 대한 글을 작성하고자 한다. 하지만 그전에 SSR과 CSR에 대해 가볍게 어떤 개념인지 알아보고 넘어가도록 하자

### SSR이란?

> SSR은 Server-side-Rendering의 약자로 React,Vue와 같은 CSR과는 반대되는 개념이다. SSR은 서버에서 HTML,CSS,JS를 연결하고 랜더링 준비를 마치면 클라이언트에서는 이미 준비된 HTML을 받아 그대로 랜더링을 하는 것이다.

### CSR이란?

> CSR은 위의 SSR의 반대되는 개념으로 빈 HTML을 서버로 받아와 클라이언트에서 함께 받아온 JS 파일을 파싱하여 빈 HTML에 동적으로 삽입하여 만드는 방식이다.

## 😶 이런 방식에는 어떤 차이가 있을까?

SSR과 CSR은 각각의 명확한 장단점을 가진다. 그 장단점은 아래와 같다.

- ### CSR 장점
- View 렌더링을 브라우저에게 담당시킹으로 서버의 부하가 줄어든다.
- 네이티브 앱과 같은 경험을 제공한다.( 부드러운 페이지 전환 )

<br>

- ### CSR 단점
- 첫 로딩 속도가 SSR에 비해 느리다.
- 검색엔진 최적화가 힘들다.

<br>

<hr>

- ### SSR 장점
- CSR에 비해 초기 로딩 속도가 빠르다
- SEO 최적화에 유리하다

- ### SSR 단점
- CSR과 같은 사용자 경험을 제공 할 수 없다.
- 초기 로딩 이후 페이지 전환시 새로고침이 되는 형태로 초기 로딩 이후 속도가 느리다

## 🫠 CSR의 단점을 극복하기 위해 만들어진 Next와 Nuxt

현제 필자가 포스팅 하고 있는 Next가 바로 CSR의 단점을 극복하기 위해 만들어진 프레임워크라고 할 수 있다. Next Js는 기본적으로 리엑트를 기반으로 제작하지만 SSR 방식으로 동작하며 원한다면 해당 페이지만을 CSR로 변경하여 사용 할 수 있고 서버코드 또한 프레임워크 안에 존재하는 api라는 폴더를 통해 엔드포인트를 생성하여 코드를 작성 할 수 있다. 이러한 점 때문에 CSR과 SSR 부분이 섞이는 요즘 사이트에서 Next의 인기가 상승하고 있다고 생각한다.