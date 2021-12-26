---
layout: post 
title: Study about Network protocol 
date: 2021-12-26
tags: [study, http, http2, https, grpc]
toc: true
---

## Long time no see...!

한 두달정도 포스트를 업데이트 안했는데, 그간 두가지 책을 좀 보고 있었습니다.
- [리얼월드 HTTP : 역사와 코드로 배우는 인터넷과 웹 기술](https://www.hanbit.co.kr/store/books/look.php?p_code=B7009240426)
- [gRPC 시작에서 운영까지 [도커와 쿠버네티스를 위한 클라우드 네이티브 애플리케이션 구축] ](http://www.acornpub.co.kr/book/grpc)

### About http

일전에 면접에서 http 관련해서 질문을 여러개 받았는데, http 1.0 과 http 2.0 의 차이 http 와 https 사이의 차이 등을 제대로 대답 못했던 기억이 있어서 시작 하게 되었습니다.

### About grpc

기본적으로 웹에서 데이터를 주고 받을때 지금 까지는 REST API 를 주로 활용을 했는데, REST API 가 under fetching, over fetching 등의 문제가 있어서 graphQL 이라는 기술로 대체를 고려 하기도 하고, 속도가 정말 중요한 경우에는 grpc 를 사용하는 경우도 있다고 해서 알아보게 되었는데, 기존에 protobuf 를 사용하는 통신 자체를 해본적이 있어서 protobuf 의 장점을 극대화 하는 통신을 공부해보았습니다.

### Review Repositories.

 - [리얼월드 HTTP review](https://github.com/hyungi/real_world_http)
   - Code 양 자체가 별로 없습니다, 일단 진도를 그렇게 많이 나간게 아니기도 하고, curl 명령어 등으로 확인해보는 경우가 많아서 repo 에 올릴만한 코드가 적긴합니다.
   - 그래도 오탈자 발견해서 report 했는데 지금 출판사에서 확인중입니다. :-)

 - [gRPC 시작에서 운영까지 review](https://github.com/hyungi/grpc_up_and_running)
   - 챕터 4 까지 지금 진도를 나가긴 했는데, 챕터 4에 기술적인 내용이 많아서 차분히 다시 보는 중입니다.
   - 챕터 4 가 기술적인 설명 위주라 챕터 3 까지만 코드가 올라가 있습니다.
   - grpc 버전이 올라가면서 protoc command 가 바뀌어서 그 부분을 적용하느라 좀 애를 먹었습니다. 그냥 낮은 버전 유지할까 하다가 최신 버전 반영해서 사용해야 하는 command 에 대한 설명 까지 repo 에 올려두었습니다.
   
### Learn and Lesson

실습을 하다보니 많은걸 배울 수 있었고, repo 에 정리해서 올리니 다시 보기도 좋고 그렇네요
http 쪽은 아무래도 linux command 를 쓸일이 더 많다보니, 그냥 여기에 포스트 형식으로 정리하는게 더 나을 수도 있겠다는 생각이 드네요.
좀더 공부해보고 마저 정리해 봐야겠네요.
 