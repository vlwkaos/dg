---
title: '이미지 CORS 에러 사례'
author: 'vlwkaos'
created: 2020-09-20:22:08:04
published: true
---

[[HTTP Cache|HTTP 요청은 성능을 위해 브라우저에서 캐싱할 수 있다]]. 

대부분의 브라우저는 캐싱할 때 [Access-Control-Max-Age](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Max-Age#examples)에 기본값을 설정한다. 이로써 [[Preflight Request]]도 같이 캐싱된다. 

HTTP 캐시는 Method와 URI에 대해 발생하므로 같은 URI를 가진 파일 리소스를 다른 용도로 요청할 때 캐싱된 결과에 의해 CORS가 발생할 수 있다.

하나 예를 들자면 이미지 파일을 HTML 이미지 태그의 src 속성을 이용하여 클라이언트로 가져온 경우다. [이미지 태그 src의 요청은 CORS 대상이 아니다](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#what_requests_use_cors). 고로 이미지 태그로 파일을 불러오면 제대로된 CORS 설정 값 없이 서버에 요청을 하게된다. 

해당 이미지 파일를 HTML 이미지가 아닌 다른 용도(예를 들어 SVG파일을 파싱 하기 위해 가져온다던가)로 쓰기 위해 다른 요청을 날리면 이전에 캐싱된 결과에 의해 CORS에러가 발생하게 된다.

이를 해결할 수 있는 가장 직관적인 방법 두가지는 다음과 같다.

1. HTML 이미지가 파일을 요청하기 전에 CORS 설정 값이 있는 요청을 먼저 한다.
2. 캐싱되지 않은 요청으로 받기 위해 파일의 URI를 `'?_'` 등을 붙여 요청한다.