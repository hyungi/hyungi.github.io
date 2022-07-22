---
layout: post
title: Http Study
date: 2022-03-01 12:01
tags: [http,http2,study]
toc: true
---

# HTTP 의 네가지 요소
- 메소드와 경로
- 헤더
- 바디
- 스테이터스 코드

# HTTP/1.1
- 통신 고속화
  - Keep-Alive
  - pipelining
- TLS 에 의한 암호화 통신 지원
- 새 메소드 추가
  - PUT, DELETE: 필수 메소드
  - OPTION, TRACE, CONNECT 추가
- 프로토콜 업그레이드
- 이름을 이용한 가상 호스트 지원
- 크기를 사전에 알 수 없는 콘텐츠의 청크 전송 인코딩 지원

## Keep-Alive
- http 의 하위 계층인 tcp/ip(L4  전송 계층) 통신을 효율화 하는 구조
- 동일한 클라이언트와 지속적으로 통신시 Keep-Alive 설정을 통해 Connection 을 열고 닫고 하지 않고 지속 사용 가능
- 요청시 아래와 같은 헤더를 요청 헤더에 추가하여 Keep-Alive 를 사용할수 있으며, 서버에서 Keep-Alive를 지원할 경우 동일한 헤더를 응답 헤더에 추가해서 반환함.
```
Connection: Keep-Alive
```
### 구체적인 동작 방식
- TCP 통신의 경우 최초 접속시 1.5회의 왕복통신이 필요함 (종료시는 2회)
  - 연결 요청 > 연결 승인 > ACK (3 way handshake - 3개의 패킷이 송신 되기 때문)
- Keep-Alive를 통해 왕복 통신시 매번 Connection 을 열고 닫고 할 필요가 없게 해줌 



https://livenow14.tistory.com/57

https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-22%ED%8E%B8-TCP-3-WayHandshake-4-WayHandshake