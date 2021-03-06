---
title: 'Preflight Request'
author: 'vlwkaos'
created: 2021-03-06:20:09:42
published: true
---

HTTP 요청이 일어날 때, 서버는 CORS [Preflight request](https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request)를 통해 현재 보내는 요청에 대한 CORS(Cross-Origin Resource Sharing)정보를 확인한다.

보통 브라우저에서 preflight request를 자동으로 보내기 때문에 프론트엔드에서 직접 만들지 않아도 된다. 또, [매우 단순한 요청](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#simple_requests)은 preflight request를 하지 않는다.

[[이미지 CORS 에러 사례|Preflight request 때문에 겪은 일]] 